<!--
opis: Notka zadania kodu ZAD-083: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-083-zadania
rodzaj: wynik
zadanie: ZAD-083
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-083 P1: Frontend — typy odpowiedzi serwera pisane ręcznie, a opi
zmiana: 30.10.2026, ZAD-083 P1: Frontend — typy odpowiedzi serwera pisane ręcznie, a opi
status: do akceptacji
-->
# ZAD-083: P1: Frontend — typy odpowiedzi serwera pisane ręcznie, a opis API nie zawiera kształtu odpowiedzi: zmiana nazwy pola na serwerze przechodzi wszystkie bramki i psuje ekran dopiero u użytkownika (tak było 17.09 w Strukturze) (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-083-zadania
- Co zmieniono: Moduły Zadania i Audyt biorą typy odpowiedzi z opisu API: serwer opisuje kształt odpowiedzi, front używa typów wygenerowanych z opisu, reguła ESLint i bramka pilnują, żeby ręczne typy nie wróciły, a nowa bramka nie pozwala obniżyć udziału opisanych odpowiedzi (35,0%). Audyt dopuszcza pusty wpis o osobie. CI uruchamia testy bramek. Struktura i reszta: ZAD-236, ZAD-237.
- Jak sprawdzić: make check i make schema-check zielone. Próba z 17.09: zmień nazwę pola w odpowiedzi Zadań (np. licznik overdue) — bez make schema czerwony dryf opisu, po make schema czerwony web typecheck wskazujący ekran. Zamiana opisu odpowiedzi na OBJECT — czerwona bramka schema_bodies. Tester sprawdził lokalnie ekrany listy, szczegółów i Audytu (raport w czacie ZAD-083).
