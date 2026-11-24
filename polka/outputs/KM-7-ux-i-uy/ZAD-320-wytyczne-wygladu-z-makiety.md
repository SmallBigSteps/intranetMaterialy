<!--
opis: Notka wyniku ZAD-320: decyzja, kontrakty core-ui 2.35, core-cockpit 3.18, core-tasks 4.17 i makieta wzorcowa 07 w repozytorium (commit 8716b2f).
rodzaj: wynik
zadanie: ZAD-320
kamien: KM-7 UX i UY
rozmowa: ZAD-320 Wytyczne wyglądu z makiety
zmiana: 19.11.2026, ZAD-320 Wytyczne wyglądu z makiety
status: do akceptacji
slowa: wytyczne wyglądu, makieta, pulpit, core-ui, core-cockpit, core-tasks, wzorce ekranów
-->
# ZAD-320 — wytyczne wyglądu z makiety

Założenia żyją w repozytorium `SmallBigSteps/intranet` (gałąź `main`), commit
[8716b2f](https://github.com/SmallBigSteps/intranet/commit/8716b2f) z 19.11.2026. Na półce leży tylko ta notka.

## Co powstało

| Co | Gdzie w repozytorium |
|---|---|
| Decyzja: co przejmujemy z makiety, zasada rozstrzygania konfliktów, opcje marki, kolejność chowania belki | `docs/decisions/wytyczne-wygladu-z-makiety-zad320.decisions.md` |
| Rama, belka, nagłówek ekranu, nowe komponenty, wiążące wzorce ekranów dla autorów aplikacji (§3.2) | `docs/contract/core-ui.contract.md` 2.35 |
| Pulpit po zalogowaniu, kafel aplikacji ze statusem i dostępem, miejsce na kolejną aplikację | `docs/contract/core-cockpit.contract.md` 3.18 |
| Lista zadań z pigułkami i grupowaniem po aplikacji zamiast układu z ZAD-298 | `docs/contract/core-tasks.contract.md` 4.17 |
| Makieta wzorcowa bez marki (fikcyjna „Firma Przykładowa”) i pięć zrzutów | `docs/external/brand/07-intranet-wzorzec.html`, `reference-07-*.png` |
| Makieta 07 jako referencja, 01/02/03/06 zastąpione | `docs/contract/tooling.contract.md` 1.8, `docs/external/brand/README.md` |

## Co zostaje do zrobienia

- ZAD-321: implementacja ramy i pulpitu oraz kroje powłoki (Poppins, DM Sans, IBM Plex Mono w obrazie).
- Wdrożenie FEBO (ZAD-031): włączenie opcji marki (kropka po tytule, kolorowe tło znaku), jeśli FEBO zechce.
- Nowy filtr „Na dziś” i liczby do pigułek w API — reguły i testy dopisuje zadanie implementacji.

## Uwaga do oglądu

W neutralnym pakiecie powłoki nagłówek ekranu jest ciemny (kolor pasa = kolor dominujący), a nie biały
jak w makiecie źródłowej. Układ w trzech częściach jest ten sam; kolor nagłówka ustala pakiet marki wdrożenia.
