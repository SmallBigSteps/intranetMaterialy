<!--
opis: Notka zadania kodu ZAD-291: https://github.com/SmallBigSteps/intranet/pull/248
rodzaj: wynik
zadanie: ZAD-291
rozmowa: ZAD-291 Niestabilny test BR-FIL-005
zmiana: 13.11.2026, ZAD-291 Niestabilny test BR-FIL-005
status: do akceptacji
-->
# ZAD-291: Niestabilny test BR-FIL-005 (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/248
- Co zmieniono: Pomocnik docx() w testach BR-FIL buduje ZIP ze stałą datą, więc dwa wywołania dają te same bajty; scalone na main (b3acc54).
- Jak sprawdzić: make test: tests/rules/test_fil.py zielone; docx() zwraca identyczne bajty niezależnie od zegara
