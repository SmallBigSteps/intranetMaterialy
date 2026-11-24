<!--
opis: Notka zadania kodu ZAD-248: https://github.com/SmallBigSteps/intranet/commit/fa881375c54710563a83c08362abc19d4a6f346c
rodzaj: wynik
zadanie: ZAD-248
rozmowa: ZAD-248 Standardy: usuwanie szkicu do kosza
zmiana: 12.11.2026, ZAD-248 Standardy: usuwanie szkicu do kosza
status: do akceptacji
-->
# ZAD-248: Standardy: usuwanie szkicu do kosza (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/commit/fa881375c54710563a83c08362abc19d4a6f346c
- Co zmieniono: Autor usuwa szkic procedury sprzed pierwszego zatwierdzenia; szkic trafia do minimalnego kosza powłoki (core.recycle) na 30 dni i wraca pod tym samym numerem; wersji zatwierdzonej nie da się usunąć. Lista Standardów ma przełącznik „Pokaż usunięte” z przywracaniem. BR-STD-019 w manifeście z testami, BR-REC-001/003/005 z testami. Gałąź zad-248-usuwanie-szkicu-do-kosza; prośbę o scalenie otwiera się jednym kliknięciem: https://github.com/SmallBigSteps/intranet/pull/new/zad-248-usuwanie-szkicu-do-kosza
- Jak sprawdzić: Na dev jako autor: założyć nową procedurę, w szczegółach szkicu „Usuń szkic” → znika z listy; zaznaczyć „Pokaż usunięte” → szkic z datą końca kosza i „Przywróć” → wraca pod tym samym numerem. Procedura kiedyś zatwierdzona nie ma przycisku usuwania. Inny autor nie widzi cudzego kosza. Testy: pytest api/tests/rules/test_rec.py i testy Standardów (181 zielonych), vitest 552 zielone. Bramka reguł pokazuje BR-COC-018–021 i BR-STD-023 tak samo jak obecny main (nie z tej zmiany); testy archiwum audytu wymagają age-keygen, którego nie ma na serwerze portalu.
