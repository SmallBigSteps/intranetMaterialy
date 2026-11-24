<!--
opis: Notka zadania kodu ZAD-090: https://github.com/SmallBigSteps/intranetFebo/pull/204
rodzaj: wynik
zadanie: ZAD-090
etap: ETP-1 Testy
rozmowa: ZAD-090 P1: Standardy — nowy pracownik po aktywacji konta nie dostaj
zmiana: 28.10.2026, ZAD-090 P1: Standardy — nowy pracownik po aktywacji konta nie dostaj
status: do akceptacji
-->
# ZAD-090: P1: Standardy — nowy pracownik po aktywacji konta nie dostaje zadań zapoznania z procedurami swojego działu (BR-STD-028), a publikacja do działu pomija konta jeszcze nieaktywowane: ewidencja „kto zna procedury” ma lukę przy nowych osobach (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/204
- Co zmieniono: Lokator Standardy: zaległe zapoznania BR-STD-028 wydawane także przy przejściu konta na ACTIVE (aktywacja linkiem, przez administratora, powrót z zawieszenia lub wyłączenia), bez powtórnego wydania; 8 testów ścieżką import → aktywacja, RULES.md doprecyzowany; testing.contract 1.8 na main (9b8c285). Imienne wskazania kont INVITED bez zmian (decyzja P2).
- Jak sprawdzić: CI na PR #204 zielone (mypy i baza wyjątków sekretów poprawione w 02b3b57); lokalnie: standards 80 i powłoka 1027 testów zielone; scenariusz: konto z importu HR jako INVITED w dziale celnym, po aktywacji linkiem ma zadania zapoznania z obowiązującymi procedurami działu, ponowna aktywacja nie dubluje.
