<!--
opis: Notka zadania kodu ZAD-217: https://github.com/SmallBigSteps/intranetFebo/commit/65c6f1b7263996919bfc123d878547aa8fd49d37
rodzaj: wynik
zadanie: ZAD-217
rozmowa: ZAD-217 Własna kolejność aplikacji w panelu
zmiana: 12.11.2026, ZAD-217 Własna kolejność aplikacji w panelu
status: do akceptacji
-->
# ZAD-217: Własna kolejność aplikacji w panelu (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/commit/65c6f1b7263996919bfc123d878547aa8fd49d37
- Co zmieniono: Własna kolejność aplikacji w panelu bocznym: tryb „Ustaw kolejność” (przeciąganie i strzałki z klawiatury, Gotowe/Anuluj/Przywróć domyślną), kolejność, zwinięcie grupy i wąski pasek zapamiętane przy koncie (/cockpit/rail/), nowe aplikacje na końcu ze znaczkiem „nowa”. Gałąź zad-217-kolejnosc-aplikacji, commity dffc8ee (serwer) i 65c6f1b (ekran); PR do założenia: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-217-kolejnosc-aplikacji
- Jak sprawdzić: Panel boczny › Aplikacje › Ustaw kolejność: przestaw aplikacje myszą albo strzałkami, Gotowe, odśwież stronę i zaloguj się w innej przeglądarce — kolejność zostaje. Przywróć domyślną wraca do kolejności katalogu. Zwinięcie grupy i wąski pasek też zostają po odświeżeniu. Testy: test_coc_rail.py, test_cockpit_rail_api.py, web/src/shell/side-nav.test.tsx (27).
