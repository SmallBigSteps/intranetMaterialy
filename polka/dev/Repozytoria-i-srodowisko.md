<!--
opis: Opis repozytoriów kodu Intranetu FEBO, kopii roboczych, stosu, poleceń, środowisk i zasad pracy z kodem (bez sekretów), stan na 16.10.2026.
rodzaj: dev
rozmowa: ZAD-174 Przeniesienie projektu do modułu PMO (półka, wiki, instrukcj
zmiana: 13.11.2026, nowy adres repozytorium SmallBigSteps/intranet (rozmowa zad-261-umiejscowienie-uprawnienie-przed)
status: roboczy
slowa: repozytorium,github,intranetFebo,febo-intranet-wdrozenie,stos,make,dev,pull request
-->
# Repozytoria i środowisko Intranetu FEBO

Stan na 16.10.2026. Sam kod jest w repozytoriach, tutaj tylko opis. Żadnych haseł, kluczy ani tokenów.

## Repozytoria

| Repozytorium | Co zawiera | Rola w projekcie |
|---|---|---|
| SmallBigSteps/intranet (dawniej intranetFebo; prywatne, gałąź main) | Powłoka Intranetu (`api/`, `web/`), aplikacje `apps/directory`, `apps/standards`, `apps/payments`, założenia: `CLAUDE.md`, `docs/contract/`, `docs/decisions/`, karty zadań, `docs/progress.md`, `.claude/` | Kod i założenia tego projektu |
| feboLogisticGit/febo-intranet-wdrozenie (prywatne) | Wdrożenie FEBO: łączniki LogTMS i zapis zapłat, marka, struktura firmy, `apps.json` | Projekt IntranetWdrozenieFebo; tu tylko odczyt przy testach z `INTRANET_DEPLOYMENT` |
| feboLogisticGit/febo-platnosci (prywatne) | Docelowy kod aplikacji Płatności | Projekt IntranetPlatnosci |
| SmallBigSteps/febo, folder `projekty/intranet20/` | Wspólne repozytorium projektów portalu: README, zadania, dziennik (wymiana co godzinę) | Pisze portal, nie rozmowy |
| Repozytorium materiałów intranet2.0 (osobne, prywatne; do założenia w ZAD-176) | Półka projektu bez `raw/` i status projektu (wysyłka co godzinę po zgodzie PM) | Pisze portal, nie rozmowy |

## Kopie robocze na serwerze portalu

- Kopia główna intranetFebo (main): katalog rozmowy `intranet20-czat-1008-1132`, podkatalog `intranetFebo`.
  Przed pracą `git pull`, po zmianach commit i push.
- Zadania kodu: osobne drzewa robocze `storage/worktrees/intranetFebo-zad<nr>` na gałęziach `zad-<nr>-<opis>`.
- Wdrożenie FEBO: `storage/worktrees/febo-intranet-wdrozenie`.

## Stos

- Backend: Python 3.12 (przypięty w obrazie), Django + DRF, Gunicorn + Uvicorn, PostgreSQL 17 (pgvector),
  kolejka Procrastinate w PostgreSQL, Redis tylko cache i sesje.
- Frontend: React + TypeScript strict, Vite, React Router; typy API generowane (drf-spectacular → openapi-typescript).
- Testy: pytest + pytest-django, jsonschema, vitest + axe-core, Playwright; bramki ruff, mypy strict, eslint, prettier.
- Kontenery: nginx, Docker Compose (`docker-compose.dev.yml`).

## Struktura repozytorium intranetFebo

- `api/` — Django: `core/` (powłoka), `tests/rules/` (test na regułę), `tests/architecture/` (niezmienniki).
- `web/src/` — `ui/`, `shell/`, `api/`, `brand/`; ekrany aplikacji kopiowane przy budowie.
- `apps/<klucz>/` — jedna aplikacja = jeden pakiet = jeden schemat bazy.
- `tools/checks/` — logika kontroli (hooki i `make review`), `e2e/`, `docker/`.
- `docs/contract/` (angielski, czyta agent), `docs/decisions/` (polski, dla ludzi), `docs/external/` (dokumenty uzgodnione z innymi).

## Polecenia

- `make status` — co następne; `make check` (lint, typy, testy) i `make review` — zadanie kończy się, gdy oba są zielone.
- `make schema` po każdej zmianie serializera; `make mutation MODULE=…` — testy mutacyjne.
- `make deploy` — wystawia na wspólny `dev` main albo wypchniętą gałąź, która go zawiera.

## Środowiska

- `dev`: https://dev.febo.redai.pl — wspólne środowisko robocze (VPS).
- `test` i `prod`: serwer lokalny FEBO; wdrożenia, migracje i obsługa należą do człowieka, agent ich nie dotyka.

## Decyzje

- 11.08.2026 (wydanie 1.1 z 12.10.2026): zmiany założeń commit i push do intranetFebo tego samego dnia, podpis `redAi <redai@febologistic.com>`
  (`docs/decisions/wersjonowanie-zalozen.decisions.md`).
- Kod: jedno zadanie = jedna gałąź = jedna sesja, pull request scala człowiek (Paweł Mazek, konto SmallBigSteps).
  Zmiany tego, co sprawdza CI (reguły, `tools/checks/`, workflowy), zawsze przez pull request.
- 16.10.2026 (ZAD-174): projekt zostaje przy obecnej praktyce; przejście na standard portalu
  (kopia główna w `storage/projects/intranet20/kod`, blokada wypychania na main) rozstrzyga ZAD-175.
