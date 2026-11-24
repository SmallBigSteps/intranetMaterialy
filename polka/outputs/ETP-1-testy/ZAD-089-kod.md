<!--
opis: Notka zadania kodu ZAD-089: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-089-asystent-wyscig?expand=1
rodzaj: wynik
zadanie: ZAD-089
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-089 P1: Standardy — spóźniona odpowiedź asystenta nadpisuje teks
zmiana: 30.10.2026, ZAD-089 P1: Standardy — spóźniona odpowiedź asystenta nadpisuje teks
status: do akceptacji
-->
# ZAD-089: P1: Standardy — spóźniona odpowiedź asystenta nadpisuje tekst procedury: kasuje ręczne poprawki autora, a nawet zmienia wersję już zgłoszoną lub zatwierdzoną bez ponownej akceptacji (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-089-asystent-wyscig?expand=1
- Co zmieniono: Wariant 1: spóźniona odpowiedź asystenta nie wchodzi, gdy wersja nie jest szkicem albo autor zapisał tekst w trakcie; konflikt zapisu autora (versionToken, 428/409, przycisk Wczytaj aktualną wersję); komunikat dla autora; testy współbieżności i styku z wygasaniem flagi z ZAD-092. Backend i frontend wchodzą razem. Opis PR z 8 otwartymi kwestiami w notatce zadania.
- Jak sprawdzić: Testy: Standardy 119, API 1049, front 495 zielone. Ręcznie na dev: poproś asystenta, w trakcie zapisz tekst albo zgłoś wersję — tekst autora zostaje, pojawia się komunikat; dwie karty z tą samą wersją — druga dostaje konflikt i przycisk Wczytaj aktualną wersję.
