<!--
opis: Notka z projektu PPWR: narzędzie PPWR będzie modułem Intranetu (wspólne logowanie i wygląd, własny moduł); makieta HTML 02.12.2026
rodzaj: zalozenie
rozmowa: PPWR — ustalenia projektu
zmiana: 18.11.2026, PPWR — ustalenia projektu
status: roboczy
slowa: PPWR, lokator, moduł, logowanie, wygląd, makieta
-->
# Nowy lokator Intranetu: moduł PPWR

**Skąd:** projekt PPWR (dostosowanie opakowań do rozporządzenia UE 2025/40), czat ustaleń, 18.11.2026.

## Co ustalono

Narzędzie projektu PPWR nie będzie osobną aplikacją. Powstanie jako **moduł w Intranecie Canexpol**:

- **wspólne logowanie** — użytkownicy wchodzą tym samym kontem co do reszty intranetu;
- **wspólny wygląd** — ten sam układ i system wizualny co platforma;
- **własny moduł** — PPWR ma swój zakres, ekrany i dane, a platforma daje mu powłokę.

## Co to znaczy dla Intranetu

- Platforma (poziom A: logowanie z rolami, pulpit, uruchamianie serwisów) dostaje drugiego lokatora,
  obok modułu „Zamówienia Surowców”. Warto to uwzględnić przy projektowaniu ról, uprawnień i menu.
- Pierwszy punkt kontrolny po stronie PPWR to **makieta HTML modułu, termin 02.12.2026**
  (kamień zgłoszony w projekcie PPWR, czeka na akceptację PM). Makieta powinna używać wyglądu intranetu.
- Moduł PPWR zbiera dane opakowaniowe od dostawców i dokumentację zgodności opakowań.
  Dane PPWR są trzymane poza WAPRO (WAPRO zostaje systemem danych podstawowych).

## Co jeszcze otwarte

- W projekcie PPWR trzeba jeszcze potwierdzić, kto podjął tę decyzję i czy rozstrzyga ona
  „budować czy kupić” moduł PPWR (zadanie w backlogu PPWR).
- Po stronie Intranetu: kiedy platforma będzie gotowa przyjąć moduł i jakie role PPWR mają dostać.

Źródło po stronie PPWR: koncepcja wdrożenia (plik 06), decyzja architektoniczna ADR-006.
