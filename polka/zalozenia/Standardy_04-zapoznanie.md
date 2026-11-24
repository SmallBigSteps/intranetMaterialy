<!--
opis: Standardy FEBO, część 04: zapoznanie pracowników z procedurą.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,zapoznanie
-->
# Karta 4 — zapoznanie z procedurą (T-57h)

Gałąź: `t-57h-zapoznanie`

## Środowisko

`ssh dev-febo`, `/home/ubuntu/febo-intranet`, `git pull` na `main`. Karty 1–3 scalone.

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/contract/plan.contract.md` — karta **T-57h**
- `api/apps/standards/RULES.md` — reguły **BR-STD-026 … BR-STD-029**
- `docs/decisions/standardy-implementacja.decisions.md` — rozdziały 9, 10, 11, 12
- `docs/contract/core-tasks.contract.md` — rozdziały 6.1 i 11, przypisanie imienne i grupowe

## Co powstaje

Procedurę można **skierować do ludzi**, a firma widzi, **kto się z nią zapoznał**.

**Nie budujemy do tego osobnego mechanizmu.** Zapoznanie jest **zadaniem na wspólnej liście
zadań powłoki** — tej samej, którą chodzi zatwierdzanie. Stamtąd bierze się za darmo
widoczność, przypomnienia o zaległych, historia i statystyki. Drugi mechanizm byłby tym,
którego nikt nie utrzymuje.

- **Odbiorcy wskazywani przy zatwierdzaniu**: pojedyncze osoby, całe działy albo jedno
  i drugie naraz. Każdy odbiorca dostaje **jedno** zadanie z linkiem i potwierdzeniem,
  z terminem domyślnie **czternastodniowym** (BR-STD-026).
- **Nowa wersja**: zatwierdzający odpowiada przy publikacji na jedno pytanie — czy odbiorcy
  mają zapoznać się ponownie — **domyślnie tak**. Wcześniejsze potwierdzenia zostają
  w historii przy swoich wersjach (BR-STD-027).
- **Nowa osoba w dziale** dostaje zaległe zapoznania z procedurami **wciąż obowiązującymi**,
  skierowanymi wcześniej do tego działu; wycofane i zastąpione pomijamy (BR-STD-028).
- **Powiadomienie o publikacji** dostają wyłącznie: autor, właściciel treści i osoby, które
  procedurę wcześniej otwierały (BR-STD-029).

## Na co uważać

**To nie jest ewidencja wykonania procedury.** Potwierdzamy **zapoznanie się z tekstem**,
nie wykonanie czynności: jedno zdarzenie na osobę i wersję, bez stanu, bez przebiegu, bez
raportowania. Jeśli w modelu zaczyna rosnąć „w trakcie" — karta zboczyła z kursu.

**Zaległe zapoznanie nie blokuje niczego.** Pokazuje się na liście osoby i na liście
zaległości jej przełożonego, bo to przełożony odpowiada za to, czy jego ludzie znają
procedury. Każdy wariant blokujący kończy się klikaniem „potwierdzam" bez czytania, żeby
odzyskać dostęp.

## Czego w tej karcie nie ma

Ekranów — te są w karcie 6. Tutaj powstaje model, zadania i reguły.

## Protokół

Plan w trybie tylko do odczytu, akceptacja, gałąź, `make check` i `make review`,
`docs/progress.md`, commit i push jako **redAi \<redai@febologistic.com\>**.

## Karta jest skończona, gdy

Reguły BR-STD-026 … BR-STD-029 mają przechodzące testy, w tym przypadek działu z procedurą
wycofaną wśród obowiązujących; **żadna ścieżka zapoznania nie potrafi zablokować innej
czynności w powłoce**.
