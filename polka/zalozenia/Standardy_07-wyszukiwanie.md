<!--
opis: Standardy FEBO, część 07: wyszukiwanie procedur.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,wyszukiwanie
-->
# Karta 7 — wyszukiwanie pytaniem zadanym po ludzku (T-57i)

Gałąź: `t-57i-wyszukiwanie`

## Środowisko

`ssh dev-febo`, `/home/ubuntu/febo-intranet`, `git pull` na `main`. Karty 1–6 scalone.

**Klucz do usługi jest już w konfiguracji serwera roboczego** — `ASSISTANT_API_BASE_URL`,
`ASSISTANT_API_KEY`, `ASSISTANT_TIMEOUT_SECONDS`. **Nie przenoś go do repozytorium**:
w projekcie ląduje sama nazwa zmiennej i pusty wpis we wzorze konfiguracji.

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/contract/plan.contract.md` — karta **T-57i**
- `docs/external/redai-instrukcja-integracji-1.0.md` — **instrukcja wykonawcy, w całości**
- `docs/external/redai-api.spec.md` — nasza umowa z dostawcą
- `api/apps/standards/RULES.md` — reguły **BR-STD-024, BR-STD-030**
- `docs/decisions/standardy-implementacja.decisions.md` — rozdziały 2–6
- `docs/contract/core-search.contract.md` — rozdział 7.2, kolumna wektora

## Co powstaje

**Pierwsze wywołanie wychodzące poza Intranet.** Usługa po stronie redAi jest zbudowana
i sprawdzona — ta karta to nasza połowa.

- **Klient API wołany wyłącznie z zadania w kolejce**, nigdy z przeglądarki: limit czasu
  **90 sekund** (nie skracać — składanie procedury potrafi dobić do 80 s), **do trzech
  ponowień z tym samym `X-Request-Id`** (powtórzenie wraca z zapisu i nie jest liczone
  drugi raz), obsługa **wszystkich** udokumentowanych kodów błędów.
- **Wektory**: liczone przy publikacji z `kind=passage`, pytanie z `kind=query`.
  **Pominięcie tego drugiego po cichu obniża trafność, a wyniki nadal wyglądają sensownie**
  — dlatego jest groźne. Wymiar **384**, wektory znormalizowane, porównanie iloczynem
  skalarnym. Rozszerzenie wektorowe jest w obrazie bazy od 31.08.2026, ale **nieaktywne** —
  **ta karta włącza je migracją**, żeby środowiska docelowe dostały je tą samą drogą.
- **Kandydatów wybieramy my**, z uwzględnieniem uprawnień pytającego. Usługa nigdy nie
  decyduje, kto co może zobaczyć.
- **Ekran ustawień asystenta** dla opiekuna aplikacji: persona, licznik zużycia za bieżący
  miesiąc, wyłącznik.
- **Rejestr wywołań**: data, osoba, rodzaj wywołania, jednostki, identyfikator żądania,
  wynik — **bez treści** (BR-STD-030). Fakt „pytanie nie znalazło żadnej procedury"
  odnotowujemy, samego pytania nie.

**Persona startowa** (opiekun poprawi ją na ekranie ustawień):

> Rzeczowy asystent spisujący procedury firmy transportowej. Pyta o jedną rzecz naraz,
> prostym językiem. Dopytuje o konkret zamiast przyjmować ogólniki — o liczby, terminy,
> nazwy systemów i osoby odpowiedzialne. Nie podpowiada gotowych odpowiedzi, bo procedurę
> zna pracownik, a nie asystent.

## Na co uważać

**Ścieżka awaryjna jest sednem tej karty, nie dodatkiem.** Przy niedostępnej usłudze zwykła
wyszukiwarka i edytor mają działać dalej, a człowiek ma zobaczyć informację, że asystent
jest chwilowo niedostępny. **Sprawdź to, kierując klienta na martwy adres** — nie przez
uznanie, że gałąź kodu wygląda poprawnie.

Zestaw kontrolny do sprawdzenia trafności jest gotowy w
`docs/external/redai-materialy/05-zestaw-kontrolny.md`: dwanaście pytań z oczekiwanym
trafieniem, **cztery, na które poprawną odpowiedzią jest pusta lista**, i cztery dwuznaczne.

## Cztery próby z instrukcji wykonawcy

Uruchom je ze środowiska roboczego i pokaż wyniki:

1. krok wywiadu z pustą listą odpowiedzi → wraca pierwsze pytanie metryki,
2. to samo wywołanie z tym samym `X-Request-Id` → identyczna odpowiedź i nagłówek
   `X-Idempotent-Replay: true`,
3. pytanie spoza zakresu przekazanych procedur → **pusta lista dopasowań**,
4. klucz roboczy pod adresem docelowym → `401` z kodem `wrong_environment`.

## Protokół

Plan w trybie tylko do odczytu, akceptacja, gałąź, `make check` i `make review`,
`docs/progress.md`, commit i push jako **redAi \<redai@febologistic.com\>**.

## Karta jest skończona, gdy

BR-STD-024 i BR-STD-030 przechodzą; cztery próby powyżej dają opisane wyniki; **w rejestrze
wywołań nie ma ani jednej treści**.
