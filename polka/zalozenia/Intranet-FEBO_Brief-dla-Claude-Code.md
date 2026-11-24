<!--
opis: Brief dla agenta Claude Code na start budowy Intranetu FEBO: co budujemy, stos i zasady pracy z kontraktami.
rodzaj: zalozenie
rozmowa: environment
zmiana: 16.10.2026, environment
status: roboczy
slowa: brief,claude code,start,S0
-->
# Brief startowy — Intranet FEBO (dla sesji Claude Code)

**Stan na 17.08.2026 · sprint S0 zamknięty · następne zadanie: T-07**

Ten plik wklejasz na początku sesji albo trzymasz otwarty obok. Zawiera to, czego
agent potrzebuje, żeby wejść w pracę bez zgadywania: kontekst, zasady, stan,
polecenia i pułapki, które już raz kosztowały czas.

---

## 1. Kontekst w pięciu zdaniach

Intranet FEBO to **powłoka aplikacyjna**, w której działają małe aplikacje
dziedzinowe zwane **lokatorami**. Powłoka daje im wspólnie: tożsamość i uprawnienia,
jedną listę zadań, kolejkę, powiadomienia, pliki, wyszukiwarkę, metryki i dziennik
audytu. Zasada przewodnia: aplikacja zaczyna jako narzędzie obsługiwane przez
człowieka i dojrzewa do usługi działającej samodzielnie — **bez przepisywania**.
Prace idą sprintami S0–S9+, a ich kolejność wynika z tego, ile lokatorów każdy
odblokowuje. Pięć aplikacji trafia do użytkowników po sprincie S5.

**Repozytorium:** `git@github.com:SmallBigSteps/intranetFebo.git`, gałąź `main`.
**Kopia robocza:** `/home/ubuntu/febo-intranet` na maszynie `dev.febo.redai.pl`.

---

## 2. Zasada nadrzędna

> **Kolumna w tabeli, pole manifestu i zakres uprawnienia nigdy nie czekają na
> późniejszy sprint. Zachowanie może.**

Pusta kolumna kosztuje zero. Migracja dwudziestu lokatorów kosztuje tydzień.

---

## 3. Stos — zamknięty

| Warstwa | Technologia |
|---|---|
| Backend | Python **3.12 z obrazu kontenera** (maszyna ma 3.14 — nie używamy jej), Django + DRF, Gunicorn + Uvicorn |
| Baza | PostgreSQL 16 |
| Kolejka | **Procrastinate w PostgreSQL** — nigdy Celery, RQ, RabbitMQ ani Redis jako broker |
| Cache i sesje | Redis — **nigdy broker** |
| Frontend | React + TypeScript (strict), Vite, React Router |
| Opis API | drf-spectacular → `api/schema.yaml` → openapi-typescript → typy TS |
| Proxy, kontenery | nginx, Docker Compose |
| Systemy źródłowe | LogTMS (MySQL), Comarch Optima (MS SQL) — **tylko odczyt** |

**Nie dokładaj zależności spoza tej listy bez pytania.** Jeśli karta zadania jej
wymaga, a stos jej nie zna — to sprzeczność: zatrzymaj się i zacytuj oba źródła.

---

## 4. Niezmienniki, które łamie się najczęściej

Pełna lista: `CLAUDE.md` §5 (38 pozycji). Te wracają najczęściej:

- **1, 3** — jedna kolejka, kolejność po priorytecie zadania
- **5** — wpis audytowy zapisuje się **synchronicznie**, w transakcji operacji; nieudany zapis cofa operację
- **8** — każde zapytanie o zadania, audyt i wyniki wyszukiwania idzie przez `visible_to(user)`
- **11** — klucze obce wyłącznie do schematów `core_*`; lokator nie sięga do lokatora
- **15, 16** — LogTMS i Optima: SELECT, egzekwowane **na bazie**; Intranet nigdy nie zapisuje
- **23** — sesje + ciasteczko HttpOnly/Secure/SameSite=Lax; **żadnych tokenów w pamięci przeglądarki**
- **25** — sekrety w zmiennych środowiskowych, **bez wartości domyślnych**
- **26** — dev **nie przechowuje** danych produkcyjnych; odczyt na żądanie wolno, zapis do fixture nie
- **32, 33, 34, 36** — tylko tokeny marki, bez pamięci przeglądarki, status nie zależy od samego koloru, bez cieni
- **37, 37a, 37b** — `test` i `prod` to praca człowieka; na `dev` wolno przygotować środowisko; **nigdy sieć, zapora, usługi systemowe**

---

## 5. Polecenia

```bash
make status          # co jest następne w planie
make check           # lint + typy + testy — brama scalenia
make review          # check + rejestr kontroli + bramka reguł
make up / down / ps  # środowisko uruchomieniowe
make seed PROFILE=x  # dane testowe: minimal, domain, load, edge, governance
make schema          # opis API i typy dla przeglądarki (po każdej zmianie serializatora)
make schema-check    # czy opis API nie rozjechał się z kodem
make migrations-check # czy model nie zmienił się bez migracji
make p6              # test niezależności warstw
make ports           # co jest wystawione na zewnątrz
```

**Zadanie jest skończone, gdy `make check` i `make review` są zielone.**

---

## 6. Pułapki, które już raz kosztowały czas

Każda z nich wyglądała na sukces. To jest ich wspólna cecha i powód, dla którego
są tutaj.

| Pułapka | Objaw | Co zrobić |
|---|---|---|
| **Testy na nieaktualnym kodzie** | zielone testy dla wersji sprzed przebudowy obrazu | katalog `api/` jest podmontowany do kontenera — nie usuwaj tego z `make test` |
| **Sonda gotowości zawsze 200** | `/readyz` odpowiadał stroną startową, także gdy baza leżała | trasy sond muszą trafiać do aplikacji, nie do części przeglądarkowej |
| **Hooki nie działały wcale** | brak reakcji wyglądał na brak naruszeń | dyspozytory liczą korzeń repo jako `parents[2]`; tryb fail-open zamienia błąd importu w ciszę |
| **MS SQL: ciche urwanie ładowania** | „gotowe” mimo braku 3000 wierszy | max 1000 wierszy na INSERT; `sqlcmd` **musi** dostać `-b`, inaczej zwraca sukces po błędzie |
| **Zmiana hasła bazy nie działa wstecz** | baza odmawia połączenia po rotacji sekretu | obraz używa hasła tylko przy pierwszej inicjalizacji; na prod potrzebny `ALTER ROLE`, nie kasowanie danych |
| **Generowany plik psuje bramkę stylu** | `schema.d.ts` wiecznie „niesformatowany” | generowanie kończy się formatowaniem; pliki generowane są poza kontrolą stylu |
| **`make schema` bez `--user`** | odmowa dostępu wskazująca na schemat | kontener pisze jako własne konto; zapis do katalogu roboczego wymaga `--user $(id -u)` |

---

## 7. Kontrole — co blokuje, co ostrzega

Jedna implementacja w `tools/checks/`, trzy wejścia: hooki, `make review`, potok CI.
**Nie pisz logiki kontroli w hooku ani w Makefile.**

- **8 blokujących** (kod wyjścia 2, operacja się nie wykonuje): sekrety, wymuszony push,
  zapis do systemu źródłowego, operacje na test/prod, kasowanie poza projektem, migracja
  na bazie źródłowej, **sieć i usługi systemowe (B7)**, zapis poza katalogiem projektu (A4)
- **13 ostrzegawczych** (nie zatrzymują edycji): pamięć przeglądarki, konwencje nazw,
  wartości stylu poza tokenami, polski tekst w kodzie, audyt przez kolejkę, obiekt modelu
  w argumencie zadania, mutacja wpisu audytowego, cień w komponencie, kategoryczne
  sformułowanie bez mechanizmu, kontrakt bez podniesienia wersji, okoliczność zamiast
  zabezpieczenia
- **11 analitycznych** — osiem raportuje `pending`, bo nie ma jeszcze czego analizować

**Bramka reguł** sprawdza wyłącznie reguły etapu bieżącego i etapów zamkniętych.
Etap czyta się ze zmiennej `CURRENT_STAGE` w potoku, a w jej braku ze stanu prac.
**Test pominięty liczy się jak brak testu.**

---

## 8. Protokół sesji

1. `make status` → weź kartę z `docs/contract/plan.contract.md`
2. **Plan przed edycją.** Każde zadanie zmieniające pliki zaczyna się od odczytu.
3. Czytaj **to, co karta wymienia**, nie cały kontrakt.
4. Jedno zadanie = jedna gałąź = jedna sesja. `/clear` między zadaniami.
5. Koniec: `make check` i `make review` zielone, migracje zatwierdzone,
   `docs/progress.md` zaktualizowany.
6. Scalenie wykonuje **człowiek** po przeglądzie.

**Trzy poziomy odstępstwa:**

- **luka** → wpis w `docs/decisions/open-gaps.md` i pracuj dalej
- **sprzeczność** → **zatrzymaj się**, zacytuj oba źródła
- **zastrzeżenie** → wykonaj zgodnie z zapisem i odnotuj zastrzeżenie

**Utknięcie:** po trzech nieudanych próbach tego samego testu zatrzymaj się i opisz,
co próbowałeś. **Nigdy nie zmieniaj testu, żeby przeszedł.**

**Wersjonowanie założeń:** każda zmiana w `CLAUDE.md`, kontraktach, kartach,
`docs/progress.md` i `.claude/` trafia do repozytorium **tego samego dnia**, podpisana
jako `redAi <redai@febologistic.com>`. Ustalenie, które zostało tylko w rozmowie,
dla agenta nie istnieje.

---

## 9. Stan na dziś

**Sprint S0 zamknięty** — dziesięć zadań: środowisko, monorepozytorium, Compose,
bezpieczeństwo, część przeglądarkowa, potok CI, rejestr kontroli z hookami,
piaskownica z testem niezależności warstw, dane testowe i atrapy.

| Miara | Wartość |
|---|---|
| Testy | 68 serwer · 66 kontrole · 20 przeglądarka |
| Pokrycie | 88% serwer (próg 80) · 78% przeglądarka (próg 60) |
| Środowisko | 8 usług zdrowych, https://dev.febo.redai.pl odpowiada |
| Bramka reguł | 160 reguł, 0 wymaganych na etapie 0 |
| Rejestr luk | 55 wpisów, 21 otwartych |

**Etap bieżący: 0 → od T-07 podnosi się na 1**, co uruchomi 33 reguły domen kont
i audytu. Od tego momentu reguła bez przechodzącego testu blokuje scalenie.

---

## 10. Czego jeszcze nie ma — i co z tego wynika

- **Zapora na maszynie dev** — zgłoszona do redAi, nie ustawiona. Maszyna stoi
  w internecie; chroni ją to, że nasłuchują trzy usługi i żadna nie przyjmuje hasła.
  **W S1 pod tym adresem pojawi się działające logowanie.**
- **Konta odczytowe do prawdziwych LogTMS i Optimy** — pracujemy na makietach.
  Makieta odtwarza strukturę, nigdy osobliwości prawdziwych danych. Potrzebne do T-54, T-56.
- **Test P6 na prawdziwych systemach** — dziś dowód dotyczy makiet.
- **Recenzenci środowiska „produkcja” w GitHubie** — bez nich krok zgody przy wydaniu
  przechodzi bez pytania.
- **Domena reguł celnych `BR-CUS`** nie ma etapu w tabeli — taka reguła nigdy nie trafi
  pod bramkę, choć wygląda na objętą.

---

## 11. Karta następnego zadania — wklej do świeżej sesji

```
Task T-07.

Read first:
  docs/contract/core-accounts.contract.md — sections 3.1, 3.2, 4
  docs/contract/roles-mapping.contract.md — section 1
  docs/contract/rules.contract.md — BR-ACC-007, BR-ACC-009

Scope: User (AbstractBaseUser, not AbstractUser), Position, a data migration
seeding the 17 positions.

Email as login, a manager field with cycle validation on save, external_id as
the AD hook, hr_id, four account states, anonymized_at.

Validation rejecting functional addresses against a pattern list, overridable
by an administrator.

NO account deletion — disabled state and anonymisation only.

Done when: BR-ACC-007 and BR-ACC-009 have passing tests; cycle detection works.
```

**Co to odblokowuje:** ekran logowania i trasy chronione z T-04 przestają być
zapowiedzią; profil danych `minimal` dostaje 17 stanowisk, 6 ról i 5 kont;
kontrola analityczna R1 przestaje raportować `pending`.

**Na co uważać w T-07:** `AbstractBaseUser`, nie `AbstractUser` — karta mówi to
wprost i różnica jest istotna. Konta się **nie kasuje**: stan `disabled` i
anonimizacja. Wykrywanie cykli w polu przełożonego działa przy zapisie, nie przy
odczycie.

---

## 12. Reszta sprintu S1

| Zadanie | Zakres |
|---|---|
| T-08 | Rejestr ról i warstwa uprawnień — zakresy `all` / `own` / `department` |
| T-09 | Dziennik audytu — model i zapis synchroniczny |
| T-10 | Dziennik audytu — widoczność i historia obiektu |
| T-11 | Uwierzytelnianie i polityka haseł — Argon2, min. 12 znaków, blokada 10/15 min |
| T-12 | TOTP i sesje — 12 h bezczynności, odświeżane przy każdym żądaniu |
| T-13 | Cykl życia konta — wyłączenie, anonimizacja, zwolnienie zadań |
| T-14 | Zastępstwa — łańcuch **nieprzechodni** |
| T-15 | Import kont i podłączenie ekranu logowania |
| T-16 | Przeglądarka audytu, eksport, archiwizacja |

**Warunek zamknięcia sprintu:** logowanie działa, role z zakresami danych są
egzekwowane w zapytaniach, dziennik audytu zapisuje każde zdarzenie z aktorem,
a 33 reguły etapu 1 mają przechodzące testy.
