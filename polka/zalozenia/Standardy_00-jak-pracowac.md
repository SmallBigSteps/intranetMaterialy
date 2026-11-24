<!--
opis: Standardy FEBO, część 00: jak pracować z aplikacją i dokumentami Standardów.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,procedury
-->
# Standardy FEBO — jak prowadzić prace

Materiał dla czatów wykonawczych w projekcie **intranet2.0**. Każdy plik z numerem to
jedna karta — treść do wklejenia na początku nowej rozmowy.

## Zasada

**Jedna karta = jeden czat = jedna gałąź.** Nie zaczynaj drugiej karty w tej samej
rozmowie, nawet jeśli pierwsza poszła szybko: kolejne zadanie dziedziczy wtedy
niewypowiedziane założenia z poprzedniego i nikt ich nie widzi.

## Kolejność

| | Plik | Karta | Co powstaje |
|---|---|---|---|
| 1 | `Standardy_01-metryka.md` | T-57b | metryka standardu i kategorie |
| 2 | `Standardy_02-bramka-publikacji.md` | T-57b2 | zatwierdza przełożony autora |
| 3 | `Standardy_03-szablony-micro-sop.md` | T-57c | szablon i kontrola kompletności |
| 4 | `Standardy_04-zapoznanie.md` | T-57h | kto ma przeczytać i kto potwierdził |
| 5 | `Standardy_05-ekrany.md` | T-57a | lista, procedura, edytor, zgłoszenie |
| 6 | `Standardy_06-ekrany-zapoznania.md` | T-57a2 | odbiorcy przy publikacji, stan potwierdzeń |
| — | **testy człowieka** | — | przeklikanie całej drogi |
| 7 | `Standardy_07-wyszukiwanie.md` | T-57i | pytanie zadane po ludzku |
| 8 | `Standardy_08-punkt-zaczepienia.md` | T-57g | „to miejsce czeka na Twoją procedurę" |
| 9 | `Standardy_09-kreator.md` | T-57f | asystent prowadzący wywiad |
| 10 | `Standardy_10-migracja-i-wygaszenie-poc.md` | — | przeniesienie FEBO-HR-01 |

Karty 1–6 nie dotykają asystenta w ogóle. Połączenie z usługą redAi wchodzi dopiero
w siódmej.

## Środowisko

Repozytorium i uruchomiona aplikacja stoją na serwerze roboczym: **`ssh dev-febo`**,
katalog **`/home/ubuntu/febo-intranet`**. Praca zaczyna się od `git pull` na gałęzi `main`.

Klucz do usługi asystenta jest już w konfiguracji tego serwera
(`ASSISTANT_API_BASE_URL`, `ASSISTANT_API_KEY`, `ASSISTANT_TIMEOUT_SECONDS`) i **nie wolno
go przenosić do repozytorium** — w projekcie ląduje sama nazwa zmiennej.

Baza to PostgreSQL 17 na obrazie z rozszerzeniem wektorowym. Rozszerzenie jest dostępne,
ale **jeszcze nieaktywne** — włącza je migracja z karty siódmej.

## Protokół obowiązujący w każdej karcie

1. **Najpierw plan, w trybie tylko do odczytu.** Które pliki się zmienią, dlaczego i jaka
   komenda udowodni, że karta jest skończona. Czekaj na akceptację człowieka.
2. Gałąź o nazwie podanej w karcie.
3. Każda reguła ma **przechodzący test nazwany jej numerem**. Testu nie zmienia się po to,
   żeby przeszedł.
4. Pętla naprawcza: uruchom → przeczytaj wynik → popraw. Trzy podejścia; po trzecim opisz,
   co wykluczyłeś, zamiast próbować dalej.
5. Na koniec `make check` i `make review`, aktualizacja `docs/progress.md`, commit i push.
6. Zmiany podpisuj jako **redAi \<redai@febologistic.com\>**.

## Skąd brać kontekst

Wszystko jest w repozytorium i **to ono rozstrzyga**, a nie ten plik:

- `docs/contract/plan.contract.md` — pełne karty zadań,
- `api/apps/standards/RULES.md` — trzydzieści reguł lokatora,
- `docs/decisions/standardy-przeniesienie-poc.decisions.md` — dwadzieścia pięć rozstrzygnięć
  z 29.08.2026 (co przenosimy z POC i dlaczego),
- `docs/decisions/standardy-implementacja.decisions.md` — trzynaście rozstrzygnięć
  z 31.08.2026 (zapoznanie z procedurą, asystent, rejestr wywołań),
- `docs/external/redai-api.spec.md` i `docs/external/redai-instrukcja-integracji-1.0.md` —
  umowa z dostawcą asystenta i jego instrukcja,
- `docs/external/redai-materialy/` — materiał wzorcowy i zestaw kontrolny.

## Czego nie ma w tym wdrożeniu

Eksportu do pliku (druk przez przeglądarkę wystarcza), wersji na telefon (wraca, gdy będzie
potrzebna), ewidencji wykonania procedury (potwierdzamy zapoznanie się z tekstem, nie
wykonanie czynności) i przypomnień o przeglądach okresowych (data i plakietka wchodzą teraz,
przypominanie po module powiadomień).
