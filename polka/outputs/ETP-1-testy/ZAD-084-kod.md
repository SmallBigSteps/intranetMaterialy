<!--
opis: Notka zadania kodu ZAD-084: https://github.com/SmallBigSteps/intranetFebo/commit/9313dcd33fa393ac0b6fca319ce06119fcbaed58
rodzaj: wynik
zadanie: ZAD-084
etap: ETP-1 Testy
rozmowa: ZAD-084 P1: Zadania i Standardy — przejście z jednej procedury lub z
zmiana: 28.10.2026, ZAD-084 P1: Zadania i Standardy — przejście z jednej procedury lub z
status: do akceptacji
-->
# ZAD-084: P1: Zadania i Standardy — przejście z jednej procedury lub zadania do drugiej (np. przez wyszukiwarkę) przenosi otwarty formularz i komunikat „Decyzja zapisana”: może ukryć decyzję do podjęcia albo wycofać nie tę procedurę (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/commit/9313dcd33fa393ac0b6fca319ce06119fcbaed58
- Co zmieniono: Gałąź zad-084-reset-stanu-przy-zmianie-obiektu (commit 9313dcd na aktualnym main). Każdy ekran adresowany identyfikatorem z adresu (procedura, wersja, zadanie, uwaga i trasy wszystkich aplikacji) zaczyna od czystego stanu po przejściu na inny obiekt. Wspólny hak pobierania danych nie pokazuje danych poprzedniego obiektu w trakcie ładowania, co naprawia też kartę osoby w Książce adresowej i filtr paczki płatności. Dochodzi 11 testów A→B. PR do założenia: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-084-reset-stanu-przy-zmianie-obiektu?expand=1
- Jak sprawdzić: lint, typecheck, format OK, npm test 473/473 (INTRANET_DEPLOYMENT=febo_deployment, INTRANET_DEPLOYMENT_DIR=febo-intranet-wdrozenie); nowe testy A→B padają na starym kodzie. Tester przeszedł scenariusze z audytu w przeglądarce (Vite + makieta API): decyzja A→B, karta wycofania A→B (jedno żądanie na pB z powodem B), zadanie A→B, opóźnienie odpowiedzi, regresja list, katalogu osób i paczki. Wszystko OK, a stary kod odtwarza usterki. Po scaleniu: na dev otworzyć procedurę A, zapisać decyzję, przez wyszukiwarkę przejść na B, gdzie powinien być blok decyzji bez komunikatu.
