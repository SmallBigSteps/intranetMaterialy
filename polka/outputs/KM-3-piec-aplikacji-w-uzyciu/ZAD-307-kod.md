<!--
opis: Notka zadania kodu ZAD-307: https://github.com/SmallBigSteps/intranet/pull/new/zad-307-wybor-trybu
rodzaj: wynik
zadanie: ZAD-307
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-307 Wybór trybu w profilu
zmiana: 16.11.2026, ZAD-307 Wybór trybu w profilu
status: do akceptacji
-->
# ZAD-307: Wybór trybu w profilu (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-307-wybor-trybu
- Co zmieniono: Wybór trybu wyglądu na koncie: Systemowy (domyślnie), Jasny, Ciemny. Pole theme w profilu (migracja 0016), odczyt GET me/, zmiana PATCH me/, kontrakt core-accounts 3.15, bez audytu. Wiersz Wygląd w karcie Ja w /profile i ikona trybu obok imienia w górnej belce (imię prowadzi do /profile, uzupełnienie decyzji a72df14 na main). data-theme ustawiany po odczycie profilu i po zmianie bez przeładowania; Systemowy = brak data-theme i prefers-color-scheme. Bez localStorage, sessionStorage i ciasteczka. Nieudany zapis z belki cicho wraca do trybu z serwera. Lista zamyka się przy zmianie rozmiaru okna. Poza zakresem: przy szerokości 320 px lista wystaje za prawą krawędź (cała strona też), od 340 px mieści się. Commit 81746dc.
- Jak sprawdzić: 1) Wybierz Ciemny w /profile, wyloguj się i zaloguj na innym komputerze: tryb zostaje. 2) Wybierz Systemowy i przełącz motyw systemu: strona zmienia się bez przeładowania. 3) grep -rn localStorage web/src nie znajduje nic nowego. 4) Ikona obok imienia w belce przełącza tryb bez przeładowania. 5) pytest tests/test_accounts_theme.py tests/test_accounts_api_shapes.py, npm run test, tsc, eslint, lint marki: zielone (vitest 605 testów).
