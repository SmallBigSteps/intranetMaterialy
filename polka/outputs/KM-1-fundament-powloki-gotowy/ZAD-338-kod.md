<!--
opis: Notka zadania kodu ZAD-338: https://github.com/SmallBigSteps/intranet/compare/main...zad-338-testy-powloki-bez-wdrozenia?expand=1
rodzaj: wynik
zadanie: ZAD-338
kamien: KM-1 Fundament powłoki gotowy
rozmowa: ZAD-338 Testy powłoki bez wdrożenia
zmiana: 24.11.2026, ZAD-338 Testy powłoki bez wdrożenia
status: do akceptacji
-->
# ZAD-338: Testy powłoki bez wdrożenia (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/compare/main...zad-338-testy-powloki-bez-wdrozenia?expand=1
- Co zmieniono: Testy powłoki przechodzą bez wdrożenia: wyjątki N+1 tylko dla zainstalowanych aplikacji, test nieobecności przeniesiony do aplikacji Struktura i kontakty, test zasiewu pomijany bez lokatora, nowe zadanie CI „Backend without a deployment”, CLAUDE.md 6.6 (skreślenie z wyjątku niezmiennika 10) z decyzją i wpisem w indeksie.
- Jak sprawdzić: make test DEPLOYMENT_DIR= DEPLOYMENT= przechodzi (na lab 1625 passed, 1 skipped); w CI pod PR zielone Backend i Backend without a deployment; w testach powłoki brak importów apps.*
