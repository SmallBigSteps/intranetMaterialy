<!--
opis: Notka zadania kodu ZAD-221: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-096-porzucone-uruchomienia?expand=1
rodzaj: wynik
zadanie: ZAD-221
rozmowa: ZAD-096 P1: Kolejka — zabicie obsługi kolejki (brak pamięci, restart · Programista
zmiana: 29.10.2026, ZAD-096 P1: Kolejka — zabicie obsługi kolejki (brak pamięci, restart · Programista
status: do akceptacji
-->
# ZAD-221: Porzucone uruchomienia nie blokują kolejki (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-096-porzucone-uruchomienia?expand=1
- Co zmieniono: Porzucone uruchomienia kolejki (worker zabity: brak pamięci, restart) nie blokują już zadań ani zegara: is_running() liczy tylko przebiegi z żywym workerem (puls do 60 s), jobs.tick na starcie zamyka porzucone jako FAILED/WorkerLost. Reguła BR-JOB-017, 10 testów z prawdziwym SIGKILL. Gałąź zad-096-porzucone-uruchomienia, commit 44fce09; założenia core-jobs 2.6 na main (cdc99b3). PR do otwarcia z linku (brak gh na serwerze).
- Jak sprawdzić: Otworzyć PR z linku, CI zielone; lokalnie: pełny zestaw 1059 passed, moduł jobs 120 passed, test_job_worker_lost 10 passed x3; ruff, mypy, bramka reguł OK. Przy scalaniu z ZAD-094: konflikt w runs.py i rules.contract.md (opis w raporcie zespołu).
