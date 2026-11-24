<!--
opis: Notka zadania kodu ZAD-327: https://github.com/SmallBigSteps/intranet/compare/main...zad-327-struktura-trzy-widoki?expand=1
rodzaj: wynik
zadanie: ZAD-327
kamien: KM-7 UX i UY
rozmowa: ZAD-327 Struktura i kontakty: trzy widoki
zmiana: 23.11.2026, ZAD-327 Struktura i kontakty: trzy widoki
status: do akceptacji
-->
# ZAD-327: Struktura i kontakty: trzy widoki (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/compare/main...zad-327-struktura-trzy-widoki?expand=1
- Co zmieniono: Struktura i kontakty: Lista (domyślna, własny dział rozwinięty, To Ty), Schemat z linkiem działu na kafelku kierownika, Widok działu (kierownik, dział nadrzędny, liczba osób, komórka); karta osoby w kolumnie z boku (Zadzwoń/E-mail, Zmień swoje dane kontaktowe). Osoba z wyłączonego działu nie znika z listy. W tym samym PR ZAD-112 (tylko komputer) i ZAD-311 w tej aplikacji. Założenia na main 38fb6ed: decyzja trzy-widoki-zad327, core-ui 2.41, RULES directory 1.5. Gałąź zad-327-struktura-trzy-widoki (a3c9d66); PR do założenia z linku i scalenia.
- Jak sprawdzić: Na dev /app/directory: otwiera się Lista z rozwiniętym własnym działem i To Ty; klik osoby pokazuje kartę obok; Widok działu pokazuje kierownika i dział nadrzędny; w Schemacie nazwa działu na kafelku prowadzi do Widoku działu; tryb jasny i ciemny bez czerwonego tekstu. Lokalnie: tsc, eslint, brand-lint, prettier, vitest 180/180; testy Pythona w CI.
