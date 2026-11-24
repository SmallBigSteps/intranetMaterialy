<!--
opis: Notka zadania kodu ZAD-295: https://github.com/SmallBigSteps/intranet/commit/2b7b174feee70fcab0944eb0ce5565c4dbfb1e27
rodzaj: wynik
zadanie: ZAD-295
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-295 Martwa kolejka ucina listę
zmiana: 13.11.2026, ZAD-295 Martwa kolejka ucina listę
status: do akceptacji
-->
# ZAD-295: Martwa kolejka ucina listę (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/commit/2b7b174feee70fcab0944eb0ce5565c4dbfb1e27
- Co zmieniono: Panel Stan techniczny › Zadania, które nie przeszły: licznik „Pokazano X z Y” przy uciętej liście i filtr „Zadanie” z liczbą wpisów przy każdym zadaniu (opiekun widzi tylko swoje). Limit 100 zostaje, bez stronicowania i bez wznawiania grupowego. Kod na gałęzi zad-295-martwa-kolejka-filtr; PR do otwarcia jednym kliknięciem: https://github.com/SmallBigSteps/intranet/pull/new/zad-295-martwa-kolejka-filtr . Kontrakt core-jobs 2.15 już na main (4c2cb0a).
- Jak sprawdzić: Na dev po scaleniu: Stan techniczny › Zadania, które nie przeszły — nad listą „Pokazano 100 z 283”, w filtrze „Archiwizacja audytu (2)”; po wyborze widać oba wpisy, każdy z osobnym przyciskiem Wznów. Testy: vitest src/jobs 24/24, pytest test_jobs* zielone, ruff i mypy czyste; schema-check potwierdzi CI.
