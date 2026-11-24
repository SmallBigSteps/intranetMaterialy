<!--
opis: Notka zadania kodu ZAD-321: https://github.com/SmallBigSteps/intranet/pull/new/zad-321-rama-panel-belka-naglowek
rodzaj: wynik
zadanie: ZAD-321
kamien: KM-7 UX i UY
rozmowa: ZAD-321 Rama: panel, belka, nagłówek
zmiana: 20.11.2026, ZAD-321 Rama: panel, belka, nagłówek
status: do akceptacji
-->
# ZAD-321: Rama: panel, belka, nagłówek (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-321-rama-panel-belka-naglowek
- Co zmieniono: Rama powłoki według makiety 07 (razem z ZAD-110): panel na całą wysokość z sekcjami, licznikami, „wkrótce” i stopką z nazwą firmy z wdrożenia; jasna belka z wyszukiwarką, kaflami Moje zadania / Po terminie / Na dziś, dzwonkiem z listą rozwijaną (nowe API powiadomień, BR-NOT) i osobą ze stanowiskiem; nagłówek z okruszkami, tytułem i zdaniem o celu na każdym ekranie. Kontrakty core-ui 2.39, core-notify 2.9, core-cockpit 3.20, rules 3.39. Osobny PR wdrożenia FEBO: https://github.com/feboLogisticGit/febo-intranet-wdrozenie/pull/new/zad-321-nazwa-firmy
- Jak sprawdzić: Po scaleniu obu PR na dev: pulpit przy 1440, 1024, 768 px i telefonie w trybie jasnym i ciemnym — bez przewijania w bok; dzwonek rozwija listę, „Oznacz wszystkie jako przeczytane” zeruje liczbę, „Wszystkie” na pulpicie otwiera dzwonek; kafle prowadzą do listy z filtrem; w stopce panelu FEBO LOGISTIC Sp. z o.o. i febologistic.com; CI zielone.
