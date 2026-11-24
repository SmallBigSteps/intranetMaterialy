<!--
opis: Notka zadania kodu ZAD-276: https://github.com/SmallBigSteps/intranet/pull/new/zad-276-znacznik-pola-wymaganego
rodzaj: wynik
zadanie: ZAD-276
rozmowa: ZAD-276 Znacznik pola wymaganego we wzorniku
zmiana: 13.11.2026, ZAD-276 Znacznik pola wymaganego we wzorniku
status: do akceptacji
-->
# ZAD-276: Znacznik pola wymaganego we wzorniku (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-276-znacznik-pola-wymaganego
- Co zmieniono: Wspólne pole Intranetu pokazuje przy etykiecie słowo „wymagane” i ma aria-required (bez natywnego required — błąd zostaje pod polem). Formularze logowania, hasła, resetu i TOTP dostały ten sam wiersz etykiety (FieldLabel). Oznaczone pola wymagane w oknie wstrzymania (powód sprawdzany teraz też na ekranie), okresach przechowywania, resecie TOTP, widokach listy zadań, strukturze, tokenach zasilania, odrzuceniu zadania, uwagach i Standardach. Kontrakty: core-ui 2.27, rules 3.35 (BR-UI-010, 232 reguły). Commit 2db3528.
- Jak sprawdzić: W web/ z INTRANET_DEPLOYMENT=febo_deployment: npm run typecheck, npm run lint, npm test (589 testów zielonych, w tym tests/rules/br_ui_010.test.tsx). Na dev: Pliki › Przechowywanie › Wstrzymaj sprawę — przy Powodzie i Dacie przeglądu słowo „wymagane”, puste pola dają błąd pod polem; ekran logowania — „wymagane” przy e-mailu i haśle; wzornik — pole Numer rejestracyjny.
