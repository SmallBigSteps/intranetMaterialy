<!--
opis: Notka zadania kodu ZAD-097: https://github.com/SmallBigSteps/intranetFebo/pull/215
rodzaj: wynik
zadanie: ZAD-097
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-097 P1: Konta i audyt — pięć nocnych zadań z kontraktu etapu 1 n
zmiana: 30.10.2026, ZAD-097 P1: Konta i audyt — pięć nocnych zadań z kontraktu etapu 1 n
status: do akceptacji
-->
# ZAD-097: P1: Konta i audyt — pięć nocnych zadań z kontraktu etapu 1 nie jest uruchamianych (odświeżanie zastępstw, czyszczenie tokenów i prób logowania, archiwizacja i czyszczenie audytu): dane osobowe rosną ponad okres retencji, a start i koniec zastępstw nie trafiają do dziennika (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/215
- Co zmieniono: Pięć nocnych zadań z kontraktu etapu 1 wpięte w harmonogram: odświeżanie zastępstw (co 15 min, start i koniec do dziennika), czyszczenie tokenów i prób logowania, archiwizacja audytu do magazynu ze strażnikiem (bez magazynu nic nie jest kasowane) i czyszczenie audytu bezpieczeństwa; kontrakty core-audit 3.6 i core-accounts 3.13, instrukcja wdrożenia krok 8a.
- Jak sprawdzić: Na dev: rejestr zadań okresowych pokazuje 5 nowych zadań z godzinami; testy test_nightly_jobs i test_aud_retention zielone; bez katalogu archiwum audit.archive kończy się w martwych zadaniach i nic nie usuwa.
