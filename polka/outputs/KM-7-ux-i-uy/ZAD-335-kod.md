<!--
opis: Notka zadania kodu ZAD-335: https://github.com/SmallBigSteps/intranet/pull/new/zad-335-osobny-kolor-panelu-logowania
rodzaj: wynik
zadanie: ZAD-335
kamien: KM-7 UX i UY
rozmowa: ZAD-335 Osobny kolor panelu logowania
zmiana: 24.11.2026, ZAD-335 Osobny kolor panelu logowania
status: do akceptacji
-->
# ZAD-335: Osobny kolor panelu logowania (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-335-osobny-kolor-panelu-logowania
- Co zmieniono: Powłoka: tokeny login-brand i on-login-brand (domyślnie aliasy band i on-band-strong), panel logowania i dobór logo na nich, brand-lint sprawdza tła logo, para kontrastu, core-ui 2.45. Wdrożenie FEBO (gałąź zad-335-ciemny-panel-logowania, scalać po powłoce): panel grafitowy, nazwa biała, w obu trybach.
- Jak sprawdzić: Na dev po scaleniu obu: /logowanie na ekranie od 1200 px — lewy panel grafitowy (w trybie ciemnym prawie czarny), napis Intranet biały, logo FEBO z białym napisem; nagłówki ekranów dalej jasne.
