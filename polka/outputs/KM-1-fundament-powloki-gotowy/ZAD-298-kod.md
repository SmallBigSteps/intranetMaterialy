<!--
opis: Notka zadania kodu ZAD-298: https://github.com/SmallBigSteps/intranet/pull/new/zad-298-lista-mniej-kolumn
rodzaj: wynik
zadanie: ZAD-298
kamien: KM-1 Fundament powłoki gotowy
rozmowa: ZAD-298 Lista zadań: mniej kolumn
zmiana: 15.11.2026, ZAD-298 Lista zadań: mniej kolumn
status: do akceptacji
-->
# ZAD-298: Lista zadań: mniej kolumn (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-298-lista-mniej-kolumn
- Co zmieniono: Lista zadań w powłoce ma sześć kolumn w kolejności: zaznaczenie, koperta, Termin, Zadanie, Aplikacja, Stan (bez Wymiarów, Priorytetu i Odbiorców). Pasek zapisanych widoków i filtr Moja rola zniknęły z ekranu; kod widoków zostaje w osobnym pliku, lista nie otwiera się już sama na widoku domyślnym, a Moja rola ze starego linku jest pomijana. Gałąź zad-298-lista-mniej-kolumn (commity 42b85fe, 07f4c17). Scenariusze odbioru KM-1 (Daniel i Paweł) poprawione we wdrożeniu FEBO: usunięte kroki 2.2 i 2.8, przenumerowane, 25 kroków; gałąź zad-298-scenariusz-kolumny (83c8001), PR do otwarcia: https://github.com/feboLogisticGit/febo-intranet-wdrozenie/pull/new/zad-298-scenariusz-kolumny. Kontrakt core-tasks 4.15 i decyzja lista-zadan-mniej-kolumn na main (9a1017f). Dev stoi na obu gałęziach do scalenia.
- Jak sprawdzić: Na https://dev.febo.redai.pl otworzyć Zadania: sześć kolumn, Termin pierwszy, nad listą brak wyboru widoku, Zapisz widok i Moja rola. Scenariusz: https://dev.febo.redai.pl/?scenariusz=odbior-fundamentu-km1, krok 2.1. Bramki frontendu: 525 testów, lint, typecheck, format, build zielone. Po scaleniu obu PR dev wraca na main: git checkout main && git pull --ff-only && make deploy (w obu katalogach).
