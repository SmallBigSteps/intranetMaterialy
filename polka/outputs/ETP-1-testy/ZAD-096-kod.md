<!--
opis: Notka zadania kodu ZAD-096: https://github.com/SmallBigSteps/intranetFebo/pull/218
rodzaj: wynik
zadanie: ZAD-096
kamien: KM-5 Środowiska test i prod gotowe
rozmowa: ZAD-096 P1: Kolejka — zabicie obsługi kolejki (brak pamięci, restart
zmiana: 30.10.2026, ZAD-096 P1: Kolejka — zabicie obsługi kolejki (brak pamięci, restart
status: do akceptacji
-->
# ZAD-096: P1: Kolejka — zabicie obsługi kolejki (brak pamięci, restart przy wdrożeniu) zostawia otwarty wpis uruchomienia: to zadanie okresowe, a przy zegarze harmonogramu cały harmonogram, jest odtąd po cichu pomijane (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/218
- Co zmieniono: Porzucone uruchomienia nie blokują kolejki: zegar harmonogramu zamyka wpis, którego proces roboczy zniknął (brak pulsu >60 s), jako failed/WorkerLost; próg wspólny z stalled_worker_timeout Procrastinate; spóźnione zakończenie zapisuje prawdziwy wynik. PR #217 (pierwsza część) i #218 scalone; kontrakt core-jobs 2.6/2.8 na main, reguła BR-JOB-017.
- Jak sprawdzić: Testy tests/rules/test_job_worker_lost.py (19, w tym prawdziwy SIGKILL workera) i test_jobs*; na dev: zabić proces workera w trakcie zadania okresowego — po ok. 1 min zegar zamyka wpis jako WorkerLost i zadanie rusza w kolejnym cyklu.
