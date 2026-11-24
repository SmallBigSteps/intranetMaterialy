<!--
opis: Notka zadania kodu ZAD-297: https://github.com/SmallBigSteps/intranet/pull/new/zad-297-profil-menu-sesje
rodzaj: wynik
zadanie: ZAD-297
kamien: KM-1 Fundament powłoki gotowy
rozmowa: ZAD-297 Profil, menu i sesje
zmiana: 15.11.2026, ZAD-297 Profil, menu i sesje
status: do akceptacji
-->
# ZAD-297: Profil, menu i sesje (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-297-profil-menu-sesje
- Co zmieniono: Profil otwierany kliknięciem imienia i nazwiska; w panelu bocznym grupy Rejestry i Konto domyślnie zwinięte (stan zapamiętany na koncie); strona Profil w trzech kartach (Ja, Co mogę, Bezpieczeństwo); sesje: jedno urządzenie = jedna sesja, wylogowanie i nocne sprzątanie zamykają wpis, odświeżana ostatnia aktywność, przeglądarka i system, znacznik bieżącej sesji, Wyloguj pozostałe z potwierdzeniem. Kontrakty core-accounts 3.14, core-cockpit 3.15, BR-COC-021.
- Jak sprawdzić: Na dev po scaleniu: dwa logowania z tej samej przeglądarki dają jedną sesję; logowanie z telefonu pokazuje Wyloguj pozostałe; ostatnia aktywność rośnie przy pracy; Rejestry i Konto zwinięte i pamiętają stan; kliknięcie imienia otwiera profil.
