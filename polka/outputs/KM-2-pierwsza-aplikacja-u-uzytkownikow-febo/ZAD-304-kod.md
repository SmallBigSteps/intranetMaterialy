<!--
opis: Notka zadania kodu ZAD-304: https://github.com/SmallBigSteps/intranet/compare/main...zad-304-asystent-poprawka-niewidoczna
rodzaj: wynik
zadanie: ZAD-304
kamien: KM-2 Pierwsza aplikacja u użytkowników FEBO
rozmowa: ZAD-304 Asystent: poprawka niewidoczna
zmiana: 16.11.2026, ZAD-304 Asystent: poprawka niewidoczna
status: do akceptacji
-->
# ZAD-304: Asystent: poprawka niewidoczna (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/compare/main...zad-304-asystent-poprawka-niewidoczna
- Co zmieniono: Asystent nie ogłasza już poprawki, której nie ma w tekście. Usługa asystenta (portal) poprawiona i wdrożona 16.11: procedura z sekcjami bez 'Krok N' nie wraca już do oryginału. Intranet: odpowiedź z tekstem bez zmian albo pustym = nieudana próba z komunikatem 'Asystent opisał zmiany, ale nie wstawił ich do tekstu…', wpis w rejestrze wywołań, prośba wraca do pola 'Co poprawić', uwagi usługi o formie pod opisem zmian. BR-STD-039, RULES.md 3.15, testy regresji pytest i vitest. Gałąź zad-304-asystent-poprawka-niewidoczna, commit 4891a1f; PR do otwarcia z linku porównania.
- Jak sprawdzić: Po scaleniu i wdrożeniu na dev: szkic procedury z sekcjami (Działanie, Dlaczego…), prośba 'Dodaj kroki o grafiku i zastępstwie' — kroki pojawiają się w tekście i zielone 'Asystent zmienił w tekście'. Gdyby usługa znów odesłała ten sam tekst: żółte 'Asystent nie pomógł tym razem' z komunikatem, a prośba zostaje w polu 'Co poprawić'.
