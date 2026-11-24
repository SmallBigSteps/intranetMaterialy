<!--
opis: Notka zadania kodu ZAD-231: https://github.com/SmallBigSteps/intranetFebo/commit/7436bc6130d04128854499ce2b3d3a15004025c3
rodzaj: wynik
zadanie: ZAD-231
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-231 Archiwum audytu partiami
zmiana: 02.11.2026, ZAD-231 Archiwum audytu partiami
status: do akceptacji
-->
# ZAD-231: Archiwum audytu partiami (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/commit/7436bc6130d04128854499ce2b3d3a15004025c3
- Co zmieniono: Gałąź zad-231-archiwum-audytu-partiami (PR do otwarcia: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-231-archiwum-audytu-partiami). audit.archive miesiąc po miesiącu i kategoria po kategorii, najwyżej 1000 wpisów naraz; ponowienie po awarii nie dopisuje duplikatów (wpisy już w pliku są pomijane, ucięta linia też); miesiąc pliku i dzień należności w czasie polskim, retention_until z polskiej daty dla nowych wpisów; purge_security 12 miesięcy kalendarzowych, partiami; kolejka zapisuje wynik każdego zadania w JobRun.context[outcome]; godziny 01:30 / 01:50 / check_expiry 01:15 (poza 02:00–03:00). Kontrakty core-audit 3.7, core-jobs 2.13, core-integrations 2.5, wdrozenie-test-prod krok 8a, progress.md.
- Jak sprawdzić: Pełne testy api: 1232 zaliczone, 1 pominięty; ruff, ruff format i mypy czyste na zmienionych plikach. Nowe testy: tests/test_audit_archive_batches.py (partie, ponowienie po awarii, polski miesiąc, 12 miesięcy kalendarzowych, brak zadań o 2:xx) i tests/test_jobs_outcome.py (wynik zadania w JobRun.context, także przez prawdziwego workera). Po wdrożeniu na dev: w panelu kolejki godziny zadań 01:15/01:30/01:50, a w przebiegu audit.archive pole outcome z liczbami.
