<!--
opis: Architektura POC „Zamówienia Surowców”: infrastruktura, model danych, integracje (WAPRO, LAB5), bezpieczeństwo, NFR, testy.
rodzaj: zalozenie
rozmowa: zad001PolkiCPrzeniesienieMaterialow
zmiana: 14.10.2026, przeniesione z projects/intranet/context (ZAD-001)
status: roboczy
slowa: architektura, model danych, integracje, WAPRO, LAB5, bezpieczeństwo, testy
-->
# ARCHITECTURE.md

**Projekt:** Aplikacja „Zamówienia Surowców" — POC modułu B platformy redAI, Canexpol Group
**Status:** POC — decyzje poniżej odzwierciedlają świadomie ograniczony zakres tego etapu (patrz sekcja 7, ryzyka do rewizji)
**Powiązanie:** dokument techniczny towarzyszący `CLAUDE.md` (standardy kodowania i pamięć projektu); nie duplikuje Person/HLO (`PRD.md`) ani rejestru ryzyk (`CLIENT_BRD.md` / `02-koncept-wdrozenia-redAI.md`)

---

## 1. System Architecture & Infrastructure

- **Hosting/Deployment:** Wzorzec „Droga B" (samodzielna aplikacja, patrz `Specyfikacja-systemu-redAI.md` §10.2). Tymczasowo (workaround POC) uruchamiana w ramach istniejącej infrastruktury VPS redAI, jako osobny proces na dedykowanym porcie, utrzymywany przez Supervisor. Docelowo (po zamknięciu POC): migracja na własną subdomenę przez nginx jako reverse proxy, proces utrzymywany przez Supervisor/systemd — szablony wdrożeniowe już istnieją w referencyjnej instalacji redAI.
- **Środowiska:** brak separacji dev/test/prod na etapie POC — świadomie zaakceptowane ryzyko (PRD `OOS-06`). Oddzielne środowiska są celem post-POC (`US-20`).
- **CI/CD:** brak formalnego pipeline'u. Deploy ręczny przez SSH + `git pull` na serwerze, bez automatycznych bramek jakościowych.
- **Kompensacja braku CI (zasada procesowa):** ponieważ nie ma automatycznej bramki, **uruchomienie pełnej lokalnej suity Pest przed każdym ręcznym deployem jest obowiązkowe** — jedyna linia obrony jakości na tym etapie.
- **Rollback:** brak sformalizowanej strategii. W razie błędnego deployu — hotfix „na żywo" wykonywany bezpośrednio przez administratora platformy.
- **Baza danych — izolacja:** dedykowana instancja PostgreSQL z własnymi poświadczeniami, w pełni odizolowana od bazy MariaDB portalu redAI (wymóg z `CLAUDE.md`).

## 2. Data Model & Schema

- **Baza:** PostgreSQL, dedykowana instancja *(potwierdzone, `CLAUDE.md`)*.
- **Core Entities** *(propozycja, `CLAUDE.md` §3 — do potwierdzenia przy migracjach)*:
  - `Recipe` (receptura) → wiele `Supplier` (model wielodostawczości — centralny model domenowy, nie spłaszczać do „jeden produkt = jeden dostawca")
  - `RawMaterial` (surowiec) → powiązany z `Recipe` i `Supplier`
  - `Order` (zamówienie) → `OrderRequirement` (zapotrzebowanie przeliczone z receptury, US-10) → `Delivery` (dostawa) → `Reservation` (rezerwacja stanu)
  - `Supplier` → lista klientów blokujących zmianę dostawcy (US-15)
- **Audyt zmian:** pola minimalne — `created_at`, `updated_at`, `user_id`. **Brak** pełnej historii wartości (bez wzorca `*_history` stosowanego w siostrzanym systemie dev5/CLab).
  - ⚠️ **Ryzyko zaakceptowane na POC:** środowisko regulowane (GMP) zwykle wymaga pełnej śladowalności zmian dostawcy/zamówienia; wybrany model daje tylko snapshot „kto ostatnio zmienił", bez historii wartości. Do rewizji przed wdrożeniem produkcyjnym, analogicznie do ryzyk już zaakceptowanych w `PRD.md` (`OOS-06`).
- **Retention/Migration:** brak automatycznego backupu bazy i plików na etapie POC — świadomie zaakceptowane ryzyko (`US-22` = post-POC, wymaga backupu z okresowym testem odtworzenia). Migracje przez standardowy mechanizm Laravel (`php artisan migrate`).

## 3. API & Integrations

- **Internal Endpoints:** REST API pod Laravel Sanctum *(propozycja, `CLAUDE.md`)*; grupy endpointów wynikające z modelu domenowego: Zamówienia, Zapotrzebowanie, Dostawcy, Przyjęcia magazynowe.
- **Third-Party Services:**
  | Serwis | Cel | Tryb dostępu |
  |---|---|---|
  | WAPRO (WF-Mag, SQL Server) | Stany magazynowe, dokumenty | **Wyłącznie odczyt** (`OOS-01`) |
  | LAB5/ELAB (dev.lab5.pl) | Receptury, INCI | Głównie odczyt; zapis tylko tam, gdzie niezbędny do obsługi kroku procesu (`OOS-03`) |
- **Poczta do dostawców (US-16):** **brak integracji mailowej.** System wyłącznie generuje treść draftu; wysyłka odbywa się ręcznie, poza systemem, ze skrzynki użytkownika. To fizycznie (nie tylko proceduralnie) wymusza zasadę „Draft + akceptacja człowieka" (`OOS-05`) — system nie ma technicznej możliwości automatycznej wysyłki.
- **Vendor Isolation:** warstwa `app/Services/Integrations/{WaproReadService, Lab5ReadService}` jako adapter *(propozycja, `CLAUDE.md`)* — izoluje logikę domenową od szczegółów połączenia z systemami źródłowymi.

## 4. Security & Compliance

- **Auth Model:** logowanie **niezależne** od redAI/WAPRO/LAB5 — dedykowani użytkownicy tej aplikacji, tokeny API przez Sanctum. Brak SSO na etapie POC.
- **Role/persony (5):** Handlowiec, Technolog/Laborant, Specjalista ds. zapotrzebowania, Specjalista ds. zamówień, Specjalista ds. przyjęć magazynowych — provisioning kont ręczny przez administratora platformy.
- **Secrets Management:** zmienne środowiskowe w pliku `.env` na serwerze (standard Laravel), bez dodatkowego narzędzia typu vault. Dotyczy poświadczeń do WAPRO SQL Server i LAB5/ELAB.
- **Data Isolation Policies:** nie dotyczy w formie RLS/multi-tenant — aplikacja ma własną, dedykowaną bazę (Droga B), nie współdzieli danych z innymi instancjami.
- **Regulatory Implementation:**
  - Zasada „Draft + akceptacja człowieka" (`OOS-05`) — żadna funkcja (mail, zapis do systemu zewnętrznego, zamówienie u dostawcy) nie wykonuje akcji o skutku zewnętrznym automatycznie; brak integracji mailowej wzmacnia to technicznie.
  - Integracje WAPRO/LAB5 ograniczone do odczytu poza wyjątkami jawnie uzasadnionymi w PRD (`OOS-01`, `OOS-03`).
  - Proces certyfikacji (bramka jakościowa przed zamówieniem produkcyjnym) nie może być pomijany ani przyspieszany przez automatyzację (`03-proces-zamowien.md`).

## 5. Non-Functional Targets

- **Performance:** czas ładowania stron < 3s *(cel nieformalny, brak twardego SLA na POC)*.
- **Scalability/Availability:** dostępność w godzinach pracy (8:00–16:00), bez formalnego SLA. Uzasadnione małą skalą (kilku wewnętrznych użytkowników) i współdzieloną infrastrukturą VPS na etapie POC.
- **Accessibility:** brak zdefiniowanego poziomu WCAG — narzędzie wewnętrzne B2B, poza zakresem POC.
- **Observability:** wyłącznie standardowe logi Laravel (`storage/logs`), przeglądane ręcznie przez SSH. Brak zewnętrznego narzędzia monitoringu/alertingu na tym etapie (spójne z `OOS-04` — alerty/powiadomienia poza zakresem POC).

## 6. Testing Strategy

- **Tooling:** Pest *(propozycja, `CLAUDE.md`)*.
- **Critical Paths (obowiązkowe pokrycie testami integracyjnymi/E2E):**
  1. Przeliczanie zapotrzebowania z receptury (`US-10`) — najwyższe ryzyko biznesowe (błąd = przestój produkcyjny); wymóg już zapisany w `CLAUDE.md`.
  2. Zmiana dostawcy na zamówieniu + blokada wg listy klientów zakazujących zmiany (`US-14`/`US-15`).
  3. Sumowanie zamówień od wielu klientów w jedno zbiorcze zamówienie surowców (`US-09`).
  4. Weryfikacja przyjęcia dostawy vs zamówienie — wykrywanie rozbieżności (`US-18`).
- **CI Gating:** brak automatycznych bramek (brak CI/CD, sekcja 1). W zamian: **obowiązkowe ręczne uruchomienie pełnej suity Pest przed każdym deployem** jako zasada procesowa kompensująca brak pipeline'u.

## 7. Ryzyka POC do rewizji przed produkcją

Poniższe decyzje są świadomie uproszczone na etapie POC i wymagają rewizji przed skalowaniem poza POC (spójne z ryzykami już zidentyfikowanymi w `02-koncept-wdrozenia-redAI.md` §5):

- Brak separacji środowisk dev/test/prod.
- Brak formalnego CI/CD i automatycznego rollbacku.
- Brak automatycznego backupu bazy i plików.
- Minimalny audyt zmian (bez pełnej historii wartości) — potencjalna luka śladowalności w środowisku regulowanym GMP.
- Brak formalnego SLA i monitoringu poza godzinami pracy.

## 8. Traceability

- **PRD.md** — wizja, HLO/KPI, persony, User Stories (US-01…US-29), granice zakresu (OOS-01…OOS-06).
- **CLAUDE.md** — standardy kodowania, stack technologiczny, struktura projektu, zasady biznesowe.
- **Specyfikacja-systemu-redAI.md** — wzorzec platformy nadrzędnej (redAI), wzorce wdrożeniowe „Droga A/B/Hybrydowa".
- **03-proces-zamowien.md** — opis procesu bieżącego (stan „as-is"), uzasadnienie zakresu automatyzacji.
- **02-koncept-wdrozenia-redAI.md** — rejestr ryzyk technicznych i warunki wstępne przed skalowaniem poza POC.
