<!--
opis: Notka zadania kodu ZAD-208: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-194-testy-zad088
rodzaj: wynik
zadanie: ZAD-208
etap: ETP-1 Testy
rozmowa: ZAD-088 P1: Standardy — gdy przełożony autora ma nieaktywne konto (n · Tester
zmiana: 29.10.2026, ZAD-088 P1: Standardy — gdy przełożony autora ma nieaktywne konto (n · Tester
status: do akceptacji
-->
# ZAD-208: Testy po ZAD-088 (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-194-testy-zad088
- Co zmieniono: Testy po ZAD-088 (a6d9d70) oraz U2 (49312ba): osoba, której otwarte zadanie zatwierdzenia przekazano albo przypisano z nazwiska po zgłoszeniu, widzi tekst wersji. RULES.md 3.9, BR-STD-007.
- Jak sprawdzić: Na lab: testy Standardów 81/81, pełny zestaw jak na main (9 znanych błędów LogTMS), ruff, mypy, makemigrations --check, bramka reguł; mutacje odbiornika łapane przez 3 nowe testy test_br_std_007_*.
