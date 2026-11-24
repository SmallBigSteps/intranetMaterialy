<!--
opis: Notka zadania kodu ZAD-007: https://github.com/SmallBigSteps/intranetFebo/pull/173
rodzaj: wynik
zadanie: ZAD-007
kamien: KM-2 Pierwsza aplikacja u użytkowników FEBO
rozmowa: zad007WczytywanieRozliczen
zmiana: 30.10.2026, zad007WczytywanieRozliczen
status: do akceptacji
-->
# ZAD-007: Wczytywanie rozliczeń z Optimy w wersji 1 z pliku wsadowego CSV/Excel zamiast połączenia z bazą — wgrywanie pliku, kontrola poprawności, znacznik świeżości danych na ekranie (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/173
- Co zmieniono: Wczytywanie pliku rozliczeń z Optimy: pomijanie znanych rozliczeń, zmienione do wyjaśnienia, status poza LOG TMS, VAT sprzedaży w walucie faktury, data danych z Optimy na ekranie; zapytanie SQL w wersji 2 (dalej w IntranetPlatnosci jako wersja 3).
- Jak sprawdzić: Płatności: wgrać ten sam plik dwa razy — drugi raz komunikat Nic nowego; ekran pokazuje Dane z Optimy na …; testy reguł BR-PAY-021, 038, 047, 048, 049 zielone.
