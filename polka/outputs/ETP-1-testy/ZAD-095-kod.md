<!--
opis: Notka zadania kodu ZAD-095: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-095-klucze-wystapien
rodzaj: wynik
zadanie: ZAD-095
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-095 P1: Kolejka i alarmy — elementy zbiorcze na liście zadań maj
zmiana: 30.10.2026, ZAD-095 P1: Kolejka i alarmy — elementy zbiorcze na liście zadań maj
status: do akceptacji
-->
# ZAD-095: P1: Kolejka i alarmy — elementy zbiorcze na liście zadań mają stały klucz, a zamkniętego nie da się wystawić ponownie: druga awaria tej samej integracji, rejestru LogTMS czy martwej kolejki nie tworzy alarmu, bezpiecznik przestaje się otwierać, a nocna synchronizacja pozostałych rejestrów staje (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-095-klucze-wystapien
- Co zmieniono: Elementy zbiorcze listy zadań dostają klucz wystąpienia {sytuacja}:{start}. Bezpiecznik, synchronizacja rejestrów, martwa kolejka, przypomnienia serwisów i przegląd kokpitu: druga awaria po zamknięciu alarmu tworzy nowy alarm, rozstrzygnięcie w trakcie znaczy 'wiem o tym', koniec wystąpienia zamyka element, a błąd listy zadań nie psuje bezpiecznika ani synchronizacji. Stare stałe klucze obsłużone bez migracji. Gałąź zad-095-klucze-wystapien, commit c4a99e7 (PR trzeba otworzyć z linku, na serwerze nie ma gh). Kontrakt core-jobs 2.7 (§4.5) na main, commit 8a514b7. Uwagi: pamięć 'wiem o tym' kończy się z archiwizacją po 90 dniach; klucz przeglądu ma 127/128 znaków.
- Jak sprawdzić: W api/: pytest tests/test_aggregate_occurrences.py tests/test_aggregate_occurrences_legacy.py tests/test_aggregate_occurrences_review.py, wynik 59 passed; pełny zestaw 1108 passed. Scenariusz: otwórz bezpiecznik, rozstrzygnij alarm, zamknij bezpiecznik, otwórz ponownie i sprawdź, że jest nowy alarm.
