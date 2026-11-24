<!--
opis: Standardy FEBO, część 06: ekrany zapoznania.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,zapoznanie,ekrany
-->
# Karta 6 — ekrany zapoznania (T-57a2)

Gałąź: `t-57a2-ekrany-zapoznania`

## Środowisko

`ssh dev-febo`, `/home/ubuntu/febo-intranet`, `git pull` na `main`. Karty 1–5 scalone.

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/contract/plan.contract.md` — karta **T-57a2**
- `api/apps/standards/RULES.md` — reguły **BR-STD-026, BR-STD-027**
- `docs/decisions/standardy-implementacja.decisions.md` — rozdziały 9–12
- ekran zatwierdzania zbudowany w karcie 5

## Co powstaje

Widoczna połowa karty 4. **Dwa miejsca, nie więcej.**

1. **Przy zatwierdzaniu**: wybór odbiorców — osoby i działy w jednym polu, obie rzeczy naraz
   — termin z wpisanymi czternastoma dniami oraz jedno pytanie o ponowne zapoznanie,
   zaznaczone domyślnie na „tak".
2. **Na procedurze**: panel dla właściciela treści i opiekuna aplikacji — kto potwierdził
   i kiedy, kto jeszcze nie, zaległi na górze.

## Na co uważać

**Panel jest faktem, nie tablicą wyników.** Bez kolorowego rankingu działów i bez procentu
w nagłówku — ta sama lista nazwisk, która jest użyteczna dla przełożonego, staje się czymś
zupełnie innym, gdy system zaczyna ją punktować.

## Po tej karcie testuje człowiek

**Tu kończy się część, którą da się sprawdzić testami.** Po scaleniu tej karty całą drogę
przechodzi człowiek: napisanie procedury, zgłoszenie, zatwierdzenie przez przełożonego,
wskazanie odbiorców, potwierdzenie zapoznania. Uwagi z tego przejścia są ważniejsze niż
trzy przeglądy wcześniej.

## Protokół

Plan w trybie tylko do odczytu, akceptacja, gałąź, `make check` i `make review`,
`docs/progress.md`, commit i push jako **redAi \<redai@febologistic.com\>**.

## Karta jest skończona, gdy

Droga opisana wyżej działa od początku do końca na szerokości tabletu, a wybór odbiorców
obsługuje dział i pojedynczą osobę w jednym zgłoszeniu.
