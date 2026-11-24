<!--
opis: Raport z odbioru wyglądu powłoki na dev w trybie jasnym i ciemnym: 13 kryteriów, usterki U1–U9 (zadania ZAD-342–347), wartości kolorów i rozmiarów do potwierdzenia.
rodzaj: wynik
zadanie: ZAD-308
kamien: KM-7 UX i UY
rozmowa: ZAD-308 Odbiór wyglądu obu trybów
zmiana: 24.11.2026, ZAD-308 Odbiór wyglądu obu trybów
status: do akceptacji
slowa: tryb ciemny, tryb jasny, marka FEBO, Tabler, odbiór wyglądu, Playwright, usterki
-->

# ZAD-308 — odbiór wyglądu obu trybów (dev, marka FEBO)

Data przejścia: 24.11.2026 · środowisko: https://dev.febo.redai.pl · narzędzie: Playwright (Chromium)
Rozdzielczości: komputer 1440 × 900, tablet 1024 × 768, telefon 390 × 844 (iPhone 13, tylko kontrola powiększania).
Konta: e2e.odbior.zadania, e2e.odbior.struktura (administrator), e2e.odbior.platnosci. Po teście wszystkie trzy wróciły na tryb „Systemowy”.

Zrzuty leżą osobno w paczce `ZAD-308-zrzuty.zip`:
- `pary/`: 27 par „jasny | ciemny” obok siebie (komputer i tablet);
- `szczegoly/`: 21 zrzutów do konkretnych kryteriów i usterek.

Zrzutów Struktury (lista, karta osoby, schemat, widok działu, telefon) i rejestru uwag nie ma w paczce, bo pokazują prawdziwe dane pracowników. Usterkę U6 opisuję słowami.

## 1. Wynik w skrócie

- Oba tryby działają na wszystkich ekranach z zakresu. Usterek blokujących brak.
- **9 usterek**, wszystkie w powłoce (repozytorium intranet). We wdrożeniu FEBO i w Płatnościach usterek wyglądu nie ma.
- Usterki założone jako 6 zadań w KM-7 (tabela w rozdz. 4).
- Kalendarza obecności nie dało się sprawdzić: jest ukryty z założenia do integracji z Outlookiem (ZAD-302).

**Ważne do czytania kryteriów:** karta ZAD-308 mówi o tekście 14 px i promieniu 0 (decyzja z 16.11). Decyzja wdrożenia z ZAD-031 (23.11) to zmieniła: tekst 16 px DM Sans, narożniki 6 px dla kart i pól, 0 na ramie, pigułki zaokrąglone. Tabler zostaje źródłem kolorów. Sprawdzałem według nowszej decyzji.

## 2. Zakres przejścia

| Ekran | Komputer | Tablet | Telefon |
|---|---|---|---|
| Logowanie, logowanie z błędem | tak | tak | tak |
| Drugi składnik logowania | tak | — | — |
| Pulpit i rama (panel, belka, menu osoby) | tak | tak | tak |
| Lista zadań | tak | tak | tak |
| Szczegóły zadania | tak | tak | — |
| Formularz z błędem (Zgłoś uwagę, nowa procedura) | tak | tak | tak |
| Katalog aplikacji | tak | tak | — |
| Struktura: Lista, Schemat, Widok działu, karta osoby | tak | tak | tak (Lista) |
| Struktura i role (administrator) | tak | tak | — |
| Standardy: lista, procedura, nowa procedura | tak | tak | tak |
| Płatności: do zrobienia, wszystkie, wgrywanie wykazu | tak | tak | tak |
| Profil | tak | tak | — |

## 3. Kryteria odbioru

| # | Kryterium | Wynik | Dowód |
|---|---|---|---|
| K1 | Kolory jak w Tablerze (neutral, czerwony akcent, ciemna belka) | spełnione, wartości do potwierdzenia w rozdz. 5 | `pary/*` |
| K2 | Narożniki według ZAD-031 (6 px karty i pola, 0 rama, pigułki okrągłe) | spełnione | tokeny w rozdz. 5 |
| K3 | Tekst 16 px nie ucina i nie zawija gorzej panelu, okruszków, nagłówków kolumn | **częściowo**: panel i okruszki w porządku; usterki w tabelach i polach (U1, U2, U2b, U3, U7) | `pary/desktop-lista-zadan.jpg`, `pary/tablet-lista-zadan.jpg` |
| K4 | Telefon: wejście w pole nie powiększa strony | spełnione: wszystkie pola mają 16 px, skala przed i po stuknięciu 1,0 | `szczegoly/telefon-*.jpg` |
| K5 | Natywne kontrolki ciemne w trybie ciemnym (data, lista, pole wyboru, pasek przewijania) | spełnione: strona ustawia ciemny schemat kontrolek, tło pól #262626 | `szczegoly/dark-*` |
| K6 | Statusy (BR-UI-001) czytelne w obu trybach | spełnione: każdy status ma tekst, kontrast jasny 5,06–9,6:1, ciemny 4,78–6,93:1 (wymóg 4,5:1) | `pary/*-standardy-lista.jpg`, `pary/*-katalog-aplikacji.jpg` |
| K7 | Kalendarz obecności czytelny w obu trybach | **nie do sprawdzenia**: ukryty do ZAD-302; widać tylko „Dziś: nikt nie wpisał” i przełącznik biuro/zdalnie | — |
| K8 | Jedna czerwona akcja na ekran | spełnione: czerwony jest tylko „Dalej” na logowaniu; przyciski główne mają kolor belki | `pary/*` |
| K9 | Wybór trybu przetrwa wylogowanie | spełnione: tryb zapisany w koncie; po wylogowaniu strona logowania idzie za ustawieniem systemu (konto jeszcze nieznane), po zalogowaniu wraca wybrany tryb | `szczegoly/utrwalenie-po-wylogowaniu.jpg` |
| K10 | Wybór trybu przetrwa zmianę komputera | spełnione: świeża przeglądarka z jasnym systemem, pusta pamięć przeglądarki, po zalogowaniu tryb ciemny z konta | `szczegoly/utrwalenie-nowy-komputer.jpg` |
| K11 | Tryb „Systemowy” reaguje na zmianę ustawienia systemu | spełnione: tło zmienia się z jasnego na ciemne bez przeładowania | `szczegoly/systemowy-po-zmianie-na-ciemny.jpg` |
| K12 | Brak błędów w konsoli | **częściowo**: po zalogowaniu czysto; na stronie logowania błędy zapytań (U9) | — |
| K13 | Formularz z błędem czytelny w obu trybach | spełnione | `pary/*-logowanie-blad.jpg`, `szczegoly/*-standardy-nowa-blad.jpg` |

## 4. Usterki

Wszystkie usterki są w powłoce (repozytorium intranet). Każda trafiła do zadania w KM-7:

| Zadanie | Usterki |
|---|---|
| ZAD-342 | U2, U2b: wyrównanie i zawijanie w tabelach |
| ZAD-343 | U3: ucięte podpowiedzi w polach |
| ZAD-344 | U4, U5: przyciski w trybie ciemnym |
| ZAD-345 | U1: etykieta „Termin” na tablecie |
| ZAD-346 | U6: Struktura na telefonie |
| ZAD-347 | U7, U8, U9: drobne usterki ekranów konta i logowania |

### U1. Tablet: etykieta „Termin” innym krojem

- **Kroki:** tablet 1024 px, zaloguj się, wejdź w Zadania.
- **Oczekiwany wynik:** wszystkie etykiety w karcie zadania tym samym krojem (DM Sans).
- **Faktyczny wynik:** „Termin” jest pismem stałej szerokości, pozostałe etykiety w DM Sans. Oba tryby.
- **Zrzut:** `pary/tablet-lista-zadan.jpg`

### U2. Nagłówki kolumn wyśrodkowane, treść do lewej

- **Kroki:** komputer, Zadania; potem Standardy FEBO.
- **Oczekiwany wynik:** nagłówek kolumny wyrównany jak jej treść (tekst do lewej, liczby do prawej).
- **Faktyczny wynik:** nagłówki ZADANIE (lista zadań) i PROCEDURA (Standardy) są wyśrodkowane, a treść pod nimi jest do lewej. Kolumna wygląda na przesuniętą.
- **Zrzut:** `pary/desktop-lista-zadan.jpg`, `pary/desktop-standardy-lista.jpg`

### U2b. Lista zadań 1440 px: nazwa aplikacji w dwóch liniach

- **Kroki:** komputer 1440 px, Zadania.
- **Oczekiwany wynik:** krótka wartość w kolumnie Aplikacja mieści się w jednej linii.
- **Faktyczny wynik:** „Lokator próbny” zawija się do dwóch linii i podwyższa wiersz. Przy tej szerokości ekranu kolumna jest za wąska.
- **Zrzut:** `pary/desktop-lista-zadan.jpg`

### U3. Ucięte podpowiedzi w polach (komputer)

- **Kroki:** komputer 1440 px:
  - spójrz na pole wyszukiwania w belce;
  - Struktura i kontakty → pole szukania;
  - Standardy FEBO → pole „Szukaj”.
- **Oczekiwany wynik:** podpowiedź w polu widać w całości, albo jest krótsza.
- **Faktyczny wynik:**
  - belka: „Szukaj: numer, kontrah…”;
  - Struktura: tekst kończy się na „…dziale lub e-m”;
  - Standardy: „np. odprawa, awizacja, reklama…”.
- **Zrzut:** `pary/desktop-pulpit.jpg`, `pary/desktop-standardy-lista.jpg`

### U4. Tryb ciemny: nieaktywny przycisk najjaśniejszy na ekranie

- **Kroki:** konto płatności, tryb Ciemny, Płatności, bez wybranego pliku.
- **Oczekiwany wynik:** nieaktywny „Wgraj wykaz” jest wyraźnie przygaszony, słabszy od aktywnych przycisków.
- **Faktyczny wynik:** tło jasnoszare #a3a3a3 z ciemnym tekstem #171717. To najjaśniejszy przycisk na ciemnym ekranie, więc wygląda na bardziej aktywny niż „Wybierz plik”. W trybie jasnym jest w porządku (#737373, biały tekst).
- **Zrzut:** `szczegoly/dark-wgraj-wykaz.jpg`, `szczegoly/light-wgraj-wykaz.jpg`

### U5. Tryb ciemny: czerwony przycisk po najechaniu zlewa się z tłem

- **Kroki:** strona logowania, tryb ciemny systemu, najedź na „Dalej”.
- **Oczekiwany wynik:** krawędź przycisku odróżnia się od tła co najmniej 3:1 (WCAG 1.4.11).
- **Faktyczny wynik:**
  - po najechaniu (czerwony przyciemniony o 15%) kontrast z tłem wynosi 2,49:1;
  - bez najechania 3,25:1;
  - tekst na przycisku 6,07:1, w porządku.
- **Zrzut:** `szczegoly/dark-dalej-hover.jpg`

### U6. Struktura na telefonie: panel karty nachodzi na listę

- **Kroki:** telefon 390 px, Struktura i kontakty, widok Lista.
- **Oczekiwany wynik:** pole szukania i lista osób na całą szerokość. Karta osoby pokazuje się dopiero po wybraniu osoby (pod listą albo jako osobny widok). Brak przewijania w bok.
- **Faktyczny wynik:**
  - pusty panel „Wybierz osobę, aby zobaczyć jej kartę” nachodzi na pole wyszukiwania i listę;
  - strona ma 429 px szerokości przy ekranie 390 px, więc przewija się w bok.
- **Zrzut:** pominięty (dane pracowników); do powtórzenia na dev.

### U7. Profil: adres e-mail łamany w środku

- **Kroki:** komputer 1440 px, menu osoby → Profil i moje dane.
- **Oczekiwany wynik:** adres w jednej linii albo łamany przy „@” lub kropce.
- **Faktyczny wynik:** adres łamie się w środku słowa („…example.co / m”).
- **Zrzut:** `pary/desktop-profil.jpg`

### U8. Drugi składnik logowania bez nagłówka ekranu

- **Kroki:** zaloguj się kontem z drugim składnikiem, wejdź na ekran „Drugi składnik logowania”.
- **Oczekiwany wynik:** nagłówek jak na innych ekranach: okruszki, tytuł z czerwoną kropką, karta z czerwoną linią.
- **Faktyczny wynik:** sam tytuł i treść, bez nagłówka ekranu.
- **Zrzut:** `szczegoly/light-desktop-drugi-skladnik.jpg`, `szczegoly/dark-desktop-drugi-skladnik.jpg`

### U9. Strona logowania: błędy w konsoli

- **Kroki:** otwórz stronę logowania z narzędziami przeglądarki (konsola); wpisz złe hasło.
- **Oczekiwany wynik:** konsola czysta. Sprawdzenie „czy jestem zalogowany” i zły login nie są błędem strony.
- **Faktyczny wynik:**
  - przy każdym wejściu w konsoli jest „Failed to load resource” 403 z adresu `/api/v1/accounts/me/`;
  - po złym haśle dochodzi drugi taki wpis, 400.
  - Dla użytkownika niewidoczne, ale zaśmieca konsolę i monitoring błędów.
- **Zrzut:** nie dotyczy.

## 5. Do potwierdzenia (Paweł Mazek)

Wartości odczytane z dev 24.11.2026. Odpowiedz numerami: „tak” albo poprawna wartość.

| Nr | Token | Do czego | Jasny | Ciemny |
|---|---|---|---|---|
| 1 | accent | czerwony marki (kropka, linia, „Dalej”) | #d63939 | #d63939 |
| 2 | accent-hover | czerwony po najechaniu | accent przyciemniony o 15% | to samo (zob. U5) |
| 3 | accent-text | czerwony tekst i linki | #c83535 | #e7726b |
| 4 | dominant / head | belka, przyciski główne, nagłówki tabel | #262626 | #0a0a0a |
| 5 | login-brand | lewa część strony logowania | #262626 | #0a0a0a |
| 6 | surface / band | karty, pasy | #ffffff | #262626 |
| 7 | canvas | tło strony | #fafafa | #171717 |
| 8 | elevated | panel boczny | #333333 | #171717 |
| 9 | ink | tekst | #404040 | #e5e5e5 |
| 10 | muted | tekst pomocniczy | #737373 | #a3a3a3 |
| 11 | border | linie i ramki | #e5e5e5 | #404040 |
| 12 | success | status pozytywny | #0f7b3f | #35b36a |
| 13 | warning | status ostrzegawczy | #9a5c00 | #e1962b |
| 14 | danger | błąd | #8a1220 | #f0616c |
| 15 | info | informacja | #1f6fb2 | #5aa8e0 |
| 16 | radius-card, radius-control | karty, pola, przyciski | 6 px | 6 px |
| 17 | radius-chrome | belka, panel | 0 | 0 |
| 18 | radius-pill | statusy, pigułki | okrągłe (999 px) | okrągłe |
| 19 | body | tekst | 16 px DM Sans | — |
| 20 | h1 | tytuł ekranu | 24 px | — |
| 21 | caption, label | podpisy, etykiety | 13 px | — |
| 22 | mono | numery, kody | 14 px | — |
| 23 | display, stat | duże liczby na pulpicie | 32 px, 36 px | — |

Pytania do decyzji:

- **24.** Panel boczny w trybie ciemnym (#171717) ma ten sam kolor co tło strony. Panel odcina tylko belka i linia. Czy tak ma być, czy panel ma być ciemniejszy, jak belka (#0a0a0a)?
- **25.** Strona logowania po wylogowaniu idzie za ustawieniem systemu, a nie za trybem ostatniego konta. Zostawiamy tak?

## 6. Uwagi poza wyglądem

- Płatności na dev pokazują dane LOG TMS nieaktualne od 27.10.2026. Ostrzeżenie na ekranie działa poprawnie, ale dane trzeba odświeżyć przed testami u użytkowników.
- Struktura na dev ma przy każdym dziale „kierownik niewskazany”. Do uzupełnienia w danych, jeśli ma być pokazywana na testach.
