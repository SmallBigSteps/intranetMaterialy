<!--
opis: Testy aplikacji Standardy FEBO na kontach ze struktury firmy (ZAD-004).
rodzaj: zalozenie
rozmowa: tenantStandardyDev
zmiana: 16.10.2026, tenantStandardyDev
status: roboczy
slowa: standardy,testy,konta,struktura
-->
# Standardy FEBO — testy na kontach ze struktury

Środowisko robocze · 18 września 2026 · przygotowane dla Pawła Mazka

## 1. Po co ta tura testów

Od poprzednich testów doszły rzeczy, o które prosiłeś w uwagach, oraz jedna duża: **asystent AI jest podłączony i działa**. Testujemy teraz na kontach prawdziwych pracowników ze struktury FEBO, żeby sprawdzić to, czego konta testowe nie pokazywały — czy zgłoszenie trafia do właściwego przełożonego i czy ludzie widzą tylko to, co powinni.

**Co jest nowe od ostatniego razu:**

- **Asystent AI** — z roboczo wpisanej notatki powstaje gotowa procedura, a poprawki zamawia się zwykłym zdaniem.
- **Ekran zadania przebudowany**: wejście do aplikacji, opis w treści, decyzja pod ręką; szczegóły pod zwinięciem.
- **Decyzja z dwóch miejsc** — z listy zadań i z poziomu samej procedury.
- **Odrzucenie dociera do autora**: dostaje zadanie z uwagami i wchodzi wprost do edytora.
- **Nowa kategoria**: autor prosi jednym kliknięciem, opiekunka nadaje kod i zatwierdza.
- **Historia zmian procedury** — oś zdarzeń na dole ekranu, zwinięta.
- **Wycofane procedury** znikają z domyślnej listy, ale odpowiadają pod starym adresem.

## 2. Konta do testów

Pięć osób ze struktury FEBO. Celowo nie ma tu Izabeli Młot-Wójcickiej, Daniela Urbaniaka ani Moniki Nowak — oni dostali własne zaproszenia i ustawiają hasła sami.

**Adres:** https://dev.febo.redai.pl
**Hasło do wszystkich pięciu kont:** `Standardy-Testy-2026`

| Kto | Stanowisko | Rola w teście | Login |
|---|---|---|---|
| Maciej Wrona | Specjalista ds. Zgłoszeń Celnych | **AUTOR** — pisze procedury | maciej.wrona@febologistic.com |
| Karolina Sztand | Kierownik Działu Celnego | **ZATWIERDZA** — przełożona Macieja | karolina.sztand@febologistic.com |
| Zofia Sznajder | Młodszy Agent Celny | **CZYTELNICZKA** — zapoznanie z procedurą | zofia.sznajder@febologistic.com |
| Piotr Szczęsny | Kierownik Floty | **INNY DZIAŁ** — sprawdzenie, kto zatwierdza | piotr.szczesny@febologistic.com |
| Magdalena Kowalczyk | Kierownik Biura | **OPIEKUNKA BIBLIOTEKI** — kategorie, wycofania | magdalena.kowalczyk@febologistic.com |

Wygodnie testuje się w dwóch oknach przeglądarki — jednym zwykłym i jednym prywatnym — żeby być zalogowanym jako dwie osoby naraz (np. autor i osoba zatwierdzająca).

## 3. Zanim zaczniesz — trzy rzeczy

- To środowisko testowe. Dane są próbne, a procedury, które napiszesz, zostaną w bibliotece.
- Nie ruszaj kont Izabeli, Daniela i Moniki — mają własne zaproszenia w toku.
- Po testach wygaszę te pięć kont (hasła i aktywacje), żeby prawdziwi pracownicy dostali normalne zaproszenia. Powiedz tylko, kiedy skończysz.

## 4. Scenariusze

### S1. Wejście i pierwsze wrażenie — Maciej Wrona

1. Zaloguj się jako Maciej Wrona.
2. Obejrzyj stronę główną: jakie aplikacje widzisz i czy wiadomo, po co każda jest.
3. Wejdź w Standardy i przejrzyj listę procedur.

**Na co patrzeć:** czy widać tylko procedury obowiązujące (szkice innych osób mają być niewidoczne) i czy da się zrozumieć, co jest czym, bez tłumaczenia.

### S2. Nowa procedura z asystentem — Maciej Wrona

1. Napisz nową procedurę w kategorii **Odprawy celne (CLO)**.
2. W polu asystenta opisz **własnymi słowami**, jak się coś u Was robi — np. jak wygląda zgłoszenie tranzytu T1 albo co robicie, gdy brakuje dokumentu od klienta. Cztery–pięć zdań wystarczy, bez porządkowania.
3. Poproś o szkic i poczekaj kilkanaście sekund, potem odśwież stronę.
4. Przeczytaj, co powstało: czy kroki są w Twojej kolejności, czy nic nie zostało zmyślone.
5. Zamów poprawkę zwykłym zdaniem, np. „skróć krok trzeci" albo „dopisz, co zrobić, gdy klient nie odbiera telefonu".
6. Sprawdź zdanie, które asystent zostawia nad tekstem — czy zgadza się z tym, co faktycznie zmienił.
7. Popraw coś jeszcze ręcznie w polu treści i zapisz szkic.

**Na co patrzeć:** czy poprawka **zachowała resztę tekstu**, czy tylko podmieniła wszystko na nowo.

### S3. Zgłoszenie i zatwierdzenie — Maciej, potem Karolina

1. Jako Maciej zgłoś procedurę do zatwierdzenia. Zobacz, kogo aplikacja wskazuje jako osobę decydującą.
2. Zaloguj się jako Karolina Sztand (okno prywatne) i wejdź na listę zadań.
3. Otwórz zadanie: czy z jednego ekranu wiadomo, co masz zrobić, i czy da się to zrobić bez szukania?
4. Zatwierdź procedurę.
5. Wróć jako Maciej i sprawdź, czy procedura obowiązuje.

**Na co patrzeć:** czy zadanie trafiło do **Karoliny** (przełożona autora) — to jest reguła, którą potwierdziłeś: procedura bywa międzydziałowa, więc decyduje przełożony autora, a nie kierownik działu, którego procedura dotyczy.

### S4. Odesłanie do poprawy — Karolina, potem Maciej

1. Jako Maciej napisz kolejną wersję istniejącej procedury i zgłoś ją.
2. Jako Karolina odeślij ją do poprawy z konkretną uwagą (np. „krok trzeci opisuje dwie różne rzeczy").
3. Zaloguj się jako Maciej i sprawdź listę zadań: czy jest tam zadanie o poprawce i czy wchodzi **wprost do edytora**.
4. Sprawdź, czy uwagi Karoliny widać nad tekstem, który masz poprawić.
5. Popraw i zgłoś ponownie. Sprawdź, czy zadanie o poprawce samo zeszło z listy.
6. Jako Karolina zatwierdź poprawioną wersję — tym razem **z poziomu samej procedury**, nie z listy zadań.

### S5. Procedura międzydziałowa — Maciej i Piotr Szczęsny

1. Jako Maciej napisz procedurę, która dotyczy pracy dwóch działów — np. przekazania dokumentów celnych kierowcy przed wyjazdem (Dział celny i Flota).
2. Zgłoś ją do zatwierdzenia.
3. Zaloguj się jako Piotr Szczęsny (Kierownik Floty) i sprawdź swoją listę zadań.

**Czego oczekiwać:** Piotr **nie** dostaje zadania — decyduje Karolina jako przełożona autora, a uzgodnienie z Piotrem odbywa się poza systemem. Tak ustaliłeś 18 września. Jeśli w praktyce okaże się to niewygodne, to jest dobry moment, żeby to powiedzieć.

### S6. Skierowanie do zapoznania — Karolina i Zofia

1. Przy zatwierdzaniu (albo zaraz po nim) skieruj procedurę do zapoznania dla działu celnego.
2. Zaloguj się jako Zofia Sznajder i sprawdź, czy ma zadanie „zapoznaj się".
3. Otwórz procedurę z poziomu zadania, przeczytaj i potwierdź zapoznanie.
4. Sprawdź, czy zadanie zeszło z listy Zofii.

**Na co patrzeć:** czy Zofia jest pytana o **przeczytanie**, a nie o zatwierdzenie — to dwie różne rzeczy.

### S7. Nowa kategoria — Maciej i Magdalena

1. Jako Maciej, pisząc procedurę, poproś o nową kategorię (np. „Tranzyt i procedury specjalne") — podaj samą nazwę i po co jest.
2. Załóż procedurę tymczasowo w istniejącej kategorii i pisz dalej.
3. Zaloguj się jako Magdalena Kowalczyk: czy ma zadanie o nowej kategorii i czy widzi podpowiedź kodu?
4. Popraw kod, jeśli podpowiedź Ci się nie podoba, i zatwierdź.
5. Wróć jako Maciej i przenieś procedurę do nowej kategorii jednym ruchem.

**Na co patrzeć:** czy po przeniesieniu **identyfikator procedury został taki sam**. Ma zostać — ludzie cytują go w mailach.

### S8. Historia zmian

1. Otwórz procedurę, przy której najwięcej się działo, i zejdź na dół ekranu.
2. Rozwiń „Pokaż, co się z tą procedurą działo".

**Na co patrzeć:** czy oś zdarzeń odpowiada na pytanie „kto to zrobił i kiedy" — założenie, zgłoszenia, zatwierdzenie, odesłanie z uzasadnieniem, skierowanie do zapoznania. Czy czegoś brakuje, żeby po pół roku dało się odtworzyć, co się stało?

### S9. Wycofanie z obiegu — Magdalena

1. Jako Magdalena wycofaj jedną z obowiązujących procedur, podając powód i wskazując następcę.
2. Sprawdź listę procedur: czy wycofana zniknęła z domyślnego widoku.
3. Zaznacz „Pokaż też wycofane" i sprawdź, czy ma etykietę **wycofana**.
4. Wejdź w nią ze starego adresu (skopiuj link przed wycofaniem) — czy mówi wprost, że nie obowiązuje, i czy wskazuje następcę?

### S10. Co kto widzi — Zofia

1. Jako Maciej zacznij nową procedurę i zostaw ją jako szkic.
2. Zaloguj się jako Zofia i poszukaj tej procedury na liście oraz w wyszukiwarce.

**Czego oczekiwać:** Zofia nie widzi cudzego szkicu ani na liście, ani w wyszukiwarce, ani w historii.

## 5. Jak zgłaszać uwagi

Najwygodniej numerem scenariusza i jednym zdaniem: co zrobiłeś, co się stało i czego oczekiwałeś. Zrzut ekranu pomaga, ale nie jest konieczny. Jeśli coś jest tylko nieładne, a nie zepsute — też to napisz, tylko zaznacz, że to uroda.

Oddzielnie warto zapisać rzeczy, które **Cię zaskoczyły**, nawet jeśli działają poprawnie. To zwykle najcenniejsze uwagi: aplikacja robi wtedy coś sensownego w sposób, którego nikt się nie spodziewa.

## 6. Czego jeszcze nie ma

- **Załączników do kreatora** (wrzucenie istniejącej procedury z pliku) — w robocie, DOCX i PDF.
- **Kanału zgłaszania uwag** z poziomu aplikacji — powstaje w osobnej rozmowie.
- **Wyszukiwania pytaniem zadanym po ludzku** — wchodzi razem z indeksem wektorowym.
