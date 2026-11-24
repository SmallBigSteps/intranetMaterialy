<!--
opis: Notka zadania kodu ZAD-322: https://github.com/SmallBigSteps/intranet/pull/new/zad-322-pulpit
rodzaj: wynik
zadanie: ZAD-322
kamien: KM-7 UX i UY
rozmowa: ZAD-322 Pulpit jako ekran startowy
zmiana: 19.11.2026, ZAD-322 Pulpit jako ekran startowy
status: do akceptacji
-->
# ZAD-322: Pulpit jako ekran startowy (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-322-pulpit
- Co zmieniono: Pulpit jako ekran startowy (gałąź zad-322-pulpit, commit cc83c27): powitanie z datą, trzy kafle zadań z filtrami, pusty kafel wskaźników, zadania na dziś, aplikacje, powiadomienia i karta Mój zespół ze Struktury przez punkt rozszerzenia; filtr Na dziś na liście zadań; nowe komponenty w katalogu; kontrakty core-tasks 4.18, core-cockpit 3.19, core-ui 2.36, rules 3.37 (BR-TSK-026, BR-COC-022, BR-DIR-025).
- Jak sprawdzić: Zaloguj się na dev: zamiast listy zadań pojawia się pulpit. Kliknij kafel Po terminie: lista otwiera się z filtrem. Przełącz tryb ciemny i zwęż okno do telefonu. Testy: web 791/791, serwer 1791 zielonych, 31 czerwonych tylko z braku programu age (archiwum audytu, niezwiązane).
