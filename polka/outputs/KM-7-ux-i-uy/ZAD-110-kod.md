<!--
opis: Notka zadania kodu ZAD-110: https://github.com/SmallBigSteps/intranet/pull/new/zad-321-rama-panel-belka-naglowek
rodzaj: wynik
zadanie: ZAD-110
kamien: KM-7 UX i UY
rozmowa: ZAD-110 Format aplikacji — wspólna rama: przejście na układ bazowy k
zmiana: 23.11.2026, ZAD-110 Format aplikacji — wspólna rama: przejście na układ bazowy k
status: do akceptacji
-->
# ZAD-110: Format aplikacji — wspólna rama: przejście na układ bazowy komputer; tablet i telefon zachowane jako formaty dodatkowe. Wynika z ZAD-010 projektu planer (06.10.2026, Paweł Mazek): niezmiennik 35 zmieniony — format aplikacji wybiera się przy tworzeniu lokatora. (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-321-rama-panel-belka-naglowek
- Co zmieniono: Rama powłoki (panel boczny, belka górna, nagłówek ekranu) w układzie bazowym komputer, tablet i telefon jako formaty dodatkowe; core-ui 2.39 §3 (kolejność ustępowania belki) i §7 (rama w rejestrze formatów). Wspólna gałąź z ZAD-321.
- Jak sprawdzić: Otworzyć PR z gałęzi zad-321-rama-panel-belka-naglowek; na dev sprawdzić ramę przy 1440, 1024 i 375 px w trybie jasnym i ciemnym: brak przewijania w bok, belka ustępuje w kolejności z core-ui §3.
