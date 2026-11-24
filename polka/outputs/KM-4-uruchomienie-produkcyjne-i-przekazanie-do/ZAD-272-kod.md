<!--
opis: Notka zadania kodu ZAD-272: https://github.com/SmallBigSteps/intranet/compare/main...zad-272-sprzatanie-osieroconej-tresci
rodzaj: wynik
zadanie: ZAD-272
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-272 Sprzątanie osieroconej treści plików
zmiana: 13.11.2026, ZAD-272 Sprzątanie osieroconej treści plików
status: do akceptacji
-->
# ZAD-272: Sprzątanie osieroconej treści plików (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/compare/main...zad-272-sprzatanie-osieroconej-tresci
- Co zmieniono: Nocne zadanie files.remove_orphans (04:50, niskie, nie ciężkie) usuwa trwale treść plików bez wpisu w rejestrze i bez wersji, starszą niż doba, tylko w układzie path_for magazynu każdej klasyfikacji; jeden wpis audytu file_orphans_removed na przebieg. store i new_version bez zmian. Kontrakt core-files 3.9 już na main (0bf2c48), kod na gałęzi zad-272-sprzatanie-osieroconej-tresci (78e550d) — PR do otwarcia z linku porównania i scalenia przez Pawła Mazka.
- Jak sprawdzić: make test albo pytest tests/rules/test_fil.py -k 'orphan or nothing_to_remove'; po scaleniu i wdrożeniu na dev: panel zdrowia kolejki pokazuje 'Sprzątanie osieroconej treści plików' o 04:50, a po przebiegu, który coś usunął, w dzienniku audytu jest wpis file_orphans_removed z liczbą i rozmiarem.
