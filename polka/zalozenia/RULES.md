<!--
opis: Reguły biznesowe modułu „Zamówienia Surowców” w formacie Gherkin: wielodostawczość, przeliczanie zapotrzebowania, przyjęcie dostawy.
rodzaj: zalozenie
rozmowa: zad001PolkiCPrzeniesienieMaterialow
zmiana: 14.10.2026, przeniesione z projects/intranet/context (ZAD-001)
status: zaakceptowany
slowa: reguły biznesowe, Gherkin, dostawcy, zapotrzebowanie, dostawa, GMP
-->
# Business Rules: Zamówienia Surowców (POC platformy redAI) — Canexpol Group

## 1. Executive Summary

Domena obejmuje logikę biznesową modułu **Zamówienia Surowców** — pierwszego serwisu budowanego na platformie redAI dla Canexpol Group (Laboratorium Kosmetyczne, producent private label). Moduł wspiera proces: od zapotrzebowania surowcowego wynikającego z zamówień klientów, przez wielodostawczy model receptur, po przyjęcie dostawy na magazyn.

Reguły w tym pliku dotyczą trzech klastrów funkcjonalnych:
- **Wielodostawczość i zmiana dostawcy** (US-05, US-06, US-07, US-11, US-14, US-15, US-16) — najważniejszy wątek funkcjonalno-regulacyjny całego PRD (kolizja z modelem WAPRO „jeden produkt = jeden dostawca").
- **Przeliczanie zapotrzebowania i sumowanie zamówień** (US-08, US-09, US-10) — najwyższe ryzyko biznesowe: błąd w tej logice ma bezpośredni skutek produkcyjny (przestój).
- **Przyjęcie dostawy** (US-18, US-19) — weryfikacja zgodności dostawy i aktualizacja stanu dostępne/zarezerwowane.

Reguły elicytowane z udziałem partnera IT Canexpol na bazie materiałów źródłowych `PRD.md`, `03-proces-zamowien.md`, `CLAUDE.md`, `ARCHITECTURE.md` — zgodnie z zasadą projektu „nie wymyślaj reguł biznesowych ani zachowań systemów źródłowych".

Wszystkie reguły są spójne z nadrzędnymi zasadami platformy: **„Draft + akceptacja człowieka"** (`OOS-05` — żadna akcja o skutku zewnętrznym nie wykonuje się automatycznie) oraz **środowiskiem regulowanym (GMP)**, które wymaga zachowania ścieżki akceptacji jakościowej (proces certyfikacji, krok 4.2).

## 2. Business Rules Registry

| ID | Rule Description | Test Scenario (Given-When-Then) — scenariusz główny | Status (B/F) |
|:---|:---|:---|:---|
| BR-SUPPLIER-001 | Wybór niecertyfikowanego dostawcy generuje miękkie ostrzeżenie; wybór dostawcy innego niż zastrzeżony przez klienta (US-15) wymaga obowiązkowego uzasadnienia tekstowego przed zapisem | **Given** klient zastrzega dostawcę dla surowca (US-15) i wybrany dostawca jest inny **When** specjalista ds. zamówień próbuje zapisać zamówienie **Then** system wymaga uzasadnienia i blokuje zapis do jego podania | B/F |
| BR-SUPPLIER-002 | Domyślny dostawca-kandydat = certyfikowany dostawca z najniższą potwierdzoną ceną; ceny aktualizowane przez drafty maili (US-16) i opcjonalnie AI/OCR, zawsze z potwierdzeniem człowieka | **Given** składnik ma kilku certyfikowanych dostawców z potwierdzonymi cenami **When** system przygotowuje zestawienie zbiorcze (US-09) **Then** wskazuje dostawcę z najniższą potwierdzoną ceną jako kandydata | B/F |
| BR-SUPPLIER-003 | Zmiana dostawcy na złożonym zamówieniu wymaga podania powodu, zapisywanego w historii; rezerwacja (BR-DEMAND-002) przechodzi automatycznie na nowego dostawcę | **Given** zamówienie ma przypisaną rezerwację u dostawcy A **When** specjalista zmienia dostawcę na B, podając powód **Then** rezerwacja zostaje przepisana na dostawcę B, powód zapisany w historii | B/F |
| BR-SUPPLIER-004 | Widok dostawców pokazuje cenę bieżącą, historię cen i status ostatniego zapytania o cenę (wysłano / brak odpowiedzi / odpowiedziano) | **Given** składnik ma wielu dostawców **When** specjalista otwiera widok dostawców **Then** system pokazuje cenę, historię cen i status zapytania dla każdego | B/F |
| BR-DEMAND-001 | Zapotrzebowanie = ilość zamówienia klienta × udział % składnika w recepturze, bez bufora strat | **Given** zamówienie na X kg wg receptury z udziałem P% składnika **When** system przelicza zapotrzebowanie **Then** wynik = X × P/100, bez dodatkowego bufora | B |
| BR-DEMAND-002 | Rezerwacja stanu surowca powstaje dopiero w momencie złożenia zamówienia u dostawcy (nie wcześniej) | **Given** zamówienie klienta jest tylko przeliczone, nie złożone u dostawcy **When** specjalista sprawdza stan surowca **Then** ilość nie jest jeszcze liczona jako zarezerwowana | B/F |
| BR-DEMAND-003 | Konsolidacja dzienna: zamówienia z zastrzeżonym dostawcą (US-15) osobno per dostawca; pozostałe zbiorczo z kandydatem „do weryfikacji"; próbki nigdy nie sumowane z produkcją | **Given** dwóch klientów potrzebuje tego samego surowca tego samego dnia, bez zastrzeżeń dostawcy **When** generowane jest zestawienie zbiorcze **Then** ilości są sumowane w jedną pozycję z kandydatem do weryfikacji | B/F |
| BR-DELIVERY-001 | Przyjęta dostawa alokowana do zamówień źródłowych (pozostaje zarezerwowana); nadwyżka ponad potrzeby przechodzi do „dostępne" | **Given** przyjęto dostawę większą niż suma zarezerwowanych potrzeb **When** specjalista ds. przyjęć oznacza przyjęcie **Then** nadwyżka otrzymuje status „dostępne" | B/F |
| BR-DELIVERY-002 | Rozbieżność ilości dostawa vs zamówienie wymaga komentarza; zamówienie pozostaje otwarte na brakującą ilość | **Given** dostarczona ilość mniejsza niż zamówiona **When** specjalista próbuje zapisać przyjęcie **Then** system wymaga komentarza i pozostawia zamówienie otwarte | B/F |

### 2.1 Szczegółowe scenariusze Gherkin

#### BR-SUPPLIER-001 — Wybór dostawcy poza certyfikacją / poza zastrzeżeniem klienta

```gherkin
Scenariusz: Wybór niecertyfikowanego dostawcy, klient nie zastrzega
  Given zamówienie dotyczy klienta spoza listy zastrzegającej dostawcę dla tego surowca (US-15)
  And wybrany dostawca nie ma zakończonego procesu certyfikacji dla tego składnika
  When specjalista ds. zamówień wybiera tego dostawcę na zamówieniu
  Then system wyświetla niekokujący komunikat ostrzegawczy o braku certyfikacji
  And zamówienie można zapisać bez dodatkowych działań

Scenariusz: Zmiana dostawcy u klienta zastrzegającego dostawcę
  Given zamówienie dotyczy klienta z listy zastrzegającej dostawcę dla tego surowca (US-15)
  And wybrany dostawca różni się od dostawcy zastrzeżonego dla tego klienta
  When specjalista ds. zamówień próbuje zapisać zamówienie z tym dostawcą
  Then system wymaga wpisania uzasadnienia tekstowego
  And blokuje zapis, dopóki uzasadnienie nie zostanie podane
  And po podaniu uzasadnienia zapisuje je przy zamówieniu i pozwala zapisać zamówienie

Scenariusz: Próba zapisu bez wymaganego uzasadnienia
  Given zachodzi warunek wymagający uzasadnienia (klient zastrzega, dostawca inny)
  When specjalista ds. zamówień próbuje zapisać zamówienie bez wypełnienia pola uzasadnienia
  Then system blokuje zapis i wskazuje brakujące pole
```

#### BR-SUPPLIER-002 — Domyślny dostawca-kandydat i aktualizacja cen

```gherkin
Scenariusz: Generowanie zapytań o cenę do dostawców
  Given składnik ma więcej niż jednego certyfikowanego dostawcę w recepturze
  When specjalista ds. zamówień uruchamia aktualizację cen dla tego składnika
  Then system generuje drafty maili z zapytaniem o cenę do każdego certyfikowanego dostawcy
  And żaden mail nie jest wysyłany automatycznie

Scenariusz: Wprowadzenie ceny z pomocą AI/OCR
  Given specjalista otrzymał odpowiedź mailową od dostawcy z ceną
  When wkleja treść odpowiedzi do narzędzia AI/OCR w systemie
  Then system proponuje wyodrębnioną wartość ceny jako edytowalny szkic
  And cena zapisuje się w systemie dopiero po potwierdzeniu przez specjalistę

Scenariusz: Wybór domyślnego kandydata do zestawienia zbiorczego
  Given składnik ma kilku certyfikowanych dostawców z zapisanymi, potwierdzonymi cenami
  When system przygotowuje zbiorcze zestawienie dziennych zamówień (US-09)
  Then jako domyślny dostawca-kandydat wskazywany jest dostawca z najniższą potwierdzoną ceną
```

#### BR-SUPPLIER-003 — Zmiana dostawcy na złożonym zamówieniu

```gherkin
Scenariusz: Zmiana dostawcy z podaniem powodu
  Given zamówienie ma przypisanego dostawcę
  And dostawca zgłosił problem z dostawą/dostępnością składnika
  When specjalista ds. zamówień zmienia dostawcę na zamówieniu
  Then system wymaga podania powodu zmiany przed zapisem
  And zapisuje powód w historii zamówienia widocznej dla handlowca i laboratorium

Scenariusz: Zmiana dostawcy u klienta zastrzegającego dostawcę
  Given klient zamówienia jest na liście zastrzegającej dostawcę dla tego surowca (US-15)
  When specjalista próbuje zmienić dostawcę na innego niż zastrzeżony
  Then obowiązuje dodatkowo BR-SUPPLIER-001 (wymagane uzasadnienie override'u)

Scenariusz: Próba zmiany bez podania powodu
  When specjalista próbuje zapisać zmianę dostawcy bez wypełnienia pola powodu
  Then system blokuje zapis i wskazuje brakujące pole

Scenariusz: Przeniesienie rezerwacji na nowego dostawcę
  Given zamówienie ma już zarezerwowaną ilość surowca u dostawcy A (BR-DEMAND-002)
  When specjalista zmienia dostawcę na dostawcę B, podając powód zmiany
  Then rezerwacja zostaje automatycznie przepisana na dostawcę B
  And ilość pozostaje "zarezerwowana" pod tym samym zamówieniem klienta przez cały czas zmiany
```

#### BR-SUPPLIER-004 — Widok dostawców z historią cen i statusem odpowiedzi

```gherkin
Scenariusz: Widok listy dostawców dla składnika
  Given składnik ma przypisanych kilku certyfikowanych dostawców
  When specjalista ds. zamówień otwiera widok dostawców dla tego składnika
  Then system wyświetla dla każdego: aktualną cenę, datę ostatniego zamówienia, historię cen i status zapytania o cenę

Scenariusz: Status "brak odpowiedzi"
  Given wysłano draft zapytania o cenę do dostawcy (BR-SUPPLIER-002)
  And nie wprowadzono jeszcze potwierdzonej ceny z odpowiedzi
  When specjalista otwiera widok dostawców
  Then status dostawcy pokazuje "brak odpowiedzi" wraz z datą wysłania zapytania

Scenariusz: Status "odpowiedziano" i aktualizacja historii
  Given specjalista potwierdził cenę wyodrębnioną z odpowiedzi dostawcy (BR-SUPPLIER-002)
  When specjalista otwiera widok dostawców
  Then status dostawcy pokazuje "odpowiedziano" z datą i nową ceną
  And poprzednia cena zostaje przeniesiona do historii cen tego dostawcy
```

#### BR-DEMAND-001 — Przeliczenie zapotrzebowania z receptury

```gherkin
Scenariusz: Przeliczenie zapotrzebowania na składnik
  Given zamówienie klienta na X kg produktu wg receptury R
  And składnik S ma udział P% wagowy w recepturze R
  When specjalista ds. zapotrzebowania przelicza zamówienie
  Then wymagana ilość składnika S = X × (P / 100)
  And żaden dodatkowy bufor nie jest doliczany
```

#### BR-DEMAND-002 — Model rezerwacji stanu surowca

```gherkin
Scenariusz: Stan przed złożeniem zamówienia u dostawcy
  Given zamówienie klienta jest w Fazie 3 (przeliczone, jeszcze nie przekazane do złożenia u dostawcy)
  When specjalista ds. zapotrzebowania sprawdza stan surowca
  Then ilość potrzebna dla tego zamówienia NIE jest jeszcze wliczona do "zarezerwowane"

Scenariusz: Rezerwacja po złożeniu zamówienia u dostawcy
  Given specjalista ds. zamówień złożył zamówienie u dostawcy na dany surowiec
  When inny użytkownik sprawdza stan tego surowca
  Then ilość z tego zamówienia jest widoczna jako "zarezerwowane"
  And nie jest liczona jako "dostępne"
```

#### BR-DEMAND-003 — Konsolidacja zbiorczego zamówienia z wielu klientów

```gherkin
Scenariusz: Konsolidacja zamówień bez zastrzeżenia dostawcy
  Given dwóch klientów z zamówieniami tego samego dnia potrzebuje tego samego surowca S
  And żaden z nich nie jest na liście zastrzegającej dostawcę dla S (US-15)
  When specjalista ds. zapotrzebowania generuje zbiorcze zestawienie dnia
  Then ilości dla surowca S są zsumowane w jedną pozycję
  And pozycja ma zaproponowanego dostawcę-kandydata oznaczonego jako "do weryfikacji"

Scenariusz: Rozdzielenie przy zastrzeżonym dostawcy
  Given klient A wymaga dostawcy X dla surowca S (US-15), klient B nie ma zastrzeżenia
  When specjalista ds. zapotrzebowania generuje zbiorcze zestawienie dnia
  Then zapotrzebowanie klienta A na surowiec S tworzy osobną linię z dostawcą X
  And zapotrzebowanie klienta B jest konsolidowane osobno wg BR-SUPPLIER-002

Scenariusz: Rozłączność zamówień próbnych i produkcyjnych
  Given laborant zamówił próbkę surowca S w kroku 4.1
  And specjalista ds. zapotrzebowania przelicza produkcyjne zapotrzebowanie na surowiec S w kroku 5.1
  When generowane jest zbiorcze zestawienie dnia
  Then zamówienie próbne NIE jest wliczane do sumy zamówienia produkcyjnego
```

#### BR-DELIVERY-001 — Alokacja przyjętej dostawy

```gherkin
Scenariusz: Przyjęcie dostawy dokładnie pokrywającej zapotrzebowanie
  Given zamówienie u dostawcy na surowiec S obejmowało łączne potrzeby klientów A i B (BR-DEMAND-003)
  When specjalista ds. przyjęć oznacza pełne przyjęcie dostawy
  Then ilości odpowiadające potrzebom klientów A i B pozostają "zarezerwowane" pod te zamówienia

Scenariusz: Przyjęcie dostawy z nadwyżką
  Given zamówiono więcej surowca S niż suma potrzeb klientów (np. wg MOQ)
  When specjalista ds. przyjęć oznacza przyjęcie dostawy
  Then nadwyżka ponad sumę zarezerwowanych potrzeb otrzymuje status "dostępne"
```

#### BR-DELIVERY-002 — Wykrywanie rozbieżności dostawy vs zamówienie

```gherkin
Scenariusz: Dostawa zgodna z zamówieniem
  Given zamówienie na ilość Q surowca S
  When specjalista ds. przyjęć oznacza przyjęcie dostawy o ilości Q
  Then dostawa jest oznaczona jako "zgodna" i zamówienie może zostać zamknięte

Scenariusz: Dostawa niepełna
  Given zamówienie na ilość Q surowca S
  When specjalista ds. przyjęć oznacza przyjęcie dostawy o ilości mniejszej niż Q
  Then system oznacza dostawę jako "częściowa/niezgodna"
  And wymaga wpisania komentarza przed zapisaniem przyjęcia
  And zamówienie pozostaje otwarte na brakującą ilość

Scenariusz: Próba zapisu bez komentarza przy rozbieżności
  Given wykryto rozbieżność ilości dostawy
  When specjalista próbuje zapisać przyjęcie bez wpisania komentarza
  Then system blokuje zapis i wskazuje wymagane pole komentarza
```

## 3. Technical Integration

- **Testing Framework:** Pest (PHP) — zgodnie z `CLAUDE.md` / `ARCHITECTURE.md`.
- **Prompt:** `Generate Pest test for BR-[ID]`
- **Traceability:** `PRD.md` (US-05…US-19, OOS-01…OOS-06), `03-proces-zamowien.md` (proces as-is), `CLAUDE.md` (standardy kodowania), `ARCHITECTURE.md` (model danych, ryzyka POC).
- **Krytyczne pokrycie testami (wg `ARCHITECTURE.md` §6):** BR-DEMAND-001 (najwyższe ryzyko biznesowe — błąd = przestój produkcji), BR-SUPPLIER-001/BR-SUPPLIER-003 (blokada zmiany dostawcy wg US-15), BR-DEMAND-003 (sumowanie zamówień), BR-DELIVERY-002 (weryfikacja przyjęcia).
- **Uwaga dot. audytu:** BR-SUPPLIER-004 wymaga jawnej historii cen (`price_history`), co rozszerza minimalny model audytu zaakceptowany w `ARCHITECTURE.md` (tylko `created_at`/`updated_at`/`user_id`) — rozszerzenie ograniczone wyłącznie do danych cenowych, nie całego systemu.
- **Uwaga proceduralna:** ten projekt nie posiada obecnie osobnego `CLIENT_BRD.md` z rejestrem ryzyk projektowych — powyższa uwaga o rozszerzeniu audytu pozostaje więc odnotowana tutaj do czasu utworzenia takiego rejestru.

## 4. Changelog

- 2026-07-12 | v1.0 | Pierwsza wersja RULES.md dla modułu Zamówienia Surowców — klastry: wielodostawczość/zmiana dostawcy (BR-SUPPLIER-001…004), przeliczanie zapotrzebowania/konsolidacja (BR-DEMAND-001…003), przyjęcie dostawy (BR-DELIVERY-001…002). US-17 potraktowane jako proste kryterium akceptacji bez osobnej reguły biznesowej.
