<!--
opis: Notka zadania kodu ZAD-088: https://github.com/SmallBigSteps/intranetFebo/pull/203
rodzaj: wynik
zadanie: ZAD-088
etap: ETP-1 Testy
rozmowa: ZAD-088 P1: Standardy — gdy przełożony autora ma nieaktywne konto (n
zmiana: 28.10.2026, ZAD-088 P1: Standardy — gdy przełożony autora ma nieaktywne konto (n
status: do akceptacji
-->
# ZAD-088: P1: Standardy — gdy przełożony autora ma nieaktywne konto (np. jeszcze go nie aktywował), zatwierdzenie trafia do opiekuna biblioteki, który nie może otworzyć tekstu procedury: zatwierdza w ciemno albo zadanie utyka (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/203
- Co zmieniono: Standardy: wersja zgłoszona do zatwierdzenia zapisuje listę rozstrzygających; opiekun biblioteki (gdy przełożony autora ma nieaktywne konto) widzi tekst wersji, którą ma rozstrzygnąć. Nowe pole, migracja 0008 z uzupełnieniem otwartych zatwierdzeń, powód na zadaniu, reguły BR-STD-007 i BR-STD-025 (RULES 3.8).
- Jak sprawdzić: Autor z nieaktywnym przełożonym zgłasza wersję; opiekun biblioteki dostaje zadanie z powodem i otwiera tekst wersji; testy apps/standards zielone (74).
