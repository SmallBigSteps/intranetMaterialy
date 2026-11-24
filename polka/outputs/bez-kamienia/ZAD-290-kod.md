<!--
opis: Notka zadania kodu ZAD-290: https://github.com/SmallBigSteps/intranet/pull/new/zad-290-kosz-czyszczenie-nagrobki
rodzaj: wynik
zadanie: ZAD-290
rozmowa: ZAD-290 Kosz: czyszczenie i nagrobki
zmiana: 13.11.2026, ZAD-290 Kosz: czyszczenie i nagrobki
status: do akceptacji
-->
# ZAD-290: Kosz: czyszczenie i nagrobki (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-290-kosz-czyszczenie-nagrobki
- Co zmieniono: Kosz powłoki dokończony (core-recycle R4, R5, R8–R10): nocne czyszczenie po 30 dniach z kaskadą plików, usuwanie na stałe przez administratora z „Pokaż usunięte” z potwierdzeniem blokad, nagrobki w panelu retencji, notka przy zadaniu o obiekcie w koszu lub usuniętym, zdejmowanie z wyszukiwarki. Kontrakty core-recycle 2.2, core-tasks 4.14, core-files 3.10, decyzja kosz-czyszczenie-i-nagrobki.
- Jak sprawdzić: Otworzyć PR z linku, CI zielone. Na dev: usunąć procedurę z plikami, w „Pokaż usunięte” wybrać „Usuń na stałe” — przy pliku z blokadą okno wymaga potwierdzenia, panel retencji pokazuje nagrobek; zadanie tej procedury pokazuje notkę „Obiekt usunięty na stałe”.
