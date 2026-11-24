<!--
opis: Notka zadania kodu ZAD-329: https://github.com/SmallBigSteps/intranet/pull/new/zad-329-logo-w-pakiecie-marki
rodzaj: wynik
zadanie: ZAD-329
kamien: KM-7 UX i UY
rozmowa: ZAD-329 Logo w pakiecie marki FEBO
zmiana: 23.11.2026, ZAD-329 Logo w pakiecie marki FEBO
status: do akceptacji
-->
# ZAD-329: Logo w pakiecie marki FEBO (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-329-logo-w-pakiecie-marki
- Co zmieniono: Logo wdrożenia w pakiecie marki powłoki (option.logo, core-ui 2.44): ekran logowania i panel boczny, w zwiniętym panelu znak; wersja na ciemne/jasne dobierana sama do tła i trybu. Drugi PR: logo FEBO (4 SVG) we wdrożeniu — https://github.com/feboLogisticGit/febo-intranet-wdrozenie/pull/new/zad-329-logo-febo (scalać po PR powłoki).
- Jak sprawdzić: Scal najpierw PR powłoki, potem wdrożenia. Na dev: ekran logowania pokazuje logo FEBO w panelu marki (tryb jasny i ciemny), panel boczny ma logo nad nazwą Intranet, zwinięty panel pokazuje orła. Bez wdrożenia (pakiet neutralny) wygląd bez zmian. CI: testy web, brand-lint, kontrast.
