<!--
opis: Notka zadania kodu ZAD-294: https://github.com/SmallBigSteps/intranet/pull/259
rodzaj: wynik
zadanie: ZAD-294
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-294 Deploy przebudowuje obraz migrate
zmiana: 13.11.2026, ZAD-294 Deploy przebudowuje obraz migrate
status: do akceptacji
-->
# ZAD-294: Deploy przebudowuje obraz migrate (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/259
- Co zmieniono: make deploy buduje także obraz usługi migrate, więc migracja i synchronizacja rejestrów idą na kodzie z tego samego wdrożenia
- Jak sprawdzić: Po make deploy na dev: kontener migrate powstał w tym wdrożeniu na obrazie o tym samym Id co api, a lista wycofanych zadań to tylko logtms.sync_dictionaries
