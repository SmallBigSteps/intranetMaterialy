<!--
opis: Notka zadania kodu ZAD-260: https://github.com/SmallBigSteps/intranet/pull/246
rodzaj: wynik
zadanie: ZAD-260
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-260 Schemat struktury: wiele korzeni
zmiana: 12.11.2026, ZAD-260 Schemat struktury: wiele korzeni
status: do akceptacji
-->
# ZAD-260: Schemat struktury: wiele korzeni (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/246
- Co zmieniono: Schemat w Strukturze i kontaktach przy kilku osobach bez przełożonego: kafelek „Cała firma”, pod nim wszystkie korzenie w rzędzie od największego zespołu, ścieżka od „Cała firma”; przy jednym korzeniu bez zmian. Tylko front lokatora directory; BR-DIR-002 rozszerzone (RULES 1.2), testy (a)–(e).
- Jak sprawdzić: Struktura i kontakty › Schemat przy kilku osobach bez przełożonego: na górze „Cała firma” i wszystkie te osoby; każda osoba osiągalna, zgodnie z Drzewem. Przy jednym korzeniu wygląd jak dawniej. CI na main+ZAD-260: rules_gate OK, vitest 559/559, tsc, eslint, prettier.
