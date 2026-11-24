<!--
opis: Notka zadania kodu ZAD-293: https://github.com/SmallBigSteps/intranet/pull/new/zad-293-pliki-czas-polski
rodzaj: wynik
zadanie: ZAD-293
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-280 Czas polski w całej powłoce
zmiana: 13.11.2026, ZAD-280 Czas polski w całej powłoce
status: do akceptacji
-->
# ZAD-293: Moduł plików na wspólny czas (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-293-pliki-czas-polski
- Co zmieniono: Moduł plików pokazuje chwilę oczekiwania na potwierdzenie okresu przez wspólne momentOf z ui (czas polski, ZAD-280); własna polishMomentOf i jej test usunięte, tomorrow bierze strefę z TIME_ZONE. Commit 78f6e5b.
- Jak sprawdzić: W web: npm run lint, typecheck, format:check, npm test (506 zielonych, także pod TZ=UTC, America/New_York, Asia/Tokyo). Okresy przechowywania: wiersz czekający na potwierdzenie pokazuje 'od DD.MM.RRRR, GG:MM' w czasie polskim.
