<!--
opis: Intranet Canexpol to kolejne wdrożenie produktu Intranet (powłoka z lokatorami), zamiast budowy od zera
rodzaj: zalozenie
rozmowa: Intranet — ustalenia projektu
zmiana: 18.11.2026, Intranet — ustalenia projektu
status: roboczy
slowa: wdrożenie, produkt, powłoka, lokator, repozytorium, marka, architektura, od zera
-->
# Intranet Canexpol jako kolejne wdrożenie produktu Intranet

**Decyzja:** Paweł Mazek (PM), 18.11.2026, czat ustaleń projektu.
**Zastępuje:** decyzję z 14.10.2026, że aplikacja docelowa będzie budowana od zera.
**Rozwija:** `zalozenia/lokator-intranetu.md` (narzędzie jako lokator Intranetu Canexpol).

## Co postanowiono

Intranet Canexpol nie powstaje od zera. Jest **kolejnym wdrożeniem produktu Intranet**, czyli powłoki
z lokatorami rozwijanej w projekcie intranet2.0 (repozytorium `SmallBigSteps/intranet`, opis w `dev/intranet.md`).
Pierwsze wdrożenie tego produktu to FEBO.

| Warstwa | Co to jest dla Canexpol | Skąd |
|---|---|---|
| Produkt Intranet (powłoka) | konta i role, logowanie, lista zadań, powiadomienia, pliki, wyszukiwarka, dziennik, kokpit z kafelkami aplikacji; aplikacje ogólne (Struktura i kontakty, Procedury) | repozytorium `SmallBigSteps/intranet`, bez zmian pod Canexpol |
| Wdrożenie Canexpol | marka (nakładka na neutralny pakiet marki), nazwa systemu, stanowiska i przypisanie do ról, lista zainstalowanych aplikacji, łączniki do systemów źródłowych (WAPRO, LAB5/ELAB) | osobny pakiet wdrożenia Canexpol, na wzór pakietu FEBO |
| Aplikacje dziedzinowe Canexpol (lokatory) | pierwsza: Zamówienia Surowców; kolejne, np. PPWR | każda jako folder aplikacji w formacie produktu |

## Skutki

- **Logowanie i wygląd** daje powłoka; to realizuje założenie o lokatorze bez własnego mechanizmu logowania.
- **Stos** wynika z produktu: Python i Django, PostgreSQL, React z TypeScriptem, Docker. Stos POC
  (Laravel, SQLite) przestaje być docelowy; kod POC zostaje materiałem źródłowym do reguł i ekranów.
- **ARCHITECTURE.md** (POC) wymaga aktualizacji: stos, logowanie, model danych i sposób instalacji aplikacji.
- **Makieta HTML (KM-1)** powstaje w układzie powłoki (belka, panel boczny, kokpit, lista zadań) z marką
  Canexpol z `zalozenia/canexpol_intranet_visual_assumptions.json`. Wzór sposobu pracy: makiety HTML marki
  w repozytorium (`docs/external/brand/`).
- **Repozytorium kodu Canexpol** (ZAD-004): pakiet wdrożenia i aplikacje Canexpol potrzebują własnego
  repozytorium; repozytorium produktu jest wspólne i tylko do odczytu z naszej strony.

## Do potwierdzenia z zespołem intranet2.0

- Czy domknięto karty przenośności: test powłoki bez aplikacji FEBO (ZAD-134) i uruchomienie drugiej,
  fikcyjnej firmy bez zmian w kodzie powłoki (ZAD-135). W stanie prac repozytorium z 16.11.2026 nie ma
  ich na liście gotowych.
- Jak zmiany w produkcie wymagane przez Canexpol trafiają do powłoki (zgłoszenie do projektu intranet2.0).
