<!--
opis: Standardy FEBO, część 03: szablony micro-SOP.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,SOP,szablony
-->
# Karta 3 — szablon Micro-SOP i kontrola kompletności (T-57c)

Gałąź: `t-57c-szablony`

## Środowisko

`ssh dev-febo`, `/home/ubuntu/febo-intranet`, `git pull` na `main`. Karty 1 i 2 scalone.

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/contract/plan.contract.md` — karta **T-57c**
- `api/apps/standards/RULES.md` — reguła **BR-STD-018**
- `docs/decisions/standardy-przeniesienie-poc.decisions.md` — rozdziały 9, 10, 11
- `docs/external/redai-materialy/01-FEBO-HR-01-zastepstwa-w-okresie-urlopowym.md` —
  **wzorzec firmowego stylu**, napisany przez właściciela procesu w FEBO
- `docs/external/redai-materialy/README.md` — sześć zasad stylu wyczytanych z tej procedury

## Co powstaje

Format przeniesiony z portalowego POC: **Działanie · Dlaczego · GO/NO-GO · Co, jeśli… ·
Dowód**. Wchodzi w dwóch miejscach i to nie jest to samo miejsce:

1. **Szablon podstawiony w edytorze przy zakładaniu procedury** — autor zaczyna od
   rusztowania, nie od pustej strony.
2. **Reguła sprawdzana przy zgłoszeniu do zatwierdzenia** — czy sekcje są obecne i niepuste.

**Sprawdzamy kompletność, nigdy jakość treści.** To odróżnia standard od notatki i zdejmuje
z przełożonego rolę korektora struktury.

**Dwa rodzaje dokumentu**, bo nie każda procedura FEBO jest procesem krokowym:

| Rodzaj | Szablon | Sprawdzenie |
|---|---|---|
| procedura krokowa | pełny Micro-SOP | wszystkie sekcje w każdym kroku + jedna sekcja Dowód na końcu |
| dokument opisowy (regulamin, zasady, słownik) | lekki: tytuł, streszczenie, sekcje | bez sprawdzania sekcji Micro-SOP |

Bez tego rozróżnienia ludzie zaczną wpisywać „nie dotyczy" w każdej rubryce i sprawdzenie
przestanie cokolwiek znaczyć.

## Na co uważać

**Treść to tekst z formatowaniem renderowany przez powłokę, nie HTML.** HTML od modelu
wymaga sanityzacji i listy dozwolonych znaczników na zawsze, przestaje być przeszukiwalny
jako jeden ciąg, a wygląd przenosi do treści — przy zmianie wizualnej trzeba by przerabiać
wszystkie zapisane procedury zamiast jednego arkusza.

**Checklisty GO/NO-GO są do czytania i druku.** Klikalna checklista z zapisem, kto i kiedy
odhaczył, to nie wariant pola, tylko nowy byt „wykonanie procedury" — z wykonawcą, datą,
stanem w toku i oczekiwaniem raportowania tuż za nim. Odrzucone 29.08.2026.

Wzorzec z materiałów dla dostawcy pokazuje docelowy poziom szczegółowości: krok zaczyna się
od czasownika w trybie rozkazującym, „Dlaczego" mówi o konsekwencji, a GO/NO-GO to warunki
sprawdzalne wzrokiem.

## Protokół

Plan w trybie tylko do odczytu, akceptacja, gałąź, `make check` i `make review`,
`docs/progress.md`, commit i push jako **redAi \<redai@febologistic.com\>**.

## Karta jest skończona, gdy

Oba szablony istnieją; BR-STD-018 przechodzi dla obu rodzajów dokumentu; procedura krokowa
z brakującą sekcją **nie da się zgłosić**, a odmowa nazywa brakującą sekcję.
