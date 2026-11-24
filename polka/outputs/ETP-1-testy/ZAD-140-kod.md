<!--
opis: Notka zadania kodu ZAD-140: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-140-przekierowanie-poczty-test?expand=1
rodzaj: wynik
zadanie: ZAD-140
kamien: KM-1 Fundament powłoki gotowy
rozmowa: ZAD-140 Przy uruchomieniu instancji testowej Intranetu automatycznie
zmiana: 13.11.2026, ZAD-140 Przy uruchomieniu instancji testowej Intranetu automatycznie
status: do akceptacji
-->
# ZAD-140: Przy uruchomieniu instancji testowej Intranetu automatycznie przestawić adresy mailowe (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/compare/main...zad-140-przekierowanie-poczty-test?expand=1
- Co zmieniono: Na teście każdy mail aplikacji idzie na skrzynkę zbiorczą (it@) poza listą dozwolonych; kopia z tematem [TEST] i listą pierwotnych odbiorców; start zablokowany, gdy test nie ma skrzynki albo prod ma przekierowanie; INTRANET_ENV trafia do kontenerów; kontrakt core-notify 2.8, decyzja przekierowanie-poczty-test (na main), krok 3 i 9 instrukcji wdrożenia
- Jak sprawdzić: ./pytest-zad140.sh tests/test_mail_redirect.py: 28 passed; z test_job, test_link_mails, test_fbk, test_notify*: 133 passed; ruff i mypy czyste. Na teście po wpisach z kroku 3: sendtestemail nikt@example.invalid → mail [TEST] na it@
