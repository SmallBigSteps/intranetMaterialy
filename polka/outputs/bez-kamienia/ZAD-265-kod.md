<!--
opis: Notka zadania kodu ZAD-265: https://github.com/SmallBigSteps/intranet/pull/new/zad-265-testy-bez-zmiennych-git
rodzaj: wynik
zadanie: ZAD-265
rozmowa: ZAD-265 Test completion bez GIT_DIR
zmiana: 13.11.2026, ZAD-265 Test completion bez GIT_DIR
status: do akceptacji
-->
# ZAD-265: Test completion bez GIT_DIR (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/new/zad-265-testy-bez-zmiennych-git
- Co zmieniono: Gałąź zad-265-testy-bez-zmiennych-git (commit a2f7756): tools/tests/conftest.py usuwa zmienne GIT_* przed testami narzędzi (fikstura sesji), więc testy uruchomione z haka gita albo z git rebase -x nie piszą już do prawdziwej kopii. Obejmuje test_completion, test_mutation i wywołania gita z completion.py. Nowy test tools/tests/test_git_isolation.py odtwarza incydent z 02.11 na repozytorium-wabiku. Kod produkcyjny i kontrakty bez zmian. PR do otwarcia z linku.
- Jak sprawdzić: W kopii gałęzi: pytest -q tools (wszystko zielone). Odtworzenie: GIT_DIR=<inne repo>/.git pytest -q tools — inne repo bez nowych commitów i bez user.name=test. Bez conftest.py test_git_isolation jest czerwony w obu wariantach.
