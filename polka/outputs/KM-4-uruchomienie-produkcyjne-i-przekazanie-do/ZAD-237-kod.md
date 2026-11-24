<!--
opis: Notka zadania kodu ZAD-237: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-237-male
rodzaj: wynik
zadanie: ZAD-237
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: ZAD-237 Typy odpowiedzi: pozostałe moduły
zmiana: 02.11.2026, ZAD-237 Typy odpowiedzi: pozostałe moduły
status: do akceptacji
-->
# ZAD-237: Typy odpowiedzi: pozostałe moduły (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-237-male
- Co zmieniono: Siedem PR-ów łańcuchem: opis odpowiedzi w API i typy frontu z opisu dla pozostałych modułów powłoki i lokatora Standardy. PR1 https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-237-integracje (widok automatów i integracje), PR2 https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-237-konta, PR3 https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-237-kokpit, PR4 https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-237-kolejka, PR5 https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-237-uwagi, PR6 https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-237-standardy (z naprawą edytora Standardów), PR7 https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-237-male (źródła, usługi, wyszukiwarka, rejestr pól, przyjmowanie danych). Każdy PR na poprzednim; po scaleniu wszystkie ciała JSON opisane (407 z 407), a moduły na liście bramki typów. Opisy PR: PR-zad237-1…7-*.md w Plikach rozmowy.
- Jak sprawdzić: Scalać po kolei PR1→PR7 (łańcuch wchodzi na main bez konfliktów). W każdym: pytest modułu i testy kształtu, manage.py spectacular --validate (wynik = schema.yaml, 0 błędów), tools/checks schema_bodies/schema_fields check i ratchet --ref origin/main, schema_types.py, pytest tools/tests; w web: apps-compose, tsc --noEmit, eslint, prettier --check, vitest (526/526). Szczegóły w sekcji Jak sprawdzić każdego opisu PR.
