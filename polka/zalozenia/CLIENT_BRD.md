<!--
opis: Dokument wymagań biznesowych dla klienta (13.07.2026): cele, zakres, wymagania platformy i POC, kanoniczny rejestr ryzyk.
rodzaj: zalozenie
rozmowa: zad001PolkiCPrzeniesienieMaterialow
zmiana: 14.10.2026, przeniesione z projects/intranet/context (ZAD-001)
status: zaakceptowany
slowa: BRD, wymagania, rejestr ryzyk, klient, zamówienia surowców
-->
# CLIENT_BRD: Intranet Canexpol Group + Moduł POC „Zamówienia Surowców" (platforma redAI)

**Status:** Zatwierdzony wynik wywiadu elicytacyjnego (Faza 3 — BRD Generation)
**Data:** 2026-07-13
**Struktura projektu:** dwuwarstwowa — **Platforma** (Intranet, Poziom A) + **POC** (Zamówienia Surowców, Poziom B)
**Odbiorca:** kontraktowy/klient (Canexpol Group)
**Status dokumentu:** niniejszy plik jest **kanonicznym rejestrem ryzyk projektowych** (Risk Register), na który powołują się `ARCHITECTURE.md` i `RULES.md`.
**Źródła:** `PRD.md` (Wizja, HLO, Persony, User Stories, OOS), `RULES.md` (reguły biznesowe), `ARCHITECTURE.md` (architektura, NFR nieformalne, ryzyka techniczne), `02-koncept-wdrozenia-redAI.md` (ryzyka techniczne wdrożenia redAI) — plus doprecyzowania uzyskane w wywiadzie elicytacyjnym z partnerem IT Canexpol (2026-07-13).

---

## 1. Executive Summary

Canexpol Group (Laboratorium Kosmetyczne, producent private label dla 500+ marek) wdraża platformę **redAI** jako docelowe środowisko do budowy i uruchamiania wewnętrznych serwisów, z pierwszym dowodem wartości w postaci modułu POC **„Zamówienia Surowców"**. Wartość biznesowa nie jest mierzona oszczędnością czasu, lecz **wpływem na P&L**: redukcją strat surowcowych (przeterminowane/niewykorzystane pozycje) oraz konsolidacją widoczności stanów surowcowych dziś rozproszonych między WAPRO, LAB5/ELAB i pocztą e-mail.

POC celowo ogranicza ryzyko do jednego procesu (zamówienia surowców, wyłącznie odczyt z systemów źródłowych, zasada „Draft + akceptacja człowieka" bez automatycznej wysyłki komunikacji) i weryfikuje zasady governance (wersjonowanie, dostęp, akceptacja człowieka), zanim Platforma obejmie kolejne obszary (materiały niewykorzystane, stany magazynowe, oferty/wyceny, planowanie produkcji). Środowisko jest regulowane (GMP) — proces certyfikacji dostawców pozostaje bramką jakościową, której żadna funkcja systemu nie może pomijać ani automatycznie przyspieszać.

---

## 2. SMART HLO Table

*Horyzont oceny sukcesu dla wszystkich pozycji: **1 miesiąc od zakończenia wdrożenia POC** (ustalone w wywiadzie elicytacyjnym).*

| ID | Warstwa | Objective (SMART) | Success Metric |
|----|---------|--------------------|-----------------|
| HLO-PLAT-001 | Platforma | Adopcja platformy przez pracowników objętych POC | Min. **1 logowanie dziennie** (dni robocze) na użytkownika, wśród 5 ról operacyjnych POC (Handlowiec, Technolog/Laborant, Spec. ds. zapotrzebowania, Spec. ds. zamówień, Spec. ds. przyjęć magazynowych), utrzymane w okresie oceny (1 miesiąc od zakończenia wdrożenia) |
| HLO-POC-001 | POC | Redukcja strat generowanych przez przeterminowane/niewykorzystane surowce | **TBD** — konkretny próg % redukcji vs. baseline zostanie ustalony po zebraniu danych baseline w trakcie POC (analogicznie do pierwotnie otwartego progu HLO-PLAT-001 w PRD); ocena w 1 miesiąc od zakończenia wdrożenia |
| HLO-POC-002 | POC | Konsolidacja widoczności stanów surowcowych | **100%** surowców objętych POC z widocznym stanem („dostępne / zarezerwowane") w jednym miejscu, bez przełączania się między WAPRO / LAB5 / mailem, w okresie 1 miesiąca od zakończenia wdrożenia |

---

## 3. Persona Map Table

*Zaimportowane 1:1 z `PRD.md` §3, bez modyfikacji na potrzeby BRD.*

| Persona | Warstwa | Primary Goals | Key Frustrations | Usage Frequency |
|---|---|---|---|---|
| **Handlowiec** | POC | Szybkie przygotowanie oferty dla klienta; sprawne ustalenie szczegółów zamówienia po akceptacji próbki; utrzymanie dobrej relacji z klientem | Oczekiwanie na informacje od laboratorium/technologa; brak wglądu w status realizacji zamówienia u dostawcy; ręczna komunikacja mailowa na każdym etapie | Codziennie, wielokrotnie w ciągu dnia |
| **Technolog / Laborant** | POC | Szybkie przygotowanie wyceny; sprawne tworzenie/modyfikacja receptury i próbki; weryfikacja dostępności surowców przed próbką; dodanie wielu dostawców do receptury (certyfikacja + negocjacja cen przy zamówieniach produkcyjnych) | Brak informacji od handlowców (oczekiwana ilość klienta); rozjazd danych WAPRO ↔ LAB5; trudny proces wykorzystania wielu dostawców w recepturze | Codziennie |
| **Specjalista ds. zapotrzebowania** | POC | Szybkie i bezbłędne przeliczenie zapotrzebowania surowcowego; zestawienie zamówień z danego dnia w jeden dokument | Wybór każdego dostawcy osobno mimo tego samego składnika w recepturze; zamawianie na zapas bez klarownych wytycznych; brak widoczności, ile towaru jest już zarezerwowane; ręczne sumowanie zamówień od klientów | Codziennie, przy każdym zamówieniu produkcyjnym |
| **Specjalista ds. zamówień** | POC | Sprawne złożenie zamówienia u dostawcy; ustalenie/śledzenie terminu dostawy; jasna komunikacja potwierdzeń/problemów; rotacja dostawców w celu negocjacji cen | Potwierdzenia zamówień tylko jako skan maila; brak możliwości negocjacji między dostawcami; brak jednego źródła prawdy i harmonogramu zamówień; długi, zmienny czas dostawy | Codziennie |
| **Specjalista ds. przyjęć magazynowych** | POC | Poprawne wprowadzenie surowców do WAPRO; jasne oznaczenie odbioru dostawy; szybka weryfikacja zgodności dostawy z zamówieniem | Śledzenie dostaw na tablicy korkowej (żółte karteczki); ryzyko pomyłki/utraty informacji przy ręcznym oznaczaniu odbioru | Codziennie, przy każdej dostawie |
| **Administrator platformy (IT)** | Platforma | Governance (wersjonowanie, backup, separacja środowisk dev/test/prod); zarządzanie użytkownikami i dostępami; nadzór nad architekturą serwisów; sprawne dodawanie nowych serwisów bez ryzyka dla działających modułów | Brak narzędzi pozwalających na samodzielne wprowadzanie zmian, serwisów czy mini-aplikacji oraz zarządzanie nimi | Codziennie w fazie POC (aktywny rozwój); docelowo rzadziej |
| **Użytkownik platformy** | Platforma | Jeden punkt startowy do pracy — dashboard z interesującymi KPI, szybki dostęp do aplikacji i ich ustawień, szybkie wyzwalanie aplikacji | Konieczność przełączania się między osobnymi narzędziami, by sprawdzić bieżący stan spraw | Codziennie |

---

## 4. MoSCoW Table

*Zasada: **Must** = funkcje bez których POC nie dowodzi wartości + najwyższe ryzyko biznesowe/regulacyjne (wg `RULES.md`); **Won't** = explicite poza zakresem POC w `PRD.md`. Waga zasobowa Must ≈ 50–55% (limit ≤60% spełniony).*

| Kategoria | Klaster / User Stories | Uzasadnienie |
|---|---|---|
| **Must** | Przeliczanie i konsolidacja zapotrzebowania (US-08–10); wielodostawczość podstawowa — certyfikowani dostawcy w recepturze, kandydat domyślny (US-05, US-07, US-11); zmiana dostawcy + blokada wg listy klientów (US-14, US-15); złożenie zamówienia i śledzenie statusu (US-02, US-12, US-13); przyjęcie dostawy i weryfikacja zgodności (US-17–19) | Najwyższe ryzyko biznesowe/regulacyjne wg `RULES.md` — błąd = przestój produkcji lub naruszenie wymagań klienta/certyfikacji |
| **Should** | Status wyceny/próbki dla handlowca (US-01); dostępność surowców przed próbką (US-04); przekazanie zamówienia klienta do laboratorium/zapotrzebowania (US-03); widok dostawców z historią cen (US-06); drafty maili do dostawców (US-16) | Istotne dla codziennej pracy, ale POC ma wartość dowodową nawet przy ręcznym obejściu na starcie |
| **Could** | Dashboard KPI i szybkie uruchamianie aplikacji (US-23, US-29); zarządzanie rolami/dostępami (US-26); dziennik aktywności (US-27); zadania cykliczne/cron (US-25) | Wartość dla Platformy, nie krytyczne dla dowiedzenia wartości samego POC |
| **Won't (POC)** | Separacja środowisk (US-20 — post-POC); backup automatyczny (US-22 — post-POC); alerty/powiadomienia (OOS-04); automatyczna wysyłka maili (OOS-05); zapis do WAPRO (OOS-01); opakowania/inne materiały (OOS-02) | Explicite poza zakresem POC w `PRD.md` |

---

## 5. Non-Functional Requirements (NFR)

*Sekcja dodana ponad kanoniczny schemat dokumentu — wymagana kontekstem regulowanego środowiska produkcji kosmetyków oraz statusem POC; ustalona w wywiadzie elicytacyjnym na bazie `ARCHITECTURE.md` §4–5.*

| Obszar | Wymaganie | Status |
|---|---|---|
| **Performance** | Czas ładowania stron < 3s | **Cel orientacyjny (niewiążący)** — zgodnie z charakterem etapu POC, brak twardego SLA |
| **Accessibility** | Brak wymaganego poziomu WCAG | Potwierdzone — narzędzie czysto wewnętrzne, 5 zdefiniowanych ról operacyjnych, poza zakresem POC |
| **Compliance** | Brak dodatkowego wymogu RODO ponad standard | Potwierdzone — system POC nie przetwarza danych osobowych wymagających odrębnej zgodności; obszar regulacyjny ograniczony do wymogów jakościowych GMP (proces certyfikacji, ograniczenie integracji WAPRO/LAB5 do odczytu — zob. §7) |
| **Security** | Logowanie niezależne od redAI/WAPRO/LAB5; brak SSO na POC; konta zakładane ręcznie przez administratora platformy; dostęp rozdzielony wg 5 ról/person | Potwierdzone jako **wystarczający poziom kontroli dostępu** dla POC |

---

## 6. Risk Register (kanoniczny rejestr ryzyk projektu)

*Skonsolidowany z `ARCHITECTURE.md` §7, `02-koncept-wdrozenia-redAI.md` §5 i `PRD.md` §6, bez duplikatów. Oceny Likelihood/Impact są propozycją Senior BA zatwierdzoną przez partnera IT Canexpol w wywiadzie elicytacyjnym (źródła opisywały ryzyka jakościowo, bez skali L/I).*

| ID | Description | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| RISK-001 | Brak separacji środowisk dev/test/prod — zmiany trafiają wprost na środowisko codziennej pracy | High | High | Separacja przed przejściem POC do stałej eksploatacji (US-20) |
| RISK-002 | Brak CI/CD i automatycznego rollbacku — deploy ręczny SSH+git, hotfix „na żywo" | Medium | Medium | Obowiązkowa pełna suita testów Pest przed każdym deployem (zasada już przyjęta) |
| RISK-003 | Brak automatycznego backupu bazy/plików — ryzyko nieodwracalnej utraty danych | Medium | High | Backup z okresowym testem odtworzenia jako warunek przed skalowaniem (US-22) |
| RISK-004 | Minimalny model audytu (bez pełnej historii wartości) — luka śladowalności w środowisku GMP | Medium | High | Rozszerzenie audytu przed wdrożeniem produkcyjnym; częściowo już zrealizowane dla cen (`price_history`, BR-SUPPLIER-004) |
| RISK-005 | Kolizja modelu wielodostawczości z modelem WAPRO „1 produkt = 1 dostawca" i certyfikacją per dostawca | High | High | Decyzja jakościowa (nie tylko systemowa) przed wdrożeniem — do ustalenia z działem jakości |
| RISK-006 | Zmiana dostawcy bez respektowania listy klientów zastrzegających dostawcę (US-15) | Medium | High | Obowiązkowe uzasadnienie tekstowe + blokada zapisu (reguła BR-SUPPLIER-001) |
| RISK-007 | Błąd w przeliczaniu zapotrzebowania z receptury (US-10) — skutek: przestój produkcyjny | Low–Medium | High | Obowiązkowe pokrycie testami E2E jako ścieżka krytyczna (BR-DEMAND-001) |
| RISK-008 | Brak formalnego SLA i monitoringu poza godzinami pracy | Medium | Low–Medium | Świadomie zaakceptowane na etapie POC; rewizja przy skalowaniu poza POC |
| RISK-009 | Brak rejestru kosztów/zużycia modelu AI platformy redAI | Medium | Medium | Rozliczanie zużycia per serwis jako warunek przed skalowaniem poza POC |
| RISK-010 | Dane receptur/klientów przesyłane do modelu AI działającego wyłącznie w chmurze (brak modelu lokalnego) | Medium | Medium–High | Polityka bezpieczeństwa danych przesyłanych do modelu AI; rewizja po dostępności modelu lokalnego |

---

## 7. Technical Context & Logic

- **Architektura/Hosting:** Wzorzec „Droga B" (samodzielna aplikacja). Tymczasowo (workaround POC) w ramach istniejącej infrastruktury VPS redAI, osobny proces na dedykowanym porcie, Supervisor. Docelowo: migracja na własną subdomenę (nginx reverse proxy).
- **Baza danych:** dedykowana instancja PostgreSQL, w pełni odizolowana od bazy portalu redAI (wymóg `CLAUDE.md`).
- **Model domenowy (centralny):** wielodostawczość — `Recipe` (receptura) → wiele `Supplier`; nie spłaszczać do „jeden produkt = jeden dostawca" (kolizja z modelem WAPRO, zob. RISK-005).
- **Integracje zewnętrzne:**
  - WAPRO (WF-Mag, SQL Server) — **wyłącznie odczyt** stanów magazynowych/dokumentów (OOS-01).
  - LAB5/ELAB (dev.lab5.pl) — głównie odczyt receptur/INCI; zapis tylko tam, gdzie niezbędny do obsługi kroku procesu (OOS-03).
  - Poczta do dostawców (US-16) — brak integracji mailowej; system generuje wyłącznie treść draftu, wysyłka ręczna poza systemem.
- **Zasada nadrzędna „Draft + akceptacja człowieka" (OOS-05):** żadna funkcja o skutku zewnętrznym (mail, zapis do systemu zewnętrznego, zamówienie u dostawcy) nie wykonuje się automatycznie; brak integracji mailowej wzmacnia to technicznie, nie tylko proceduralnie.
- **Bramka jakościowa:** proces certyfikacji dostawców pozostaje warunkiem przed zamówieniem produkcyjnym — żadna funkcja POC (w tym rotacja/zmiana dostawcy) nie może go pomijać ani przyspieszać automatycznie.
- **Testing:** Pest (PHP); obowiązkowe pokrycie ścieżek krytycznych — przeliczanie zapotrzebowania (US-10), zmiana dostawcy + blokada (US-14/15), sumowanie zamówień (US-09), weryfikacja przyjęcia dostawy (US-18).

---

## 8. Traceability

- **`PRD.md`** — Wizja, HLO/KPI (bazowe), Persony, User Stories (US-01…US-29), granice zakresu (OOS-01…OOS-06).
- **`RULES.md`** — reguły biznesowe (BR-SUPPLIER-001…004, BR-DEMAND-001…003, BR-DELIVERY-001…002).
- **`ARCHITECTURE.md`** — architektura techniczna, model danych, NFR nieformalne (bazowe), ryzyka POC do rewizji.
- **`02-koncept-wdrozenia-redAI.md`** — ryzyka techniczne platformy redAI, warunki wstępne przed skalowaniem poza POC.
- Niniejszy dokument (`CLIENT_BRD.md`) formalizuje HLO do języka kontraktowego (SMART), stanowi kanoniczny Risk Register oraz domyka NFR — pozostałe dokumenty powinny odwoływać się do niego, a nie duplikować te sekcje.

## 9. Changelog

- 2026-07-13 | v1.0 | Pierwsza wersja `CLIENT_BRD.md` — import HLO/Person z `PRD.md`, formalizacja SMART (horyzont 1 miesiąc), MoSCoW, NFR, konsolidacja kanonicznego Risk Register (10 pozycji) z `ARCHITECTURE.md`/`02-koncept-wdrozenia-redAI.md`/`PRD.md`, ustalone w wywiadzie elicytacyjnym z partnerem IT Canexpol.
