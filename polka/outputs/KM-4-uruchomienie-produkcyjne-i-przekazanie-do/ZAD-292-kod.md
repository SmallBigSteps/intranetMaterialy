<!--
opis: Notka zadania kodu ZAD-292: https://github.com/feboLogisticGit/febo-intranet-wdrozenie/pull/new/zad-292-nowy-adres
rodzaj: wynik
zadanie: ZAD-292
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-292 Nowy adres repozytorium
zmiana: 13.11.2026, ZAD-292 Nowy adres repozytorium
status: do akceptacji
-->
# ZAD-292: Nowy adres repozytorium (zmiana w kodzie)

- PR: https://github.com/feboLogisticGit/febo-intranet-wdrozenie/pull/new/zad-292-nowy-adres
- Co zmieniono: Nowy adres SmallBigSteps/intranet. Repozytorium powłoki: commit 724e7e4 na main (00-INDEKS, START, wdrozenie-test-prod). Repozytorium wdrożenia FEBO: gałąź zad-292-nowy-adres, commit 6475ba7 (README, RULES, kontrakt łącznika LOG TMS, opis zestawu z Optimy) do scalenia przez PR. Kontekst projektu, _WIKI.md i dev/Repozytoria-i-srodowisko.md na półce oraz zdalne adresy 45 kopii na serwerze wskazują nowy adres. Instrukcja projektu czeka w skrzynce zmian PM (zmiana 108). Bez zmian zostają zapisy historyczne: podsumowania sprintów S1–S4, stare notki zadań kodu (GitHub przekierowuje stare linki) i kopia core-intake.openapi.yaml (nie edytować tutaj).
- Jak sprawdzić: W repo powłoki: git grep SmallBigSteps/intranetFebo origin/main zwraca tylko podsumowania sprintów S1–S4. W repo wdrożenia po scaleniu PR: brak trafień. Na serwerze: git remote get-url origin w kopiach daje git@github.com:SmallBigSteps/intranet.git.
