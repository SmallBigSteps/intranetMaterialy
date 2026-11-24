<!--
opis: Notka zadania kodu ZAD-330: https://github.com/SmallBigSteps/intranet/pull/new/zad-330-strona-glowna-bez-cache
rodzaj: wynik
zadanie: ZAD-330
kamien: KM-7 UX i UY
rozmowa: ZAD-330 Strona główna bez pamięci podręcznej
zmiana: 23.11.2026, ZAD-330 Strona główna bez pamięci podręcznej
status: do akceptacji
-->
# ZAD-330: Strona główna bez pamięci podręcznej (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-330-strona-glowna-bez-cache
- Co zmieniono: Brama (docker/nginx/default.conf, porty 80 i 443): index.html i trasy SPA z Cache-Control: no-cache, /assets/ z max-age=31536000, immutable, brakujący plik z /assets/ to 404 z no-store zamiast index.html. Nowa ścieżka e2e 09-cache pilnuje wszystkich trzech.
- Jak sprawdzić: Po wystawieniu na dev: curl -sI https://dev.febo.redai.pl/zadania pokazuje Cache-Control: no-cache; curl -sI https://dev.febo.redai.pl/assets/nie-ma.js zwraca 404. W CI zielona ścieżka 09-cache.
