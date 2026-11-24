<!--
opis: Notka zadania kodu ZAD-234: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-234-zawieszony-watek-przy-zatrzymaniu
rodzaj: wynik
zadanie: ZAD-234
kamien: KM-5 Środowiska test i prod gotowe
rozmowa: ZAD-234 Zawieszony wątek przy zatrzymaniu workera
zmiana: 30.10.2026, ZAD-234 Zawieszony wątek przy zatrzymaniu workera
status: do akceptacji
-->
# ZAD-234: Zawieszony wątek przy zatrzymaniu workera (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/new/zad-234-zawieszony-watek-przy-zatrzymaniu
- Co zmieniono: Zatrzymywany worker czeka na zwykłe zadania jak dotąd (Compose daje im 5 minut, stop_grace_period: 5m), a gdy zostaje mu tylko zawieszona praca, kończy proces od razu i zapisuje to w logu. Okno z ZAD-094 R2, w którym zawieszone zadanie mogło jeszcze coś zapisać lub wysłać, znika. Nowa reguła BR-JOB-018 z testami na prawdziwym procesie. Przy okazji odtworzone BR-JOB-015/016 zgubione przy scalaniu ZAD-094 z ZAD-096 (rejestr reguł 3.29). Kontrakt core-jobs 2.11 już na main (ce6522f).
- Jak sprawdzić: 1) Otworzyć PR z gałęzi zad-234-zawieszony-watek-przy-zatrzymaniu i scalić po zielonym CI. 2) Testy: tests/rules/test_job_worker_stop.py (bez poprawki worker czekał ok. 18 s na zawieszony wątek, z poprawką kończy się od razu). 3) Po scaleniu na dev: zawiesić zadanie próbne, docker stop workera; worker kończy się w kilka sekund, w logu worker_stopped_hung_work_abandoned, przebieg zamknięty jako WorkerLost. Wynik dopiszemy do zadania.
