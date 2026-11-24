<!--
opis: Notka zadania kodu ZAD-264: https://github.com/SmallBigSteps/intranet/compare/main...zad-264-sprzatanie-testow-transakcyjnych
rodzaj: wynik
zadanie: ZAD-264
rozmowa: ZAD-264 Sprzątanie po testach transakcyjnych
zmiana: 13.11.2026, ZAD-264 Sprzątanie po testach transakcyjnych
status: do akceptacji
-->
# ZAD-264: Sprzątanie po testach transakcyjnych (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/compare/main...zad-264-sprzatanie-testow-transakcyjnych
- Co zmieniono: Po każdym teście transakcyjnym baza wraca do stanu po migracjach i zasiewie: wtyczka api/tests/db_cleanup.py (cała sesja, także testy aplikacji i wdrożenia) czyści tabele schematów core_* i app_* oraz procrastinate_*, a tabele z listy wyjątków SEEDED_TABLES przywraca do stanu sprzed testu. Strażnik api/tests/test_db_cleanup.py pada, gdy po zasiewie wiersze ma tabela spoza listy albo lista wymienia nieistniejącą tabelę. Przyczyna: flush Django pomija tabele nazwane ze schematem. Kontrakt testing 1.9 §7 na main (5391e75, 4d67924). Gałąź zad-264-sprzatanie-testow-transakcyjnych (d650a0c); PR otwiera i scala Paweł Mazek.
- Jak sprawdzić: Pełny zestaw (powłoka, wdrożenie, trzy aplikacje): 2045 zielonych, 3 pominięte; 6 błędów to tylko testy LogTMS wdrożenia bez sterownika MySQLdb w lokalnym środowisku (CI je uruchomi), a test_logtms_rules.py pominięty z tego samego powodu. ruff i mypy czyste. Sprawdzenie: pytest tests/test_db_cleanup.py tests/rules/test_job.py tests/rules/test_job_worker_lost.py tests/test_jobs_outcome.py — liczba wierszy job_run i procrastinate_workers między testami się nie zmienia.
