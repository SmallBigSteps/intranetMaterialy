<!--
opis: Notka zadania kodu ZAD-238: https://github.com/SmallBigSteps/intranetFebo/pull/228
rodzaj: wynik
zadanie: ZAD-238
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-238 Uszczelnienie bramek typów odpowiedzi
zmiana: 02.11.2026, ZAD-238 Uszczelnienie bramek typów odpowiedzi
status: do akceptacji
-->
# ZAD-238: Uszczelnienie bramek typów odpowiedzi (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/228
- Co zmieniono: Bramki typów odpowiedzi uszczelnione: nowe ciało JSON bez opisu czerwone zawsze (przyjęcie po nazwie z powodem), jedna reguła literałów obiektu dla całego modułu łącznie z types.ts, nowa bramka zmian łamiących (lista 770 pól w 72 ciałach), ratchet obu list wobec main; kontrakty hooków 2.3, narzędzi 1.6, listy recenzenta 1.13, decyzja bramki-typow-odpowiedzi-zad238.
- Jak sprawdzić: pytest tools/tests (próby Testera R1, R2, R4 czerwone na starym kodzie, zielone na nowym); make schema-check; w CI kroki Described bodies, Fields of described bodies, Body and field lists against main.
