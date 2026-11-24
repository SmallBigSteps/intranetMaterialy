<!--
opis: Raport przeniesienia projektu intranet2.0 do modułu PMO: co trafiło na półki, co zostało w rozmowach i dlaczego, wyjaśnienie uwag kontroli, otwarte sprawy dla PM.
rodzaj: wynik
zadanie: ZAD-174
etap: ETP-2 Pierwsze aplikacje u użytkowników
rozmowa: ZAD-174 Przeniesienie projektu do modułu PMO (półka, wiki, instrukcj
zmiana: 16.10.2026, ZAD-174 Przeniesienie projektu do modułu PMO (półka, wiki, instrukcj
status: do akceptacji
slowa: przeniesienie, półka, PMO, ZAD-174, raport
-->
# Raport przeniesienia projektu intranet2.0 do modułu PMO (ZAD-174)

Data: 16.10.2026. Akceptuje: Paweł Mazek (PM). Wzorce: czat 680 (integracjePromowe) i czat 681 (IntranetPlatnosci).

## 1. Wynik w skrócie

| Kryterium | Stan |
|---|---|
| Półka włączona, zadania mają etapy | tak; wszystkie zadania projektu mają etap (ZAD-128 i ZAD-129 dostały ETP-3) |
| Każdy plik raz, z opisem | tak; 59 plików na półce intranet20 i 20 na półce IntranetPlatnosci, bez dubli (md5), każdy z nagłówkiem albo wierszem spisu |
| Wiki do 150 linii, instrukcja w skrzynce | `_WIKI.md` ma 80 linii; nowa instrukcja czeka w skrzynce zmian (zgłoszenie 59) |
| Wyniki zrobionych zadań wskazują półkę, statusy bez zmian | ZAD-005 i ZAD-038 wskazują pliki w `outputs/`; jedyna zmiana statusu dziś to ZAD-174 (do zrobienia → w toku) |
| Kontrola półki | 14 otwartych uwag, każda wyjaśniona w punkcie 5 |
| Raport odłożony jako wynik ZAD-174 | ten plik |

Kopia zapasowa backlogu sprzed zmian (tabele zadań i zdarzeń): `storage/kopie/20261016-051551-zad-174-przed-przeniesieniem-plikow-na-p`.

## 2. Co trafiło na półkę intranet20

- `zalozenia/` (49): podsumowanie i brief dla wykonawcy, prompt-wytyczne lokatora, formatka listy pracowników
  i prośba do kadr (bez danych osobowych), przegląd UX i kodu z makietami (4 docx, 7 png), plan środowisk
  i opisy serwera (docx i 5 md), zrzuty z S3 (4 png), domknięcie S4, Standardy FEBO (11 md, 4 docx,
  instrukcja integracji API, uzupełnienia API, instrukcja i opis testów, podsumowanie), baza wiedzy
  audytu kodu AI, instrukcja uruchomienia test i prod (KM-5).
- `raw/` (5): specyfikacja serwera dev, PDF struktury FEBO z 17.08.2026 (jedna kopia), dwa maile
  (zasoby, hurtownia danych), notatka o refaktoryzacji z AI.
- `outputs/` (4): ETP-2 scenariusz testowy struktury i kontaktów oraz zamówienie API LogTMS (ZAD-005);
  ETP-1 raport i załączniki audytu kodu (ZAD-038).
- `dev/Repozytoria-i-srodowisko.md`: repozytoria, kopie robocze, stos, środowiska, decyzje o praktyce pracy z kodem.
- `_WIKI.md` projektu (80 linii): po co i dla kogo, co budujemy, założenia, gdzie co jest, otwarte pytania.

## 3. Co trafiło na półkę IntranetPlatnosci (decyzja PM: Płatności osobno)

- `zalozenia/` (16): 6 dokumentów o Płatnościach i Optimie, zrzut wykazu po poprawkach, 8 plików CSV
  z danymi testowymi, przykład rozliczeń, weryfikacja pliku rozliczeń z Optimy.
- `raw/` (4): widoki Optimy (ods), dwa zrzuty SQL Optimy, mail o potwierdzeniach zapłat.
- Drobiazg: `feboOptima.sql` ma końce linii CRLF, a nagłówek dodany przy odkładaniu ma LF; treść nienaruszona.

## 4. Repozytorium i instrukcja

- intranetFebo: README i 00-INDEKS mówią, że materiały robocze i wiki są na półce, a repozytorium zostaje
  jedynym źródłem założeń (commit 9b3c417 na `main`, podpis redAi, wypchnięty 16.10.2026).
- Instrukcja projektu (skrzynka zmian, zgłoszenie 59): „Co gdzie trafia” bez zmian, 5 zasad projektu
  i sekcja „Repozytoria projektu” (adresy, gałęzie, kto scala, odesłanie do ZAD-175).
- Praca z kodem zostaje jak dotąd (decyzja PM 16.10.2026); przejście na standard PMO rozstrzyga ZAD-175.

## 5. Uwagi kontroli półki (16.10.2026) i wyjaśnienia

| Uwaga | Wyjaśnienie |
|---|---|
| Instrukcja bez sekcji „Repozytoria projektu” | sekcja jest w nowej instrukcji w skrzynce (zgłoszenie 59); zniknie po akceptacji PM |
| Czat 363: 15 plików jako kopia zamiast skrótu | zamiana na skróty zgłoszona do skrzynki (zgłoszenie 55), czeka na PM |
| ZAD-007: DO AKCEPTACJI bez pliku w outputs | zadanie kodu, scalone w PR #173 (SmallBigSteps/intranetFebo); oddanie przez `pmo:odloz --pr` należy do czatu ZAD-007, dokumenty zadania są na półce IntranetPlatnosci |
| Limit linii: `zalozenia/baza-wiedzy-audyt-kodu-ai.md` (642) | materiał źródłowy w jednym kawałku; podział zostawiony do decyzji PM |
| Limit linii: `zalozenia/Serwer-lokalny-zasoby-kopie-kontenery-dostepy.md` (471) | jak wyżej |
| Czat 227 (inputData): 306 dokumentów | kopia robocza repozytorium intranetFebo (kod i założenia żyją w repo) oraz pliki modelu danych LogTMS i W-10b zgodne z repo |
| Czat 226: podsumowanie dla wykonawcy (md) | bliźniak dokumentu docx z półki |
| Czat 228: prompt-wytyczne lokatora (md) | bliźniak dokumentu docx z półki |
| Czat 278: status po S1 | podsumowanie sprintu, które jest w repo (`docs/sprints/`) |
| Czat 298: podsumowanie S2 | w repo (`docs/sprints/s2-podsumowanie-wejscie-do-s3.md`) |
| Czat 309: podsumowanie S3 | w repo (`docs/sprints/s3-podsumowanie-wejscie-do-s4.md`) |
| Czat 363: instrukcja testów Standardów (md) | bliźniak dokumentu docx z półki |
| Czat 452: higiena kontekstu | starsza wersja procedury z półki „standard” (`procedury/higiena-kontekstu.md`) |
| Czat 688 (ta rozmowa): plik instrukcji | uwaga z chwili odkładania; plik roboczy usunięty z rozmowy, treść jest w skrzynce |

## 6. Co zostało w rozmowach i dlaczego

- Kopie plików z repozytorium (zgodne treścią): W-10b, plany S3 i S4, podsumowania sprintów (czaty 278, 298,
  309, 318), model danych LogTMS (wgrane w czacie 227), zapytanie do Optimy (czat 536); s2-plan i specyfikacja
  WhatsApp różnią się o kilka linii od wersji w repo, obowiązuje repo.
- Starsze wersje plików z repozytorium: START.md (226), opis marki json (227), s1-plan (278).
- Bliźniaki md dokumentów docx z półki: czaty 226, 228, 363.
- Duplikaty wgranych plików: brief i podsumowanie (278), instrukcja integracji (355), podsumowanie Standardów (363),
  drugi PDF struktury (439).
- Wypełniona lista pracowników: dane osobowe, decyzja PM, nie na półkę.
- Materiały z diagnozy usterek (wklejki, logi, opisy błędów): decyzja PM, nie na półkę.
- Zapis przewijania czatu 362 i plik uwag do PMO 2.8.1 wgrany w tej rozmowie: materiały techniczne.

## 7. Otwarte sprawy dla PM

1. Akceptacja instrukcji projektu (skrzynka, zgłoszenie 59) i zamiany kopii w czacie 363 (zgłoszenie 55).
2. ZAD-176 Repozytorium materiałów: półka trafi do osobnego prywatnego repozytorium projektu (do założenia,
   z adresem i kluczem w portalu); dopiero wtedy w sekcji Repozytorium na stronie projektu pojawi się przycisk
   zgody PM. We wspólnym SmallBigSteps/febo (`projekty/intranet20/`) portal trzyma tylko opis, zadania i dziennik.
3. ZAD-175: przejście na standard repozytoriów (kopia poza katalogiem rozmowy, blokada wypychania na `main`).
4. ZAD-007: oddanie zadania kodu adresem PR #173.
5. Ewentualny podział dwóch długich plików w `zalozenia/`.
