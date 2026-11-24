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
| KM-1 Fundament powłoki gotowy | 17.11.2026 (pierwotnie 25.08.2026) | do akceptacji PM |
| KM-2 Pierwsza aplikacja u użytkowników FEBO | 26.11.2026 (pierwotnie 31.10.2026) | otwarty |
| KM-5 Środowiska test i prod gotowe | 27.11.2026 (pierwotnie 31.10.2026) | otwarty |
| KM-7 UX i UY | 27.11.2026 (pierwotnie 20.11.2026) | otwarty |
| KM-3 Pięć aplikacji w użyciu | 31.01.2027 | otwarty |
| KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania | 31.03.2027 | otwarty |
| KM-6 Wdrożenie po uruchomieniu prod | 30.06.2027 | otwarty |

## Postęp

- Zadania: 233 z 321 zrobionych (73%), w toku: 6, do akceptacji: 1, po terminie: 4, zablokowane: 15.
- Bez kamienia: 145/145
- KM-1 Fundament powłoki gotowy (17.11.2026): 11/11
- KM-2 Pierwsza aplikacja u użytkowników FEBO (26.11.2026): 21/31
- KM-5 Środowiska test i prod gotowe (27.11.2026): 5/28
- KM-7 UX i UY (27.11.2026): 18/21
- KM-3 Pięć aplikacji w użyciu (31.01.2027): 20/40
- KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania (31.03.2027): 13/45
- KM-6 Wdrożenie po uruchomieniu prod (30.06.2027): brak zadań

## Ostatnio dostarczone

| Zadanie | Status | Wynik | Pliki |
|---|---|---|---|
| ZAD-333 Scenariusze odbioru bez zapisanych widoków | DO AKCEPTACJI | PR: https://github.com/feboLogisticGit/febo-intranet-wdrozenie/compare/main...zad-333-scenariusze-po-km7?expand=1 | [ZAD-333-kod.md](outputs/KM-7-ux-i-uy/ZAD-333-kod.md) |
| ZAD-339 Bez zdania o celu | ZROBIONE | PR ZAD-339 scalony 24.11.2026 (gałąź zad-339-bez-zdania-o-celu); założenia na main 4d4d196 | [ZAD-339-kod.md](outputs/KM-7-ux-i-uy/ZAD-339-kod.md) |
| ZAD-338 Testy powłoki bez wdrożenia | ZROBIONE | Scalone do main: https://github.com/SmallBigSteps/intranet/compare/main...zad-338-testy-powloki-bez-wdrozenia (testy powłoki bez wdrożenia, CI Backend without a deployment, CLAUDE.md 6.6) | [ZAD-338-kod.md](outputs/KM-1-fundament-powloki-gotowy/ZAD-338-kod.md) |
| ZAD-335 Osobny kolor panelu logowania | ZROBIONE | PR #289 w SmallBigSteps/intranet (tokeny login-brand, on-login-brand) i PR #28 w febo-intranet-wdrozenie (ciemny panel logowania FEBO), oba scalone 24.11.2026 | [ZAD-335-kod.md](outputs/KM-7-ux-i-uy/ZAD-335-kod.md) |
| ZAD-176 Repozytorium materiałów | ZROBIONE | Wymiana materiałów działa od 24.11.2026, zgoda: Pawel Mazek 24.11.2026. |  |
| ZAD-337 Test ról Kadr | ZROBIONE | PR: https://github.com/SmallBigSteps/intranet/compare/main...zad-337-test-rol-kadr?expand=1 (gałąź zad-337-test-rol-kadr, commit c59d997); notka na półce outputs/bez-kamienia/ZAD-337-kod.md | [ZAD-337-kod.md](outputs/bez-kamienia/ZAD-337-kod.md) |
| ZAD-336 Deklaracje źródeł z wdrożenia | ZROBIONE | PR scalony: https://github.com/SmallBigSteps/intranet/pull/new/zad-336-deklaracje-zrodel-z-wdrozenia (gałąź zad-336-deklaracje-zrodel-z-wdrozenia, commit 9bbb86d) | [ZAD-336-kod.md](outputs/bez-kamienia/ZAD-336-kod.md) |
| ZAD-329 Logo w pakiecie marki FEBO | ZROBIONE | PR powłoki SmallBigSteps/intranet #287 (scalony) i PR wdrożenia feboLogisticGit/febo-intranet-wdrozenie zad-329-logo-febo (scalony); opis na półce outputs/KM-7-ux-i-uy/ZAD-329-kod.md | [ZAD-329-kod.md](outputs/KM-7-ux-i-uy/ZAD-329-kod.md) |
| ZAD-330 Strona główna bez pamięci podręcznej | ZROBIONE | PR #286 scalony do main (merge c9f6205): nginx wysyła no-cache dla index.html i tras SPA, rok z immutable dla /assets/, 404 no-store dla brakujących plików; ścieżka e2e 09-cache | [ZAD-330-kod.md](outputs/KM-7-ux-i-uy/ZAD-330-kod.md) |
| ZAD-113 Format aplikacji — Standardy: przejście na układ bazowy komputer; tablet i telefon zachowane jako formaty dodatkowe. Wynika z ZAD-010 projektu planer (06.10.2026, Paweł Mazek): niezmiennik 35 zmieniony — format aplikacji wybiera się przy tworzeniu lokatora. | ZROBIONE | PR #282 scalony 23.11.2026 (razem z ZAD-328): https://github.com/SmallBigSteps/intranet/pull/282 |  |
