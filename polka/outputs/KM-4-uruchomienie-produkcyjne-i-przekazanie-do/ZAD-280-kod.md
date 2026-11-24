<!--
opis: Notka zadania kodu ZAD-280: https://github.com/SmallBigSteps/intranet/pull/new/zad-280-czas-polski
rodzaj: wynik
zadanie: ZAD-280
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-280 Czas polski w całej powłoce
zmiana: 13.11.2026, ZAD-280 Czas polski w całej powłoce
status: do akceptacji
-->
# ZAD-280: Czas polski w całej powłoce (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-280-czas-polski
- Co zmieniono: Wszystkie daty i godziny w powłoce pokazują czas polski (Europe/Warsaw) zamiast czasu przeglądarki: wspólne dayOf, momentOf i nowe briefMomentOf w web/src/ui/dates.ts; 7 ekranów z własnym formatowaniem przełączonych (kolejka zadań: historia i wykres, audyt, termin zadania, wyszukiwarka, marker świeżości, dane źródła). Pozostaje: po scaleniu ZAD-046 zastąpić polishMomentOf funkcją momentOf z ui.
- Jak sprawdzić: W web/: npm run lint, npm run typecheck, npm test (412 testów zielonych); dodatkowo TZ=America/New_York npx vitest run i TZ=Asia/Tokyo npx vitest run dają ten sam wynik. W przeglądarce z ustawioną inną strefą czasową godziny w Kolejce, Audycie i historii zadania zgadzają się z czasem w Polsce.
