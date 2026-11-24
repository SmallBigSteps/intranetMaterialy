<!--
opis: Notka zadania kodu ZAD-302: https://github.com/SmallBigSteps/intranet/pull/new/zad-302-kontakty-przycisk-kalendarz
rodzaj: wynik
zadanie: ZAD-302
kamien: KM-2 Pierwsza aplikacja u użytkowników FEBO
rozmowa: ZAD-302 Kontakty: przycisk, ukryty kalendarz
zmiana: 16.11.2026, ZAD-302 Kontakty: przycisk, ukryty kalendarz
status: do akceptacji
-->
# ZAD-302: Kontakty: przycisk, ukryty kalendarz (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-302-kontakty-przycisk-kalendarz
- Co zmieniono: Kontakty: zakładka Kalendarz obecności ukryta do integracji z Outlookiem (ZAD-033), stary adres widoku otwiera drzewo, kod i reguły kalendarza zostają (RULES.md 1.3); „Dodaj / Zmień” przy własnym telefonie to małe słowo w linii zamiast przycisku w ramce. Scenariusze odbioru KM-1 bez kroku 5.3 (febo-intranet-wdrozenie, gałąź zad-302-scenariusz-bez-kalendarza: https://github.com/feboLogisticGit/febo-intranet-wdrozenie/pull/new/zad-302-scenariusz-bez-kalendarza). Rejestr uwag na dev przenumerowany (#11 oznaczona jako usunięty krok, #12–#15 na 5.3–5.6).
- Jak sprawdzić: Po scaleniu obu PR i wdrożeniu na dev: Struktura i kontakty ma zakładki Drzewo, Schemat, Działy; /app/directory?widok=kalendarz pokazuje drzewo; na własnej karcie przy Telefon małe „Zmień”/„Dodaj” w linii, klik otwiera pole i Zapisz; scenariusz KM-1 ma 24 kroki, część 5 kończy się na 5.6.
