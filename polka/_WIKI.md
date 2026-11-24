<!--
opis: Wiki projektu intranet2.0: po co i dla kogo, co budujemy, najważniejsze założenia, gdzie co jest, otwarte pytania.
rodzaj: wiki
rozmowa: ZAD-174 Przeniesienie projektu do modułu PMO (półka, wiki, instrukcj
zmiana: 16.10.2026, ZAD-174 Przeniesienie projektu do modułu PMO (półka, wiki, instrukcj
status: roboczy
slowa: wiki,intranet,powloka,lokatory
-->
# intranet2.0 — Intranet FEBO

## Po co i dla kogo

Intranet FEBO to jedna firmowa powłoka aplikacyjna zamiast rozproszonych narzędzi i arkuszy.
Procesy FEBO działają w niej jako małe aplikacje (lokatory) ze wspólną tożsamością, listą zadań,
powiadomieniami, plikami, wyszukiwarką i dziennikiem audytu. Aplikacja zaczyna jako narzędzie
obsługiwane przez człowieka i dojrzewa do usługi działającej samodzielnie, bez przepisywania.

- Dla kogo: pracownicy biurowi FEBO (spedycja, finanse, kadry, kierownictwo), administrator techniczny.
- PM: Paweł Mazek, PO: Daniel Urbaniak. Ramy: 11.08.2026 – 31.03.2027 (kamienie w `_STAN.md`).
- Mierzalny efekt: przenośna powłoka z aplikacjami ogólnymi (Struktura i kontakty, Procedury,
  WhatsApp) w realnym użyciu w FEBO, potwierdzonym logami, i odbiór w drugiej, fikcyjnej konfiguracji firmy.

## Co budujemy i jak to działa

- **Powłoka** (moduły core: konta i role, audyt, kolejka, powiadomienia, lista zadań, pliki,
  wyszukiwarka, metryki, rejestry, API zasilania źródeł) i **lokatory** w `apps/<klucz>/`,
  każdy z własnym schematem bazy i jednym połączeniem powłoki.
- Aplikacje dziś: **Struktura i kontakty** (`directory`), **Standardy FEBO → Procedury** (`standards`),
  **Płatności** (`payments`, prowadzone w projekcie IntranetPlatnosci). WhatsApp: decyzje gotowe, budowa do KM-4.
- Stos zamknięty: Python 3.12, Django + DRF, PostgreSQL 17, kolejka Procrastinate w PostgreSQL,
  Redis tylko cache i sesje, React + TypeScript strict + Vite, nginx + Docker Compose.
- Systemy źródłowe tylko do odczytu: LogTMS (MySQL) przez łącznik wdrożenia FEBO, Comarch Optima
  przez serwis firmy i API zasilania. Zapis do LOG TMS tylko w dwóch przypadkach z decyzji (zapłaty, rejestr użytkowników).
- Środowiska: `dev` (dev.febo.redai.pl, wspólne, wystawiane `make deploy`), `test` i `prod` na serwerze
  lokalnym FEBO — tych nie dotyka agent, tylko człowiek.
- Praca agenta: jedno zadanie = jedna gałąź = jedna sesja; karta zadania z `docs/contract/plan/`,
  koniec zadania = zielone `make check` i `make review`.

## Najważniejsze założenia i decyzje

Pełne założenia są w repozytorium (`CLAUDE.md` z 38 niezmiennikami, `docs/contract/`, `docs/decisions/`).
Najważniejsze kierunki:

- **Intranet jako produkt przenośny** (09.10.2026): powłoka bez wiedzy o FEBO; marka, struktura firmy,
  łączniki i lista aplikacji przychodzą z wdrożenia (`feboLogisticGit/febo-intranet-wdrozenie`).
  Płatności, SENT, Sankcje i Odprawy celne idą do osobnych projektów.
- **Rejestracja aplikacji przez konfigurację wdrożenia** (`apps.json`), aplikacje jako pakiety.
- **API zasilania źródeł** (12.10.2026): serwis firmy wysyła dane, których Intranet sam nie czyta (Optima).
- **Środowiska test i prod** od października 2026, ścieżka dev → test → prod (KM-5).
- **Zmiana założeń = commit i push do repozytorium tego samego dnia**; decyzja, której nie ma w repozytorium, nie istnieje.
- Kolumna, pole manifestu i zakres uprawnienia nigdy nie czekają na późniejszy sprint; zachowanie może.

Materiały robocze z rozmów (przeglądy UX i kodu, plan serwera, dokumenty Standardów, instrukcja KM-5)
leżą w `zalozenia/`; to tło decyzji, a obowiązuje wersja w repozytorium.

## Gdzie co jest

- `_STAN.md`: cel, osoby, kamienie i postęp (pisze portal co godzinę).
- `_INSTRUKCJA-PROJEKTU.md`: jak się pracuje w tym projekcie, co gdzie trafia, repozytoria.
- `dev/`: opis repozytoriów kodu — `Repozytoria-i-srodowisko.md`.
- `zalozenia/`: dokumenty z rozmów projektu (serwer i środowiska, przegląd UX i kodu, Standardy FEBO,
  brief i podsumowanie dla wykonawcy, wytyczne lokatora, formatka i prośba do kadr, KM-5).
- `raw/`: materiały od ludzi (schemat struktury FEBO z 17.08.2026, maile o zasobach i hurtowni danych,
  specyfikacja serwera dev, materiał o refaktoryzacji z AI).
- `outputs/`: wyniki zadań według etapów (m.in. ZAD-005 Struktura i kontakty, ZAD-038 audyt kodu).
- Repozytorium kodu i założeń: `SmallBigSteps/intranet` — tam kontrakty, karty zadań, `docs/progress.md`.
- Wspólne repozytorium projektów portalu: `SmallBigSteps/febo`, folder `projekty/intranet20/`
  (zadania i dziennik, wymiana co godzinę).
- Sąsiednie projekty: IntranetPlatnosci (aplikacja Płatności, jej testy i materiały Optimy),
  IntranetWdrozenieFebo (wdrożenie FEBO: łączniki, marka, struktura).

## Otwarte pytania i ryzyka

- **KM-2** (31.10.2026) zależy od Płatności na prod i potwierdzenia korzystania przez Monikę Nowak (ZAD-020, ZAD-148).
- **KM-5** (13.11.2026): odbiór test i prod po stronie FEBO (ZAD-162).
- **Przenośność**: bramka i odbiór w fikcyjnej konfiguracji (ZAD-134, ZAD-135) czekają.
- **Repozytorium kodu a standard portalu** (ZAD-175): kopia główna leży w katalogu rozmowy, a nie
  w `storage/projects/intranet20/kod`, i nie ma blokady wypychania na main — decyzja PM, czy przejść na standard.
- **Półka w repozytorium materiałów** (ZAD-176): półka pójdzie do osobnego prywatnego repozytorium projektu,
  którego jeszcze nie ma; po jego podłączeniu PM daje zgodę przyciskiem w sekcji Repozytorium na stronie projektu.
- WhatsApp: połączenie nieoficjalne — automaty ryzykują blokadę numeru pracownika (decyzja 23.09.2026).
