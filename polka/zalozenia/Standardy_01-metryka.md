<!--
opis: Standardy FEBO, część 01: metryka procedury.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,metryka
-->
# Karta 1 — metryka standardu (T-57b)

Gałąź: `t-57b-metryka`

## Środowisko

Repozytorium i aplikacja: `ssh dev-febo`, katalog `/home/ubuntu/febo-intranet`.
Zacznij od `git pull` na gałęzi `main`.

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/contract/plan.contract.md` — karta **T-57b**
- `api/apps/standards/RULES.md` — reguły **BR-STD-015 … BR-STD-022**
- `docs/decisions/standardy-przeniesienie-poc.decisions.md` — rozdziały 8 i 14
- `docs/decisions/standardy-implementacja.decisions.md` — rozdział 13
- `docs/contract/core-accounts.contract.md` — stanowiska
- `.claude/skills/tenant-contract/SKILL.md`

## Co powstaje

Metryka, którą niesie każda procedura. **Struktura wchodzi przed ekranami** — dołożona
później oznacza przerabianie ekranów, nie ich rozszerzanie.

- **identyfikator `FEBO-<KOD>-<NR>`**, nadawany automatycznie z kodu kategorii i pierwszego
  wolnego numeru, **niezmienny**: nie rusza go nowa wersja, wycofanie ani przeniesienie do
  innej kategorii,
- **właściciel treści jako stanowisko**, nigdy jako osoba — rotacja nie może wymagać
  przejścia po całej bibliotece,
- **streszczenie** 1–2 zdania, **wymagane przy zgłoszeniu do zatwierdzenia**,
- **tagi** (swobodne) obok jednej kategorii,
- **rodzaj dokumentu**: procedura krokowa / dokument opisowy,
- **data następnego przeglądu**, liczona przy zatwierdzeniu, domyślnie 12 miesięcy,
  ustawialna per procedura,
- **wskazanie następcy**, używane przy wycofaniu,
- **dwanaście kategorii startowych z kodami**, zasianych migracją:

| Kategoria | Kod | Kategoria | Kod |
|---|---|---|---|
| Spedycja | `SPE` | Sprzedaż i obsługa klienta | `SPR` |
| Odprawy celne | `CLO` | BHP | `BHP` |
| Magazyn | `MAG` | Jakość i reklamacje | `JAK` |
| Flota i transport | `FLO` | Finanse i księgowość | `FIN` |
| HR | `HR` | Administracja | `ADM` |
| IT | `IT` | Windykacja | `WIN` |

Kategorie prowadzi wyłącznie opiekun aplikacji (BR-STD-022).

## Na co uważać

**Numer wersji zostaje liczbą całkowitą** — „wersja 3", nigdy 1.0 / 1.1 / 2.0. Numeracja
z kropką wymaga od autora oceny „duża zmiana czy drobna", której nikt nie podejmuje
konsekwentnie.

**Deklaracja typu dokumentu w manifeście obiecuje dziś pliki PDF**, a karta eksportu została
skreślona 29.08.2026. Popraw deklarację tutaj, zamiast zostawiać ją opisującą nieistniejące
zachowanie.

Identyfikator przetrwa zmianę kategorii — to jest cena jego użyteczności, nie niedopatrzenie.
Ludzie posługują się nim w mailu i w rozmowie, więc `FEBO-HR-01` musi wskazywać to samo za
trzy lata.

## Czego w tej karcie nie ma

Ekranów, obiegu zatwierdzania na linii podległości, szablonów Micro-SOP, zapoznania
z procedurą i czegokolwiek związanego z asystentem AI. To osobne karty i mają swoją kolejność.

## Protokół

Najpierw **plan w trybie tylko do odczytu**: które pliki zmienisz, dlaczego i jaka komenda
udowodni, że karta jest skończona. Poczekaj na akceptację. Potem gałąź, praca, `make check`
i `make review`, aktualizacja `docs/progress.md`, commit i push. Każda reguła ma mieć
przechodzący test nazwany jej numerem. Zmiany podpisuj jako **redAi \<redai@febologistic.com\>**.

## Karta jest skończona, gdy

Wszystkie pola są w modelu i w manifeście tam, gdzie manifest je niesie; reguły
BR-STD-015 … BR-STD-022 mają przechodzące testy; zasiane kategorie realnie produkują
identyfikatory.
