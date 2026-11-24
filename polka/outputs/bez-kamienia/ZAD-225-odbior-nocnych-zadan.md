<!--
opis: Odbiór testerski na dev pięciu nocnych zadań kont i audytu (ZAD-097): 7 punktów OK, 8 uwag bez blokerów, werdykt do PR.
rodzaj: wynik
zadanie: ZAD-225
rozmowa: ZAD-097 P1: Konta i audyt — pięć nocnych zadań z kontraktu etapu 1 n · Tester
zmiana: 29.10.2026, ZAD-097 P1: Konta i audyt — pięć nocnych zadań z kontraktu etapu 1 n · Tester
status: do akceptacji
slowa: ZAD-097, nocne zadania, audit.archive, purge_security, purge_tokens, purge_login_attempts, refresh_substitutions, retencja, bezpiecznik archiwum
-->

# ZAD-225 / Z-78 — odbiór pięciu nocnych zadań kont i audytu na dev

Gałąź `zad-097-nocne-zadania` @03fe7be, środowisko dev, 29.10.2026 (godziny w UTC, czas polski = UTC+1).
Kodu na gałęzi nie zmieniałem, nic nie scalałem, test/prod, LogTMS, Optima i repo wdrożenia nietknięte.

## Werdykt: **DO PR** (bez blokerów; dwie uwagi średniej wagi do decyzji przed prod)

## Tabela punktów

| # | Punkt | Wynik | Dowód |
|---|---|---|---|
| 1 | Rejestr i harmonogramy, automat */15 | OK | `core_jobs.job_definition`: accounts.refresh_substitutions `*/15` (normal, 300 s), audit.archive `30 2` (low, 1800 s), audit.purge_security `50 2` (low, 600 s), accounts.purge_tokens `25 4` (low, 300 s), accounts.purge_login_attempts `35 4` (low, 300 s); brak nadpisań harmonogramów. Przebiegi z harmonogramu (trigger=schedule) refresh_substitutions o 14:30, 14:45, 15:00, 15:15, 15:30 — wszystkie success (procrastinate 132204, 132224, 132244, 132267 i kolejny). |
| 2 | Zastępstwa w dzienniku audytu | OK | Zastępstwo testowe 8c027c45-… (rola directory.reader, 15:27:56–15:32:56). run_now 132286 → jeden wpis `substitution_started` (15:29:34). Przebieg z harmonogramu 15:30 i run_now 132293/132295/132297 przed końcem — brak duplikatów. run_now 132299 (15:33:44) → jeden wpis `substitution_ended`. Zastępstwo usunięte po teście. |
| 3 | purge_tokens i purge_login_attempts po obu stronach granicy | OK | 8 tokenów testowych; purge_tokens (run 132291, 6 ms) usunął: użyty 25 h temu, wygasły 25 h temu, użyty 24 h 1 min temu; zostały: użyty 23 h, wygasły 23 h, żywy, 1 min, 23 h 59 min. Próby logowania (run 132292): usunięte 91 d i 90 d 1 min, zostały 89 d 23 h i 89 d (433 → 431). Po teście: tokeny testowe 0, próby 429 (stan wyjściowy), zastępstwa testowe 0. |
| 4 | purge_security i archive na postarzonych wpisach | OK (testami) | Na dev nie ma nic należnego (najstarsze `retention_until` 2027-08-19/21). Wstawienie postarzonych wpisów wymagałoby obejścia niezmienności dziennika, więc zgodnie ze zleceniem nie robiłem tego na dev. W worktree /home/ubuntu/worktrees/zad097: `test_nightly_jobs.py` (19), `test_aud_retention.py` (9, BR-AUD-009/010), `test_acc_auth.py` (14) — **42 passed w 15,2 s** (z zamontowanym pakietem wdrożenia FEBO tylko do odczytu, bez niego setup pada na konfiguracji). |
| 5 | Bezpiecznik: bez znacznika odmowa, nic nie usunięte | OK | Znacznik przemianowany → run_now audit.archive (procrastinate 132303) **failed**, `ArchiveUnavailableError` (PermanentError, bez ponowień), 2 ms. Wiersze audytu 3006 → 3007, jedyny przyrost to wpis `job_run_manually`. Martwa kolejka audit.archive 1 → 2. Panel zdrowia: failures_last_hour 0 → 1, last_run audit.archive = failed, alert dead_letter_waiting 630 → 631. Znacznik przywrócony → run_now (procrastinate 132304) **success**, 9 ms; audyt 3007 → 3008 (znów tylko `job_run_manually`); w katalogu tylko `.intranet-archive`, bez pliku sondy. |
| 6 | Log workera | OK | Ostatnie 3 h: dokładnie 2 wpisy ERROR — zamierzone odmowy audit.archive 132194 (programista) i 132303 (mój test). Innych ERROR/CRITICAL brak. |
| 7 | Przegląd diffu | OK z uwagami | 16 plików, +536/−19, scalenie z aktualnym main czyste. fsync pliku i katalogu przed usunięciem wierszy — poprawnie. Godziny nie kolidują (25 4 / 35 4 obok 20 4 sesji; 02:30/02:50 obok 02:15 integrations.check_expiry; */15 dzieli takt z publish_backlog_metric, nieszkodliwe). Uwagi niżej. |

## Uwagi i błędy (od najcięższych)

1. **Średnia — rozjazd ścieżki archiwum na test/prod.** Pakiet wdrożenia FEBO ustawia na sztywno `/var/backups/febo-intranet/audit`, a compose montuje magazyn pod `${AUDIT_ARCHIVE_ROOT:-/var/backups/intranet/audit}`. Jeśli .env test/prod nie ma zmiennej, archiwum trafi w niezamontowany katalog i odmówi (bezpiecznie, ale nocne archiwum nie ruszy). Do dopisania w instrukcji wdrożenia albo ujednolicenia.
2. **Średnio-niska — archiwum ładuje wszystkie należne wpisy naraz do pamięci.** Na dev bez znaczenia; pierwszy przebieg na prod po roku może objąć dużo wierszy. Warto partiami (np. po miesiącu).
3. **Niska — duplikaty przy append po awarii** między zapisem pliku a usunięciem wierszy (zgłoszone też przez programistę). Kolejny przebieg dopisze te same wpisy drugi raz; dane się nie gubią.
4. **Niska — miesiąc pliku i `retention_until` liczone w UTC**, nie czasie polskim: wpis z 1. dnia miesiąca przed 01:00/02:00 trafia do pliku poprzedniego miesiąca (kod sprzed zmiany).
5. **Niska — purge_security = 365 dni zamiast „12 miesięcy”** (w roku przestępnym różnica jednego dnia).
6. **Niska — zmiana czasu:** w marcu 02:30 i 02:50 nie istnieją (przebieg przepada na jedną noc), w październiku mogą pójść dwa razy (zadania idempotentne, więc bez szkody).
7. **Niska — `context` JobRun zostaje `{}`**: liczby rozpoczętych/zakończonych zastępstw i usuniętych wierszy nie są zapisane przy przebiegu.
8. **Niska (stan dev, nie gałąź) — lista martwej kolejki w panelu** pokazuje 100 pozycji sortowanych po dacie wygaśnięcia; wpisów audit.archive nie widać, bo zasłania je 630 starych `notify.scan_due_tasks` z 30.09. Licznik i alert działają.

## Stan dev po teście

- Znacznik `.intranet-archive` na miejscu, AUDIT_ARCHIVE_ROOT w .env bez zmian, plik sondy usunięty.
- Rekordy testowe usunięte (tokeny, próby logowania, zastępstwo). Wpisy audytu z testu (`job_run_manually`, `substitution_started/ended`) zostają — dziennik jest niezmienny.
- W martwej kolejce zostają dwa wpisy audit.archive (cf53126d programisty i mój ze 132303) jako dowód odmowy; można je zamknąć z panelu.
