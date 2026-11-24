<!--
opis: Notka zadania kodu ZAD-251: https://github.com/SmallBigSteps/intranetFebo/pull/226
rodzaj: wynik
zadanie: ZAD-251
kamien: KM-5 Środowiska test i prod gotowe
rozmowa: ZAD-096 P1: Kolejka — zabicie obsługi kolejki (brak pamięci, restart
zmiana: 02.11.2026, ZAD-096 P1: Kolejka — zabicie obsługi kolejki (brak pamięci, restart
status: do akceptacji
-->
# ZAD-251: Kolejka: wyścig zamknięcia WorkerLost (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/226
- Co zmieniono: Zegar kolejki nie nadpisuje prawdziwego zakończenia zadania domysłem WorkerLost (BR-JOB-017, N1–N3), testy pilnują poprzedniego typu błędu i sprzątają po sobie (U1, U3); kontrakt core-jobs 2.12
- Jak sprawdzić: ./pytest-zad251.sh tests/rules/test_job_worker_lost.py: 28 passed; pełny zestaw 642 passed, 1 skipped
