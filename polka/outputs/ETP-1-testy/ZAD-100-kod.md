<!--
opis: Notka zadania kodu ZAD-100: https://github.com/SmallBigSteps/intranetFebo/pull/216
rodzaj: wynik
zadanie: ZAD-100
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-100 P1: Panel kolejki zadań w tle — opiekun aplikacji (rola *.ma
zmiana: 30.10.2026, ZAD-100 P1: Panel kolejki zadań w tle — opiekun aplikacji (rola *.ma
status: do akceptacji
-->
# ZAD-100: P1: Panel kolejki zadań w tle — opiekun aplikacji (rola *.maintainer) rozpoznawany po samym posiadaniu roli, bez sprawdzenia drugiego składnika (TOTP, BR-ACC-013): core/jobs/views.py:58 i core/jobs/health.py:60-64 biorą effective_roles zamiast ról możliwych do użycia (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/216
- Co zmieniono: Panel kolejki zadań w tle rozpoznaje opiekuna aplikacji (rola *.maintainer) tylko po rolach możliwych do użycia (BR-ACC-013): bez potwierdzonego drugiego składnika opiekun nie widzi i nie uruchamia zadań. Poprawione 4 miejsca (widoki, stan kolejki, wywołania, martwe zadania) przez wspólną funkcję; 4 nowe testy reguł.
- Jak sprawdzić: Testy: api/tests/rules/test_acc_totp.py (4 nowe) i pełny zestaw 1053 zielone, ruff i mypy czyste. Ręcznie: opiekun bez urządzenia TOTP dostaje 403 na 11 trasach /api/v1/jobs (wcześniej ręczne uruchomienie dawało 202); po potwierdzeniu urządzenia zakres jak dotąd; osoba tylko process_owner 403. Raport testera: zespol/wyniki/2-tester.md.
