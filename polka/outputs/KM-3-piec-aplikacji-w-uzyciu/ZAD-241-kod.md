<!--
opis: Notka zadania kodu ZAD-241: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-241-asystent-decyzje
rodzaj: wynik
zadanie: ZAD-241
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-089 P1: Standardy — spóźniona odpowiedź asystenta nadpisuje teks
zmiana: 02.11.2026, ZAD-089 P1: Standardy — spóźniona odpowiedź asystenta nadpisuje teks
status: do akceptacji
-->
# ZAD-241: Standardy: asystent po ZAD-089 (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-241-asystent-decyzje
- Co zmieniono: Asystent w Standardach według ośmiu decyzji po ZAD-089: blokada Zgłoś i edycji, gdy asystent pracuje, przycisk zatrzymania asystenta, prośba niesie tekst i wszystkie pola edytora (z walidacją jak szkic), poprawione komunikaty; reguły BR-STD-036, testy i 11 mutacji Testera. Gałąź scalona z aktualnym main.
- Jak sprawdzić: Standardy 155, api 1182 i web 523 testów zielone po scaleniu z main; lint, tsc i format bez uwag. Na dev: poprosić asystenta, w trakcie spróbować edytować i zgłosić (zablokowane), zatrzymać asystenta, zmienić tytuł i opis przed prośbą i sprawdzić, że zostały.
