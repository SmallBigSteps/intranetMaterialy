<!--
opis: Standardy FEBO, część 08: punkt zaczepienia procedury w innych aplikacjach.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,punkt zaczepienia
-->
# Karta 8 — punkt zaczepienia procedury i wniosek o jej napisanie (T-57g)

Gałąź: `t-57g-zaczepienie`

## Środowisko

`ssh dev-febo`, `/home/ubuntu/febo-intranet`, `git pull` na `main`. Karty 1–7 scalone.

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/contract/plan.contract.md` — karta **T-57g**
- `api/apps/standards/RULES.md` — reguły **BR-STD-009, BR-STD-023**
- `docs/decisions/standardy-przeniesienie-poc.decisions.md` — rozdział 17

## Co powstaje

Pomoc kontekstowa **w odwróconej roli**. Stały adres procedury już istnieje i nigdy się nie
zmienia; brakuje miejsca, które go użyje — a w pierwszych miesiącach jeszcze bardziej
przydaje się miejsce, które **samo zbiera potrzeby**.

- **Punkt zaczepienia**, który inne ekrany powłoki mogą nieść (najpierw szczegóły zadania).
- Z podpiętą procedurą pokazuje link.
- Bez podpiętej pokazuje zaproszenie **„to miejsce czeka na Twoją procedurę"** z dwiema
  akcjami: **napisz ją** (otwiera nową procedurę z ustawioną kategorią i zapamiętanym
  miejscem) oraz **poproś o jej przygotowanie** (zakłada zadanie dla opiekuna aplikacji
  z miejscem pochodzenia i wnioskodawcą).
- **Drugi wniosek z tego samego miejsca dopisuje się do istniejącego zadania**, zamiast
  tworzyć drugie (BR-STD-023).

## Dlaczego teraz, a nie po kilkudziesięciu procedurach

Zgromadzone wnioski same pokażą, które ekrany naprawdę wołają o podpowiedź. Bez nich karta
musiałaby to zgadywać.

## Protokół

Plan w trybie tylko do odczytu, akceptacja, gałąź, `make check` i `make review`,
`docs/progress.md`, commit i push jako **redAi \<redai@febologistic.com\>**.

## Karta jest skończona, gdy

Obie akcje działają z co najmniej jednego prawdziwego miejsca w powłoce; BR-STD-023
przechodzi, łącznie z drugim wnioskiem z tego samego miejsca.
