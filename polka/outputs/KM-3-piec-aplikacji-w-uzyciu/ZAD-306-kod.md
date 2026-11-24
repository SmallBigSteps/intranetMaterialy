<!--
opis: Notka zadania kodu ZAD-306: https://github.com/SmallBigSteps/intranet/pull/new/zad-306-tryb-ciemny
rodzaj: wynik
zadanie: ZAD-306
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-306 Tryb ciemny i tokeny marki
zmiana: 16.11.2026, ZAD-306 Tryb ciemny i tokeny marki
status: do akceptacji
-->
# ZAD-306: Tryb ciemny i tokeny marki (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-306-tryb-ciemny
- Co zmieniono: Gałąź zad-306-tryb-ciemny @ 5b5869e: tryb ciemny powłoki (color-dark, :root / [data-theme] / prefers-color-scheme, color-scheme), tokeny accent-text, size-field-label, size-input-touch i on-dark-strong (core-ui 2.33), rozdzielenie ról tekstu akcentowego i etykiet, kontrast obu trybów w testach na pakiecie scalonym, krok CI pilnujący brand.json wdrożenia. Po przeglądzie zespołu (programista, frontend, tester) poprawione kontrasty w ciemnym trybie, formatowanie Płatności i nagłówek karty Standardów. Scalić przed ZAD-025, tego samego dnia.
- Jak sprawdzić: Na dev (https://dev.febo.redai.pl, gałąź wdrożona) zalogować się i obejrzeć panel, Płatności i Standardy w trybie jasnym i ciemnym (ustawienie systemu). Lokalnie w web/ z konfiguracją wdrożenia FEBO: npm run lint, format:check, typecheck, npm test — 75 plików / 803 testy zielone.
