<!--
opis: Przegląd stylów ZAD-306 (tryb ciemny): znaleziska kontrastu i pól na telefonie, odpowiedź tryb jasny bez zmian TAK
rodzaj: wynik
zadanie: ZAD-310
rozmowa: ZAD-306 Tryb ciemny i tokeny marki · Programista frontend
zmiana: 16.11.2026, ZAD-306 Tryb ciemny i tokeny marki · Programista frontend
status: do akceptacji
slowa: tryb ciemny, tokeny marki, kontrast, accent-text, size-input-touch, przegląd, ZAD-306
-->

# Zlecenie 02 — przegląd stylów ZAD-306, tryb ciemny (Programista frontend)

Gałąź `zad-306-tryb-ciemny`, drzewo `storage/worktrees/intranetFebo-zad306`, zakres `git diff origin/main...HEAD -- web/`.
Kontrakt: core-ui 2.32 (§2.1–2.3, §2.6), wartości FEBO porównane także z 2.33 z `main`.
Nic nie commitowane ani nie pushowane, drzewo robocze bez zmian (build szedł do katalogu poza repozytorium).

## Odpowiedź wprost

**Tryb jasny bez zmian: TAK.**

Blok `:root` z `origin/main` porównany z blokiem z gałęzi (wygenerowany oboma generatorami, z nakładką FEBO i bez):
różnica to wyłącznie cztery DODANE linie — `--color-accent-text: var(--color-accent)`, `--type-size-input-touch: 1rem`,
`--type-size-field-label: 0.8125rem`, `color-scheme: light`. Żadna istniejąca wartość nie zmieniła się.
`accent-text` w jasnym to alias akcentu, `size-field-label` to te same 13 px co wcześniej, więc wszystkie miejsca
przepięte na nowe tokeny wyglądają identycznie. Jedyna celowa zmiana w jasnym: pola na telefonie (< 768 px) dostają 16 px;
przy rozmiarze treści 1rem większość pól i tak go miała. Zrzuty 01 i 04 (jasny wybrany przy ciemnym systemie) są bajtowo równe.

## Znaleziska (bez dublowania 01-programista.md)

Kontrast podany jako: powłoka jasny / powłoka ciemny / FEBO jasny 2.33 / FEBO ciemny 2.33. Próg dla tekstu 4,5, dla elementów graficznych 3.

### 1. Przycisk destrukcyjny po najechaniu — biały napis na jasnoczerwonym tle w ciemnym
- **Miejsce:** `web/src/ui/ui.css:107–108` (`.ui-button--dangerous:hover` — `background: var(--color-danger); color: var(--color-accent-ink)`).
- **Co źle:** `accent-ink` (biały) na `danger`: 9,60 / **3,17** / 9,60 / **3,17**. W ciemnym `danger` jest rozjaśniony (#f0616c), a napis zostaje biały.
- **Dla użytkownika:** w chwili, gdy ktoś celuje w „Usuń”, napis przycisku robi się słabo czytelny.
- **Waga:** warto poprawić (wysoko — przycisk destrukcyjny).
- **Poprawka:** token tekstu na tle `danger` (np. `danger-ink`: w jasnym alias `accent-ink`, w ciemnym ciemny kolor) albo `color: var(--color-surface)` — w ciemnym powłoki daje 4,85, w jasnym zostaje biały (bez zmiany wyglądu). Lepsza `dominant`: 6,06 w ciemnym, ale w jasnym zmieniłaby wygląd.

### 2. Alert w Płatnościach po najechaniu — wykrzyknik znika w ciemnym
- **Miejsce:** `apps/payments/web/payments.css:112–115` (`.payments-alert:hover` — `background: var(--color-danger); color: var(--color-on-dark)`).
- **Co źle:** `on-dark` na `danger`: 7,18 / **2,37** / 6,47 / **2,14**.
- **Dla użytkownika:** znacznik alertu przy wierszu płatności po najechaniu zlewa się z tłem.
- **Waga:** warto poprawić (ten sam typ co pkt 2 Programisty, inna linia).
- **Poprawka:** jak w pkt 1 — ten sam token tekstu na `danger`.

### 3. Czerwona akcja po najechaniu — biały na jaśniejszym akcencie (też w jasnym, sprzed zmiany)
- **Miejsce:** `web/src/ui/ui.css:115` (`.ui-button--accent:hover` — tło `accent-on-dark`, napis `accent-ink`).
- **Co źle:** 2,74 / 2,47 / 2,99 / 2,99. Problem istnieje już w jasnym na `main`, a wartości FEBO z 2.33 (#E7726B) go utrwalają.
- **Dla użytkownika:** jedyna czerwona akcja ekranu (BR-UI-002) po najechaniu ma blady, słabo czytelny napis.
- **Waga:** warto poprawić — osobne zadanie albo ZAD-308 (nie blokuje ZAD-306, bo nie jest regresją).
- **Poprawka:** hover przez ciemniejszy akcent zamiast jaśniejszego, przez obramowanie, albo osobny token `accent-hover`.

### 4. Przycisk nieaktywny w ciemnym — najjaśniejszy element ekranu
- **Miejsce:** `web/src/ui/ui.css:120–122` i `web/src/brand/base.css:506–508` (`[disabled]` — napis `canvas`, tło `muted`).
- **Co źle:** kontrast w porządku (7,11), ale w ciemnym `muted` jest jasnoszary (#a0a5ad / #A3A3A3), więc nieaktywny przycisk to jasny blok, mocniejszy od aktywnych.
- **Dla użytkownika:** odwrócona hierarchia — wyłączony przycisk przyciąga wzrok bardziej niż ten, który działa (widać na zrzutach 02/03).
- **Waga:** warto poprawić (wygląd), do decyzji w ZAD-308.
- **Poprawka:** w ciemnym tło `canvas`/przezroczyste, napis `muted`, ramka `border` — tak jak już robi `.ui-card__header .ui-button[disabled]`.

### 5. Ramki w kolorze `dominant` znikają w ciemnym
`dominant` na `surface` 1,25–1,31, na `canvas` 1,09–1,10.
- `web/src/brand/base.css:799` `.app-tile:hover { border-color: var(--color-dominant) }` — **warto poprawić**: najechanie na kafel aplikacji wygląda jak zniknięcie ramki, a nie jej wzmocnienie.
- `apps/directory/web/directory.css:282` `.dir-box__deeper:hover` — **warto poprawić**, ten sam efekt w Katalogu.
- `web/src/brand/base.css:379` `.page__header` (kreska 3 px pod nagłówkiem strony) — **drobiazg**: kreska znika, nagłówek dalej czytelny.
- `web/src/ui/ui.css:52` obrys przycisku podstawowego — **drobiazg**, napis czytelny.
- **Poprawka:** hover i kreska przez `--color-ink` albo `--color-muted` (w jasnym zmiana minimalna, więc decyzja człowieka), albo osobny token ramki mocnej z wartością ciemną/jasną.

### 6. Reguła pól na telefonie nie obejmuje trzech pól Katalogu
- **Miejsce:** `apps/directory/web/directory.css:362–363` (`.dir-calendar__pick select`), `531–532` (`.dir-absence__form input`, pole daty), `641–642` (`.dir-panel__edit input`) — `font: inherit` przy tej samej specyficzności (0,1,1) co reguła powłoki, a arkusz Katalogu ładuje się później, więc wygrywa.
- **Zmierzone w Chrome:** te trzy pola mają 13 px przy szerokości 500 i 1200 (rodzic ma rozmiar podpisu). Pola powłoki dostają 16 px.
- **Dla użytkownika:** na iPhonie Safari powiększy stronę przy wejściu w wybór miesiąca, datę nieobecności i edycję w panelu.
- **Waga:** warto poprawić (pkt 3 zlecenia).
- **Poprawka:** podnieść specyficzność reguły powłoki, np. `:root :is(input, select, textarea):not([type='checkbox'], [type='radio'])` (0,2,1) — działa dla każdego lokatora bez dotykania jego arkusza; albo w Katalogu `font-family: inherit` zamiast `font: inherit`.

### 7. Drobiazgi sprzed zmiany (do wiadomości)
- `web/src/ui/ui.css:1054` `color: var(--color-text)` oraz `apps/payments/web/payments.css:268, 284` `color: var(--color-ink-muted)` — tokeny nie istnieją. Kolor dziedziczy się, więc w obu trybach wygląda poprawnie, ale brand-lint tego nie łapie. **Drobiazg:** zamienić na `--color-ink` / `--color-muted`.

## Sprawdzone i w porządku
- Akcent jako tekst w powłoce wszędzie przez `--color-accent-text`; jedyne `color: var(--color-accent)` zostały w Katalogu (poza zakresem decyzją PM z 16.11).
- Brak kolorów wpisanych na sztywno w CSS (#hex, rgb, white, black — trafienia to tylko komentarze i `white-space`) i w TSX.
- SVG: Glyph, DurationChart, TaskListScreen biorą `currentColor`; `.chart svg` dostał kolor z `accent-text`. Brak bibliotek wykresów i map, brak plików .svg. `ShortcutBar.tsx:27` używa `getComputedStyle` tylko do breakpointu.
- `apps/directory/web/directory.css:480–492` (komórki kalendarza): ciemny tekst na jasnych kolorach statusu, 5,6–6,3 — czytelne w ciemnym.
- Krawędź grafu „never” (akcent) w ciemnym 3,70 / 3,84 — powyżej 3:1; etykiety grafu w `ink`.
- Obecność wybrana (`accent-ink` na `accent`) 4,66; hover przycisku podstawowego (`on-dark` na `elevated`) 12–13; kropka w szynie (`accent` na `dominant`) 4,03 / 4,25.
- Belka i nagłówek tabeli w ciemnym są ciemniejsze od karty — zgodnie z §2.1. Zasłona modala działa w ciemnym.
- Natywne pola daty, listy i paski przewijania zmieniają się z trybem (meta `color-scheme` + `color-scheme` w blokach) — widać na zrzutach.
- Reguła telefonu: 767 px zgadza się z `breakpoint-tablet` 768; działa dla `.ui-field__input`, `.shell-search__input`, `textarea`, `select`, `.filters__field select`, `.dir-search input` (16 px); nie rusza checkboxów i radio.
- Etykiety pól (`.ui-field__label`, `label`) 13 px — bez zmiany.
- Trzy warianty wyboru trybu: systemowy ciemny (zrzut 02) = `data-theme="dark"` (03), a `data-theme="light"` przy ciemnym systemie (04) = jasny (01) — bajtowo.
- Build `vite build` (do katalogu poza repozytorium): OK, 2,19 s, jedno ostrzeżenie o wielkości paczki (sprzed zmiany).
- Brak zależności od Tablera.

## Do wiadomości (potwierdzone, nie dubluję)
- Na zrzutach widać znane znaleziska Programisty: tytuł belki i „Wyloguj” niewidoczne w ciemnym, hover „Wgraj plik” bez napisu, węzły grafu znikają (base.css:146/708/1225, payments.css:74–77, ui.css:1000).
- Pkt 11 i 12 Programisty: Katalog z czerwonym tekstem; FEBO bez `color-dark` w nakładce ma w ciemnym niebieski akcent powłoki — widać na zrzucie, do ZAD-025.

## Zrzuty
Katalog `zespol/wyniki/zrzuty-02/` (strona testowa z prawdziwym zbudowanym CSS gałęzi i klasami komponentów; stany hover pokazane stylem z tymi samymi tokenami):
- `01-komponenty-jasny.png`
- `02-komponenty-ciemny-systemowy.png`
- `03-komponenty-ciemny-wybrany.png`
- `04-jasny-wybrany-przy-ciemnym-systemie.png`

Ograniczenie: zrzutów prawdziwych ekranów aplikacji nie ma — podgląd wymaga API i logowania na dev, a zlecenie wyklucza wdrażanie. Warto je zrobić przy odbiorze na dev.

## Podsumowanie wag
- Blokuje: brak nowych (blokujące są w 01-programista.md).
- Warto poprawić przed scaleniem: 1, 2, 6, 5 (kafle i Katalog).
- Warto poprawić osobno / ZAD-308: 3, 4.
- Drobiazgi: 5 (nagłówek strony, obrys primary), 7.
