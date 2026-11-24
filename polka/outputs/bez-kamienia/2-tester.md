<!--
opis: Raport testera ZAD-100: opiekun bez drugiego składnika nie ma dostępu do kolejki; werdykt PRZECHODZI
rodzaj: wynik
zadanie: ZAD-223
rozmowa: ZAD-100 P1: Panel kolejki zadań w tle — opiekun aplikacji (rola *.ma · Tester
zmiana: 29.10.2026, ZAD-100 P1: Panel kolejki zadań w tle — opiekun aplikacji (rola *.ma · Tester
status: do akceptacji
slowa: ZAD-100, ZAD-223, BR-ACC-013, TOTP, maintainer, core.jobs, test, regresja
-->

# Z-76 / ZAD-223: test opiekuna bez drugiego składnika w kolejce

## Werdykt: **PRZECHODZI**

Gałąź `zad-100-maintainer-totp`, commit **2f45390**. Nie znalazłem pominiętego miejsca. Każda trasa `/api/v1/jobs/...` odmawia opiekunowi bez urządzenia. Po potwierdzeniu urządzenia opiekun widzi to samo co przed zmianą. Administrator techniczny i opiekunowie ról bez wymogu drugiego składnika widzą dokładnie to samo co na kodzie sprzed zmiany (82a22ec). Pełny zestaw testów, ruff i mypy są zielone.

Kodu na gałęzi nie zmieniałem. Tymczasowy plik testów leżał w `api/tests/` tylko na czas przebiegu i został usunięty (`git status` czysty).

## Jak sprawdzone

Napisałem tymczasowy plik testów pytest. Uruchomiłem go dwa razy na tej samej bazie testowej:
- **na gałęzi** (2f45390), przez `pytest-zad100.sh`;
- **na kodzie sprzed zmiany** (82a22ec, `git archive` do `/tmp`, ten sam skrypt z podmienionym drzewem).

Dane w każdym teście:
- zadania `probe.*` z rejestru, do tego `other.job` (inna aplikacja) i `lite.job` (aplikacja z rolą opiekuna bez wymogu drugiego składnika);
- po jednym przebiegu `JobRun` dla `probe.sync`, `other.job` i `lite.job`;
- po jednym martwym zadaniu dla `probe.record`, `other.job` i `lite.job`;
- wyłącznik `logtms.ledger` i po jednym wywołaniu zewnętrznym z `source_key` probe, other i lite.

Przy każdej trasie zapisywałem kod odpowiedzi, to, czy w treści są dane `probe`, i skutek uboczny (liczba przebiegów i stan martwych zadań przed i po).

## 1. Opiekun `probe.maintainer` (`requires_totp=True`): trasy bez urządzenia i po potwierdzeniu

| Trasa | Przed zmianą, bez urządzenia | **Po zmianie, bez urządzenia** | Po zmianie, po potwierdzeniu |
|---|---|---|---|
| GET `jobs/health/` | 200 (dane probe) | **403** | 200 |
| GET `jobs/definitions/` | 200 (13 zadań probe) | **403** | 200, tylko `probe.*` |
| GET `jobs/definitions/probe.sync/schedule/` | 403 | **403** | 403 (tylko admin, jak dotąd) |
| PATCH `jobs/definitions/probe.sync/schedule/` | 403 | **403** | 403 (jak dotąd) |
| POST `jobs/definitions/probe.sync/run/` | **202, zadanie uruchomione** | **403, bez skutku** | 202 |
| GET `jobs/runs/` | 200 | **403** | 200, tylko `probe.sync` |
| GET `jobs/runs/?job=probe.sync` | 200 | **403** | 200 |
| GET `jobs/dead-letter/` | 500 (znana usterka) | **403** | 500 (znana usterka, niżej) |
| POST `jobs/dead-letter/<id>/resume/` | 403 (`may_resume`) | **403** | 200, wznowione |
| GET `jobs/integrations/` | 200 (wywołania probe) | **403** | 200, tylko wywołania probe |
| POST `jobs/integrations/logtms.ledger/reset/` | 403 | **403** | 403 (tylko admin, jak dotąd) |

- Przy odmowie przez wejście treść odpowiedzi to zawsze dotychczasowy komunikat: „Panel zadań w tle jest dla administracji technicznej i opiekunów aplikacji.” Harmonogram i reset wyłącznika mają swój komunikat: „Ta operacja należy do administracji technicznej.”, jak przed zmianą.
- Bez urządzenia żadna trasa nie zwróciła danych i nie zmieniła stanu (liczba przebiegów i martwych zadań bez zmian).
- Funkcje bez urządzenia: `visible_definitions` jest pusty, `DeadLetterJob.visible_to` jest pusty, `calls.panel(...)["recent"]` jest pusty.
- Po potwierdzeniu: zakres to `probe`, martwe zadanie `probe.record`, wywołanie `probe-FV/1`.
- Najpoważniejsza dziura przed zmianą to **ręczne uruchomienie zadania (202) bez drugiego składnika**. Po zmianie jest zamknięta.

### Przypadek mieszany (ważny dla zakresu)

Osoba ma `lite.maintainer` bez wymogu oraz `probe.maintainer` z wymogiem i nie ma urządzenia.

- **Przed zmianą:** zakres `lite + probe`, uruchomienie `probe.sync` dawało 202.
- **Po zmianie:** wejście na panel jest otwarte (200, słusznie, przez `lite`), ale zakres to tylko `lite`. Widać `lite.job`, martwe zadanie `lite.job` i wywołanie `lite-FV/1`. W odpowiedziach health, definitions, runs i integrations nie ma śladu `probe`.
- Uruchomienie `probe.sync` daje **404**, wznowienie martwego `probe.record` daje **404**.

## 2. `process_owner` (pkt 3 zlecenia)

**Osoba z `probe.maintainer` z wymogiem, bez urządzenia, `process_owner` zadania `other.job`:**
- wszystkie 11 tras dają **403**;
- POST `jobs/definitions/other.job/run/` daje **403**;
- stan bez zmian.

Wniosek: brak wejścia na panel, zgodnie z oczekiwaniem.

Uwaga: same funkcje zakresu nadal zwracają zadanie po `process_owner` (`visible_definitions` → `['other.job']`, `DeadLetterJob.visible_to` → `['other.job']`). Robią to z założenia: `process_owner` to pole zadania, nie rola, i tak to opisuje docstring `visibility.py`. Te funkcje wywołują wyłącznie widoki za wejściem `AnswersForJobs` oraz `health.snapshot`. Innych wywołań w `api/` nie ma (sprawdzone grepem), więc dostępu to nie daje.

**Ta sama osoba po potwierdzeniu urządzenia:** wejście 200, zakres to `probe.*` plus `other.job` (po `process_owner`), jak przed zmianą.

**Osoba z urządzeniem, będąca tylko `process_owner`, bez żadnej roli opiekuna:** wszystkie trasy dają 403, bez zmian względem stanu sprzed poprawki.

**Do decyzji (nie błąd tej zmiany):** opiekun z działającą rolą jednej aplikacji, który jest też `process_owner` zadania innej aplikacji, widzi i uruchamia to zadanie bez sprawdzania drugiego składnika. Powód: `process_owner` nie jest rolą. To zachowanie istniało przed zmianą i PM wprost zostawił ten zakres bez zmian.

## 3. Przegląd kodu: inne miejsca (pkt 1 zlecenia)

Przeszukałem całe `api/` (nie tylko `core/jobs`) pod kątem `effective_roles`, `.maintainer`, `has_role`, `visible_definitions`, `visible_to`, `calls.panel` i odczytów modeli kolejki (`JobRun`, `DeadLetterJob`, `ExternalCall`, `JobDefinition`, `IntegrationCircuit`).

| Miejsce | Do czego służy | Czy wpuszcza opiekuna bez urządzenia |
|---|---|---|
| `core/jobs/views.py` `_answers_for_any_application` / `AnswersForJobs` | wejście na panel (health, definitions, run, runs, dead-letter, resume, integrations) | **nie**, poprawione (`maintained_applications`) |
| `core/jobs/health.py` `visible_definitions` | lista zadań, `RunNowView`, `runs_for`, `periodic_section`, `duration_chart` | **nie**, poprawione |
| `core/jobs/calls.py` `_visible` → `calls.panel` → `integrations_section` | rejestr wywołań zewnętrznych | **nie**, poprawione |
| `core/jobs/models.py` `DeadLetterQuerySet.visible_to` | `dead_letter_section`, `DeadLetterListView`, `DeadLetterResumeView` | **nie**, poprawione |
| `core/jobs/dead_letter.py` `may_resume` | wznowienie martwego zadania | nie, już wcześniej `may_use_role` |
| `ScheduleView`, `IntegrationResetView` (`IsTechnicalAdmin`) | harmonogram, reset wyłącznika | nie, `may_use_role("shell.admin_tech")` |
| `core/jobs/manual.py`, `core/jobs/schedule.py` | uruchomienie i harmonogram | nie sprawdzają ról same, wołane tylko z widoków za wejściem |
| `health.snapshot`: `queue_section`, stan wyłączników, `_shared_services_section` (`core/services/health.py`), `_tenant_registry_section`, `_critical_roles_section` | sekcje panelu niezależne od osoby | nie, tylko za wejściem `AnswersForJobs` (bez zmian względem stanu sprzed poprawki) |
| zadania z kolejki do listy zadań (`aggregate.announce` w `dead_letter.py`, `circuits.py`) | powiadomienia o awarii | nie, kierowane tylko do `shell.admin_tech`; lista zadań filtruje po `totp.usable_roles` (`core/tasks/listing.py`, `core/tasks/visibility.py`) |
| `core/integrations/views.py:84` | tokeny integracji opiekuna aplikacji (poza kolejką) | nie, `may_use_role(f"{tenant}.maintainer")` |
| `core/accounts/views.py` `HasAnyRole` | wspólne uprawnienie po kluczu roli | nie, `may_use_role` |
| `core/cockpit/review.py` (`core.jobs.aggregate`) | sygnały kokpitu do administratora biznesowego | nie dotyczy danych kolejki ani roli opiekuna |
| pozostałe `effective_roles` (`totp.py`, `substitutions.py`, `accounts/views.py:633` „moje role”, migawka sesji) | definicja uprawnień, zastępstwa, profil | nie dotyczą kolejki |

W `core/jobs` nie ma już odczytu `effective_roles` (jedyne wystąpienie jest w docstringu `visibility.py`). **Pominiętego miejsca nie znalazłem.**

## 4. Brak regresji

Ten sam odcisk (kody odpowiedzi health, definitions, runs i integrations, klucze sekcji panelu, lista zadań okresowych, martwe zadania, zakres `visible_definitions`, `visible_to`, `calls.panel`) zebrałem dla czterech profili na kodzie sprzed zmiany i po zmianie.

| Profil | Wynik |
|---|---|
| `shell.admin_tech` z urządzeniem | **identyczny** (wszystkie zadania, wszystkie martwe, wszystkie wywołania) |
| `lite.maintainer` bez wymogu, bez urządzenia | **identyczny** (tylko `lite`) |
| `lite.maintainer` bez wymogu, z urządzeniem | **identyczny** |
| `probe.maintainer` z wymogiem, z urządzeniem | **identyczny** (tylko `probe`) |

Narzędzia na `api/`:

| Sprawdzenie | Wynik |
|---|---|
| Pełny zestaw API (`pytest-zad100.sh` bez argumentów, domyślnie bez `slow` i `reads_source`) | **1053 passed**, kod wyjścia 0 |
| Testy `slow` z `test_jobs_health.py` (`-m slow`) | 1 passed |
| `ruff check api apps` | All checks passed |
| `ruff format --check api apps` | 429 files already formatted |
| `mypy .` w `api/` | Success, no issues found in 350 source files |

Jedno ostrzeżenie pytest przy sprzątaniu bazy testowej („database test_intranet is being accessed by other users”) pochodzi ze środowiska lokalnego, nie z testów.

Tymczasowy plik testów na kodzie sprzed zmiany: 3 failed, 5 passed (opiekun bez urządzenia dostawał 200 i uruchamiał zadanie). Na gałęzi: 8 passed. Mamy więc czerwone przed i zielone po, niezależnie od testów programisty.

## Znana usterka poza zakresem: potwierdzenie

`GET /api/v1/jobs/dead-letter/` kończy się `FieldError: Cannot resolve keyword 'started_at'` (500) dla każdego, kto przejdzie przez wejście. Błąd pojawia się **tak samo na kodzie sprzed zmiany (82a22ec)**. Przyczyna: `RunPagination` z `ordering="-started_at"` na modelu `DeadLetterJob`. **Nie wynika z tej zmiany.** Opiekun bez urządzenia dostaje teraz na tej trasie 403, zanim dojdzie do błędu.

Dodatkowo, artefakt moich danych, nie błąd: wznowienie martwego zadania `other.job` po potwierdzeniu dało `DeclarationError`. Zadanie `other.job` dopisałem ręcznie i nie ma go w rejestrze kolejki. Prawdziwe zadania z rejestru (`probe.record`) wznawiają się poprawnie (200).

## Uwagi dla koordynatora

1. `origin/main` poszedł o dwa commity dalej (cdc99b3, 0c793d6: kontrakt kolejki 2.5 i 2.6, tylko `docs/`). Dlatego `git diff origin/main..HEAD` pokazuje też różnice w `docs/contract/core-jobs.contract.md` i `docs/decisions/open-gaps.md`, których ta gałąź nie zmienia. Próbne scalenie (`git merge-tree`) przechodzi **bez konfliktów**.
2. Ekran `web/` nie był klikany w przeglądarce, bo zlecenie mówi, że jest bez zmian. Werdykt opiera się na API i testach.

## Jak odtworzyć

1. Przebieg na gałęzi: `cd …/intranetFebo-zad100/api && /var/www/febo-portal/storage/worktrees/pytest-zad100.sh`.
2. Odtworzenie dziury na starym kodzie:
   - konto z grantem roli `probe.maintainer` (`requires_totp=True`), bez `TotpDevice`;
   - `POST /api/v1/jobs/definitions/probe.sync/run/`;
   - przed zmianą wynik 202, po zmianie 403.
