<!--
opis: Notka zadania kodu ZAD-091: https://github.com/SmallBigSteps/intranetFebo/pull/202
rodzaj: wynik
zadanie: ZAD-091
etap: ETP-1 Testy
rozmowa: ZAD-091 P1: Powiadomienia o terminach — godzinny przegląd terminów d
zmiana: 28.10.2026, ZAD-091 P1: Powiadomienia o terminach — godzinny przegląd terminów d
status: do akceptacji
-->
# ZAD-091: P1: Powiadomienia o terminach — godzinny przegląd terminów działa poza transakcją, więc wysyłka pilnego maila zawsze się wywraca: nikt nie dostaje maila o terminie, a w dzwonku co godzinę przybywa duplikat (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/202
- Co zmieniono: Wysyłka powiadomień (send, send_many) otwiera własną transakcję, więc godzinny przegląd terminów w tle kolejkuje pilny mail; ostrzeżenie i znacznik zapisują się razem na każde zadanie, błąd jednego zadania nie zatrzymuje reszty, a przebieg kończy się błędem kolejki. 5 testów z prawdziwą transakcją, kontrakt core-notify 2.6. Scalone do main (6589a31).
- Jak sprawdzić: Wynik CI na PR #202 (tests/test_notify_scan.py); na dev po wdrożeniu main: zadanie z terminem w przeszłości dostaje jedno powiadomienie w dzwonku i mail, kolejne przebiegi godzinne nie dodają duplikatu, w martwej kolejce brak notify.scan_due_tasks.
