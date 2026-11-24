<!--
opis: Notka zadania kodu ZAD-092: https://github.com/SmallBigSteps/intranetFebo/pull/206
rodzaj: wynik
zadanie: ZAD-092
etap: ETP-1 Testy
rozmowa: ZAD-092 P1: Standardy — prośba do asystenta (tam, gdzie asystent jes
zmiana: 29.10.2026, ZAD-092 P1: Standardy — prośba do asystenta (tam, gdzie asystent jes
status: do akceptacji
-->
# ZAD-092: P1: Standardy — prośba do asystenta (tam, gdzie asystent jest włączony) kończy się błędem serwera i trwale zostawia szkic w stanie „asystent pracuje” (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/206
- Co zmieniono: Asystent w Standardach nie zawisa w stanie „pracuje”: znacznik wygasa po 5 minutach, a błąd zadania zdejmuje go od razu z komunikatem „Asystent nie odpowiedział. Twój tekst jest bez zmian, możesz poprosić jeszcze raz.”; przycisk prośby znów działa. Scalone w main (PR #206).
- Jak sprawdzić: Na dev: szkic ze znacznikiem starszym niż 5 minut, po odświeżeniu edytora komunikat i aktywny przycisk; ponowna prośba kończy się sukcesem. Testy: backend Standardów 80/80, web 463.
