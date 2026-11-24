<!--
opis: Opis repozytorium produktu Intranet (SmallBigSteps/intranet): stos, struktura, dostęp z lab, decyzje
rodzaj: wiki
rozmowa: Intranet — ustalenia projektu
zmiana: 18.11.2026, Intranet — ustalenia projektu
status: roboczy
slowa: repozytorium, intranet, powłoka, lokator, wdrożenie, Django, React, marka, makieta, dostęp
-->
# Repozytorium produktu Intranet: `SmallBigSteps/intranet`

Stan na 18.11.2026 (gałąź `main`, ostatnia zmiana 16.11.2026, ok. 1010 zmian, ok. 1300 plików).
To repozytorium **produktu**, wspólnego z wdrożeniem FEBO. Nie jest to repozytorium kodu Canexpol (ZAD-004).

## Adres i dostęp

| Pole | Wartość |
|---|---|
| Adres | `git@github.com:SmallBigSteps/intranet.git` |
| Właściciel prac | projekt intranet2.0 (portal FEBO), PM Paweł Mazek |
| Dostęp Canexpol | tylko odczyt, tylko z maszyny `lab`: klucz `intranet_ro`, host w konfiguracji SSH `github-intranet` |
| Z serwera portalu | brak dostępu (żaden klucz portalu nie ma prawa do tego repozytorium) |
| Kopia do czytania | na `lab`, katalog `~/zwiad/intranet` (aktualizacja: `git pull` w tym katalogu) |

## Czym jest

Powłoka aplikacyjna, w której działają małe aplikacje dziedzinowe nazywane lokatorami. Powłoka daje im
wspólnie konta i role, logowanie (z drugim składnikiem), listę zadań, kolejkę, powiadomienia, pliki,
wyszukiwarkę, dziennik audytu, kokpit z kafelkami aplikacji i rejestr pól. Od 09.10.2026 jest to
**produkt przenośny**: kod powłoki nie zna żadnej firmy, a marka, nazwa systemu, stanowiska, lista aplikacji
i łączniki do systemów źródłowych przychodzą z pakietu wdrożenia firmy.

## Stos

Python 3.12, Django i DRF, PostgreSQL 17, kolejka Procrastinate w PostgreSQL, Redis tylko na cache i sesje,
React z TypeScriptem (strict) i Vite, nginx, Docker Compose. Systemy źródłowe tylko do odczytu.

## Struktura

| Katalog | Zawartość |
|---|---|
| `api/` | serwer powłoki (`core/`, `config/`, testy) |
| `web/src/` | ekrany powłoki; `ui/` komponenty, `brand/` neutralny pakiet marki (`brand.json`), `shell/`, `cockpit/` |
| `apps/<klucz>/` | aplikacje (lokatory): serwer z migracjami, ekrany, `RULES.md`; dziś Struktura i kontakty, Procedury (dawne Standardy), Płatności |
| `deploy/` | skrypty wdrożenia na test i prod (wdrożenie, wycofanie, kopia, odtworzenie, monitoring) |
| `docs/contract/` | kontrakty dla agentów (po angielsku), 110 kart zadań, reguły BR-XXX |
| `docs/decisions/` | uzasadnienia decyzji (po polsku) |
| `docs/external/brand/` | makiety HTML marki z arkuszem stylów i zrzutami: lista zadań, szczegóły zadania, formularz z błędem, komponenty |
| `CLAUDE.md`, `START.md`, `00-INDEKS.md` | pamięć projektu dla agenta, procedura startu, indeks dokumentacji |

## Jak podpina się firma

- Pakiet wdrożenia firmy (dla FEBO: osobne repozytorium `febo-intranet-wdrozenie`) podaje w `apps.json`
  listę aplikacji do zainstalowania; powłoka nie ma własnej listy.
- Marka firmy to nakładka `brand/brand.json` na neutralny pakiet powłoki: podaje tylko zmieniane wartości.
  Tryb ciemny jest zdolnością powłoki (od 16.11.2026).
- Nazwa systemu to ustawienie (`SYSTEM_NAME`); struktura firmy (grupy, stanowiska, działy, role) to dane
  startowe wdrożenia.
- Każda aplikacja ma wpisany format bazowy (komputer, tablet albo telefon); bez niego nie powstaje.

## Uruchomienie

Polecenia `make` w katalogu głównym: `install`, `check` (lint, typy, testy), `up` i `down` (Docker Compose),
`status` (co następne w planie). Pakiet wdrożenia wskazuje zmienna `INTRANET_DEPLOYMENT_DIR`; bez niej
powłoka startuje bez aplikacji. Szczegóły: `START.md`, `deploy/README.md`.

## Decyzje ważne dla Canexpol

| Data | Decyzja | Plik w repozytorium |
|---|---|---|
| 09.10.2026 | Intranet jako produkt przenośny | `docs/decisions/intranet-jako-produkt.decisions.md` |
| 13.10.2026 | Rejestracja aplikacji przez konfigurację wdrożenia | `docs/decisions/rejestracja-aplikacji.decisions.md` |
| 13.10.2026 | Marka, nazwa systemu i struktura jako konfiguracja wdrożenia | `docs/decisions/marka-i-struktura-we-wdrozeniu.decisions.md` |
| 06.10.2026 | Format aplikacji wybierany przy tworzeniu lokatora | `docs/decisions/format-aplikacji.decisions.md` |
| 16.11.2026 | Marka według Tablera i tryb ciemny w powłoce | `docs/decisions/marka-tabler-i-tryb-ciemny.decisions.md` |

Po stronie Canexpol: `zalozenia/wdrozenie-produktu-intranet.md` (18.11.2026).
