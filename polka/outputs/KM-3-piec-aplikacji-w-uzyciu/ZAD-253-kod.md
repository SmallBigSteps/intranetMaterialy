<!--
opis: Notka zadania kodu ZAD-253: https://github.com/SmallBigSteps/intranetFebo/pull/227
rodzaj: wynik
zadanie: ZAD-253
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-253 Ponawianie e2e tylko budowania
zmiana: 02.11.2026, ZAD-253 Ponawianie e2e tylko budowania
status: do akceptacji
-->
# ZAD-253: Ponawianie e2e tylko budowania (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/227
- Co zmieniono: CI e2e: pobieranie i budowanie obrazów z ponawianiem (ponow.sh), start środowiska i ścieżki e2e raz; cel e2e w Makefile w tej samej kolejności; wpis open-gaps 17.09.2026 domknięty wykonaniem.
- Jak sprawdzić: Przebieg CI na PR #227 i na main: kroki Environment i Paths w zadaniu e2e — ponow.sh tylko przy pull/build, up --wait i run e2e bez ponawiania.
