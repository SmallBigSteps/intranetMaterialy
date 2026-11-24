<!--
opis: Standardy FEBO, część 02: bramka publikacji procedury.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,publikacja,akceptacja
-->
# Karta 2 — bramka publikacji na linii podległości (T-57b2)

Gałąź: `t-57b2-bramka`

## Środowisko

`ssh dev-febo`, katalog `/home/ubuntu/febo-intranet`, `git pull` na `main`.
Karta 1 (metryka) musi być scalona.

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/contract/plan.contract.md` — karta **T-57b2**
- `api/apps/standards/RULES.md` — reguły **BR-STD-002, 007, 013, 014, 025**
- `docs/decisions/standardy-przeniesienie-poc.decisions.md` — rozdziały 5 i 6
- `docs/decisions/standardy-implementacja.decisions.md` — rozdział 7
- `docs/contract/core-accounts.contract.md` — pole przełożonego
- `docs/contract/core-tasks.contract.md` — rozdziały 6.1, 6.4, przypisanie imienne

## Co powstaje

**To jest zmiana projektu, nie doprecyzowanie.** Bramka publikacji przestaje należeć do
jednej nazwanej roli i przechodzi na strukturę organizacyjną.

- **Zgłoszenie trafia imiennie do przełożonego autora.**
- **Autor bez przełożonego** (zarząd) — zgłoszenie idzie do jego **bezpośrednich
  podwładnych**; rozstrzyga pierwszy, który podejmie zadanie, a podjęcie zdejmuje je
  pozostałym (BR-STD-014).
- **Autor bez przełożonego i bez podwładnych** — zgłoszenie trafia do **opiekuna aplikacji,
  z widocznym powodem**, że stało się tak z braku przełożonego (BR-STD-025).
- **Widoczność szkicu** idzie za tą samą parą: autor i osoba rozstrzygająca (BR-STD-007).
- **Rozdział obowiązków zostaje twardy** i obejmuje również opiekuna aplikacji: nikt nie
  zatwierdza własnego tekstu (BR-STD-013).
- **Rola opiekuna zmienia znaczenie**: prowadzi kategorie, rozstrzyga spory, przyjmuje
  wnioski o procedury — **nie zatwierdza wszystkiego**. Prawo zatwierdzania przestaje być
  rolą nazwaną i wynika z relacji.

## Dlaczego to jest ważne poza tą aplikacją

Przy jednej nazwanej roli publikującej ta osoba nie mogła wypuścić własnej procedury inaczej
niż odstępstwem administratora technicznego — czyli stałą furtką obok obiegu, który sami
zaprojektowaliśmy. Linia podległości usuwa ten przypadek zamiast go obsługiwać, i to właśnie
odblokowuje wejście na produkcję.

## Na co uważać

Dane środowiska roboczego zawierają wszystkie trzy przypadki: **83 konta, 71 z przełożonym,
12 bez** — w tym Prezes Zarządu z ośmiorgiem bezpośrednich podwładnych oraz konta bez
przełożonego i bez podwładnych. Testy mają obejmować całą trójkę, bo wszystkie trzy zdarzają
się naprawdę.

Zadanie `procedure_approval` przestaje być adresowane do roli — adresuj imiennie.

## Czego w tej karcie nie ma

Ekranów. Decyzja zatwierdzającego to zwykły ekran zadania powłoki, nie ekran tej aplikacji.

## Protokół

Plan w trybie tylko do odczytu, akceptacja, gałąź, `make check` i `make review`,
`docs/progress.md`, commit i push jako **redAi \<redai@febologistic.com\>**.

## Karta jest skończona, gdy

Pięć wymienionych reguł ma przechodzące testy, w tym przypadek zarządu i przypadek pustej
linii podległości; **żadna procedura nie wymaga odstępstwa administratora, żeby dojść do
publikacji**.
