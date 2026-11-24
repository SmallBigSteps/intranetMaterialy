<!--
opis: Notka zadania kodu ZAD-296: https://github.com/SmallBigSteps/intranet/commit/2042fa74562c151d9f723e315a1b1f6824ee6932
rodzaj: wynik
zadanie: ZAD-296
kamien: KM-1 Fundament powłoki gotowy
rozmowa: ZAD-016 Odbiór fundamentu powłoki przez Daniela Urbaniaka (właścicie
zmiana: 15.11.2026, ZAD-016 Odbiór fundamentu powłoki przez Daniela Urbaniaka (właścicie
status: do akceptacji
-->
# ZAD-296: Panel scenariusza odbioru na dev (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/commit/2042fa74562c151d9f723e315a1b1f6824ee6932
- Co zmieniono: Panel scenariusza testowego wysuwany z prawej strony Intranetu (otwierany adresem ?scenariusz=<klucz>), krok po kroku: co zrobić, czego się spodziewać, Zgodne, Zgłoś uwagę do kroku do rejestru uwag, podsumowanie. Mechanizm w powłoce (gałąź zad-296-panel-scenariusza), treść scenariusza odbioru KM-1 we wdrożeniu FEBO (gałąź zad-296-scenariusz-odbioru, commit 732f84e). PR do otwarcia: https://github.com/SmallBigSteps/intranet/pull/new/zad-296-panel-scenariusza oraz https://github.com/feboLogisticGit/febo-intranet-wdrozenie/pull/new/zad-296-scenariusz-odbioru. Dev stoi na tych gałęziach do scalenia.
- Jak sprawdzić: Otworzyć https://dev.febo.redai.pl/?scenariusz=odbior-fundamentu-km1, kliknąć Zaczynam, przejść kroki; uwaga do kroku po zalogowaniu trafia do rejestru uwag z numerem kroku. Bramki frontendu: 518 testów, lint, typecheck, format zielone.
