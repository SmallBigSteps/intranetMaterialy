<!--
opis: Notka zadania kodu ZAD-093: https://github.com/SmallBigSteps/intranetFebo/pull/208
rodzaj: wynik
zadanie: ZAD-093
etap: ETP-1 Testy
rozmowa: ZAD-093 P1: Konta — maile aktywacyjne przy imporcie pracowników wych
zmiana: 29.10.2026, ZAD-093 P1: Konta — maile aktywacyjne przy imporcie pracowników wych
status: do akceptacji
-->
# ZAD-093: P1: Konta — maile aktywacyjne przy imporcie pracowników wychodzą w trakcie transakcji, bez limitu czasu: awaria poczty w połowie cofa import, a osoby, do których mail już wyszedł, dostają linki, które nie działają (na środowisku roboczym jeszcze niewidoczne) (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/208
- Co zmieniono: Maile aktywacyjne i resetu hasła wychodzą dopiero po zatwierdzeniu transakcji, przez kolejkę z limitem czasu i ponowieniem (do 5 prób); awaria poczty nie cofa już importu, a link wydaje się pod blokadą konta (FOR NO KEY UPDATE), więc przy wyścigu zostaje jeden ważny link i nie ma zakleszczeń. Kontrakt core-accounts 3.12. Scalone do main w PR #208.
- Jak sprawdzić: Na dev: import pracowników przy wyłączonej poczcie kończy się sukcesem, a maile wychodzą po jej włączeniu; ponowne wysłanie linku dwa razy naraz zostawia jeden ważny link. Testy api/tests/test_link_mails.py (R1-R6) zielone, pełny zestaw 720 passed, mutacje 14/14.
