<!--
opis: Notka zadania kodu ZAD-336: https://github.com/SmallBigSteps/intranet/pull/new/zad-336-deklaracje-zrodel-z-wdrozenia
rodzaj: wynik
zadanie: ZAD-336
rozmowa: ZAD-336 Deklaracje źródeł z wdrożenia
zmiana: 23.11.2026, ZAD-336 Deklaracje źródeł z wdrożenia
status: do akceptacji
-->
# ZAD-336: Deklaracje źródeł z wdrożenia (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-336-deklaracje-zrodel-z-wdrozenia
- Co zmieniono: Powłoka sama czyta deklaracje źródeł z katalogu sources/ wdrożenia: obraz api kopiuje go z kontekstu deployment, start dokłada go przy ustawionym INTRANET_DEPLOYMENT, bez wpisu w .env na test i prod. INTAKE_DECLARATIONS_DIR zostaje dodatkiem, podwójna deklaracja nadal zatrzymuje start. Reguła BR-ITK-017 z pięcioma testami, kontrakt core-intake 1.4, rules.contract 3.41.
- Jak sprawdzić: CI zielone (testy, mypy, e2e). Po scaleniu i wystawieniu na dev: Zasilanie pokazuje deklarację optima/settlements obok fixture, próbna dostawa optima/settlements nie zwraca unknown_dataset (IntranetWdrozenieFebo ZAD-019).
