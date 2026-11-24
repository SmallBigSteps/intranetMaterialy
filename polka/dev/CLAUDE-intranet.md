<!--
opis: Pamięć projektu kodu (dawny CLAUDE.md projektu): stos technologiczny, standardy kodu i instrukcje dla agentów AI budujących aplikację.
rodzaj: dev
rozmowa: zad001PolkiCPrzeniesienieMaterialow
zmiana: 14.10.2026, przeniesione z projects/intranet/context (ZAD-001)
status: roboczy
slowa: CLAUDE.md, stos, standardy kodu, agenci AI, Laravel
-->
# CLAUDE.md (Project Memory)

**Projekt:** Aplikacja „Zamówienia Surowców" — POC modułu B platformy redAI, Canexpol Group
**Wzorzec budowy:** Droga B — samodzielna aplikacja, własna baza, własne API (patrz `Specyfikacja-systemu-redAI.md`, rozdz. 10.2)
**Status:** POC — brak separacji dev/test/prod, brak automatycznego backupu (świadomie zaakceptowane ryzyko, patrz PRD `OOS-06`)

> Ten plik łączy fakty potwierdzone w dokumentacji projektowej (PRD, opis procesu) z **propozycjami konwencji technicznych** oznaczonymi jako *(propozycja)* — do zweryfikowania/skorygowania przy starcie implementacji.

---

## 1. Technical Stack

- **Backend:** Laravel 11.x (PHP 8.3) + Laravel Sanctum (własne API tokenowe) *(propozycja — spójność z ekosystemem redAI/CLab)*
- **Frontend:** Blade + Alpine.js + Tailwind CSS (build Vite) *(propozycja — spójne z silnikiem redAI)*
- **Baza danych:** **PostgreSQL**, dedykowana instancja z własnymi poświadczeniami — pełna izolacja od bazy MariaDB portalu redAI
- **Testing:** Pest *(propozycja — standard dla nowych projektów Laravel)*
- **Kolejka/cache:** do potwierdzenia; jeśli potrzebna — Redis (osobna instancja/DB index, nie współdzielona z redAI)

## 2. Build & Run Commands

```bash
# Instalacja
composer install
npm install

# Konfiguracja
cp .env.example .env
php artisan key:generate
php artisan migrate

# Dev run
php artisan serve
npm run dev

# Build produkcyjny
npm run build

# Testy
php artisan test                          # cała suita
php artisan test --filter=NazwaTestu      # pojedynczy test
./vendor/bin/pest tests/Feature/Sciezka.php
```

*(propozycja — do potwierdzenia po założeniu repo; brak dziś istniejącego `composer.json`/`package.json` dla tego projektu)*

## 3. Project Structure

*(propozycja — standardowa struktura Laravel, nazewnictwo dopasowane do domeny z PRD/`03-proces-zamowien.md`)*

- `app/Models/` → encje domenowe: `Recipe` (receptura, wielodostawczość), `RawMaterial`, `Supplier`, `Order`, `OrderRequirement` (zapotrzebowanie), `Delivery`, `Reservation`
- `app/Http/Controllers/Api/` → kontrolery REST (chronione Sanctum)
- `app/Http/Controllers/` → kontrolery widoków Blade (ekrany operacyjne)
- `app/Services/Integrations/` → klienci integracyjni: `WaproReadService`, `Lab5ReadService` — **wyłącznie odczyt**
- `app/Jobs/` → joby w tle (przeliczanie zapotrzebowania, generowanie draftów maili do dostawców)
- `resources/views/` → widoki Blade (Zamówienia, Zapotrzebowanie, Przyjęcia magazynowe, Dostawcy)
- `database/migrations/` → schemat PostgreSQL
- `routes/api.php` → endpointy API (Sanctum)
- `routes/web.php` → routing UI
- `tests/Feature/`, `tests/Unit/` → testy Pest

## 4. Coding Standards & Patterns

- **Architektura:** warstwowa — cienki Controller → Service → Model. Zasada „Rule = Test = Documentation": każda reguła biznesowa ma odpowiadający test.
- **Zasady:** KISS, DRY. Etap POC — unikać nadmiarowej abstrakcji i przedwczesnej generalizacji.
- **Naming:** `snake_case` dla kolumn DB, `PascalCase` dla klas PHP, `camelCase` dla metod.
- **Integracje zewnętrzne (WAPRO, LAB5/ELAB):** wyłącznie **odczyt**, chyba że PRD jawnie wymaga zapisu (`OOS-01`, `OOS-03`). Każdy wyjątek od zasady read-only musi mieć komentarz w kodzie odnoszący się do numeru User Story z PRD, który go uzasadnia.
- **Zasada „Draft + akceptacja człowieka" (`OOS-05`):** żadna funkcja nie wysyła maila ani nie składa zamówienia u dostawcy automatycznie. Każda taka akcja kończy się stanem `draft`/`pending_approval` oczekującym na potwierdzenie przez człowieka.
- **Model wielodostawczości** (US-05, US-11, US-14, US-15) jest centralnym modelem domenowym tej aplikacji — nie spłaszczać do wzorca „jeden produkt = jeden dostawca" znanego z WAPRO. To newralgiczny punkt całego PRD (patrz sekcja 6 PRD).
- **Zmiana dostawcy na zamówieniu** (US-14) musi zawsze sprawdzać listę klientów niepozwalających na zmianę dostawcy (US-15) przed wykonaniem operacji.

## 5. Agent Instructions (Critical)

- **Wersjonowanie jest obowiązkowe mimo statusu POC.** Referencyjne wdrożenie (FEBO) pokazało, że najświeższy kod bywa niezacommitowany i niebackupowany — to nie ma się powtórzyć tutaj. Każda zmiana trafia do gita.
- **Hosting tymczasowy (workaround POC):** dedykowana subdomena/własny serwer są dziś niedostępne. Na czas POC aplikację uruchamiaj w ramach istniejącej infrastruktury redAI (ten sam VPS), jako **osobny proces na dedykowanym porcie**, analogicznie do „Kategorii 3" ze wzorca referencyjnego (samodzielna aplikacja w katalogu roboczym, własne zależności, uruchamiana doraźnie/jako trwały proces przez Supervisor — nie wbudowana w kod portalu). To rozwiązanie **tymczasowe**: docelowa migracja na pełną „Drogę B" (własna subdomena przez nginx reverse proxy, stały proces przez Supervisor/systemd — szablony już istnieją w referencyjnym wdrożeniu) jest zaplanowana po zamknięciu POC.
- **Baza danych PostgreSQL musi pozostać odizolowana** od MariaDB portalu redAI — osobne poświadczenia, osobny host/instancja jeśli to możliwe na dostępnej infrastrukturze.
- Przed dodaniem nowej reguły biznesowej sprawdź `PRD.md` (User Stories US-01…US-29) i `03-proces-zamowien.md` (opis stanu obecnego) — nie wymyślaj reguł biznesowych ani zachowań systemów źródłowych (WAPRO, LAB5).
- **Środowisko regulowane:** żadna zmiana dot. receptur, surowców czy zamówień produkcyjnych nie może pomijać ścieżki akceptacji jakościowej (proces certyfikacji — patrz PRD sekcja 6, „Proces certyfikacji pozostaje bramką jakościową").
- Generuj testy Pest dla każdej nowej reguły przeliczania zapotrzebowania (US-10) — błąd w tej logice ma bezpośredni skutek produkcyjny (przestój), to najwyższe ryzyko w projekcie.
- Zakres POC obejmuje **wyłącznie surowce** (`OOS-02`) — nie rozszerzaj funkcjonalności na opakowania bez wyraźnej decyzji.
- Brak automatycznego backupu i separacji środowisk to świadomie zaakceptowane ryzyko fazy POC (`OOS-06`) — nie trzeba go domykać teraz, ale każda decyzja zwiększająca to ryzyko (np. destrukcyjne migracje bez kopii) wymaga jawnego ostrzeżenia użytkownika.

---

**Dokumenty źródłowe (kanoniczne, importować — nie re-elicytować):**
`PRD.md` · `03-proces-zamowien.md` · `02-koncept-wdrozenia-redAI.md` · `Specyfikacja-systemu-redAI.md` · `01-cele-biznesowe-transformacji-cyfrowej.md`
