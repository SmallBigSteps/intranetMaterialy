<!--
opis: Notka zadania kodu ZAD-261: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-261-umiejscowienie
rodzaj: wynik
zadanie: ZAD-261
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-261 Umiejscowienie: uprawnienie przed danymi
zmiana: 12.11.2026, ZAD-261 Umiejscowienie: uprawnienie przed danymi
status: do akceptacji
-->
# ZAD-261: Umiejscowienie: uprawnienie przed danymi (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-261-umiejscowienie
- Co zmieniono: Struktura i kontakty: zmiana umiejscowienia najpierw sprawdza rolę kadr (directory.admin), dopiero potem szuka stanowiska, działu i przełożonego. Pracownik bez roli dostaje 403 bez względu na to, czy klucz istnieje; kadry z nieznanym kluczem nadal 400. Nowa funkcja ensure_can_place w api.py, test BR-DIR-008 dla trzech pól. Commit faaf289.
- Jak sprawdzić: Pracownik bez roli kadr wysyła PATCH /api/v1/app/directory/people/<id>/placement/ z nieistniejącym stanowiskiem: ma być 403, nie 400. Testy lokatora: 37 zielonych; ruff, mypy i schemat API bez zmian. Błędy w testach archiwum audytu wynikają z braku programu age w lokalnym środowisku, nie z tej zmiany.
