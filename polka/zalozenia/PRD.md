<!--
opis: PRD intranetu Canexpol i modułu POC „Zamówienia Surowców”: wizja, HLO/KPI, persony, user stories, zakres (kanoniczne źródło person i HLO).
rodzaj: zalozenie
rozmowa: zad001PolkiCPrzeniesienieMaterialow
zmiana: 14.10.2026, przeniesione z projects/intranet/context (ZAD-001)
status: zaakceptowany
slowa: PRD, wizja, persony, user stories, HLO, zakres, zamówienia surowców
-->
# PRD: Intranet Canexpol Group + Moduł POC „Zamówienia Surowców” (platforma redAI)

**Status:** Zatwierdzony wynik wywiadu elicytacyjnego (Faza 3 — PRD Generation)
**Data:** 2026-07-12
**Struktura projektu:** dwuwarstwowa — **Platforma** (Intranet, Poziom A) + **POC** (Zamówienia Surowców, Poziom B)
**Odbiorca:** techniczno-biznesowy; dokument jest kanonicznym źródłem Wizji, HLO/KPI i Person dla dalszych dokumentów projektu (np. BRD, specyfikacje modułów), które powinny je importować, a nie re-elicytować.

---

## 1. Product Vision

### 1.1 Wizja — Platforma (Intranet)
Zebranie w jednym miejscu narzędzi usprawniających i przyspieszających pracę, z możliwością ich szybkiego budowania i dostosowywania.

### 1.2 Wizja — Moduł POC (Zamówienia Surowców)
Uszczelnienie i cyfrowa transformacja procesu zamówień surowców w celu jego optymalizacji pod kątem kosztowym, jakościowym i czasowym.

*Relacja między warstwami:* Moduł POC jest pierwszym dowodem wartości Platformy — weryfikuje zasady governance (wersjonowanie, dostęp, akceptacja człowieka) w praktyce, zanim Platforma obejmie kolejne obszary (materiały niewykorzystane, stany magazynowe, oferty/wyceny, planowanie produkcji).

---

## 2. High-Level Objectives (KPIs)

| ID | Warstwa | Objective (SMART) | Success Metric |
|----|---------|--------------------|-----------------|
| HLO-PLAT-001 | Platforma | Adopcja platformy przez pracowników | Liczba logowań oraz wykorzystanie poszczególnych serwisów/narzędzi (dziennik zdarzeń per serwis). **Próg liczbowy celowo otwarty** — do doprecyzowania po fazie POC. |
| HLO-POC-001 | POC | Redukcja strat generowanych przez przeterminowane/niewykorzystane surowce | Wartość strat surowcowych (PLN) w okresie po wdrożeniu vs. baseline sprzed wdrożenia |
| HLO-POC-002 | POC | Konsolidacja widoczności stanów surowcowych | % surowców, dla których aktualny stan jest widoczny w jednym miejscu, bez przełączania się między WAPRO / LAB5 / mailem |

---

## 3. User Personas

| Persona | Warstwa | Primary Goals | Key Frustrations | Usage Frequency |
|---|---|---|---|---|
| **Handlowiec** | POC | Szybkie przygotowanie oferty dla klienta; sprawne ustalenie szczegółów zamówienia po akceptacji próbki; utrzymanie dobrej relacji z klientem | Oczekiwanie na informacje od laboratorium/technologa; brak wglądu w status realizacji zamówienia u dostawcy; ręczna komunikacja mailowa na każdym etapie | Codziennie, wielokrotnie w ciągu dnia |
| **Technolog / Laborant** | POC | Szybkie przygotowanie wyceny; sprawne tworzenie/modyfikacja receptury i próbki; weryfikacja dostępności surowców przed próbką; dodanie wielu dostawców do receptury (certyfikacja + negocjacja cen przy zamówieniach produkcyjnych) | Brak informacji od handlowców (oczekiwana ilość klienta); rozjazd danych WAPRO ↔ LAB5; trudny proces wykorzystania wielu dostawców w recepturze | Codziennie |
| **Specjalista ds. zapotrzebowania** | POC | Szybkie i bezbłędne przeliczenie zapotrzebowania surowcowego; zestawienie zamówień z danego dnia w jeden dokument | Wybór każdego dostawcy osobno mimo tego samego składnika w recepturze; zamawianie na zapas bez klarownych wytycznych; brak widoczności, ile towaru jest już zarezerwowane; ręczne sumowanie zamówień od klientów | Codziennie, przy każdym zamówieniu produkcyjnym |
| **Specjalista ds. zamówień** | POC | Sprawne złożenie zamówienia u dostawcy; ustalenie/śledzenie terminu dostawy; jasna komunikacja potwierdzeń/problemów; rotacja dostawców w celu negocjacji cen | Potwierdzenia zamówień tylko jako skan maila; brak możliwości negocjacji między dostawcami; brak jednego źródła prawdy i harmonogramu zamówień; długi, zmienny czas dostawy | Codziennie |
| **Specjalista ds. przyjęć magazynowych** | POC | Poprawne wprowadzenie surowców do WAPRO; jasne oznaczenie odbioru dostawy; szybka weryfikacja zgodności dostawy z zamówieniem | Śledzenie dostaw na tablicy korkowej (żółte karteczki); ryzyko pomyłki/utraty informacji przy ręcznym oznaczaniu odbioru | Codziennie, przy każdej dostawie |
| **Administrator platformy (IT)** | Platforma | Governance (wersjonowanie, backup, separacja środowisk dev/test/prod); zarządzanie użytkownikami i dostępami; nadzór nad architekturą serwisów; sprawne dodawanie nowych serwisów bez ryzyka dla działających modułów | Brak narzędzi pozwalających na samodzielne wprowadzanie zmian, serwisów czy mini-aplikacji oraz zarządzanie nimi | Codziennie w fazie POC (aktywny rozwój); docelowo rzadziej |
| **Użytkownik platformy** | Platforma | Jeden punkt startowy do pracy — dashboard z interesującymi KPI, szybki dostęp do aplikacji i ich ustawień, szybkie wyzwalanie aplikacji | Konieczność przełączania się między osobnymi narzędziami, by sprawdzić bieżący stan spraw | Codziennie |

---

## 4. Key User Stories

### 4.1 Handlowiec
- **US-01:** Jako Handlowiec, chcę widzieć w jednym miejscu aktualny status wyceny/próbki, aby informować klienta bez dopytywania laboratorium mailowo.
- **US-02:** Jako Handlowiec, chcę mieć wgląd w status złożonego zamówienia u dostawcy (potwierdzenie, planowany termin dostawy), aby proaktywnie informować klienta bez oczekiwania na rozproszone potwierdzenia mailowe.
- **US-03:** Jako Handlowiec, chcę przekazywać zamówienie/zapytanie od klienta do laboratorium i działu zapotrzebowania w łatwy i dostępny dla wszystkich zainteresowanych sposób, aby informacja była od razu widoczna i spójna dla wszystkich zaangażowanych osób.

### 4.2 Technolog / Laborant
- **US-04:** Jako Technolog/Laborant, chcę szybko sprawdzić dostępność potrzebnych surowców (z uwzględnieniem tego, co już zarezerwowane), aby wiedzieć, czy mogę od razu przygotować próbkę.
- **US-05:** Jako Technolog/Laborant, chcę dodać do receptury wielu alternatywnych dostawców danego składnika, aby certyfikować produkt na kilku dostawcach i umożliwić negocjację cen przy zamówieniach produkcyjnych.
- **US-06:** Jako Technolog/Laborant, chcę w łatwy sposób udostępnić recepturę z wieloma dostawcami działowi zapotrzebowania, aby wiedział, spośród których dostawców może wybierać.

### 4.3 Specjalista ds. zapotrzebowania
- **US-07:** Jako Specjalista ds. zapotrzebowania, chcę widzieć jednego, zsumowanego dostawcę-kandydata dla składnika występującego u wielu dostawców, aby szybciej przygotować zestawienie zamówienia.
- **US-08:** Jako Specjalista ds. zapotrzebowania, chcę widzieć stan surowca z rozbiciem na ilość dostępną i zarezerwowaną pod inne zamówienia, aby nie zamawiać na zapas bez pokrycia w realnym zapotrzebowaniu.
- **US-09:** Jako Specjalista ds. zapotrzebowania, chcę automatycznie sumować zamówienia od różnych klientów w jedno zbiorcze zamówienie surowców, aby nie robić tego ręcznie każdego dnia.
- **US-10:** Jako Specjalista ds. zapotrzebowania, chcę automatycznie przeliczać zamówienie od klienta zgodnie z recepturą na wymagane ilości składników, aby uniknąć błędów rachunkowych.

### 4.4 Specjalista ds. zamówień
- **US-11:** Jako Specjalista ds. zamówień, chcę widzieć wielu dostawców przypisanych do składnika wraz z cenami, aby rotacyjnie negocjować i wybierać najkorzystniejszą ofertę.
- **US-12:** Jako Specjalista ds. zamówień, chcę mieć jedno wspólne źródło prawdy i harmonogram wszystkich złożonych zamówień, aby nie polegać na skanach maili i tablicy korkowej.
- **US-13:** Jako Specjalista ds. zamówień, chcę rejestrować potwierdzenie/problem zgłoszony przez dostawcę bezpośrednio przy zamówieniu, aby handlowiec i laboratorium mieli wgląd bez dopytywania mailem.
- **US-14:** Jako Specjalista ds. zamówień, chcę mieć możliwość zmiany dostawcy na zamówieniu przy problemach z dostawą/dostępnością, aby szybko reagować bez przestoju produkcji.
- **US-15:** Jako Specjalista ds. zamówień, chcę widzieć, którzy klienci nie pozwalają na zmianę dostawcy danego składnika, aby nie podmienić dostawcy tam, gdzie jest to niedozwolone.
- **US-16:** Jako Specjalista ds. zamówień, chcę mieć listę kontaktową dostawców wraz z gotowymi draftami maili, aby przyspieszyć i ustandaryzować komunikację.

### 4.5 Specjalista ds. przyjęć magazynowych
- **US-17:** Jako Specjalista ds. przyjęć magazynowych, chcę widzieć cyfrową listę oczekiwanych dostaw zamiast tablicy korkowej, aby wiedzieć co i kiedy powinno dotrzeć.
- **US-18:** Jako Specjalista ds. przyjęć magazynowych, chcę oznaczyć odbiór dostawy w systemie i porównać ją z zamówieniem, aby od razu wychwycić rozbieżności.
- **US-19:** Jako Specjalista ds. przyjęć magazynowych, chcę aby przyjęty surowiec automatycznie aktualizował widoczny stan „dostępne / zarezerwowane”, aby inne role nie musiały ręcznie sprawdzać stanu w WAPRO.

### 4.6 Administrator platformy (IT) — *Platforma*
- **US-20 *(post-POC)*:** Jako Administrator platformy, chcę mieć oddzielne środowiska dev/test/prod, aby zmiany nie trafiały bezpośrednio na narzędzie używane codziennie.
- **US-21:** Jako Administrator platformy, chcę mieć narzędzia do samodzielnego wprowadzania zmian, dodawania serwisów/mini-aplikacji i zarządzania nimi, aby nie być zależnym od ręcznej ingerencji w kod.
- **US-22 *(post-POC)*:** Jako Administrator platformy, chcę mieć automatyczny backup bazy danych i plików (z okresowym testem odtworzenia), aby zminimalizować ryzyko nieodwracalnej utraty danych.
- **US-25:** Jako Administrator platformy, chcę zlecać zadania cykliczne (harmonogram, jak cron) wykonywane automatycznie w tle, aby powtarzalna praca działała bez ręcznego uruchamiania.
- **US-26:** Jako Administrator platformy, chcę zarządzać rolami, grupami i uprawnieniami użytkowników do poszczególnych serwisów, aby każdy dział miał dostęp tylko do swoich narzędzi i danych.
- **US-27:** Jako Administrator platformy, chcę mieć wgląd w dziennik aktywności, aby móc rozliczać działania i diagnozować problemy.

### 4.7 Użytkownik platformy — *Platforma*
- **US-23:** Jako Użytkownik platformy, chcę mieć dashboard pokazujący interesujące mnie KPI (sprzedaż, ilość zamówień, stany magazynowe) oraz łatwy dostęp do aplikacji i ich ustawień, aby mieć jeden punkt startowy do pracy.
- **US-29:** Jako Użytkownik platformy, chcę szybko wyzwalać interesujące mnie aplikacje/serwisy oraz podglądać część ich informacji na dashboardzie platformy, aby nie wchodzić do każdej aplikacji osobno.

---

## 5. Project Boundaries (Out-of-Scope)

- **OOS-01:** Integracja z WAPRO w POC ograniczona wyłącznie do **odczytu** danych (bez zapisu/synchronizacji zwrotnej).
- **OOS-02:** Zakres POC obejmuje **wyłącznie surowce** — opakowania i inne materiały poza zakresem.
- **OOS-03:** Integracja z LAB5 (ELAB) — głównie odczyt (receptura/INCI); zapis (update/insert) dopuszczalny **tylko tam, gdzie rzeczywiście niezbędny** do obsługi kroku procesu. *(Zasada dotyczy całej Platformy, nie tylko POC.)*
- **OOS-04:** Alerty/powiadomienia (zaległe zamówienia, terminy dostaw, receptury czekające na akceptację) — poza zakresem POC.
- **OOS-05:** Brak automatycznego wysyłania maili/dokumentów bez akceptacji człowieka — system wyłącznie przygotowuje **drafty** do ręcznej wysyłki. *(Zasada „Draft + akceptacja człowieka” dotyczy całej Platformy.)*
- **OOS-06:** W fazie POC — **jedno środowisko** (brak separacji dev/test/prod) i **brak automatycznego backupu**; świadomie zaakceptowane ryzyko fazy POC. US-20 i US-22 są wymaganiami **post-POC**, do spełnienia przed skalowaniem Platformy na kolejne moduły.

**W zakresie POC (jawnie potwierdzone):** obie ścieżki procesu — nowy produkt bez istniejącej receptury (brief produktowy → receptura) oraz zamówienie wg istniejącej receptury — są objęte POC.

---

## 6. Ryzyka i uwagi przekrojowe

*(Sekcja dodana ponad kanoniczny schemat — wymagana kontekstem regulowanego środowiska produkcji kosmetyków; nie duplikuje treści BRD/RULES, lecz sygnalizuje ryzyka wynikające wprost z Person i User Stories powyżej.)*

- **Wielodostawczość w recepturze** (US-05, US-06, US-07, US-11, US-14, US-15, US-16) koliduje z modelem danych WAPRO „jeden produkt = jeden dostawca” oraz z zasadą, że certyfikat wydawany jest na konkretnego dostawcę. To najważniejszy wątek funkcjonalno-regulacyjny całego PRD — wymaga decyzji jakościowej (nie tylko systemowej) przed wdrożeniem.
- **Zmiana dostawcy na zamówieniu** (US-14) musi bezwzględnie respektować listę klientów niezezwalających na zmianę dostawcy (US-15) — błąd tutaj ma bezpośredni skutek dla zgodności z wymaganiami klienta/certyfikacją produktu.
- **Rezygnacja z separacji środowisk i backupu w POC** (OOS-06) to świadomie zaakceptowane ryzyko utraty danych/pracy — musi zostać zamknięte (US-20, US-22) przed przejściem z POC do stałej eksploatacji lub przed objęciem Platformą kolejnych obszarów (materiały niewykorzystane, stany magazynowe, oferty/wyceny, planowanie produkcji).
- **Proces certyfikacji** pozostaje bramką jakościową przed zamówieniem produkcyjnym — żadna funkcja POC (w tym rotacja/zmiana dostawcy) nie powinna go pomijać ani przyspieszać automatycznie.
