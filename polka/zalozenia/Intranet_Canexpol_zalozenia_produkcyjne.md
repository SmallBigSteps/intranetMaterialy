<!--
opis: Synteza kontekstu intranetu i propozycja założeń dla wersji produkcyjnej (04.08.2026): wskazuje zmiany wymagane w PRD, BRD, RULES i ARCHITECTURE; do zatwierdzenia.
rodzaj: zalozenie
rozmowa: zad001PolkiCPrzeniesienieMaterialow
zmiana: 14.10.2026, przeniesione z rozmowy intranet-czat-0408-1738 (ZAD-001)
status: roboczy
slowa: wersja produkcyjna, założenia, synteza, POC, zmiany w dokumentacji
-->
# Intranet Canexpol Group
## Podsumowanie kontekstu projektu i założenia dla wersji produkcyjnej

**Dokument:** synteza analityczna (Senior BA / Master Product Orchestrator)
**Data:** 4 sierpnia 2026
**Podstawa:** `PRD.md`, `CLIENT_BRD.md`, `RULES.md`, `ARCHITECTURE.md`, `CLAUDE.md`, `canexpol_intranet_visual_assumptions.json` oraz kod POC „Zamówienia Surowców”
**Status:** propozycja założeń do zatwierdzenia — nie zastępuje dokumentów kanonicznych, wskazuje wymagane w nich zmiany
**Odbiorca:** techniczno-biznesowy (Canexpol Group + zespół wdrożeniowy redAI)

---

## 1. Streszczenie zarządcze

Projekt ma dziś **kompletny, spójny zestaw dokumentacji kanonicznej** (PRD, CLIENT_BRD, RULES, ARCHITECTURE, CLAUDE) oraz **działający dowód wartości** — mini-aplikację „Zamówienia Surowców” zbudowaną na powłoce intranetu. Cały zakres **Must** z macierzy MoSCoW został zaimplementowany i pokryty testami krytycznych reguł biznesowych, a znaczna część zakresu **Should** działa w wersji podstawowej.

Wniosek analityczny: **warstwa funkcjonalna i domenowa jest dojrzała, warstwa eksploatacyjna nie istnieje.** POC świadomie odłożył wszystko, co decyduje o tym, czy system wolno wpuścić do codziennej pracy w środowisku regulowanym GMP: separację środowisk, backup, pełną śladowalność zmian, realne uwierzytelnianie, docelową bazę danych i realne (nie mockowane) integracje z WAPRO i LAB5. To nie są usterki — to zaakceptowany dług etapu POC, ujęty w rejestrze ryzyk RISK-001…RISK-010.

Przejście POC → produkcja to zatem **nie kontynuacja rozwoju funkcji, lecz odrębna faza inżynierii eksploatacyjnej i zgodności.** Niniejszy dokument porządkuje, co zostało ustalone, i formułuje 10 grup założeń produkcyjnych (P-01…P-10) wraz z bramkami wejścia do eksploatacji oraz listą decyzji, których nie da się podjąć bez klienta.

Najważniejsza pojedyncza decyzja przed produkcją nie jest techniczna: **model wielodostawczości (RISK-005) koliduje z modelem WAPRO „jeden produkt = jeden dostawca” i z zasadą certyfikacji per dostawca.** Wymaga formalnego stanowiska działu jakości, nie rozstrzygnięcia programistycznego.

---

## 2. Inwentaryzacja materiału źródłowego

Analizie poddano sześć artefaktów kanonicznych oraz kod POC. Poniżej to, co każdy z nich wnosi i jaki ma status.

| Artefakt | Rola w projekcie | Wnosi | Status |
|---|---|---|---|
| **PRD.md** (2026-07-12) | Kanoniczne źródło Wizji, HLO, Person, User Stories | Wizja dwuwarstwowa (Platforma + POC), 3 HLO, 7 person, 29 User Stories (US-01…US-29), 6 granic zakresu (OOS-01…OOS-06) | Zatwierdzony |
| **CLIENT_BRD.md** (2026-07-13) | Dokument kontraktowy, kanoniczny Risk Register | HLO w formie SMART (horyzont: 1 mies. po wdrożeniu), MoSCoW, NFR, 10 ryzyk RISK-001…010 | Zatwierdzony |
| **RULES.md** (2026-07-12) | Reguły biznesowe w Gherkin | 9 reguł: BR-SUPPLIER-001…004, BR-DEMAND-001…003, BR-DELIVERY-001…002 + scenariusze testowe | Zatwierdzony |
| **ARCHITECTURE.md** | Architektura, model danych, NFR, strategia testów | Wzorzec „Droga B”, PostgreSQL, adaptery integracyjne, 4 ścieżki krytyczne do pokrycia testami | Status POC |
| **CLAUDE.md** (projektowy) | Pamięć projektu, standardy kodu, instrukcje dla agentów AI | Stack, struktura katalogów, zasady: read-only integracje, „Draft + akceptacja człowieka”, obowiązek wersjonowania | Częściowo propozycje |
| **visual_assumptions.json** | System wizualny wywiedziony z canexpol.waw.pl | Paleta (Navy `#16294A`, Orange `#E37A2C`, kolory semantyczne, Compliance Blue-Gray), typografia, siatka, komponenty, mikrokopia | Inferencja — do weryfikacji z CSS marki |
| **Kod POC „Zamówienia Surowców”** | Dowód wartości (Poziom A + B) | 18 encji domenowych, ~12 ekranów, 6 komponentów design systemu, 3 pliki testów Pest, katalog serwisów platformy | Działający, dane demonstracyjne |

> **Luka dokumentacyjna:** dokumenty kanoniczne powołują się na cztery źródła, których nie ma w kontekście projektu: `01-cele-biznesowe-transformacji-cyfrowej.md`, `02-koncept-wdrozenia-redAI.md`, `03-proces-zamowien.md` oraz `Specyfikacja-systemu-redAI.md`. Ryzyka RISK-008…010 oraz opis procesu „as-is” pochodzą właśnie stamtąd. Przed produkcją należy je dołączyć albo formalnie przenieść ich treść do dokumentów kanonicznych — inaczej traceability jest niepełna.

---

## 3. Co POC faktycznie udowodnił

### 3.1 Zrealizowany zakres funkcjonalny

| Obszar | User Stories | Reguły biznesowe | Pokrycie testami |
|---|---|---|---|
| Przeliczanie i konsolidacja zapotrzebowania | US-08, US-09, US-10 | BR-DEMAND-001/002/003 | Tak (Pest) |
| Wielodostawczość: certyfikowani dostawcy, kandydat domyślny | US-05, US-07, US-11 | BR-SUPPLIER-002 | Pośrednio |
| Zmiana dostawcy + blokada wg zastrzeżeń klienta | US-14, US-15 | BR-SUPPLIER-001/003 | Tak (Pest) |
| Zamówienie u dostawcy i śledzenie statusu | US-02, US-12, US-13 | — | Nie |
| Przyjęcie dostawy i weryfikacja zgodności | US-17, US-18, US-19 | BR-DELIVERY-001/002 | Tak (Pest) |
| Powłoka intranetu: role, dashboard KPI, katalog serwisów | US-23, US-29 | — | Nie |
| Zakres Should (podstawowy) | US-01, US-03, US-04, US-06, US-16 | BR-SUPPLIER-004 | Nie |

### 3.2 Wartość architektoniczna, którą warto zachować

- **Model domenowy nie został spłaszczony.** Łańcuch `Recipe → RecipeItem → RawMaterial → wielu Supplier`, z certyfikacją i ceną per para (surowiec, dostawca), to rdzeń wartości całego przedsięwzięcia. Utrzymać bez kompromisów.
- **Zasada „Draft + akceptacja człowieka” jest wymuszona technicznie, nie proceduralnie.** Brak integracji mailowej oznacza, że system fizycznie nie może wysłać nic automatycznie. To mocniejsze zabezpieczenie niż zapis w regulaminie — i argument, by nie dodawać integracji SMTP bezrefleksyjnie.
- **Katalog serwisów w konfiguracji platformy.** Dodanie kolejnego serwisu to wpis konfiguracyjny, a nie przebudowa powłoki — to bezpośrednio realizuje wizję Platformy (US-21) i przesądza o skalowalności na kolejne obszary.
- **Warstwa adapterów integracyjnych** (`WaproReadService`, `Lab5ReadService`) izoluje domenę od systemów źródłowych. Dziś zwracają dane z seedów — podmiana na realne połączenie nie dotyka logiki biznesowej.
- **Design system wywiedziony z marki.** Sześć komponentów (karta KPI, chip statusu, kroki numerowane, alert, karta, nagłówek) pokrywa większość potrzeb ekranów operacyjnych.

---

## 4. Analiza luki POC → produkcja

Poniżej wszystkie różnice między stanem obecnym a stanem wymaganym do eksploatacji. Kolumna „Waga” oznacza: **Blokująca** = bez tego nie wolno uruchomić produkcyjnie; **Wysoka** = do zamknięcia w pierwszym kwartale eksploatacji; **Średnia** = planowo.

| Obszar | Stan w POC | Wymagane produkcyjnie | Waga | Ryzyko |
|---|---|---|---|---|
| Baza danych | SQLite w pliku | PostgreSQL, dedykowana instancja, własne poświadczenia, pełna izolacja od bazy portalu | **Blokująca** | — |
| Uwierzytelnianie | Mockowe — wybór roli kafelkiem, bez hasła | Realne konta z hasłem, polityka haseł, sesje, MFA dla ról uprzywilejowanych; SSO do decyzji | **Blokująca** | — |
| Środowiska | Jedno | Separacja dev / test / prod, promocja zmian tylko przez test | **Blokująca** | RISK-001 |
| Backup | Brak | Automatyczny backup bazy i plików + okresowy test odtworzenia (potwierdzony protokołem) | **Blokująca** | RISK-003 |
| Śladowalność (GMP) | `created_at` / `updated_at` / `created_by`; historia tylko dla cen | Pełny audit trail: kto, kiedy, co, wartość przed i po, powód — dla receptur, dostawców, zamówień, przyjęć | **Blokująca** | RISK-004 |
| Integracje WAPRO / LAB5 | Adaptery zwracające dane z seedów | Realne połączenia read-only, harmonogram synchronizacji, obsługa niedostępności źródła, log synchronizacji | **Blokująca** | — |
| Dane | Demonstracyjne, fikcyjne | Migracja słowników rzeczywistych (surowce, dostawcy, klienci, receptury, zastrzeżenia US-15) + walidacja jakości danych | **Blokująca** | — |
| CI/CD i rollback | Deploy ręczny SSH + `git pull`, hotfix „na żywo” | Pipeline z bramką testową, wersjonowane wydania, zdefiniowana ścieżka wycofania | Wysoka | RISK-002 |
| Hosting | Proces na porcie w infrastrukturze VPS redAI | Własna subdomena, nginx jako reverse proxy, HTTPS, proces trwały (Supervisor/systemd) | Wysoka | — |
| Monitoring i SLA | Logi Laravel czytane ręcznie przez SSH | Monitoring dostępności i błędów, alert do administratora, uzgodnione okno wsparcia i czasy reakcji | Wysoka | RISK-008 |
| Model wielodostawczości vs WAPRO | Rozwiązany w modelu aplikacji; brak decyzji jakościowej | Formalne stanowisko działu jakości: jak certyfikacja per dostawca współistnieje z modelem WAPRO | **Blokująca** | RISK-005 |
| Alerty i powiadomienia | Poza zakresem (OOS-04) | Do rozważenia: zaległe zamówienia, terminy dostaw, receptury czekające na akceptację | Średnia | — |
| Rozliczanie zużycia AI | Brak | Rejestr kosztów/zużycia modelu per serwis | Średnia | RISK-009 |
| Dane w modelu AI | Brak polityki | Polityka bezpieczeństwa danych przekazywanych do modelu (receptury, dane klientów) | Wysoka | RISK-010 |
| Testy | 3 pliki Feature — reguły krytyczne | Rozszerzenie o BR-SUPPLIER-002/004, ścieżki uprawnień, integracje, testy regresji przed każdym wydaniem | Wysoka | RISK-007 |

---

## 5. Założenia dla wersji produkcyjnej

Dziesięć grup założeń (P-01…P-10). Każde jest propozycją do zatwierdzenia; założenia oznaczone **[DECYZJA]** wymagają rozstrzygnięcia po stronie Canexpol.

### P-01. Architektura i infrastruktura

- Docelowy wzorzec „Droga B” zostaje utrzymany: samodzielna aplikacja, własna baza, własne API — bez wtapiania w kod portalu redAI.
- Aplikacja przenosi się na **własną subdomenę** obsługiwaną przez nginx jako reverse proxy, z certyfikatem TLS i automatycznym odnawianiem. Proces utrzymywany trwale (Supervisor/systemd), z automatycznym restartem po awarii.
- **PostgreSQL** jako docelowa baza: dedykowana instancja, własne poświadczenia, brak jakiegokolwiek współdzielenia z bazą portalu. Migracja z SQLite przez standardowy mechanizm migracji Laravel, z jednorazowym przeniesieniem danych referencyjnych.
- Trzy środowiska: **dev** (praca zespołu), **test/staging** (odbiór funkcjonalny i UAT, dane maskowane lub syntetyczne), **prod** (eksploatacja). Zmiana trafia na produkcję wyłącznie przez test — bez wyjątków „na skróty”.
- **[DECYZJA]** Docelowa lokalizacja hostingu: pozostać na VPS dostawcy czy przenieść na infrastrukturę Canexpol. Wpływa na politykę backupu, dostępów i odpowiedzialność za SLA.

### P-02. Bezpieczeństwo, tożsamość i uprawnienia

- Logowanie mockowe zostaje zastąpione realnym uwierzytelnianiem: konta imienne, polityka haseł, wygasanie sesji, blokada po nieudanych próbach.
- **Konta muszą być imienne, nie funkcyjne.** W środowisku GMP zapis „zmiany dokonał dział zamówień” nie ma wartości dowodowej — potrzebne jest wskazanie osoby.
- Model uprawnień: role odwzorowujące 6 person (5 operacyjnych + administrator), z rozdziałem uprawnień per serwis platformy — każdy dział widzi wyłącznie swoje narzędzia i dane (US-26).
- **MFA obowiązkowe dla administratora platformy** i ról mogących zmieniać dostawcę na zamówieniu produkcyjnym.
- Sekrety (poświadczenia WAPRO, LAB5) pozostają w zmiennych środowiskowych z ograniczonymi prawami dostępu do pliku; rotacja przy każdej zmianie personalnej w zespole utrzymania.
- **[DECYZJA]** SSO z domeną Canexpol — czy wdrażać od razu, czy po pierwszym kwartale eksploatacji. Wpływa na koszt i na czas uruchomienia.

### P-03. Śladowalność i zgodność GMP

To najistotniejsza zmiana jakościowa względem POC i warunek dopuszczenia systemu do pracy w procesie regulowanym.

- Pełny **audit trail** dla obiektów krytycznych: receptura, powiązanie surowiec–dostawca, certyfikacja, zamówienie u dostawcy, zmiana dostawcy, przyjęcie dostawy, rezerwacja. Rejestrowane: kto, kiedy, jakie pole, wartość przed i po, powód zmiany.
- Zapisy audytowe są **nieusuwalne i niemodyfikowalne** z poziomu interfejsu — również dla administratora.
- Każda akcja wymagająca uzasadnienia (BR-SUPPLIER-001, BR-SUPPLIER-003, BR-DELIVERY-002) zapisuje uzasadnienie jako trwały element śladu, nie jako komentarz swobodny do nadpisania.
- **Certyfikacja pozostaje bramką jakościową.** Rekomendacja na produkcję: dla zamówień produkcyjnych ostrzeżenie miękkie z POC należy podnieść do **blokady z możliwością nadpisania przez rolę jakościową** — dziś system dopuszcza zapis samym kliknięciem, co w audycie jest trudne do obrony. Dla próbek ostrzeżenie miękkie wystarcza.
- Retencja danych i zapisów audytowych zgodna z wymaganiami dokumentacji jakościowej Canexpol; eksport śladu do postaci czytelnej dla audytora.
- **[DECYZJA]** Okres retencji oraz zakres wymaganych zapisów — do potwierdzenia z działem jakości.
- **[DECYZJA]** Czy akceptacje w systemie mają mieć charakter podpisu elektronicznego (ponowne uwierzytelnienie przy zatwierdzeniu), czy wystarcza zapis audytowy.

### P-04. Integracje z systemami źródłowymi

- **WAPRO (WF-Mag, SQL Server) — wyłącznie odczyt** (OOS-01). Zakaz zapisu pozostaje w mocy również w wersji produkcyjnej. Odczyt przez konto o uprawnieniach wyłącznie do odczytu, na wybranych obiektach — ograniczenie egzekwowane po stronie bazy, nie tylko kodu.
- **LAB5/ELAB — głównie odczyt** receptur i INCI; każdy zapis musi mieć w kodzie komentarz wskazujący numer User Story, który go uzasadnia (OOS-03).
- Synchronizacja cykliczna przez harmonogram (US-25), z rejestrem przebiegów: co, kiedy, ile rekordów, jakie błędy.
- Wymagana jawna obsługa niedostępności źródła: system pokazuje **datę ostatniej udanej synchronizacji** przy danych pochodzących z zewnątrz. Użytkownik nigdy nie może zobaczyć nieaktualnego stanu magazynowego bez oznaczenia, że jest nieaktualny — to bezpośrednie ryzyko produkcyjne.
- Poczta do dostawców: rekomendacja **utrzymania braku integracji mailowej**. Techniczna niemożność wysyłki jest najsilniejszym gwarantem zasady „Draft + akceptacja człowieka” (OOS-05).
- **[DECYZJA]** Jeśli mimo to integracja mailowa ma powstać — wymaga osobnej decyzji, obowiązkowego kroku potwierdzenia przez człowieka przed każdą wysyłką oraz pełnego logu wysłanych wiadomości.

### P-05. Model domenowy i wielodostawczość

- Model **Recipe → wielu Supplier** pozostaje centralnym modelem domenowym i nie podlega spłaszczeniu do wzorca WAPRO „jeden produkt = jeden dostawca”.
- **RISK-005 wymaga rozstrzygnięcia przed produkcją i jest to decyzja jakościowa, nie systemowa.** Do ustalenia z działem jakości: w jaki sposób certyfikat wydany na konkretnego dostawcę współistnieje z recepturą dopuszczającą kilku dostawców tego samego surowca oraz jak ten stan jest odwzorowany w dokumentacji WAPRO.
- Lista klientów zastrzegających dostawcę (US-15) staje się **danymi krytycznymi**: wymaga właściciela biznesowego, procedury aktualizacji i śladu zmian. Błąd w tej liście skutkuje naruszeniem wymagań klienta lub certyfikacji produktu (RISK-006).
- Reguła BR-DEMAND-001 (zapotrzebowanie bez bufora strat) pozostaje bez zmian; ewentualny bufor technologiczny to osobna decyzja biznesowa, nie poprawka w kodzie.

### P-06. Wymagania niefunkcjonalne dla eksploatacji

| Obszar | POC | Założenie produkcyjne |
|---|---|---|
| Wydajność | < 3 s, cel orientacyjny | Ekrany operacyjne < 2 s; zestawienie zbiorcze i przeliczenia < 5 s przy pełnym wolumenie danych |
| Dostępność | Godziny pracy, bez SLA | Uzgodnione okno dostępności i czasy reakcji na awarię; awaria krytyczna = blokada zamówień surowcowych |
| Odtwarzalność | Brak backupu | Zdefiniowane RPO (maks. dopuszczalna utrata danych) i RTO (maks. czas przywrócenia), potwierdzone testem odtworzenia |
| Monitoring | Logi czytane ręcznie | Automatyczne wykrywanie niedostępności i błędów aplikacji, powiadomienie administratora |
| Dostępność cyfrowa | Brak wymogu WCAG | Utrzymane jako narzędzie wewnętrzne; obowiązkowe minimum: kontrast tekstu, widoczny focus klawiatury, status nigdy nie kodowany wyłącznie kolorem |
| Ochrona danych osobowych | Brak wymogu ponad standard | Do ponownej oceny — konta imienne i audit trail to przetwarzanie danych pracowników; potrzebna podstawa i okres retencji |

**[DECYZJA]** Konkretne wartości SLA, RPO i RTO wymagają potwierdzenia przez Canexpol — wynikają z tolerancji biznesowej na przestój procesu zamówień, nie z możliwości technicznych.

### P-07. Platforma jako platforma

- Katalog serwisów pozostaje mechanizmem rozszerzania: dodanie nowego serwisu = wpis w katalogu i nadanie uprawnień, bez przebudowy powłoki (US-21).
- Dashboard z KPI zależnymi od roli oraz szybkie uruchamianie serwisów (US-23, US-29) przechodzą z kategorii „Could” do standardowego wyposażenia — to jedyny mechanizm realizujący HLO-PLAT-001 (adopcja).
- Dziennik aktywności platformy (US-27) obejmuje logowania, zmiany uprawnień i uruchomienia serwisów — odrębnie od audit trail domenowego (P-03), który dotyczy danych jakościowych.
- Zadania cykliczne (US-25) obsługują synchronizacje i przeliczenia; każdy przebieg zapisuje wynik, żeby cicha awaria harmonogramu była wykrywalna.
- Kolejne obszary do objęcia Platformą, wskazane w PRD: materiały niewykorzystane, stany magazynowe, oferty i wyceny, planowanie produkcji. Kolejność do ustalenia po zebraniu danych baseline.

### P-08. Warstwa wizualna i treść interfejsu

- System wizualny z pliku założeń zostaje utrzymany: Navy jako chrome (nawigacja, nagłówki, stopka), Orange **wyłącznie jako pojedyncza akcja główna na ekranie**, neutralne tła dla ~80% powierzchni ekranów operacyjnych.
- Compliance Blue-Gray i cztery kolory semantyczne (sukces, ostrzeżenie, błąd, informacja) to **jedyne** kolory niosące znaczenie statusu. Żaden inny kolor nie może sugerować stanu — w audycie dwuznaczność statusu jest wadą.
- Każdy status ma etykietę tekstową lub ikonę obok koloru; chipy GO / NO-GO zarezerwowane wyłącznie dla formalnych decyzji zwolnienia jakościowego.
- Mikrokopia: język operacyjny, bez sformułowań perswazyjnych; kontrolowane słownictwo dla terminów regulowanych (partia, receptura, seria, niezgodność, zwolnienie partii) — synonimy utrudniają audyt.
- Tabele danych na urządzeniach mobilnych zamieniają się w karty zamiast przewijania poziomego — realni użytkownicy mobilni to magazyn i produkcja.
- **[DECYZJA]** Paleta i typografia w pliku założeń są **inferencją** z serwisu publicznego, nie odczytem z materiałów marki. Przed produkcją wymagają potwierdzenia dokładnych wartości (kolory, krój pisma) z działem marketingu Canexpol.

### P-09. Jakość, testy i zarządzanie zmianą

- Cztery ścieżki krytyczne pozostają obowiązkowo pokryte testami: przeliczanie zapotrzebowania (US-10), zmiana dostawcy z blokadą (US-14/15), konsolidacja zamówień (US-09), weryfikacja przyjęcia dostawy (US-18).
- Rozszerzenie pokrycia o BR-SUPPLIER-002 (kandydat domyślny wg najniższej ceny), BR-SUPPLIER-004 (historia cen i statusy zapytań) oraz o testy uprawnień — czy rola widzi wyłącznie to, co powinna.
- Zasada „Rule = Test = Documentation” zostaje utrzymana: każda nowa reguła biznesowa trafia jednocześnie do `RULES.md` i do testu.
- Wersjonowanie obowiązkowe — każda zmiana w repozytorium, żaden kod produkcyjny nie istnieje wyłącznie na serwerze.
- Zdefiniowana ścieżka wycofania wydania. Hotfix „na żywo” na produkcji przestaje być dopuszczalny.

### P-10. Dane, wdrożenie i przyjęcie przez użytkowników

- Migracja danych rzeczywistych: słowniki surowców, dostawców i klientów, receptury, powiązania surowiec–dostawca z certyfikacją i cenami, lista zastrzeżeń dostawcy (US-15). Każdy zbiór wymaga wskazanego właściciela biznesowego i walidacji kompletności przed startem.
- Wszystkie dane demonstracyjne zostają usunięte, a baner „Środowisko POC” zdjęty dopiero po potwierdzeniu, że w systemie nie ma danych fikcyjnych.
- **Zebranie danych baseline jest warunkiem rozliczenia HLO-POC-001** (redukcja strat surowcowych) — bez wartości wyjściowej strat sprzed wdrożenia cel pozostaje niemierzalny i nierozliczalny.
- Szkolenia per rola oraz krótkie instrukcje stanowiskowe; w środowisku GMP zapis o przeszkoleniu użytkownika systemu bywa przedmiotem audytu.
- Okres pracy równoległej z dotychczasowym trybem (tablica korkowa, poczta) przed odcięciem starego procesu — z góry ustalona data odcięcia, żeby uniknąć trwałego dublowania pracy.

---

## 6. Rejestr ryzyk — status i domknięcie

Dziesięć ryzyk z kanonicznego rejestru `CLIENT_BRD` §6, uzupełnionych o dwa ryzyka wynikające wprost z analizy stanu POC.

| ID | Ryzyko | Status po POC | Domknięcie produkcyjne |
|---|---|---|---|
| RISK-001 | Brak separacji środowisk | Otwarte | P-01 — blokujące uruchomienie |
| RISK-002 | Brak CI/CD i rollbacku | Otwarte, kompensowane ręczną suitą testów | P-09 — pipeline z bramką testową |
| RISK-003 | Brak backupu | Otwarte | P-01 — backup + protokół testu odtworzenia |
| RISK-004 | Minimalny audyt | Częściowo zamknięte (historia cen) | P-03 — pełny audit trail; blokujące dla GMP |
| RISK-005 | Kolizja wielodostawczości z modelem WAPRO | Rozwiązane w modelu aplikacji, decyzja jakościowa otwarta | P-05 — stanowisko działu jakości; **blokujące** |
| RISK-006 | Zmiana dostawcy wbrew zastrzeżeniu klienta | Zamknięte technicznie | P-05 — właściciel i procedura aktualizacji listy |
| RISK-007 | Błąd przeliczenia zapotrzebowania | Zamknięte testami | P-09 — utrzymanie pokrycia przy każdej zmianie |
| RISK-008 | Brak SLA i monitoringu | Otwarte | P-06 — uzgodnione SLA, monitoring, alerting |
| RISK-009 | Brak rejestru kosztów AI | Otwarte | P-07 — rozliczenie zużycia per serwis |
| RISK-010 | Dane w modelu AI w chmurze | Otwarte | P-04 / P-03 — polityka danych przekazywanych do modelu |
| **RISK-011** *(nowe)* | Uwierzytelnianie mockowe — brak przypisania działania do osoby | Otwarte | P-02 — konta imienne, MFA dla ról uprzywilejowanych |
| **RISK-012** *(nowe)* | Praca na nieaktualnych danych z WAPRO przy cichej awarii synchronizacji | Nieadresowane (integracje mockowe) | P-04 — data ostatniej synchronizacji widoczna przy danych zewnętrznych |

---

## 7. Decyzje wymagane po stronie Canexpol

Poniższych rozstrzygnięć nie da się podjąć w oparciu o dokumentację — wymagają stanowiska biznesowego lub jakościowego. Uszeregowane wg wpływu na harmonogram.

| # | Decyzja | Kto decyduje | Blokuje |
|---|---|---|---|
| D-01 | Współistnienie certyfikacji per dostawca z wielodostawczą recepturą (RISK-005) | Dział jakości | Uruchomienie produkcyjne |
| D-02 | Zakres audit trail, okres retencji, forma akceptacji (zapis vs podpis elektroniczny) | Dział jakości + IT | Projekt techniczny P-03 |
| D-03 | Blokada twarda czy ostrzeżenie miękkie przy dostawcy niecertyfikowanym na zamówieniu produkcyjnym | Dział jakości | Zmiana reguły BR-SUPPLIER-001 |
| D-04 | Docelowy hosting: VPS dostawcy czy infrastruktura Canexpol | IT Canexpol | Plan migracji, polityka backupu |
| D-05 | SSO od startu czy w kolejnym etapie | IT Canexpol | Zakres etapu 1 |
| D-06 | Wartości SLA, RPO i RTO | Właściciel procesu + IT | Umowa utrzymaniowa |
| D-07 | Wartość baseline strat surowcowych i docelowy próg redukcji (HLO-POC-001) | Właściciel procesu / finanse | Rozliczenie efektu wdrożenia |
| D-08 | Właściciel biznesowy listy klientów zastrzegających dostawcę (US-15) | Sprzedaż + jakość | Migracja danych |
| D-09 | Dostęp read-only do WAPRO i LAB5 — konta, zakres obiektów, okno synchronizacji | IT Canexpol | Realne integracje |
| D-10 | Czy powstaje integracja mailowa (rekomendacja: nie) | Właściciel procesu | Zakres P-04 |
| D-11 | Potwierdzenie palety i typografii marki | Marketing | Finalizacja design systemu |
| D-12 | Kolejność obejmowania Platformą kolejnych obszarów | Zarząd / właściciele procesów | Roadmapa po etapie 1 |

---

## 8. Rekomendowana ścieżka wdrożenia

Cztery etapy z jawnymi bramkami wyjścia. Bramka niespełniona = etap nie jest zamknięty, niezależnie od postępu prac w pozostałych obszarach.

| Etap | Zakres | Bramka wyjścia |
|---|---|---|
| **E1 — Fundament eksploatacyjny** | PostgreSQL, trzy środowiska, backup z testem odtworzenia, realne uwierzytelnianie z rolami, własna subdomena z TLS, pipeline z bramką testową | Odtworzenie bazy z backupu wykonane i udokumentowane; brak dostępu bez uwierzytelnienia |
| **E2 — Zgodność i śladowalność** | Pełny audit trail obiektów krytycznych, decyzja jakościowa D-01/D-02/D-03, eksport śladu dla audytora, polityka danych przekazywanych do modelu AI | Stanowisko działu jakości na piśmie; ślad zmiany dostawcy odtwarzalny od decyzji do skutku |
| **E3 — Realne dane i integracje** | Połączenia read-only z WAPRO i LAB5, harmonogram synchronizacji z rejestrem przebiegów, migracja słowników rzeczywistych, usunięcie danych demonstracyjnych | Stan magazynowy zgodny z WAPRO na próbie kontrolnej; zero rekordów demonstracyjnych w bazie |
| **E4 — Eksploatacja i rozliczenie** | Szkolenia per rola, praca równoległa z datą odcięcia, monitoring i alerting, pomiar HLO po miesiącu | HLO-PLAT-001 i HLO-POC-002 zmierzone; HLO-POC-001 rozliczone względem baseline z D-07 |

> Etap **E2 jest krytyczny i najczęściej niedoszacowywany**: dotyczy zgodności, więc jego opóźnienie blokuje start niezależnie od gotowości funkcjonalnej. Warto uruchomić rozmowy z działem jakości równolegle z pracami etapu E1, nie po nich.

---

## 9. Weryfikacja spójności dokumentacji (CoVe)

Kontrola krzyżowa dokumentów kanonicznych. Poniższe rozbieżności należy usunąć — dziś dokumenty rozchodzą się między sobą albo z rzeczywistym stanem kodu.

| # | Rozbieżność | Rekomendacja |
|---|---|---|
| C-01 | `ARCHITECTURE` i `CLAUDE` wskazują PostgreSQL; POC działa na SQLite | Utrzymać PostgreSQL jako cel; odnotować SQLite jako stan wyłącznie POC z terminem migracji |
| C-02 | `RULES` §3 stwierdza, że projekt nie ma `CLIENT_BRD` z rejestrem ryzyk — a `CLIENT_BRD` istnieje od 2026-07-13 | Zaktualizować uwagę proceduralną w `RULES`; wskazać `CLIENT_BRD` §6 jako kanoniczny rejestr |
| C-03 | `CLIENT_BRD` klasyfikuje dashboard, role i dziennik aktywności jako „Could”, a POC je zrealizował | Przenieść do zakresu standardowego — bez nich HLO-PLAT-001 jest niemierzalne |
| C-04 | HLO-POC-001 pozostaje TBD od 2026-07-13 (brak progu redukcji strat) | Domknąć decyzją D-07; bez baseline cel jest nierozliczalny |
| C-05 | `CLIENT_BRD` stwierdza brak wymogów RODO ponad standard; produkcja wprowadza konta imienne i audit trail | Ponowna ocena — przetwarzanie danych pracowników wymaga podstawy i okresu retencji |
| C-06 | Cztery dokumenty źródłowe cytowane w traceability są niedostępne w kontekście projektu | Dołączyć lub przenieść ich treść do dokumentów kanonicznych |
| C-07 | `CLAUDE` zakłada Laravel 11; wersje frameworka i PHP wymagają potwierdzenia wsparcia w horyzoncie eksploatacji | Ustalić wersję z aktywnym wsparciem bezpieczeństwa na cały okres utrzymania |
| C-08 | Wartości kolorów i typografii w pliku wizualnym to inferencja, nie odczyt z materiałów marki | Potwierdzić z marketingiem (D-11) przed utrwaleniem design systemu |

---

## 10. Wpływ na dokumenty kanoniczne

| Dokument | Wymagana aktualizacja |
|---|---|
| **PRD.md** | US-20 i US-22 przechodzą z „post-POC” do zakresu obowiązkowego etapu E1; dopisać wymaganie realnego uwierzytelniania i widoczności aktualności danych z systemów źródłowych |
| **CLIENT_BRD.md** | Domknąć HLO-POC-001 (D-07); dopisać RISK-011 i RISK-012; zaktualizować NFR o SLA, RPO i RTO; ponowna ocena zapisu o RODO; przenieść pozycje „Could” zrealizowane w POC |
| **RULES.md** | Usunąć nieaktualną uwagę o braku `CLIENT_BRD`; ewentualna zmiana BR-SUPPLIER-001 po decyzji D-03; dopisać reguły audytu i widoczności danych zewnętrznych |
| **ARCHITECTURE.md** | Przepisać sekcje 1, 4, 5 i 7 na warunki produkcyjne: środowiska, CI/CD, backup, audit trail, monitoring, realne integracje |
| **CLAUDE.md** (projektowy) | Zdjąć status POC; zastąpić propozycje potwierdzonymi decyzjami; dopisać zakaz hotfixów na produkcji i zasadę kont imiennych |
| **visual_assumptions.json** | Zastąpić wartości inferowane potwierdzonymi wartościami marki (D-11) |

---

*Dokument opracowany na podstawie analizy pełnego kontekstu projektu Intranet Canexpol Group: dokumentacji kanonicznej (PRD, CLIENT_BRD, RULES, ARCHITECTURE, CLAUDE), założeń wizualnych oraz kodu i notatek POC „Zamówienia Surowców”. Nie zastępuje dokumentów kanonicznych — wskazuje wymagane w nich zmiany (sekcja 10).*
