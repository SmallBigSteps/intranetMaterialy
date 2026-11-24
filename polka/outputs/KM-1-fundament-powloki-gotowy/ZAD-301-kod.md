<!--
opis: Notka zadania kodu ZAD-301: https://github.com/SmallBigSteps/intranet/compare/main...zad-301-uwagi-przejscie-do-zgloszen?expand=1
rodzaj: wynik
zadanie: ZAD-301
kamien: KM-1 Fundament powłoki gotowy
rozmowa: ZAD-301 Uwagi: przejście do zgłoszeń
zmiana: 15.11.2026, ZAD-301 Uwagi: przejście do zgłoszeń
status: do akceptacji
-->
# ZAD-301: Uwagi: przejście do zgłoszeń (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/compare/main...zad-301-uwagi-przejscie-do-zgloszen?expand=1
- Co zmieniono: Po wysłaniu uwagi stopka okna ma główny przycisk „Przejdź do Moich zgłoszeń” (przejście w aplikacji, postęp panelu scenariusza zostaje) i „Zamknij”; odnośnik w komunikacie pogrubiony z grubszym podkreśleniem. Kroki 4.1 i 4.2 scenariuszy KM-1 Pawła i Daniela poprawione w febo-intranet-wdrozenie (gałąź zad-301-krok-4-2: https://github.com/feboLogisticGit/febo-intranet-wdrozenie/pull/new/zad-301-krok-4-2). Kontrakty core-ui 2.29 i core-feedback 1.2 na main.
- Jak sprawdzić: Na dev: Zgłoś uwagę → Wyślij → w stopce Przejdź do Moich zgłoszeń i Zamknij; pierwszy zamyka okno i otwiera listę z nowym zgłoszeniem. W scenariuszu ?scenariusz=odbior-fundamentu-km1-pawel krok 4.2 przez ten przycisk, panel zachowuje postęp. Kontrole: typecheck, lint, 522 testy, build zielone.
