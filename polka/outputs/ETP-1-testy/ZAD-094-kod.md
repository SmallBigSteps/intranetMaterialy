<!--
opis: Notka zadania kodu ZAD-094: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-094-limit-czasu-alarm
rodzaj: wynik
zadanie: ZAD-094
kamien: KM-5 Środowiska test i prod gotowe
rozmowa: ZAD-094 P1: Kolejka — po przekroczeniu limitu czasu zadanie jest pon
zmiana: 30.10.2026, ZAD-094 P1: Kolejka — po przekroczeniu limitu czasu zadanie jest pon
status: do akceptacji
-->
# ZAD-094: P1: Kolejka — po przekroczeniu limitu czasu zadanie jest ponawiane, choć pierwsze wykonanie wciąż działa w tle: mail może wyjść dwa razy, a wiszące wątki zapychają obsługę kolejki; dotyczy też kontraktu kolejki (zakłada zatrzymanie pierwszego wykonania) (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-094-limit-czasu-alarm
- Co zmieniono: Limit czasu zadania w kolejce działa teraz jak alarm, a nie przerwanie: zadanie po limicie pracuje dalej i nie jest ponawiane, więc mail nie wychodzi dwa razy. Ponowienie następuje dopiero po faktycznym końcu i tylko po błędzie. Przy dwukrotności limitu zadanie jest oznaczone jako zawieszone i trafia do kolejki martwych zadań bez ponowienia. Zawieszone zadanie trzyma blokadę tylko wtedy, gdy jego worker daje znak życia (60 s, kontrakt 2.6). Kontrola przy starcie sprawdza, czy EMAIL_TIMEOUT jest krótszy niż limit zadań mailowych. Panel zdrowia pokazuje oba stany. Dochodzą reguły BR-JOB-015 i 016. Gałąź zad-094-limit-czasu-alarm, 5 commitów na main po kontrakcie 2.6. Ten PR i PR ZAD-096 zmieniają ten sam plik runs.py, więc ten, który zostanie scalony jako drugi, trzeba przenieść na nowy main. Wspólny próg i nazwa stałej są już ujednolicone.
- Jak sprawdzić: W api: pytest testów kolejki, powiadomień, health i security oraz tests/rules/test_job*.py (198 passed); cały api/tests 1058 passed (tester ZAD-229); web: vitest i tsc bez błędów; python -m tools.checks.rules_gate kończy się kodem 0. Ręcznie: zadanie z limitem 1 s, które pracuje 2 s i wysyła mail, wykonuje się raz i wysyła jeden mail, a w panelu zdrowia jest oznaczone jako ponad limitem. Przy EMAIL_TIMEOUT brak albo 120 asgi i wsgi odmawiają startu.
