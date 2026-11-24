<!--
opis: Notka zadania kodu ZAD-270: https://github.com/SmallBigSteps/intranet/commit/356d799
rodzaj: wynik
zadanie: ZAD-270
rozmowa: ZAD-270 Pułapka fokusu w oknach
zmiana: 13.11.2026, ZAD-270 Pułapka fokusu w oknach
status: do akceptacji
slowa: modal,fokus,klawiatura,dostepnosc,WCAG
-->
# ZAD-270: Pułapka fokusu w oknach (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/commit/356d799
- Co zmieniono: Wspólne okno dialogowe trzyma Tab i Shift+Tab w sobie (grupa radio liczy się jako jeden przystanek), a po zamknięciu oddaje fokus elementowi, który je otworzył, albo głównej treści strony; dotyczy wszystkich okien naraz. Gałąź zad-270-pulapka-fokusu; PR do założenia: https://github.com/SmallBigSteps/intranet/pull/new/zad-270-pulapka-fokusu. Kontrakt core-ui 2.27 §6 już na main (dd7dc68).
- Jak sprawdzić: npm test w web: 6 nowych testów okna w components.test.tsx (padają bez poprawki); job e2e w CI: ścieżka 7, test 'a dialogue keeps Tab inside it and gives focus back when it closes'; ręcznie: otwórz Zgłoś uwagę, Tab i Shift+Tab nie wychodzą poza okno, Escape zamyka i fokus wraca na przycisk.
