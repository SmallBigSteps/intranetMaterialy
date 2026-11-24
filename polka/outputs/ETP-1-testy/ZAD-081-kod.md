<!--
opis: Notka zadania kodu ZAD-081: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-081-kalendarz-bez-koloru?expand=1
rodzaj: wynik
zadanie: ZAD-081
etap: ETP-1 Testy
rozmowa: ZAD-081 P1: Struktura i kontakty — kalendarz obecności pokazuje „w b
zmiana: 28.10.2026, ZAD-081 P1: Struktura i kontakty — kalendarz obecności pokazuje „w b
status: do akceptacji
-->
# ZAD-081: P1: Struktura i kontakty — kalendarz obecności pokazuje „w biurze / zdalnie / nieobecny” samym kolorem (niezmiennik 34): osoba z zaburzeniem widzenia barw, zwłaszcza na tablecie, nie odróżni obecnych od nieobecnych (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-081-kalendarz-bez-koloru?expand=1
- Co zmieniono: Kalendarz obecności: każdy dzień ma literę B/Z/N/– i pełny opis dla czytnika ekranu, legenda w kolorach siatki, bez legendy w drzewie, tablet bez poziomego rozjeżdżania. Gałąź zad-081-kalendarz-bez-koloru (3 commity, do 886e15e); PR do otwarcia z linku, opis w zespol/opis-PR.md rozmowy.
- Jak sprawdzić: Zakładka Kalendarz obecności na tablecie 768 px i w trybie skali szarości: stany rozróżnialne literą; czytnik ekranu czyta nazwę stanu i datę; widok Drzewo bez legendy. Bramki: vitest src/apps/directory, tsc, eslint, brand-lint, rules_gate.
