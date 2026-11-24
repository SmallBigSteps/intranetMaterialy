<!--
opis: Notka zadania kodu ZAD-334: https://github.com/SmallBigSteps/intranet/pull/new/zad-334-kapsulki-i-opcje-marki
rodzaj: wynik
zadanie: ZAD-334
kamien: KM-7 UX i UY
rozmowa: ZAD-334 Kapsułki i opcje marki
zmiana: 23.11.2026, ZAD-334 Kapsułki i opcje marki
status: do akceptacji
-->
# ZAD-334: Kapsułki i opcje marki (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-334-kapsulki-i-opcje-marki
- Co zmieniono: Opcje marki title-mark i mark-fill, token radius-pill dla znaczników statusu (core-ui 2.43, core-cockpit 3.23). Gałąź zawiera ZAD-323 (testy BR-TSK-027–029, FilterPill), więc scalać po PR ZAD-323 albo razem z nim.
- Jak sprawdzić: CI zielone (bramka reguł OK, 714 testów web); na dev z brand.json wdrożenia: option.title-mark=true daje kółko po tytule, option.mark-fill z kolorem aplikacji daje pole pod znakiem w katalogu, oknie szczegółów, na pulpicie i w szczegółach zadania
