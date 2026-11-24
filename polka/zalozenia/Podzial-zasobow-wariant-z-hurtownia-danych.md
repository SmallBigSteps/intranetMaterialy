<!--
opis: Podział zasobów serwera w wariancie z hurtownią danych.
rodzaj: zalozenie
rozmowa: resources
zmiana: 16.10.2026, resources
status: roboczy
slowa: zasoby,hurtownia danych,serwer
-->
# Podział zasobów z uwzględnieniem hurtowni danych

**Wersja 1.1 — po decyzjach z 18.08.2026** · serwer lokalny FEBO · projekt *intranet2.0* + prototyp hurtowni danych · redAi

Dokument uzupełnia *Serwer lokalny FEBO — zasoby, kopie zapasowe, kontenery i dostępy* (wersja 1.3) o maszynę
hurtowni danych opisaną w korespondencji „Prototyp hurtowni danych” (Small Big Steps / pewne.it, lipiec 2026).


> ## ⚠ Aktualizacja 20.08.2026 — hurtownia schodzi z serwera lokalnego
>
> Ten dokument jest **nieaktualny w całości w części dotyczącej przydziałów**. Obowiązuje dokument
> **„Intranet FEBO — odpowiedzi dla administratora serwera i zaktualizowany podział zasobów"
> (wersja 1.1)**.
>
> **Decyzja FEBO z 20.08.2026: prototyp hurtowni uruchamiamy na zewnętrznym VPS**, a jeżeli się sprawdzi
> i będzie taka możliwość — przeniesiemy go na zasoby lokalne. Maszyna `febo-dwh` na serwerze FEBO
> na razie nie powstaje.
>
> | Co ten dokument mówi | Stan obowiązujący od 20.08.2026 |
> |---|---|
> | Hurtownia jako maszyna na serwerze FEBO | **Prototyp na zewnętrznym VPS**, powrót na serwer do rozważenia po prototypie |
> | Hurtownia 16 GB pamięci i 170 GB dysku | **Wymagania bez zmian po stronie VPS**: 4 rdzenie, 12–16 GB, 200 GB SSD |
> | Ciężkie ładowanie wyłącznie w oknie nocnym — warunek wiążący | **Wymóg nieaktualny** — hurtownia nie stoi już na dyskach FEBO |
> | Wspólne lustro dysków jako główne ryzyko; rezerwa 46 GB | **Nieaktualne** — serwer ma trzy niezależne pule dyskowe i ponad 2 TB wolnego |
> | Zasoby serwera obcięte, żeby zmieścić hurtownię | **Cofnięte** — Intranet przydziela 28 GB z 52,77 GB |
>
> **Nowe ryzyko, którego ten dokument nie zna:** hurtownia poza siecią FEBO oznacza, że dane operacyjne
> firmy wychodzą na maszynę u zewnętrznego operatora i że trzeba zbudować drogę z tego VPS do bazy TMS.
> To decyzja o tym, gdzie leżą dane, a nie wybór techniczny — czeka na odpowiedź administratora serwera.
>
> W mocy pozostają: wykaz wymagań hurtowni, propozycja tańszych kopii oraz uwaga o możliwym wykorzystaniu
> hurtowni jako repliki LogTMS dla Intranetu — ta ostatnia jednak **dopiero po ewentualnym przeniesieniu
> hurtowni na serwer lokalny**.
>
---

## 1. Przyjęte rozstrzygnięcia

| # | Decyzja | Skutek |
|---|---|---|
| **1** | **Hurtownia staje na tym samym serwerze, bez dokładania pamięci i dysku.** | Wszystkie pozostałe przydziały przycięte. Podział mieści się w 60 GB pamięci co do gigabajta i zostawia 46 GB wolnego dysku. |
| **2** | **Ciężkie ładowanie i przeliczenia hurtowni wyłącznie w oknie nocnym**, dopóki nie zwiększymy zasobów. | Warunek wiążący. To jedyne, co realnie rozdziela hurtownię od produkcji na wspólnym lustrze dysków. |
| **3** | To ten sam serwer, o którym pisał administrator; **maszyna hurtowni jeszcze nie istnieje**. | Planujemy od zera na 60 GB. Zastrzeżenie: te 60 GB jest wolne, **dopóki archiwalny TMS pozostaje wyłączony**. |

---

## 2. Czego wymaga hurtownia

| Element | Wymaganie z korespondencji |
|---|---|
| Maszyna | 1 maszyna wirtualna: **4 vCPU, 16 GB RAM, 200+ GB SSD** |
| System | **Rocky Linux** (rodzina RHEL) — inny standard niż Ubuntu na maszynach Intranetu |
| Konteneryzacja | Podman albo Docker, decyzja zespołu hurtowni przy konfiguracji |
| Składniki | Airbyte (integracja danych), PostgreSQL jako hurtownia, Metabase (BI), lekka aplikacja webowa do obsługi wyjątków, serwer HTTP jako proxy |
| Dostęp administracyjny | konto zespołu zewnętrznego z uprawnieniami **root (`sudo`)** — instalacja bazy, zapory, konteneryzacji i proxy po ich stronie |
| Sieć | wyjście na świat dla Airbyte (pobieranie z bazy TMS), PostgreSQL dostępny z wybranych podsieci firmowych, aplikacje web i BI za proxy, dostęp do API zewnętrznych (GPS, geokodowanie) |
| Kopie | raz na dobę, **preferowana migawka maszyny** |
| Monitoring | maszyna oraz wybrane usługi, częściowo po stronie zespołu hurtowni |

---

## 3. Podział przyjęty — pamięć

Budżet: **60 GB wolnej pamięci**, rozdzielony **co do gigabajta**.

| Maszyna | Przydział | Zmiana wobec planu bez hurtowni | Co oddaje |
|---|---|---|---|
| `febo-prod` — produkcja Intranetu | **20 GB** | −4 | zapas na wzrost; realne zapotrzebowanie pilotażu to ~16 GB, więc margines spada z ośmiu gigabajtów do czterech |
| `febo-dwh` — hurtownia | **16 GB** | nowa | wartość z wniosku, bez zapasu |
| `febo-test` | **8 GB** | −4 | odwzorowanie produkcji; test przestaje nadawać się do pomiarów wydolności |
| `febo-dev` | **8 GB** | −2 | dokładnie minimum z karty T-00; skaner plików pozostaje wyłączony, tak jak dziś |
| `febo-ops` — zaplecze | **4 GB** | −2 | szybkość potoku CI i długość historii monitoringu |
| Rezerwa gospodarza | **4 GB** | −4 | bufor na skoki zużycia samego gospodarza |

**Procesor:** produkcja 8 wątków w gwarancji (na jednym gnieździe), hurtownia 4, test 3, dev 3, zaplecze 3 —
razem 21 z 32. Reszta jako zapas szczytowy, z pierwszeństwem dla produkcji.

---

## 4. Podział przyjęty — dysk

Budżet: **691 GB**.

| Przeznaczenie | Przydział | Zmiana | Uwaga |
|---|---|---|---|
| `febo-prod` | **300 GB** | +20 | **dokładnie minimum z kontraktu**; baza 130, pliki 140, dzienniki 30 |
| `febo-dwh` | **170 GB** | nowa | poniżej wnioskowanych 200+ GB — patrz §5 |
| `febo-test` | **60 GB** | −40 | na profil obciążeniowy trzeba pożyczyć z rezerwy |
| `febo-dev` | **50 GB** | −20 | minimum z karty T-00 |
| Kopie zapasowe | **40 GB** | −80 | wykonalne **wyłącznie** dzięki decyzji o minimalnym zakresie kopii |
| `febo-ops` | **25 GB** | −15 | krótsza retencja obrazów |
| Rezerwa | **46 GB** | −35 | poniżej 7% — próg alarmowy, nie zapas |

---

## 5. Co trzeba powiedzieć wprost zespołowi hurtowni

1. **Zamiast 200+ GB dostajecie 170 GB.** Brakujące 30 GB leży w rezerwie i można je dołożyć bez przestoju,
   ale wtedy rezerwa serwera schodzi do 16 GB — czyli praktycznie do zera.
2. **Ciężkie operacje wyłącznie nocą.** Nie jest to prośba o uprzejmość: hurtownia czyta wielkimi porcjami,
   Intranet robi krótkie, częste zapytania, a jedno i drugie idzie na to samo lustro dysków. Limity procesora
   i pamięci tego nie rozdzielą — rozdziela to tylko czas.
3. **Dobowa migawka maszyny jest droga.** Propozycja tańsza w §6.
4. **Root kończy się na granicy tej maszyny.** Konto z pełnymi uprawnieniami na `febo-dwh` — tak; jakikolwiek
   dostęp do maszyn Intranetu — nie.

---

## 6. Kopie hurtowni — propozycja tańsza niż wnioskowana

Hurtownia jest jedynym systemem w tym zestawieniu, który **odtwarza się z systemów źródłowych**. Airbyte potrafi
załadować dane ponownie. Wartością nie do odtworzenia jest konfiguracja połączeń, definicje przekształceń
i pulpity w Metabase — czyli megabajty, nie gigabajty.

| Co | Częstotliwość | Miejsce |
|---|---|---|
| Migawka maszyny **po konfiguracji** i przed każdą większą zmianą | zdarzeniowo, 2 sztuki | lokalnie |
| Zrzut baz konfiguracyjnych (Airbyte, Metabase, aplikacja wyjątków) | codziennie | lokalnie + poza serwerem |
| Zrzut danych hurtowni | **tygodniowo**, tylko jeśli ponowne załadowanie z TMS trwa zbyt długo | lokalnie |

Oszczędność wobec dobowej migawki: kilkadziesiąt gigabajtów — przy 46 GB rezerwy to różnica między „mieści się”
a „nie mieści się”. **Do uzgodnienia z zespołem hurtowni:** jeżeli pełne załadowanie danych z TMS trwa wiele
godzin, argument się odwraca i migawka wraca do harmonogramu.

---

## 7. Sieć i granice

1. **Własny segment sieci, bez trasy do środowisk Intranetu** — w obie strony. Obie strony czytają z LogTMS
   niezależnie.
2. **Baza hurtowni jest celowo dostępna z sieci firmowej** (analityka, zapora zawężona do wskazanych podsieci).
   To **odwrotność** reguły obowiązującej Intranet, gdzie baza nie opuszcza sieci kontenerów. Dwa systemy,
   dwie różne reguły — nie wolno ich pomieszać przy konfiguracji zapory.
3. **Ruch wychodzący z listy dozwolonych celów**: baza TMS, API zewnętrzne (GPS, geokodowanie), źródła pakietów
   i obrazów.
4. **Konta imienne z `sudo` wyłącznie na `febo-dwh`.** Ta sama osoba nie ma konta na maszynach Intranetu.

---

## 8. Ryzyka przyjęte świadomie

| Ryzyko | Skutek | Kiedy się zmaterializuje |
|---|---|---|
| **Brak zapasu pamięci** | każde nowe zapotrzebowanie wymaga odebrania komuś innemu | przy pierwszym wzroście któregokolwiek systemu |
| **Powrót archiwalnego TMS** | podział przestaje obowiązywać — wolne 60 GB istnieje tylko dlatego, że tamten system jest wyłączony | decyzja poza tym projektem, może zapaść w każdej chwili |
| **Wspólne lustro dysków** | przeliczenie w hurtowni spowalnia Intranet | przy pierwszym ładowaniu poza oknem nocnym |
| **Produkcja na minimum kontraktu** | brak miejsca na wzrost wolumenu dokumentów z pięcioletnim okresem przechowywania | po kilku miesiącach pracy z dokumentami |
| **Rezerwa gospodarza 4 GB** | użycie pliku wymiany, w skrajnym przypadku zabicie procesu z braku pamięci | przy skoku zużycia poza maszynami wirtualnymi |
| **Test przestaje odwzorowywać produkcję** | wynik pomiaru wydolności z testu byłby bezwartościowy | karta T-70 — pomiar musi odbyć się na pustej produkcji |

**Wniosek operacyjny:** ten podział działa, ale nie ma w nim marginesu. Pierwsza rzecz do zrobienia po pojawieniu
się budżetu to **dołożenie pamięci**, a nie kolejne przesunięcia w tabeli.

---

## 9. Ścieżka rozbudowy — co się zmienia po dołożeniu zasobów

| Dołożone | Co odzyskujemy, w tej kolejności |
|---|---|
| **+16 GB pamięci** | produkcja wraca do 24 GB, rezerwa gospodarza do 8 GB — znika najostrzejsze ryzyko |
| **+32 GB pamięci** | produkcja osiąga **wariant zalecany w kontrakcie (32 GB)**, test wraca do 12 GB, dev do 10 GB (skaner plików włączony), zaplecze do 6 GB |
| **+500 GB przestrzeni** | hurtownia dostaje wnioskowane 250 GB, produkcja 400 GB, kopie wracają do pełnego harmonogramu, rezerwa przekracza 15% |
| **Osobny nośnik na dane hurtowni** | znika konkurencja o dyski — **okno nocne przestaje być warunkiem** i hurtownia może ładować dane w ciągu dnia |

Ostatni wiersz jest najtańszy i najszybciej odczuwalny: pojedynczy nośnik zdejmuje ograniczenie, które dziś
najbardziej krępuje zespół hurtowni. Warto sprawdzić, czy serwer ma wolne zatoki i porty kontrolera.

---

## 10. Możliwość, o którą warto zapytać zespół hurtowni

Intranet ma na liście blokad wpis: *„replika LogTMS tylko do odczytu — do ustalenia z twórcą LogTMS”*.
Powód: pobieranie danych do modułów Intranetu obciąża bazę, z której zespół korzysta na co dzień.

Hurtownia buduje dokładnie to samo — regularnie ładowany, uporządkowany odczytowy obraz danych z TMS.
**Jeżeli hurtownia powstanie, Intranet może czytać część danych z niej, zamiast odpytywać LogTMS drugi raz.**
Zyskują obie strony: mniejsze obciążenie systemu produkcyjnego i jedno miejsce, w którym dane są uporządkowane.

Wymaga rozmowy o zakresie i o świeżości danych — Intranet w części zastosowań potrzebuje stanu bieżącego,
a nie z nocnego ładowania — ale jest to najtańsza integracja, jaka się w tym zestawieniu pojawia.

---

## 11. Do potwierdzenia u administratora serwera

1. Co zajmuje pozostałe 68 GB pamięci i czy archiwalny TMS pozostaje wyłączony na stałe.
2. Czy serwer ma **wolne zatoki dyskowe i porty kontrolera** — przesądza o zdjęciu okna nocnego (§9).
3. Czy przydział dyskowy da się powiększać bez przestoju (rezerwa 46 GB ma sens tylko wtedy).
4. Kto po stronie FEBO jest właścicielem dostępu zespołu hurtowni i kiedy jest przegląd tego dostępu.

---

*Dokument przygotowany dla FEBO LOGISTIC Sp. z o.o. · redAi · 18.08.2026*
