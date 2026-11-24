<!--
opis: Założenie: narzędzie jest lokatorem Intranetu Canexpol (wspólne logowanie i wygląd, własny moduł).
rodzaj: zalozenie
rozmowa: Intranet — ustalenia projektu
zmiana: 18.11.2026, Intranet — ustalenia projektu
status: roboczy
slowa: lokator, intranet, logowanie, SSO, wygląd, moduł, architektura
-->
# Założenie: narzędzie jako lokator Intranetu Canexpol

## Treść założenia

Narzędzie budowane w tym projekcie jest **lokatorem Intranetu Canexpol**, a nie osobną aplikacją:

| Obszar | Ustalenie |
|---|---|
| Logowanie | Wspólne z Intranetem Canexpol: jedno konto i jedna sesja, bez osobnych użytkowników narzędzia. |
| Wygląd | Wspólny z Intranetem: ten sam system wizualny, nawigacja i komponenty. |
| Kod i dane | Własny moduł narzędzia: osobne ekrany, logika i tabele, wpięte w powłokę Intranetu. |

## Skutki dla dokumentacji (do zweryfikowania)

- **ARCHITECTURE.md** (Auth Model) opisuje POC z logowaniem **niezależnym**, bez SSO. Przy budowie od zera
  to założenie zastępuje tamten zapis; ARCHITECTURE wymaga aktualizacji.
- System wizualny: punkt wyjścia `zalozenia/canexpol_intranet_visual_assumptions.json`.
- Role i uprawnienia narzędzia: do ustalenia, czy biorą się z ról Intranetu, czy narzędzie ma własne role w ramach wspólnego konta.

## Źródło

Decyzja zgłoszona w rozmowie ustaleń projektu, 18.11.2026.
