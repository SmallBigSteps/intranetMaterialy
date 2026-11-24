<!--
opis: Stan projektu intranet2.0: cel, osoby, kamienie, postęp i ostatnio dostarczone wyniki. Pisze portal, nie edytujcie.
rodzaj: stan
status: automat
-->
# Stan projektu: intranet2.0

## Karta

- Cel: Zastąpić rozproszone narzędzia i arkusze jedną firmową powłoką aplikacyjną, w której procesy FEBO działają jako aplikacje ze wspólną tożsamością, listą zadań i śladem audytowym — aby w sposób usystematyzowany odciążać pracowników od pracy ręcznej: każda aplikacja zaczyna jako narzędzie obsługiwane przez człowieka i dojrzewa do usługi działającej samodzielnie, bez przepisywania.
- Mierzalny efekt: Intranet jest przenośnym produktem: powłoka z aplikacjami ogólnymi (Struktura i kontakty, Procedury, WhatsApp) działa w FEBO i przechodzi odbiór w drugiej, fikcyjnej konfiguracji firmy bez zmiany kodu powłoki. Aplikacje ogólne są w realnym użyciu w FEBO, co potwierdzają zapisy korzystania (logi), a nie deklaracje; próg użycia ustala się osobno dla każdej aplikacji, zgodnie z rytmem pracy z nią. Aplikacje dziedzinowe FEBO (Płatności, SENT, Sankcje, Odprawy celne) są prowadzone w osobnych projektach.
- Klient: (brak)
- PM: Paweł Mazek, PO: Daniel Urbaniak
- Ramy: 11.08.2026 – 31.03.2027, faza: w toku
- Status: zagrożony

## Kamienie milowe

| Kamień | Termin | Status |
|---|---|---|
| KM-1 Fundament powłoki gotowy | 17.11.2026 (pierwotnie 25.08.2026) | otwarty |
| KM-2 Pierwsza aplikacja u użytkowników FEBO | 26.11.2026 (pierwotnie 31.10.2026) | otwarty |
| KM-5 Środowiska test i prod gotowe | 27.11.2026 (pierwotnie 31.10.2026) | otwarty |
| KM-7 UX i UY | 27.11.2026 (pierwotnie 20.11.2026) | otwarty |
| KM-3 Pięć aplikacji w użyciu | 31.01.2027 | otwarty |
| KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania | 31.03.2027 | otwarty |
| KM-6 Wdrożenie po uruchomieniu prod | 30.06.2027 | otwarty |

## Postęp

- Zadania: 230 z 320 zrobionych (72%), w toku: 6, do akceptacji: 2, po terminie: 4, zablokowane: 16.
- Bez kamienia: 145/145
- KM-1 Fundament powłoki gotowy (17.11.2026): 10/11
- KM-2 Pierwsza aplikacja u użytkowników FEBO (26.11.2026): 21/31
- KM-5 Środowiska test i prod gotowe (27.11.2026): 5/28
- KM-7 UX i UY (27.11.2026): 16/20
- KM-3 Pięć aplikacji w użyciu (31.01.2027): 20/40
- KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania (31.03.2027): 13/45
- KM-6 Wdrożenie po uruchomieniu prod (30.06.2027): brak zadań

## Ostatnio dostarczone

| Zadanie | Status | Wynik | Pliki |
|---|---|---|---|
| ZAD-176 Repozytorium materiałów | ZROBIONE | Wymiana materiałów działa od 24.11.2026, zgoda: Pawel Mazek 24.11.2026. |  |
| ZAD-339 Bez zdania o celu | DO AKCEPTACJI | PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-339-bez-zdania-o-celu | [ZAD-339-kod.md](outputs/KM-7-ux-i-uy/ZAD-339-kod.md) |
| ZAD-337 Test ról Kadr | ZROBIONE | PR: https://github.com/SmallBigSteps/intranet/compare/main...zad-337-test-rol-kadr?expand=1 (gałąź zad-337-test-rol-kadr, commit c59d997); notka na półce outputs/bez-kamienia/ZAD-337-kod.md | [ZAD-337-kod.md](outputs/bez-kamienia/ZAD-337-kod.md) |
| ZAD-335 Osobny kolor panelu logowania | DO AKCEPTACJI | PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-335-osobny-kolor-panelu-logowania | [ZAD-335-kod.md](outputs/KM-7-ux-i-uy/ZAD-335-kod.md) |
| ZAD-336 Deklaracje źródeł z wdrożenia | ZROBIONE | PR scalony: https://github.com/SmallBigSteps/intranet/pull/new/zad-336-deklaracje-zrodel-z-wdrozenia (gałąź zad-336-deklaracje-zrodel-z-wdrozenia, commit 9bbb86d) | [ZAD-336-kod.md](outputs/bez-kamienia/ZAD-336-kod.md) |
| ZAD-329 Logo w pakiecie marki FEBO | ZROBIONE | PR powłoki SmallBigSteps/intranet #287 (scalony) i PR wdrożenia feboLogisticGit/febo-intranet-wdrozenie zad-329-logo-febo (scalony); opis na półce outputs/KM-7-ux-i-uy/ZAD-329-kod.md | [ZAD-329-kod.md](outputs/KM-7-ux-i-uy/ZAD-329-kod.md) |
| ZAD-330 Strona główna bez pamięci podręcznej | ZROBIONE | PR #286 scalony do main (merge c9f6205): nginx wysyła no-cache dla index.html i tras SPA, rok z immutable dla /assets/, 404 no-store dla brakujących plików; ścieżka e2e 09-cache | [ZAD-330-kod.md](outputs/KM-7-ux-i-uy/ZAD-330-kod.md) |
| ZAD-113 Format aplikacji — Standardy: przejście na układ bazowy komputer; tablet i telefon zachowane jako formaty dodatkowe. Wynika z ZAD-010 projektu planer (06.10.2026, Paweł Mazek): niezmiennik 35 zmieniony — format aplikacji wybiera się przy tworzeniu lokatora. | ZROBIONE | PR #282 scalony 23.11.2026 (razem z ZAD-328): https://github.com/SmallBigSteps/intranet/pull/282 |  |
| ZAD-328 Standardy w stylu wytycznych | ZROBIONE | PR #282 scalony 23.11.2026: https://github.com/SmallBigSteps/intranet/pull/282 | [ZAD-328-kod.md](outputs/KM-7-ux-i-uy/ZAD-328-kod.md) |
| ZAD-334 Kapsułki i opcje marki | ZROBIONE | PR #284 scalony do main 23.11.2026 (core-ui 2.43, core-cockpit 3.23); wpis w docs/progress.md | [ZAD-334-kod.md](outputs/KM-7-ux-i-uy/ZAD-334-kod.md) |
