<!--
opis: Notka zadania kodu ZAD-249: https://github.com/SmallBigSteps/intranet/commit/b08b6d7a4c0a977af3910f2bbe57ee5ecd358dc5
rodzaj: wynik
zadanie: ZAD-249
rozmowa: ZAD-249 Standardy: wniosek z punktu zaczepienia
zmiana: 12.11.2026, ZAD-249 Standardy: wniosek z punktu zaczepienia
status: do akceptacji
-->
# ZAD-249: Standardy: wniosek z punktu zaczepienia (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/commit/b08b6d7a4c0a977af3910f2bbe57ee5ecd358dc5
- Co zmieniono: Wniosek o procedurę z punktu zaczepienia (T-57g): przy każdym zadaniu okienko Standardów z linkiem do procedury albo przyciskami „Poproś o procedurę” i „Napisz ją”; drugi wniosek z tego samego rodzaju zadań dopisuje się do zadania opiekuna; BR-STD-023 w manifeście z testem. Gałąź zad-249-punkt-zaczepienia, PR do założenia: https://github.com/SmallBigSteps/intranet/pull/new/zad-249-punkt-zaczepienia
- Jak sprawdzić: Otworzyć dowolne zadanie innej aplikacji: bez procedury widać dwa przyciski; dwie osoby proszą — opiekun ma jedno zadanie z dwoma wnioskodawcami; procedura napisana z „Napisz ją” po publikacji pokazuje się jako link przy tych zadaniach. Testy: test_br_std_023_a_second_request_joins_the_open_task, task-place-panel.test.tsx, task-detail.test.tsx.
