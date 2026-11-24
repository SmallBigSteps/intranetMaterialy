<!--
opis: Odpowiedzi na pytania administratora serwera FEBO o wymagania Intranetu.
rodzaj: zalozenie
rozmowa: resources
zmiana: 16.10.2026, resources
status: roboczy
slowa: serwer,administrator,wymagania
-->
# Intranet FEBO — odpowiedzi dla administratora serwera i zaktualizowany podział zasobów

**Wersja 1.1 · 20.08.2026 · redAi**
Dokument odpowiada punkt po punkcie na pytania z maila administratora i zastępuje wcześniejsze
ustalenia dotyczące podziału pamięci, dysku, kopii zapasowych i maszyny zaplecza.

Zmiany w wersji 1.1 wobec 1.0, po odpowiedziach zwrotnych: **hurtownia danych schodzi z serwera**
(prototyp na zewnętrznym VPS — patrz rozdział 6, tam czekają dwa pytania); **przydziały dyskowe
startują mniejsze i rosną w miarę potrzeb**, skoro przestój przy powiększeniu jest planowany;
**archiwalny TMS przestaje być stałym obciążeniem planu**.

---

## 0. Co zmienił mail administratora

| Ustalenie wcześniejsze | Stan faktyczny | Skutek |
|---|---|---|
| ~60 GB wolnej pamięci | **52,77 GB**, z czego części nie można przydzielić | Rezygnujemy z maszyny zaplecza; hurtownia schodzi z serwera; przydzielamy **28 GB** |
| Jedna pula dyskowa 691 GB | **Trzy pule**: SAS SSD RAID1 691 GB, SATA SSD RAID10 622 GB, HDD RAID5 2,08 TB | Dysk przestaje być ograniczeniem; znika wymóg okna nocnego dla ciężkich operacji |
| Powiększenie dysku ryzykowne | **Przestój planowany, bez znaczenia** | Startujemy z mniejszymi przydziałami i rośniemy realnym tempem |
| Archiwalny TMS mógł wrócić w każdej chwili | **Włączany wyłącznie przy sprawach karnych, z wyprzedzeniem** | Nie trzymamy stałego bufora; przy planowanym włączeniu na czas jego pracy schodzimy z pamięcią środowiska testowego |
| Kopie do zbudowania od zera | **Dobowe kopie maszyn już działają** (21:00 / 23:30) | Nasz zakres schodzi do jednego spójnego zrzutu bazy i kopii poza serwerem |
| Dev wraca do biura | **Dev zostaje na serwerze redAi** | W biurze nie powstaje maszyna dev |
| Maszyna zaplecza `febo-ops` | **Nie powstaje** | Monitoring wchodzi później; skład wersji ląduje na maszynie testowej |

---

## 1. Odpowiedzi na pytania

### 1.1. Model wdrożenia — VM czy LXC, co z Kubernetes

**Maszyny wirtualne, wewnątrz nich Docker z Docker Compose.** Kubernetes odpada, potwierdzamy —
projekt nigdy go nie zakładał.

Prosimy o VM, nie LXC, dla maszyn produkcyjnych. Powód jest praktyczny: procedura wdrożenia przewiduje
**migawkę maszyny przed każdą aktualizacją** jako sposób wycofania zmiany. Dla maszyny testowej LXC jest
do przyjęcia, jeśli tak jest Wam wygodniej.

### 1.2. Czym jest `febo-apps-prod` i czy każda usługa dostaje własną maszynę

**Nie, nie zmieniliśmy koncepcji. Podział jest po środowiskach i po bazie, nie po usługach.**

Nieporozumienie wynikało z naszego nazewnictwa i je poprawiamy. `febo-apps-prod` (od tej wersji:
`febo-prod-app`) **to maszyna wirtualna będąca hostem dla Docker Compose.** Wymienione wcześniej nginx,
aplikacja, worker, scheduler i Redis to **kontenery uruchamiane wewnątrz tej jednej maszyny** jednym
plikiem compose. Nie są to osobne maszyny i nigdy nie miały nimi być.

Maszyny są trzy i każda istnieje z innego powodu:

| Maszyna | Dlaczego osobna |
|---|---|
| `febo-prod-app` | Środowisko produkcyjne — wszystkie usługi aplikacji jako kontenery w jednym stosie |
| `febo-prod-db` | Bo tak ustaliliśmy na Twój wniosek: baza wydzielona ze względu na administrację, aktualizacje i kopie |
| `febo-test` | Bo test musi być oddzielony od produkcji — to jedyny sens jego istnienia |

Innymi słowy: **trzy maszyny, nie sześć.** Gdyby nie wydzielenie bazy, byłyby dwie.

Od tej wersji używamy nazw `febo-prod-app`, `febo-prod-db`, `febo-test`. Sformułowanie „oba kontenery
na start" dotyczyło środowisk, nie kontenerów — było mylące i wycofujemy je.

### 1.3. Kto buduje stos aplikacyjny

**Podział jest taki, jak zakładasz w drugiej części pytania: infrastruktura po Waszej stronie,
kompletny stos aplikacyjny po naszej.**

Dostarczamy: gotowe, ponumerowane obrazy kontenerów; plik `compose.yaml` na środowisko; wykaz zmiennych
środowiskowych z opisem i wartościami przykładowymi (bez sekretów); wykaz portów, wolumenów, kontroli
zdrowia i zależności między usługami; procedurę pierwszego uruchomienia, migracji schematu bazy,
aktualizacji i wycofania; wykaz wymagań wobec bazy (wersja, rozszerzenia, użytkownicy, parametry).

Nie oczekujemy od Was konfigurowania aplikacji — tylko maszyn, sieci, bazy i kopii.

### 1.4. Po co produkcji dostęp do GitHub, npm, pip

**Nie jest potrzebny i go nie będzie.** Twoje zastrzeżenie jest słuszne, przyjmujemy je w całości.

Proces wygląda tak:

1. Kod powstaje na środowisku dev (serwer redAi, poza siecią FEBO) i trafia do repozytorium na GitHubie.
2. **Budowanie odbywa się na maszynie `febo-test`** — to jedyna maszyna w biurze, która potrzebuje
   dostępu do GitHuba i repozytoriów bibliotek.
3. Zbudowana, ponumerowana wersja ląduje w **składzie wersji (rejestrze obrazów) uruchomionym
   na `febo-test`**.
4. **`febo-prod-app` pobiera obrazy wyłącznie ze składu.** Żadnego dostępu do internetu.

W zaporze wymaga to **jednego wyjątku od zasady rozdzielenia środowisk**: ruch z `febo-prod-app`
do składu na `febo-test`, jednokierunkowo, jeden port, tylko na czas wdrożenia. Prosimy o świadome
odnotowanie tego wyjątku — poza nim między środowiskami nie ma żadnej drogi.

Alternatywa, gdyby ten wyjątek był nie do przyjęcia: przekazujemy każde wydanie jako plik i administrator
wgrywa je ręcznie. Działa, ale obciąża Was przy każdej aktualizacji.

### 1.5. PostgreSQL — wydzielenie

**Zgoda. Baza produkcyjna staje na osobnej maszynie `febo-prod-db` i to FEBO ją administruje.**
Twoje argumenty — administracja, aktualizacje, kopie — przyjmujemy.

Trzy rzeczy, o których trzeba wiedzieć przy tej decyzji:

- **W tej bazie żyje kolejka zadań aplikacji.** Intranet nie używa osobnego brokera (ani Celery,
  ani RabbitMQ) — zadania w tle stoją w tabelach PostgreSQL. Każde zadanie to kilka zapytań, więc
  opóźnienie sieci między aplikacją a bazą przekłada się wprost na przepustowość. Prosimy, żeby obie
  maszyny stały na tym samym hoście i w tej samej podsieci.
- **Restart bazy zatrzymuje kolejkę**, nie tylko odczyty. Prosimy o uzgodnione okno serwisowe zamiast
  restartów doraźnych.
- **Migracje schematu wykonuje aplikacja**, jako krok poprzedzający start nowej wersji. Potrzebujemy
  do tego użytkownika bazy z prawem zmiany schematu — osobnego od użytkownika, którym aplikacja
  pracuje na co dzień.

Wymagania: PostgreSQL **16**, kodowanie UTF-8, strefa Europe/Warsaw, rozszerzenia `pg_trgm`
i `unaccent`; opcjonalnie `pgvector`, jeśli w przyszłości dojdzie wyszukiwanie znaczeniowe (dziś nie).

### 1.6. Kopie zapasowe — RPO, RTO, retencja, szyfrowanie, miejsce, odpowiedzialność

Na etapie pilotażu Intranet nie zawiera jeszcze rozwiązań krytycznych, dlatego zaczynamy od zakresu
minimalnego i podnosimy go wraz z każdą kolejną aplikacją o istotnym znaczeniu.

| Parametr | Wartość na start | Kiedy zmieniamy |
|---|---|---|
| **RPO** (dopuszczalna utrata pracy) | **24 godziny** | Przy pierwszej aplikacji, w której utrata dnia pracy jest kosztowna |
| **RTO** (czas powrotu) | **8 godzin roboczych** | Jw. |
| **Retencja** | 7 kopii dobowych bazy lokalnie + istniejące kopie maszyn | Po pilotażu: 4 kopie tygodniowe i 12 miesięcznych |
| **Szyfrowanie** | Obowiązkowe dla kopii opuszczających serwer; obowiązkowe dla wszystkich kopii od momentu, gdy w Intranecie pojawią się dane osobowe lub dokumenty poufne | Dziś danych osobowych pracowników jeszcze nie ma |
| **Miejsce** | Kopie robocze lokalnie na puli HDD; **kopia poza serwerem na istniejące zaplecze kopii FEBO** | — |
| **Kto wykonuje** | Kopie maszyn i bazy — FEBO. Zrzut logiczny bazy uruchamiany zadaniem systemowym, konfiguracja od nas | — |
| **Kto odtwarza** | FEBO, według naszej procedury; my asystujemy | — |

**Do zakresu, który już macie, dokładamy jedną rzecz: nocny zrzut logiczny bazy produkcyjnej o 01:30.**
Powód: kopia całej maszyny z działającą bazą jest z punktu widzenia bazy równoważna odcięciu zasilania.
Zwykle da się z niej wstać, ale nie jest to gwarantowane i nie pozwala odtworzyć samej bazy bez odtwarzania
całej maszyny. Zrzut logiczny to kilkanaście minut pracy raz na dobę i usuwa oba problemy.

**Jednorazowy test odtworzenia przed uruchomieniem produkcyjnym** — prosimy o wspólne przeprowadzenie
i krótką notatkę z wynikiem i zmierzonym czasem. Bez tego nie wiemy, czy RTO 8 godzin jest realne.

### 1.7. Dane wymagające trwałego składowania

| Dane | Na start | Docelowo | Uwagi |
|---|---|---|---|
| Baza PostgreSQL produkcji (razem z kolejką zadań i dziennikiem audytu) | ~10 GB | **150 GB** | Dziennik audytu jest niezmienny i rośnie stale — to główny czynnik wzrostu |
| Pliki aplikacji (załączniki, dokumenty, wyniki eksportów) | ~20 GB | **200 GB** | Zgodnie z Twoim szacunkiem |
| Skład wersji (obrazy) na `febo-test` | ~20 GB | 40 GB | Trzymamy kilka ostatnich wydań, starsze kasujemy |
| Środowisko testowe (baza + pliki) | ~10 GB | 120 GB | Nie wymaga kopii |
| Redis | **nie wymaga trwałego składowania** | — | Wyłącznie pamięć podręczna i sesje; utrata oznacza wylogowanie użytkowników, nic więcej |

### 1.8. Czy kopia bazy i plików musi być spójna wzajemnie

**Tak, to istotne** — i dziękujemy za to pytanie, bo łatwo je przeoczyć. Baza przechowuje opisy plików,
a dysk same pliki. Kopia z dwóch różnych momentów daje albo pliki, do których nikt nie ma odnośnika,
albo odnośniki do plików, których nie ma — a to drugie użytkownik widzi jako błąd.

Rozwiązanie nie wymaga zatrzymywania aplikacji: **najpierw kopia plików, potem zrzut bazy.** Przy takiej
kolejności jedyną możliwą rozbieżnością jest plik zapisany na dysku bez wpisu w bazie, czyli sytuacja
nieszkodliwa. Dostarczamy dodatkowo polecenie uzgadniające, które po odtworzeniu wskazuje rozbieżności.

### 1.9. Podział odpowiedzialności po uruchomieniu

| Obszar | Odpowiada |
|---|---|
| Maszyny wirtualne, system operacyjny, aktualizacje systemu | **FEBO / administrator serwera** |
| Sieć, zapora, adresacja, certyfikaty | **FEBO / administrator serwera** |
| PostgreSQL produkcji — instalacja, wersja, aktualizacje, kopie, odtworzenie | **FEBO / administrator serwera** |
| Kopie zapasowe i procedura odtworzenia | **FEBO / administrator serwera** |
| Aplikacja i jej kontenery, w tym Redis wewnątrz stosu | **Mateusz (strona FEBO)** |
| Aktualizacja wersji na produkcji i wycofanie zmiany | **Mateusz**, w razie potrzeby w uzgodnieniu z administratorem serwera |
| Środowisko dev na serwerze redAi | **redAi** |
| Dostarczenie obrazów, konfiguracji i procedur | **redAi** |
| Środowisko produkcyjne i testowe | **redAi nie ma do nich dostępu** |

Warto spisać jedną rzecz na styku: **kto reaguje pierwszy, gdy aplikacja nie wstaje po aktualizacji.**
Proponujemy: Mateusz wycofuje wersję według procedury; jeśli problem leży w bazie, sieci lub maszynie,
przejmuje administrator serwera.

### 1.10. Co miało działać w `febo-ops`

Miały tam stać: monitoring, skład wersji i zadania kopii. **Rezygnujemy z tej maszyny.** Monitoring
wchodzi dopiero na późniejszym etapie projektu i budowanie go dziś byłoby pracą przed czasem; skład
wersji przenosimy na `febo-test`; zadania kopii wykonuje infrastruktura FEBO. Oszczędność: 4 GB pamięci
i jedna maszyna mniej do utrzymania.

---

## 2. Podział zasobów po zmianach

### 2.1. Pamięć — 28 GB z 52,77 GB

| Maszyna | Pamięć | Rdzenie (wątki) | Rola |
|---|---|---|---|
| `febo-prod-app` | **8 GB** | 6 | Aplikacja produkcyjna: nginx, aplikacja, worker, scheduler, Redis |
| `febo-prod-db` | **12 GB** | 4 | PostgreSQL 16 produkcji, administrowany przez FEBO |
| `febo-test` | **8 GB** | 4 | Środowisko testowe + skład wersji + budowanie |
| **Razem** | **28 GB** | **14 wątków z 32** | |

Po zejściu hurtowni z serwera i rezygnacji z maszyny zaplecza przydzielamy niecałe 53% wolnej pamięci.
**Nie potrzebujemy już znać dokładnej rezerwy gospodarza maszyn** — przy tym zapasie mieścimy się
niezależnie od tego, ile ona wynosi. Zapas ma trzy konkretne zastosowania:

1. **Planowane włączenie archiwalnego TMS** — przy sprawie karnej. Prosimy o informację, ile pamięci
   ten system zajmuje, żebyśmy wiedzieli, czy wystarczy zapas, czy trzeba na ten czas zejść z pamięcią
   środowiska testowego.
2. **Czasowe podniesienie `febo-test` do rozmiaru produkcji** na czas pomiaru wydolności — dzięki temu
   pomiar nie musi odbywać się na produkcji.
3. **Powrót hurtowni z VPS na serwer**, jeśli tak zdecydujemy po prototypie.

### 2.2. Dysk — startujemy mniejsi i rośniemy

Skoro przestój przy powiększaniu przydziału jest planowany i bez znaczenia, **nie blokujemy miejsca
na zapas**. Przydziały poniżej to wartości startowe, w nawiasie wartość docelowa.

Zasada rozkładu: **bazy tam, gdzie jest najwięcej operacji na sekundę; pliki i kopie tam, gdzie jest
najwięcej miejsca; szybki SAS zostawiamy Optimie.**

| Pula | Przydział startowy | Suma | Zostaje wolne |
|---|---|---|---|
| **SATA SSD RAID10** (622 GB) | Baza produkcji 80 GB (docelowo 150) · środowisko testowe 80 GB (docelowo 120) | **160 GB** | ~462 GB |
| **HDD RAID5** (2,08 TB) | Pliki aplikacji 100 GB (docelowo 200) · kopie i zrzuty 150 GB (docelowo 300) | **250 GB** | ~1,83 TB |
| **SAS SSD RAID1** (691 GB, tam stoi Optima) | Dyski systemowe trzech maszyn, 40 GB każdy | **120 GB** | ~571 GB |

Dwie uwagi:

1. **Nic naszego nie wchodzi na pulę, na której pracuje Optima**, poza trzema dyskami systemowymi
   o znikomym ruchu. Jeśli wolisz mieć tę pulę całkiem wolną, dyski systemowe równie dobrze zmieszczą
   się na SATA SSD — daj znać.
2. **Pliki aplikacji na puli HDD są w porządku** — to duże, sekwencyjne odczyty i zapisy przy
   trzydziestu użytkownikach jednocześnie. Bazy na HDD nie byłyby.

---

## 3. Maszyny do utworzenia

| Nazwa | System | Wątki | Pamięć | Dysk systemowy | Dysk danych |
|---|---|---|---|---|---|
| `febo-prod-app` | Ubuntu 24.04 LTS | 6 | 8 GB | 40 GB (SAS) | 100 GB na pliki (HDD), docelowo 200 |
| `febo-prod-db` | Ubuntu 24.04 LTS | 4 | 12 GB | 40 GB (SAS) | 80 GB na bazę (SATA SSD), docelowo 150 |
| `febo-test` | Ubuntu 24.04 LTS | 4 | 8 GB | 40 GB (SAS) | 80 GB (SATA SSD), docelowo 120 |

Dla wszystkich: strefa czasowa Europe/Warsaw, synchronizacja czasu, konta imienne, logowanie kluczem,
plik wymiany 4 GB jako bufor przed zatrzymaniem procesów przy braku pamięci.

Kolejność uruchamiania: `febo-test` (jako pierwsza, bo tam budujemy) → `febo-prod-db` → `febo-prod-app`.

---

## 4. Sieć — cztery reguły

1. **Każde środowisko w osobnym segmencie**, bez tras między nimi.
2. **`febo-prod-app` ↔ `febo-prod-db`** — ruch tylko na porcie bazy, tylko między tymi dwiema maszynami.
3. **Jeden świadomy wyjątek:** `febo-prod-app` pobiera obrazy ze składu na `febo-test`, jednokierunkowo,
   jeden port. Poza tym produkcja i test się nie widzą.
4. **`febo-test` ma dostęp wychodzący do internetu** (GitHub, repozytoria bibliotek, źródła obrazów).
   **`febo-prod-app` i `febo-prod-db` nie mają żadnego dostępu do internetu.**

Adresacji, zapory, certyfikatów i tras nie konfigurujemy — to zostaje po stronie FEBO.

---

## 5. Środowisko dev — zostaje na serwerze redAi

Dev nie wraca do biura. Konsekwencja, którą trzeba znać: **środowisko dev nie ma drogi do sieci FEBO**,
więc integracja z testową Optimą i z LogTMS nie może być weryfikowana automatycznie na dev.

Sposób obejścia bez otwierania czegokolwiek: pracę nad integracją prowadzimy na dev na podstawie opisu
i danych zastępczych, a **weryfikacja na żywych systemach odbywa się na `febo-test`**, uruchamiana
przez Mateusza. Wolniej, ale nie wymaga żadnego wejścia z zewnątrz do sieci FEBO — i to przesądziło.

Potrzebujemy do tego dwóch rzeczy od FEBO, obu tylko dla `febo-test`:
- konto odczytowe do **testowej instancji Optimy**,
- konto odczytowe do **LogTMS** (uprawnienia wyłącznie do odczytu, nadane na bazie).

---

## 6. Hurtownia danych — prototyp poza serwerem, dwa pytania do Ciebie

**Decyzja FEBO: prototyp hurtowni danych (Small Big Steps / pewne.it) uruchamiamy na zewnętrznym VPS,
a jeżeli sprawdzi się i będzie taka możliwość — przeniesiemy go na zasoby lokalne.** Maszyny `febo-dwh`
na serwerze na razie nie tworzymy. Zasoby na nią są i zostają wolne.

Czego prototyp wymaga: 4 rdzenie, 12–16 GB pamięci, 200 GB SSD, Rocky Linux, Podman albo Docker,
uprawnienia administracyjne dla zespołu zewnętrznego w granicach tej jednej maszyny, ruch wychodzący
do internetu oraz baza hurtowni dostępna dla analityki z wybranych podsieci.

### Pytanie 1 — czy doradzasz i konfigurujesz VPS, czy wybieramy go sami

Wolelibyśmy Twoją rekomendację. Jeśli FEBO ma sprawdzonego dostawcę albo już gdzieś płaci za zasoby,
sensowniej użyć tego niż zakładać kolejne konto u przypadkowego operatora. Daj proszę znać, czy bierzesz
to na siebie (wybór, założenie, podstawowe utwardzenie systemu), czy mamy dobrać VPS z dostawcą hurtowni
we własnym zakresie.

### Pytanie 2 — którędy hurtownia sięgnie po dane z TMS

**To jest właściwy problem tego wariantu i bez Twojej decyzji nie ruszy.** Hurtownia ciągnie dane
z bazy TMS. Postawiona poza siecią FEBO oznacza, że:

- dane operacyjne firmy — klienci, trasy, stawki, dane kontaktowe — **trafiają na maszynę poza firmą**,
  co jest decyzją o tym, gdzie leżą dane, a nie tylko wyborem technicznym;
- ktoś musi zbudować drogę z tego VPS do bazy TMS.

**Odradzamy wystawianie bazy TMS na adres publiczny w jakiejkolwiek postaci.** Rozwiązania, które
wydają się nam czystsze, do Twojej oceny: tunel zestawiany **z sieci FEBO na zewnątrz** (żaden port
FEBO nie jest otwierany od strony internetu) albo wypychanie przygotowanych porcji danych z wnętrza
sieci do hurtowni, zamiast pobierania ich przez hurtownię.

### Uwaga na później

Skoro zakładamy możliwy powrót hurtowni na serwer lokalny, warto **od początku poprosić zespół
zewnętrzny, żeby nie przywiązywał rozwiązania do konkretnego operatora** — czyli żeby całość dała się
odtworzyć z opisu konfiguracji na dowolnej maszynie. Inaczej „przeniesiemy, jak się sprawdzi" okaże się
budowaniem od nowa.

Druga rzecz: Intranet ma na liście blokad prośbę o replikę LogTMS wyłącznie do odczytu, a hurtownia
buduje dokładnie taki obraz danych. Dopóki hurtownia stoi na zewnętrznym VPS, Intranet nie będzie z niej
czytał — wróci to do rozmowy dopiero przy przeniesieniu na serwer lokalny.

---

## 7. Stan pytań

**Odpowiedzi już otrzymane:**

| Pytanie | Odpowiedź | Co z tego wynika |
|---|---|---|
| Czy 52,77 GB można przydzielić w całości | Nie | Przydzielamy 28 GB — mieścimy się niezależnie od wielkości rezerwy |
| Czy dysk da się powiększyć bez przestoju | Przestój planowany, bez znaczenia | Startujemy z mniejszymi przydziałami i rośniemy realnym tempem |
| Czy archiwalny TMS wraca | Tylko przy sprawach karnych, z wyprzedzeniem | Nie trzymamy stałego bufora; wymaga jednorazowego planowania |

**Pytania otwarte:**

1. **Ile pamięci zajmuje archiwalny TMS**, żebyśmy wiedzieli, skąd ją wziąć przy planowanym włączeniu.
2. **Czy zaplecze kopii FEBO** — to, na które ma trafiać kopia bazy produkcyjnej — stoi fizycznie poza
   tym serwerem, i jaka jest tam retencja.
3. **Zgoda na jeden wyjątek w zaporze:** pobieranie obrazów z `febo-test` przez `febo-prod-app`.
4. **Termin wspólnego testu odtworzenia bazy** przed uruchomieniem produkcyjnym.
5. **VPS pod prototyp hurtowni** — Twoja rekomendacja i konfiguracja, czy dobieramy sami (rozdział 6).
6. **Droga z VPS hurtowni do danych TMS** — Twoja propozycja (rozdział 6).
7. Czy dyski systemowe trzech maszyn mogą stać na puli SAS obok Optimy, czy wolisz je na SATA SSD.
