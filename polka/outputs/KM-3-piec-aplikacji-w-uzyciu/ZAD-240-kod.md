<!--
opis: Notka zadania kodu ZAD-240: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-240-usun-liste-martwych
rodzaj: wynik
zadanie: ZAD-240
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-240 Lista martwych zadań: błąd 500
zmiana: 30.10.2026, ZAD-240 Lista martwych zadań: błąd 500
status: do akceptacji
-->
# ZAD-240: Lista martwych zadań: błąd 500 (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-240-usun-liste-martwych
- Co zmieniono: Usunięta trasa GET /api/v1/jobs/dead-letter/ (dawała 500 każdemu, nic z niej nie korzystało); testy, opis API i typy web na nowo; bramka schema_bodies nie liczy już znikającej trasy jako spadku. Kontrakt core-jobs 2.10 na main (309a855).
- Jak sprawdzić: Otworzyć PR z linku compare; make test/lint/typecheck; po wdrożeniu na dev /api/v1/jobs/dead-letter/ = 404, a ekran Zdrowie kolejki dalej pokazuje martwe zadania i wznowienie.
