<!--
opis: Standardy FEBO, część 10: migracja i wygaszenie POC.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,migracja,POC
-->
# Karta 10 — migracja treści i wygaszenie POC

Gałąź: `standardy-migracja-poc`

## Środowisko

`ssh dev-febo`, `/home/ubuntu/febo-intranet`, `git pull` na `main`. Karty 1–9 scalone,
aplikacja przetestowana przez człowieka.

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/decisions/standardy-przeniesienie-poc.decisions.md` — rozdziały 1, 18, 19
- `docs/decisions/standardy-implementacja.decisions.md` — rozdział 13
- `docs/external/redai-materialy/01-FEBO-HR-01-zastepstwa-w-okresie-urlopowym.md` — treść
  do przeniesienia, już w formacie docelowym

## Co powstaje

**Przenosimy jedną procedurę** — `FEBO-HR-01` „Zastępstwa w okresie urlopowym", napisaną
przez właściciela procesu w portalowym POC.

| Pole | Wartość |
|---|---|
| Identyfikator | `FEBO-HR-01` (zachowany) |
| Kategoria | **HR** — w POC było „Administracja", co przeczyło identyfikatorowi |
| Właściciel treści | stanowisko **Specjalista ds. Kadr i Płac** |
| Stan | **szkic do zatwierdzenia**, nie procedura obowiązująca |
| Autor | Izabela Młot-Wójcicka |

**Wchodzi jako szkic celowo**: obieg akceptacji ma zadziałać przynajmniej raz naprawdę,
zanim ktokolwiek na nim polegnie. Zatwierdzi ją ktoś z bezpośrednich podwładnych autorki,
zgodnie z regułą o linii podległości.

**Czego nie przenosimy**: procedury testowej `FEBO-IT-99` ani pustego szkicu. Pierwsze
wrażenie z nowej aplikacji nie powinno być pustym rekordem. Dokumentów spoza POC (Word,
dysk sieciowy, segregatory) ta migracja nie obejmuje.

## Wygaszenie POC

Narzędzie „Standardy" w portalu FEBO przechodzi **w tryb tylko do odczytu w dniu
uruchomienia lokatora**, a po przeniesieniu treści znika. Dwa miejsca z procedurami to po
miesiącu dwie różne odpowiedzi na pytanie, która wersja obowiązuje.

**To jest zmiana w portalu, nie w Intranecie** — wykonuje się ją poza tym repozytorium
i wymaga osobnego uzgodnienia z osobą prowadzącą portal.

## Na co uważać

Treść przenieś **bez zmian merytorycznych**. Formatowanie tak, sens nie — to procedura
zatwierdzona przez człowieka, a nie materiał do przeredagowania.

## Karta jest skończona, gdy

Procedura jest w bibliotece jako szkic z kompletną metryką, otwiera się pod stałym adresem,
a jej zgłoszenie do zatwierdzenia trafia do właściwej osoby.
