<!--
opis: Serwer lokalny FEBO: zasoby, kopie, kontenery i dostępy dla środowisk Intranetu.
rodzaj: zalozenie
rozmowa: resources
zmiana: 16.10.2026, resources
status: roboczy
slowa: serwer,zasoby,kopie,kontenery
-->
# Serwer lokalny FEBO — zasoby, kopie zapasowe, kontenery i dostępy

**Dokument dla administratora zasobów** · projekt *intranet2.0* · **wersja 1.3** · 18.08.2026 · redAi

Wersja 1.3 uwzględnia decyzje z 18.08.2026: **wszystkie cztery maszyny powstają od razu**, środowisko dev
jest przygotowane jako miejsce docelowe dla kontenera przenoszonego z serwera redAi, na dev będzie dostęp
do **testowej instancji Comarch Optima**, a agent (Claude Code) ma pracować na dev tak samo jak dziś na
serwerze zewnętrznym, a **kopie zapasowe startują w zakresie minimalnym, obejmującym wyłącznie produkcję**.
Wersja 1.3 dokłada do podziału **maszynę hurtowni danych** (prototyp Small Big Steps) i przycina wszystkie
pozostałe przydziały, żeby zmieścić ją bez dokładania pamięci i dysku — szczegóły i skutki uboczne w osobnym
dokumencie *Podział zasobów z uwzględnieniem hurtowni danych*.


> ## ⚠ Aktualizacja 20.08.2026 — czytaj razem z dokumentem *Odpowiedzi dla administratora serwera*
>
> Po mailu administratora serwera z 20.08.2026 część ustaleń tego dokumentu jest **nieaktualna**.
> Obowiązujące wartości znajdują się w dokumencie **„Intranet FEBO — odpowiedzi dla administratora
> serwera i zaktualizowany podział zasobów" (wersja 1.0)**. Poniżej lista zmian, żeby nie czytać
> tego dokumentu w dobrej wierze w złych miejscach.
>
> | Co ten dokument mówi | Stan obowiązujący od 20.08.2026 |
> |---|---|
> | ~60 GB wolnej pamięci | **52,77 GB** |
> | Jedna pula dyskowa 691 GB | **Trzy pule**: SAS SSD RAID1 691 GB, SATA SSD RAID10 622 GB, HDD RAID5 2,08 TB |
> | Cztery maszyny, w tym dev i `febo-ops` | **Trzy maszyny: `febo-prod-app`, `febo-prod-db`, `febo-test`.** Dev **zostaje na serwerze redAi**, `febo-ops` **nie powstaje**, hurtownia **idzie na zewnętrzny VPS** |
> | PostgreSQL wewnątrz stosu produkcyjnego | **Osobna maszyna `febo-prod-db`, administrowana przez FEBO** |
> | Prod 20 GB / dwh 16 GB / test 8 / dev 8 / ops 4 / rezerwa 4 | **Prod 8 + 12 GB / test 8 GB — razem 28 GB. Hurtownia schodzi z serwera na zewnętrzny VPS** |
> | Okno nocne dla hurtowni jako warunek wiążący | **Wymóg nieaktualny** — hurtownia nie stoi już na dyskach FEBO |
> | Kopie do zbudowania od zera | **Dobowe kopie maszyn już działają** (21:00 / 23:30); dokładamy nocny zrzut logiczny bazy o 01:30 |
> | Kopia poza serwerem — do ustalenia | **Istniejące zaplecze kopii FEBO** |
> | Agent redAi pracuje na dev w biurze, warianty dostępu A/B/C | **Nieaktualne w całości** — dev zostaje w redAi, żadne wejście do sieci FEBO nie powstaje. Integrację z Optimą i LogTMS weryfikuje Mateusz na `febo-test` |
> | Sekcja 3.5 — przeniesienie dev do biura | **Odwołana** |
> | RPO / RTO nieokreślone | **RPO 24 h, RTO 8 h**, podnoszone wraz z aplikacjami o znaczeniu krytycznym |
>
> Rozdziały o warstwie kontenerów, wymaganiach twardych, zasadach sieciowych i dostępach do systemów
> źródłowych pozostają w mocy — zmieniły się liczby i rozkład maszyn, nie zasady.

---

## 0. Decyzje, na których stoi ten dokument

| # | Decyzja | Skutek |
|---|---|---|
| D1 | **Powstają od razu wszystkie cztery maszyny:** zaplecze, dev, test i produkcja. | Pełny podział zasobów z §1 obowiązuje od pierwszego dnia; nic nie czeka „w rezerwie”. |
| D2 | **Środowisko dev nie jest budowane od zera — przyjmuje kontener przenoszony z serwera redAi.** | Maszyna `febo-dev` powstaje jako miejsce docelowe o tym samym kształcie co dzisiejszy serwer; przeniesienie to osobna, zaplanowana operacja (§3.5). |
| D3 | **Test i produkcja to dwie osobne maszyny.** | Wymaga poprawki karty T-65 („jedna maszyna, osobne instancje” → dwie maszyny wirtualne na jednym gospodarzu). |
| D4 | **Na dev będzie dostęp do testowej instancji Comarch Optima.** | Znika blokada karty T-56 — warstwę odczytu można zbudować i sprawdzić na dev, bez dotykania danych produkcyjnych. Warunek: przeniesienie dev następuje **przed sprintem S4**. |
| D5 | **Agent pracuje na `febo-dev` tak jak dziś na serwerze zewnętrznym.** | Potrzebna droga sieciowa od redAi do maszyny w sieci firmowej oraz lista dozwolonych połączeń wychodzących (§5.1). To jedyna maszyna z dostępem agenta. |
| D6 | Zajętość pozostałych 68 GB pamięci gospodarza — do weryfikacji później. | Produkcja startuje z 24 GB; podniesienie do 32 GB (wariant zalecany w kontrakcie) pozostaje otwarte. |
| D7 | **Kopie zapasowe w zakresie minimalnym i wyłącznie dla produkcji**, dopóki system nie pracuje w większej skali. | Zakres z §2.1; rozbudowa po przekroczeniu progów z §2.4. |
| D8 | **Hurtownia danych staje na tym samym serwerze, bez dokładania pamięci i dysku.** | Wszystkie przydziały przycięte (§1). Produkcja schodzi do 20 GB i do 300 GB dysku — **dokładnie minimum z kontraktu**. Rezerwa na kopie zmniejszona ze 120 do 40 GB, co jest wykonalne wyłącznie dzięki decyzji D7. |
| D9 | **Ciężkie ładowanie i przeliczenia hurtowni wyłącznie w oknie nocnym**, dopóki nie zwiększymy zasobów. | Warunek wiążący, nie zalecenie: hurtownia i produkcja dzielą to samo lustro dysków i tylko rozdzielenie w czasie je rozdziela. Do uzgodnienia z zespołem hurtowni przed uruchomieniem. |

---

## 1. Podział zasobów

Budżet: **32 wątki procesora**, **60 GB wolnej pamięci**, **691 GB wolnej przestrzeni**.

### 1.1. Procesor i pamięć

| Maszyna | Rola | Procesor (gwarancja / szczyt) | Pamięć | Priorytet |
|---|---|---|---|---|
| **`febo-prod`** | produkcja Intranetu | 8 wątków / 12 | **20 GB** (po rozbudowie 32) | najwyższy |
| **`febo-dwh`** | hurtownia danych (zespół zewnętrzny) | 4 wątki / 6 | 16 GB | średni, ale **tylko w oknie nocnym** dla operacji ciężkich |
| **`febo-test`** | środowisko testowe | 3 wątki / 5 | 8 GB | średni |
| **`febo-dev`** | środowisko budowy (agent) | 3 wątki / 6 | 8 GB | niski |
| **`febo-ops`** | zaplecze: CI, rejestr obrazów, monitoring, kopie | 3 wątki / 6 | 4 GB | najniższy |
| Rezerwa gospodarza | — | reszta | 4 GB | — |

Razem **21 z 32 wątków w gwarancji** i pełne 60 GB wolnej pamięci — **co do gigabajta, bez zapasu**.
Szczyty mogą sięgać wyżej, bo maszyny nie pracują w tym samym rytmie: testy i praca ludzi w dzień,
ładowanie hurtowni i synchronizacje nocą.

**Zasady przydziału**

- Produkcja przypięta do **jednego gniazda procesora wraz z jego pamięcią** — sięganie po pamięć drugiego
  gniazda kosztuje i psuje powtarzalność czasów odpowiedzi.
- W razie kolizji rozstrzyga priorytet: **produkcja wygrywa zawsze**, zaplecze ustępuje wszystkim. Kompilacja
  i testy nie mogą spowalniać produkcji.
- **Hurtownia ma własne okno.** Ładowanie danych i przeliczenia analityczne wyłącznie nocą (decyzja D9).
  Limity procesora i pamięci nie rozdzielą hurtowni od produkcji, bo problemem jest wspólne lustro dysków,
  a nie rdzenie — rozdziela je tylko czas.
- `febo-dev` dostaje **8 GB**, czyli dokładnie minimum z karty T-00. Skaner plików pozostaje na dev wyłączony,
  tak jak dziś — na jego włączenie brakuje pamięci i wraca to dopiero po rozbudowie.
- **Do czasu uruchomienia produkcji (sprint S9) jej 20 GB jest wolne.** Można je pożyczyć środowisku testowemu
  na odbiory i na pomiar wydolności z karty T-70, z obowiązkiem oddania przed przekazaniem produkcji ludziom.
- **Pomiar wydolności z karty T-70 musi odbyć się na pustej produkcji**, nie na teście. Test przy 8 GB nigdy
  nie odwzoruje produkcji, więc wynik z testu byłby nic nie znaczący.
- **Rezerwa gospodarza 4 GB jest cienka.** Użycie pliku wymiany i zabicie procesu z braku pamięci to alarmy
  o najwyższym priorytecie, a nie zdarzenia do przejrzenia następnego dnia.
- **Ponowne uruchomienie archiwalnego systemu TMS unieważnia ten podział.** Wolne 60 GB istnieje dlatego,
  że tamten system jest wyłączony. Jego powrót wymaga dołożenia pamięci, a nie przesunięć w tabeli.

### 1.2. Przestrzeń dyskowa

| Przeznaczenie | Przydział | Zawartość |
|---|---|---|
| `febo-prod` | **300 GB** | baza 130 GB · wolumen plików 140 GB · dzienniki 30 GB. To **minimum z kontraktu**, bez zapasu |
| `febo-dwh` | 170 GB | system, PostgreSQL hurtowni, Airbyte, Metabase, aplikacja wyjątków (wniosek mówił o 200+ GB — patrz dokument o hurtowni) |
| `febo-test` | 60 GB | dane testowe; na profil obciążeniowy trzeba pożyczyć z rezerwy |
| `febo-dev` | 50 GB | dokładnie minimum z karty T-00 |
| Kopie zapasowe (lokalne) | 40 GB | zakres minimalny z §2 — 7 dobowych zrzutów produkcji plus wolumen plików |
| `febo-ops` | 25 GB | rejestr obrazów z krótszą retencją, dane monitoringu, przestrzeń robocza CI |
| Rezerwa niezaalokowana | 46 GB | **poniżej 7% — to próg alarmowy, nie zapas.** Pierwsze powiększenie wolumenu plików produkcji zje jej połowę |

**Wymagania techniczne wobec dysku**

- Każdy wolumen jako **osobny wolumen logiczny** (baza, pliki, dzienniki, kopie). Zapełnienie jednego nie może
  zatrzymać pozostałych — zapchane dzienniki nie mogą zatrzymać bazy.
- Kopie zapasowe **poza wolumenami maszyn** i najlepiej poza tą samą grupą wolumenów.
- Alarm przy **85% zapełnienia** każdego wolumenu, akcja naprawcza: powiększenie z rezerwy (karta T-67).
- Wolumen plików rośnie stale — dokumenty mają **pięcioletni okres przechowywania** (karta T-66). To jedyna
  pozycja, która nigdy nie maleje.

---

## 2. Kopie zapasowe — zakres minimalny na start

**Decyzja D7: dopóki system nie pracuje w większej skali, kopie zapasowe obejmują wyłącznie środowisko
produkcyjne i mają zakres minimalny.** Rozbudowa następuje później, po przekroczeniu progów z §2.4.

Powód jest praktyczny: pełny reżim kopii z karty T-66 kosztuje miejsce, czas administratora i comiesięczne
próby odtworzenia, a chroni dane, których jeszcze nie ma. Dev i test odtwarza się z repozytorium i z generatora
danych testowych — nie ma tam nic, czego nie da się wytworzyć ponownie w kilkanaście minut.

### 2.1. Zakres minimalny

| Co | Częstotliwość | Retencja lokalna | Poza serwerem | Szyfrowanie |
|---|---|---|---|---|
| **Baza produkcji — pełny zrzut** | codziennie 01:30 | 7 dni | ostatnia kopia, codziennie | **tak** |
| **Wolumen plików produkcji** | codziennie 02:00, razem ze zrzutem | 7 dni | ostatnia kopia, codziennie | **tak**, gdy pojawią się dokumenty *osobowe* lub *poufne* |
| **Migawka maszyny produkcyjnej** | **przed każdym wdrożeniem** | 2 sztuki | nie | — |
| **Odtworzenie próbne** | **jednorazowo po uruchomieniu produkcji**, potem po każdej zmianie sposobu kopii | raport zachowany | — | — |

To wszystko. Jedno zadanie cykliczne, jeden zbiór, jeden klucz szyfrujący.

### 2.2. Czego świadomie nie robimy na starcie

| Pominięte | Konsekwencja, którą przyjmujemy | Kiedy wraca |
|---|---|---|
| Archiwum dziennika transakcji (odtworzenie do minuty) | przy awarii tracimy pracę wykonaną **od 01:30 danego dnia** | próg P1 lub P3 z §2.4 |
| Kopie środowisk dev i test | odtwarzamy je z repozytorium i generatora danych — kilkanaście minut pracy | nie planujemy w ogóle |
| Kopie tygodniowe, miesięczne i archiwa wieloletnie | brak możliwości sięgnięcia po stan sprzed tygodnia | próg P2 |
| Migawki cykliczne maszyn | zostaje migawka przedwdrożeniowa, która pokrywa najczęstszy przypadek | próg P1 |
| Comiesięczne próby odtworzenia | pewność odtworzenia potwierdzona raz, nie utrzymywana w czasie | próg P1 |

### 2.3. Trzy rzeczy, które obowiązują od pierwszego dnia mimo minimalnego zakresu

1. **Kopia musi opuszczać serwer.** Lustro RAID-1 chroni przed awarią dysku, nie przed skasowaniem danych,
   oprogramowaniem szyfrującym ani pożarem. Jedna kopia dobowa poza serwerem to koszt kilku gigabajtów.
2. **Szyfrowanie dokumentów osobowych i poufnych nie jest opcją.** W chwili, gdy pierwszy taki dokument trafi
   do systemu, kopia bez szyfrowania przestaje być dopuszczalna — to wymóg kontraktowy, nie kwestia skali.
3. **Sekrety nie jadą w kopii.** Hasła, klucze i tokeny żyją w zmiennych środowiskowych, a ich egzemplarz
   zapasowy trzyma człowiek w menedżerze haseł (niezmiennik 25).

### 2.4. Progi rozbudowy — kiedy wracamy do pełnego reżimu

Kopie rozbudowujemy, gdy zajdzie **którykolwiek** z poniższych warunków. Przegląd przy zamknięciu każdego sprintu.

| Próg | Warunek | Co dochodzi |
|---|---|---|
| **P1** | pierwsza fala użytkowników po sprincie S5 — pięć aplikacji w codziennej pracy | archiwum dziennika transakcji, kopie tygodniowe, comiesięczne odtworzenie próbne |
| **P2** | wolumen plików przekracza 20 GB albo pojawiają się dokumenty o pięcioletnim okresie przechowywania | kopie przyrostowe co 6 h, kopia tygodniowa, retencja miesięczna i archiwa JSONL |
| **P3** | pierwsza aplikacja, której odtworzenie danych z poprzedniego dnia byłoby dla firmy realną stratą (np. odprawy celne) | pełny harmonogram z karty T-66 bez skracania |

Do czasu przekroczenia progów **rezerwa 120 GB na kopie pozostaje wolna** — nie przydzielamy jej innym
maszynom, żeby rozbudowa nie wymagała przebudowy podziału dysku.

---

## 3. Rozwiązanie kontenerowe — co administrator ma przygotować

### 3.1. Warstwa maszyn wirtualnych

| Element | Wymaganie |
|---|---|
| Hipernadzorca | pełna wirtualizacja (KVM/Proxmox/ESXi). **Kontener systemowy zamiast maszyny wirtualnej jest dopuszczalny tylko dla `febo-ops`**; dla `febo-dev` i `febo-test` wymaga zagnieżdżenia, bo w środku pracuje Docker; produkcja: zawsze pełna maszyna wirtualna. |
| System | Ubuntu Server **24.04 LTS**, instalacja minimalna — dotyczy maszyn Intranetu. Maszyna hurtowni pracuje na **Rocky Linux** i jest administrowana przez zespół zewnętrzny; ten dokument opisuje wyłącznie jej przydział zasobów i granice sieciowe. Wersja systemu nie wpływa na aplikację — interpreter jest przypięty w obrazie kontenera — więc wybieramy wydanie z długim wsparciem, nie najnowsze. |
| Strefa czasu i zegar | `Europe/Warsaw`, synchronizacja czasu włączona. Rozjazd zegara psuje dziennik audytu i pomiary. |
| Plik wymiany | 4 GB na każdej maszynie, ale **jako bufor, nie jako pamięć** — zdarzenie użycia wymiany jest alarmem monitoringu. |
| Aktualizacje | automatyczne aktualizacje bezpieczeństwa włączone; restart maszyny w oknie serwisowym, nigdy automatycznie na produkcji. |
| Konta | konta imienne, logowanie wyłącznie kluczem, hasła wyłączone, `sudo` zapisywane w dzienniku. **Bez kont współdzielonych.** |

### 3.2. Warstwa kontenerów — na każdej maszynie środowiskowej

Skład złożenia jest identyczny na dev, test i produkcji; różnią się wyłącznie zmienne środowiskowe i limity
(wymóg karty T-65 — **ten sam obraz**, inna konfiguracja).

| Usługa | Rola | Uwagi wdrożeniowe |
|---|---|---|
| brama systemowa (poza kontenerami) | zakończenie TLS, certyfikat | pracuje na gospodarzu, przekazuje ruch na adres lokalny — certyfikat przeżywa wtedy każdą przebudowę kontenerów |
| brama wewnętrzna | ruch do aplikacji i do plików interfejsu | jedyna usługa nasłuchująca, wyłącznie na adresie lokalnym |
| aplikacja (API) | serwer aplikacyjny | limit 1,6 GB; bezstanowa — cały stan w bazie i w wolumenie plików |
| proces roboczy kolejki | wykonywanie zleceń | limit 1,5 GB, zbieżność 4; limit wyznacza generowanie dokumentów |
| harmonogram | wyzwalanie zadań cyklicznych | zbieżność 1, 128–512 MB |
| PostgreSQL 16 | baza i **kolejka zadań** | osobna instancja per środowisko, osobny wolumen; kolejka pracuje w bazie — nie instalujemy żadnego brokera |
| Redis | pamięć podręczna i sesje | osobna instancja per środowisko, twardy limit pamięci; **nigdy jako kolejka** |
| skaner plików | kontrola antywirusowa wgrywanych plików | zdefiniowany, ale **wyłączony przełącznikiem**; na dev przy 10 GB pamięci można go wreszcie włączyć do sprawdzenia |
| mierniki | dane dla monitoringu | format Prometheus (karta T-67) |
| makiety systemów źródłowych | wyłącznie na `febo-dev` | odpowiedniki LogTMS (MySQL) i Optima (MS SQL) z kontem tylko do odczytu — istnieją i działają; **zostają nawet po podłączeniu testowej Optimy**, bo testy muszą działać bez sieci |

**Wymagania twarde**

1. **Baza i pamięć podręczna nie publikują portów poza sieć kontenerów.** Żadnego wystawienia na adres maszyny
   (karta T-71). Silnik kontenerów potrafi sam dopisać reguły do zapory gospodarza z pominięciem jej konfiguracji —
   dlatego publikowanie portów jest zakazane, a nie tylko odradzane.
2. **Kontrola stanu (healthcheck) dla każdej usługi.** Wdrożenie kończy się dopiero, gdy wszystkie są zdrowe.
3. **Migracje bazy jako krok poprzedzający start nowej wersji**, nie jako część startu aplikacji.
4. **Limity procesora i pamięci na każdym kontenerze** — brak limitu oznacza, że jedna usługa może zabrać maszynę.
5. **Wolumeny nazwane**: baza, pamięć podręczna (z zapisem trwałym), pliki. Wolumen plików objęty kopią z §2.
6. **Rejestr obrazów** na `febo-ops` albo rejestr dostawcy repozytorium — z uwierzytelnianiem i TLS. Obraz budowany
   **raz** przez potok CI, oznaczany wersją i tylko przenoszony między środowiskami. Wycofanie zmiany = wskazanie
   poprzedniej wersji obrazu.
7. **Agent nie ma konta na `febo-test` ani `febo-prod`** — ani konta, ani klucza, ani dostępu do rejestru z prawem
   zapisu. Niezmiennik 37 ma być faktem konfiguracyjnym, nie deklaracją.

### 3.3. Maszyna zaplecza `febo-ops`

| Element | Zakres |
|---|---|
| Wykonawca potoku CI | podpięty do repozytorium projektu; buduje, testuje i publikuje obraz. **Bez dostępu do produkcji.** |
| Rejestr obrazów | prywatny, z uwierzytelnianiem; retencja: 20 ostatnich wersji + wszystkie wersje wdrożone na produkcję |
| Monitoring | zbieranie mierników ze wszystkich środowisk, reguły alarmowe z karty T-67: czas odpowiedzi (95. centyl), czas oczekiwania w kolejce, wykorzystanie połączeń do bazy, zapełnienie wolumenów i bazy, pamięć i plik wymiany, zdarzenia zabicia procesu z braku pamięci, szczyty procesora. **Każdy alarm z opisaną akcją naprawczą.** |
| Kopie zapasowe | zadania z §2, klucze szyfrujące, wysyłka poza serwer, comiesięczne próbne odtworzenie |
| Serwer czasu / pośrednik pakietów | opcjonalnie, jeśli maszyny nie mają wyjścia na zewnątrz |

### 3.4. Kolejność uruchamiania

Wszystkie maszyny powstają w jednym podejściu, ale w tej kolejności — każda następna korzysta z poprzedniej.

1. **`febo-ops`** — potok CI, rejestr obrazów, monitoring, kopie. Bez tego nie ma czym wdrażać ani czym mierzyć.
2. **`febo-dev`** — miejsce docelowe dla przenoszonego kontenera (§3.5) wraz z dostępem agenta (§5.1)
   i testową Optimą (§5.3).
3. **`febo-test`** — musi być gotowa przed sprintem S5, kiedy pięć aplikacji trafia do ludzi.
4. **`febo-prod`** — konfiguracja i uruchomienie w sprincie S9, karty T-65 … T-71. **Wdrożenie wykonuje człowiek.**
   Maszyna powinna stać gotowa wcześniej — pusta produkcja jest jedynym miejscem, gdzie da się rzetelnie wykonać
   pomiar wydolności z karty T-70.
5. **`febo-dwh`** — niezależnie od powyższych, w terminie zespołu hurtowni. Wymaga wyłącznie przydziału zasobów,
   segmentu sieci i konta z uprawnieniami administracyjnymi; instalację wykonuje zespół zewnętrzny.

### 3.5. Przeniesienie środowiska dev z serwera redAi

Dzisiejsze dev to maszyna z publicznym adresem, z pełnym środowiskiem budowy: Docker z złożeniem siedmiu usług,
narzędzia, kopia robocza repozytorium, makiety systemów źródłowych i kontekst pracy agenta.

**Dwa warianty przeniesienia**

| Wariant | Na czym polega | Ocena |
|---|---|---|
| **A — odtworzenie z repozytorium** *(rekomendowany)* | nowa maszyna stawiana od zera zgodnie z kartą T-00, potem pobranie repozytorium i uruchomienie złożenia; przenosimy wyłącznie stan roboczy: gałęzie w toku, konfigurację lokalną, kontekst agenta | procedura jest w pełni udokumentowana i już raz wykonana; kończy się środowiskiem bez śladów po maszynie, która stała w publicznym internecie; **dane odtwarza generator z ustalonym ziarnem — te same dane co do bitu** |
| **B — przeniesienie obrazu maszyny** | eksport całej maszyny z serwera redAi i import na gospodarza | szybsze o kilka godzin, ale przenosi także wszystko, czego nie chcemy: konfigurację pod publiczny adres, stare klucze, historię wystawienia na świat. Do rozważenia tylko przy presji czasu |

**Lista kontrolna przeniesienia (wariant A)**

- [ ] repozytorium wypchnięte w całości, żadna gałąź w toku nie została tylko lokalnie
- [ ] migawka `febo-dev` **przed** przeniesieniem — jednorazowa, poza reżimem kopii z §2
- [ ] środowisko budowy stawiane zgodnie z kartą T-00: silnik kontenerów, narzędzia budowy, biblioteki
- [ ] złożenie kontenerów wstaje w całości — wszystkie usługi zdrowe, `/healthz` odpowiada
- [ ] makiety LogTMS i Optima uruchomione, konto tylko do odczytu działa (test niezależności warstw z karty T-05b)
- [ ] **`make check` i `make review` przechodzą na zielono** — to jest kryterium odbioru, nie „strona się otwiera”
- [ ] kontekst pracy agenta i konfiguracja lokalna przeniesione, dostęp z §5.1 sprawdzony końcem-do-końca
- [ ] podłączona testowa Optima, odczyt sprawdzony (§5.3)
- [ ] **sekrety wygenerowane od nowa**, nie skopiowane ze starej maszyny
- [ ] stary serwer **wyłączony dopiero po odbiorze** i po zachowaniu jego migawki na 30 dni
- [ ] wpis w dokumentacji projektu: zmiana środowiska dev, nowy powód niezmiennika 37b, zdjęcie blokady Optimy

**Termin:** przeniesienie powinno nastąpić **przed sprintem S4** (systemy źródłowe). Wtedy testowa Optima jest
dostępna dokładnie wtedy, gdy zaczyna być potrzebna, i nie trzeba budować warstwy odczytu w ciemno.

---

## 4. Sieć

| Zasada | Realizacja |
|---|---|
| Każde środowisko w osobnym segmencie sieci | osobna sieć wirtualna / VLAN dla `prod`, `test`, `dev`, `ops` oraz **osobny segment dla `febo-dwh`** |
| Ruch między środowiskami zablokowany | dev **nie ma** trasy do bazy testowej ani produkcyjnej, test nie ma trasy do produkcji. To warunek niezmiennika 26, a nie kwestia dobrych intencji |
| Hurtownia odcięta od Intranetu w obie strony | `febo-dwh` nie ma trasy do żadnej bazy Intranetu i odwrotnie. Obie strony czytają z LogTMS niezależnie. Baza hurtowni jest **celowo** dostępna z sieci firmowej — to odwrotność reguły obowiązującej Intranet i nie wolno tych dwóch konfiguracji pomieszać |
| Dostęp użytkowników | port 443 z sieci biura do `febo-prod`; test wyłącznie z sieci biura, z zawężeniem do zespołu wdrożeniowego; dev niedostępny dla użytkowników |
| Dostęp administracyjny | SSH wyłącznie z podsieci administracyjnej lub przez punkt pośredni, klucze, bez haseł |
| Dostęp agenta | wyłącznie do `febo-dev`, drogą opisaną w §5.1 |
| Ruch wychodzący | domyślnie zamknięty, otwierany **listą dozwolonych celów** (§5) |
| Nazwy i certyfikaty | nazwa w sieci firmowej dla każdego środowiska; certyfikat z wewnętrznego urzędu certyfikacji albo publiczny — do rozstrzygnięcia z dostawcą |
| **Zaporę, adresację i certyfikaty konfiguruje człowiek** | agent nie dotyka sieci, zapory ani usług systemowych na żadnym środowisku (niezmiennik 37b) |

---

## 5. Dostępy do skonfigurowania

Zasada nadrzędna: **osobne poświadczenia dla każdego środowiska**, konta wyłącznie do odczytu tam, gdzie czytamy,
prawo odczytu nadane **po stronie bazy**, a nie zapewniane w kodzie. Żadne poświadczenie nie trafia do repozytorium
ani do katalogu roboczego agenta (niezmienniki 25 i 26).

### 5.1. Dostęp redAi — praca agenta na `febo-dev`

Dziś działa to trywialnie, bo serwer dev ma publiczny adres. **Po przeniesieniu maszyna stoi w sieci firmowej,
więc trzeba świadomie otworzyć jedną drogę — i tylko jedną.** To jest jedyny nowy dostęp „do środka”, jaki ten
projekt wymaga.

**Wariant drogi sieciowej — do wyboru przez FEBO i dostawcę**

| Wariant | Na czym polega | Ocena |
|---|---|---|
| **A — tunel wychodzący z `febo-dev`** *(rekomendowany)* | maszyna sama nawiązuje trwałe połączenie do punktu redAi; **na zaporze FEBO nie otwiera się żaden port przychodzący** | najmniej ingerencji w sieć firmową, najłatwiejszy do zaakceptowania i do wycofania — wyłączenie usługi zamyka dostęp natychmiast |
| **B — tunel VPN między redAi a FEBO** | stałe połączenie sieciowe, zawężone wyłącznie do adresu `febo-dev` | rozwiązanie czyste i wygodne, ale to zmiana w infrastrukturze sieciowej firmy |
| **C — SSH wystawiony z listą dozwolonych adresów** | port administracyjny otwarty wyłącznie dla adresu redAi | najprostszy, ale otwiera port przychodzący na styku sieci — najmniej chętnie rekomendowany |

**Konto agenta na `febo-dev`** — odwzorowanie tego, co działa dziś:

- konto systemowe z logowaniem **wyłącznie kluczem**, hasło wyłączone
- członkostwo w grupie silnika kontenerów (praca ze złożeniem bez podnoszenia uprawnień)
- `sudo` w zakresie potrzebnym do instalowania pakietów i zarządzania złożeniem — **zapisywane w dzienniku**
- katalog projektu jako jedyne miejsce zapisu; kontrola odmawiająca zapisu poza nim jest już wdrożona
- **żadnego odpowiednika tego konta na `febo-test` i `febo-prod`**

**Połączenia wychodzące, bez których agent nie pracuje** (lista dozwolonych celów dla `febo-dev`):

| Cel | Do czego |
|---|---|
| usługa modelu językowego (redAi / Anthropic) | działanie samego agenta — **bez tego nie ruszy nic** |
| repozytorium projektu (GitHub) | pobieranie i wypychanie zmian, potok CI |
| źródła pakietów systemu, Pythona, Node i obrazów kontenerów | budowa środowiska i obrazów |
| testowa instancja Comarch Optima | §5.3 |
| LogTMS | §5.2 |
| rejestr obrazów na `febo-ops` | pobieranie i publikowanie obrazów |

Jeśli maszyny mają nie wychodzić do internetu bezpośrednio, wszystkie powyższe da się przeprowadzić przez
pośrednika pakietów na `febo-ops` — poza usługą modelu językowego, która musi mieć drogę wychodzącą.

**Pozostałe dostępy redAi (ludzie, nie agent)**

| Co | Zakres |
|---|---|
| Kanał serwisowy do `febo-test` i `febo-prod` | konta **imienne** dla osób prowadzących wdrożenie, klucze, dostęp z podsieci administracyjnej lub przez punkt pośredni, `sudo` w dzienniku |
| Rejestr obrazów | zapis dla potoku CI, odczyt dla maszyn środowiskowych — dwa różne poświadczenia |
| Repozytorium projektu | klucz wdrożeniowy dla `febo-ops` (odczyt), klucz z prawem zapisu wyłącznie na `febo-dev` |
| Poczta wychodząca | konto SMTP dla powiadomień, osobne dla test i produkcji, nadawca uzgodniony z FEBO |

### 5.2. LogTMS

| Co | Zakres |
|---|---|
| Konto bazodanowe **wyłącznie do odczytu** | osobne dla dev, test i produkcji; uprawnienie `SELECT` nadane na bazie |
| Droga sieciowa | zezwolenie na ruch wychodzący z `febo-dev`, `febo-test` i `febo-prod` do serwera LogTMS (MySQL) |
| Kopia do odczytu | rekomendacja z listy blokad projektu: **replika tylko do odczytu** — odpytywanie bazy roboczej przy skali pilotażu obciąża system, z którego zespół korzysta na co dzień. Do ustalenia z twórcą LogTMS |
| Stan | wniosek o konto deweloperskie jest **przygotowany** i czeka na wysłanie (`docs/external/wnioski/konto-dev-logtms.md`) |

### 5.3. Comarch Optima — instancja testowa na dev

Decyzja D4 zdejmuje blokadę, która ciągnęła się od przeprowadzki dev na serwer zewnętrzny.

| Co | Zakres |
|---|---|
| Konto bazodanowe **wyłącznie do odczytu** na instancji testowej | uprawnienie `SELECT` nadane na bazie MS SQL, osobne poświadczenia niż dla test i produkcji |
| Droga sieciowa | zezwolenie na ruch z `febo-dev` do serwera testowej Optimy (port MS SQL), wyłącznie w tę stronę |
| Konta produkcyjne | osobne konta odczytowe dla `febo-test` i `febo-prod` do **produkcyjnej** Optimy — konfigurowane dopiero przy tych środowiskach |
| Zasada, która nie znika | dev **nie przechowuje** danych produkcyjnych. Instancja testowa upraszcza sprawę, ale nie zwalnia z reguły: odczyt na żądanie tak, zapis wyniku do repozytorium czy do danych testowych nie (niezmiennik 26) |
| Co dalej trzeba zmierzyć | zachowanie pod obciążeniem i czasy odpowiedzi sprawdzamy na `febo-test` wobec instancji produkcyjnej (karta T-69) — instancja testowa nadaje się do zbudowania warstwy odczytu, nie do ustalania limitów czasu |
| Stan | wniosek o konto jest **przygotowany** (`docs/external/wnioski/konto-dev-optima.md`) — wymaga przeredagowania pod instancję testową |

### 5.4. Pozostałe usługi zewnętrzne

| Usługa | Do czego | Co przygotować |
|---|---|---|
| DocuPipe | odczyt dokumentów w module celnym (karta T-62) | klucz API od dostawcy, osobny dla każdego środowiska, limit zapytań |
| ISZTAR / TARIC | klasyfikacja taryfowa (karta T-64) | zezwolenie na ruch wychodzący, bez uwierzytelniania; wyniki buforowane |
| Lista sankcyjna (OpenSanctions) | rejestr sankcji (karta T-60) | zezwolenie na ruch wychodzący; **144 MB danych** — miejsce przechowywania jest osobną decyzją z listy blokad |
| Poczta wychodząca (SMTP) | powiadomienia i zestawienia | konto techniczne, uwierzytelnianie, nadawca; osobne dla każdego środowiska. Na dev poczta idzie do atrapy, nie do ludzi |
| Dostawca osadzeń (wyszukiwanie znaczeniowe) | **dopiero po czterech warunkach** z kontraktu wyszukiwarki | nie konfigurować teraz — wymaga wyboru dostawcy i umowy powierzenia danych |
| Katalog firmowy / logowanie jednym kontem | opcjonalnie, jeśli FEBO ma katalog użytkowników | do rozstrzygnięcia — dziś projekt zakłada konta własne z drugim składnikiem dla roli administratora technicznego |

### 5.5. Hurtownia danych (zespół zewnętrzny)

| Co | Zakres |
|---|---|
| Konto administracyjne | konto imienne zespołu hurtowni z `sudo` **na maszynie `febo-dwh` i nigdzie indziej**; logowanie kluczem, `sudo` zapisywane w dzienniku |
| Ruch wychodzący | baza TMS, API zewnętrzne (GPS, geokodowanie), źródła pakietów i obrazów — lista dozwolonych celów jak dla pozostałych maszyn |
| Ruch przychodzący | PostgreSQL hurtowni z wybranych podsieci firmowych (analityka), aplikacje web i BI za serwerem proxy |
| Czego nie ma | żadnej trasy do maszyn Intranetu, żadnego konta na `febo-prod`, `febo-test`, `febo-dev` ani `febo-ops` |
| Okno pracy | ciężkie ładowanie i przeliczenia **wyłącznie nocą** (decyzja D9), dopóki zasoby nie zostaną zwiększone |
| Kopie | propozycja lżejsza niż dobowa migawka maszyny — opisana w dokumencie o hurtowni; do uzgodnienia z zespołem |

### 5.6. Reguła dla wszystkich powyższych

- Ruch wychodzący ze wszystkich maszyn domyślnie **zamknięty**, otwierany wyłącznie do wymienionych celów.
- Każdy dostęp ma **właściciela po stronie FEBO** i datę przeglądu — konto odczytowe bez właściciela po roku
  jest kontem, o którym nikt nie pamięta.
- **Poświadczenia produkcyjne generuje i przechowuje człowiek.** Agent ich nie tworzy, nie przechowuje i nie widzi.

---

## 6. Lista kontrolna dla administratora

**Maszyny**
- [ ] `febo-ops` — 3 wątki / 4 GB / 25 GB
- [ ] `febo-dev` — 3 wątki / 8 GB / 50 GB
- [ ] `febo-test` — 3 wątki / 8 GB / 60 GB
- [ ] `febo-prod` — 8 wątków / 20 GB / 300 GB, przypięta do jednego gniazda procesora
- [ ] `febo-dwh` — 4 wątki / 16 GB / 170 GB, Rocky Linux, własny segment sieci
- [ ] osobne wolumeny logiczne: baza, pliki, dzienniki, kopie
- [ ] alarm zapełnienia przy 85% na każdym wolumenie
- [ ] alarm użycia pliku wymiany i zabicia procesu z braku pamięci — **priorytet najwyższy**, rezerwa gospodarza to tylko 4 GB
- [ ] potwierdzone u administratora serwera: co zajmuje pozostałe 68 GB pamięci i czy archiwalny TMS pozostaje wyłączony

**Kontenery**
- [ ] silnik kontenerów z narzędziem złożenia na wszystkich czterech maszynach
- [ ] brama TLS na gospodarzu, przekazanie ruchu na adres lokalny
- [ ] zakaz publikowania portów bazy i pamięci podręcznej — zweryfikowany po uruchomieniu
- [ ] rejestr obrazów z uwierzytelnianiem, dwa poświadczenia (zapis dla CI, odczyt dla środowisk)
- [ ] wykonawca potoku CI na `febo-ops`, bez dostępu do produkcji

**Przeniesienie dev**
- [ ] wybrany wariant przeniesienia (rekomendacja: odtworzenie z repozytorium)
- [ ] lista kontrolna z §3.5 wykonana w całości
- [ ] kryterium odbioru spełnione: złożenie zdrowe **i** komplet kontroli jakości na zielono
- [ ] stary serwer wyłączony po odbiorze, migawka zachowana na 30 dni

**Kopie — zakres minimalny (dopiero przy uruchomieniu produkcji)**
- [ ] jedno zadanie cykliczne: dobowy zrzut bazy produkcji + wolumen plików
- [ ] klucz szyfrujący utworzony i zdeponowany poza serwerem
- [ ] miejsce poza serwerem uzgodnione, kopia dobowa tam dociera
- [ ] migawka produkcji przed każdym wdrożeniem
- [ ] jednorazowe odtworzenie próbne wykonane, raport zachowany
- [ ] rezerwa 120 GB na kopie **pozostaje wolna** — nie przydzielona innym maszynom
- [ ] progi rozbudowy z §2.4 wpisane do przeglądu na koniec sprintu

**Sieć i dostępy**
- [ ] segmenty sieci rozdzielone, ruch między środowiskami zablokowany i **sprawdzony próbą połączenia**
- [ ] droga dostępu agenta do `febo-dev` uruchomiona i sprawdzona końcem-do-końca
- [ ] lista dozwolonych połączeń wychodzących dla `febo-dev` (§5.1) skonfigurowana
- [ ] konto odczytowe do **testowej** Optimy działa z `febo-dev`
- [ ] konta odczytowe do LogTMS, osobne dla każdego środowiska
- [ ] konta imienne dla wdrożeniowców, bez kont współdzielonych, logowanie kluczem
- [ ] konta SMTP: atrapa na dev, konta techniczne na test i produkcji
- [ ] potwierdzone: **agent nie ma konta na test ani produkcji**

**Monitoring**
- [ ] mierniki zbierane ze wszystkich środowisk
- [ ] alarmy z karty T-67 skonfigurowane, każdy z akcją naprawczą
- [ ] powiadomienia alarmowe trafiają do wskazanej osoby, nie do wspólnej skrzynki

---

## 7. Co trzeba dopisać do założeń projektu

Obowiązuje zasada: ustalenie, którego nie ma w repozytorium, dla agenta nie istnieje.

| Dokument | Zmiana |
|---|---|
| Plan wdrożenia, etap 11 | wpisanie specyfikacji maszyn — zamyka blokadę „specyfikacja maszyny produkcyjnej” |
| Karta T-65 | „jedna maszyna, osobne instancje” → **dwie osobne maszyny wirtualne** na jednym gospodarzu |
| Karta T-66 | **wprowadzenie etapowości**: zakres minimalny na start (tylko produkcja, dobowy zrzut, jedna kopia poza serwerem), pełny harmonogram po przekroczeniu progów P1–P3 z §2.4. Karta w obecnym brzmieniu wymaga od razu pełnego reżimu — to jest rozjazd z decyzją D7 i musi zostać zapisany, a nie przemilczany |
| Rdzeń założeń §1 i niezmiennik 37b, uzasadnienie środowiska dev | przeniesienie dev do sieci firmowej: nowy powód niezmiennika (maszyna nie stoi już w internecie, ale sąsiaduje z systemami firmy), opis drogi dostępu agenta |
| Lista blokad projektu | zdjęcie blokady „Optima z dev nieosiągalna”; wniosek o konto przeredagowany pod instancję testową |
| Karty T-54, T-55, T-56 | doprecyzowanie: warstwa odczytu budowana wobec instancji testowej, pomiary czasów i limitów wobec produkcyjnej, z `febo-test` |
| Kolejność sprintów | przeniesienie dev **przed sprintem S4** jako warunek, nie jako plan |
| Plan wdrożenia, etap 11 (drugi wpis) | zapisać, że produkcja startuje w **wariancie minimalnym** kontraktu (20 GB, 300 GB), a nie zalecanym, oraz warunek powrotu do wariantu zalecanego: dołożenie pamięci |
| Karta T-70 | pomiar wydolności wykonywany **na pustej produkcji**, nie na środowisku testowym — test przy 8 GB nie odwzorowuje produkcji |

---

*Dokument przygotowany dla FEBO LOGISTIC Sp. z o.o. · redAi · 18.08.2026*
