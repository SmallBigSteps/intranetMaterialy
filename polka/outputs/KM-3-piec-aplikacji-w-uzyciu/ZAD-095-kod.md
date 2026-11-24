<!--
opis: Notka zadania kodu ZAD-095: https://github.com/SmallBigSteps/intranetFebo/pull/213
rodzaj: wynik
zadanie: ZAD-095
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-095 P1: Kolejka i alarmy — elementy zbiorcze na liście zadań maj
zmiana: 30.10.2026, ZAD-095 P1: Kolejka i alarmy — elementy zbiorcze na liście zadań maj
status: do akceptacji
-->
# ZAD-095: P1: Kolejka i alarmy — elementy zbiorcze na liście zadań mają stały klucz, a zamkniętego nie da się wystawić ponownie: druga awaria tej samej integracji, rejestru LogTMS czy martwej kolejki nie tworzy alarmu, bezpiecznik przestaje się otwierać, a nocna synchronizacja pozostałych rejestrów staje (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/213
- Co zmieniono: Elementy zbiorcze listy zadań (alarmy integracji, nieudana synchronizacja rejestru LogTMS, martwa kolejka, bezpiecznik, przegląd kokpitu) mają klucz wystąpienia {sytuacja}:{znacznik}: trwająca awaria aktualizuje jeden alarm, powrót po rozstrzygnięciu wystawia nowy, bezpiecznik znów się otwiera, a nieudana synchronizacja jednego rejestru nie blokuje pozostałych. Scalone: PR #213 (3840e92), kontrakt core-jobs 2.9 (fc2413f), testy wdrożenia PR #8 (1c51045).
- Jak sprawdzić: CI main zielone (testy powłoki, w tym test_aggregate_occurrences* i test_logtms_sync.py wdrożenia); na dev: wywołać awarię integracji, rozstrzygnąć alarm, wywołać ją ponownie — na liście zadań pojawia się nowy alarm.
