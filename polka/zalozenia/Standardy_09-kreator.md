<!--
opis: Standardy FEBO, część 09: kreator procedury z notatki.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,kreator,AI
-->
# Karta 9 — kreator SOP z asystentem (T-57f)

Gałąź: `t-57f-kreator`

## Środowisko

`ssh dev-febo`, `/home/ubuntu/febo-intranet`, `git pull` na `main`. Karty 1–8 scalone —
zwłaszcza 3 (szablony), 5 (edytor) i 7 (klient API).

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/contract/plan.contract.md` — karta **T-57f**
- `docs/external/redai-instrukcja-integracji-1.0.md` — rozdziały o wywiadzie i składaniu
  procedury
- `api/apps/standards/RULES.md` — reguła **BR-STD-024**
- `docs/decisions/standardy-przeniesienie-poc.decisions.md` — rozdział 3
- klient API zbudowany w karcie 7 — **używaj go, nie pisz drugiego**

## Co powstaje

Funkcja, która była rdzeniem portalowego POC: **asystent prowadzi pracownika przez wywiad
i składa z niego gotową procedurę**.

- **Wywiad w czterech etapach**: metryka → kontekst → kroki → dowód. O domknięciu etapu
  decyduje usługa, nie sztywna liczba pytań.
- **Podpowiedź pod polem odpowiedzi** — usługa zwraca ją przy każdym pytaniu; można ją
  pokazać albo pominąć, ale warto pokazać: to ona podnosi jakość odpowiedzi.
- **Podgląd i wstawienie gotowego tekstu do edytora**, gdzie autor go poprawia. Tekst wraca
  z kompletem sekcji — usługa sprawdza to u siebie i nie zwraca niekompletnej procedury.
- **Sygnał o niepewnej kategorii**: gdy usługa nie znalazła pasującej kategorii i wskazała
  najbliższą, **pokaż to autorowi przy zapisie** — to jego decyzja, nie automatu.
- Usługa proponuje też streszczenie i tagi.

## Na co uważać

**Asystent to pomoc, nigdy jedyna droga.** Przy niedostępnej usłudze autor pisze procedurę
ręcznie, na szablonie z karty 3, a edytor działa bez zmian (BR-STD-024).

**Nie pozwól asystentowi pisać procedury za człowieka.** Persona jest tak sformułowana
celowo: asystent dopytuje o konkret, ale nie podpowiada treści kroków. Procedurę zna
pracownik — asystent zna tylko format.

Wywołania idą **przez kolejkę**, tak jak wszystkie pozostałe, i trafiają do rejestru
wywołań **bez treści**.

## Sprawdzenie jakości

Dziesięć sesji z tym samym tematem („zgłaszanie szkody w ładunku"), prowadzonych do końca.
Zaliczenie: wszystkie dziesięć złożonych procedur ma komplet sekcji, streszczenie długości
jednego–dwóch zdań i od trzech do sześciu tagów. Opis próby:
`docs/external/redai-materialy/05-zestaw-kontrolny.md`.

## Protokół

Plan w trybie tylko do odczytu, akceptacja, gałąź, `make check` i `make review`,
`docs/progress.md`, commit i push jako **redAi \<redai@febologistic.com\>**.

## Karta jest skończona, gdy

Wywiad da się przejść od pierwszego pytania do wstawienia tekstu w edytorze; próba
dziesięciu sesji wypada zgodnie z kryterium; wyłączenie asystenta na ekranie ustawień nie
psuje pisania ręcznego.
