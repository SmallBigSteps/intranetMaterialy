<!--
opis: Raport testera: niezależne sprawdzenie poprawki ZAD-088 (opiekun widzi tekst wersji) — bramki, migracja 0008, scenariusze widoczności, teksty powodu; 0 błędów blokujących, 3 uwagi.
rodzaj: wynik
zadanie: ZAD-194
etap: ETP-1 Testy
rozmowa: ZAD-088 P1: Standardy — gdy przełożony autora ma nieaktywne konto (n · Tester
zmiana: 28.10.2026, ZAD-088 P1: Standardy — gdy przełożony autora ma nieaktywne konto (n · Tester
status: do akceptacji
slowa: ZAD-088, ZAD-194, standards, deciders, migracja 0008, widoczność szkicu, opiekun biblioteki
-->

# ZAD-194 — sprawdzenie poprawki ZAD-088 „opiekun widzi tekst wersji”

Tester, 28.10.2026. Commit `52e8e01` z gałęzi `zad-088-opiekun-widzi-szkic`, porównany z bazą `2227ded`.

## Werdykt

**OK, bez błędów blokujących.** Opiekun, do którego trafia zatwierdzenie, otwiera tekst wersji i procedurę w obu przypadkach:
- przełożony autora jest nieaktywny (INVITED, SUSPENDED albo DISABLED);
- autor nie ma przełożonego, a jego podwładni są nieaktywni.

Obca osoba nadal dostaje 404, a autor nie rozstrzyga sam. W pozostałych przypadkach widoczność działa jak dotąd. Wszystkie bramki dają ten sam wynik co na bazie. Mam 3 uwagi (żadna nie blokuje) i 1 drobną propozycję.

**Ważne:** w czasie mojego sprawdzania PR #203 został już scalony z `main` (`ecd3a5f`, potem `33ebf2a` w progress: „ZAD-088 GOTOWE”). Obecny `origin/main` różni się od `52e8e01` tylko dwoma plikami dokumentacji (`testing.contract.md`, `progress.md`), więc ten raport dotyczy też obecnego main. Drzewo `storage/worktrees/intranetFebo-zad088` już nie istniało, dlatego kod wziąłem z commita (`git archive`).

## Środowisko

- Lab, katalog `/srv/zad194`:
  - kopia gałęzi `52e8e01`, kopia bazy `2227ded`;
  - `febo-intranet-wdrozenie` (HEAD `841b076`);
  - Python 3.12 w venv, osobna rola i baza PostgreSQL `intranet_zad194`.
- Dane wyłącznie z fixtur testowych, nic z bazy portalu.
- Po pracy usunąłem katalog `/srv/zad194`, bazy `intranet_zad194` i `test_intranet_zad194` oraz rolę. Sprawdziłem, że nic nie zostało.
- W repozytorium niczego nie zmieniałem, niczego nie commitowałem ani nie wypychałem. Jedyna operacja: `git fetch` w kopii głównej, żeby porównać z bieżącym main.

## 1. Testy i bramki

| Polecenie | Gałąź 52e8e01 | Baza / main | Wynik |
|---|---|---|---|
| `pytest` testy Standardów | 74 passed | — | OK, zgodnie z programistą |
| pełny zestaw jak w `make test`: `tests` + testy powłoki z wdrożenia + standards, directory, payments | 1558 passed, 2 skipped, 9 failed | — | OK, zgodnie z programistą |
| 9 testów `test_logtms_*` na kodzie bazy | — | 9 failed (`Can't connect to MySQL server on 127.0.0.1:3306`) | OK: te same 9 testów, ta sama przyczyna (brak makiety MySQL), to nie regresja |
| `pytest tools -q` | wszystkie zielone | — | OK |
| `ruff check api apps` / `ruff format --check api apps` | All checks passed / 427 files already formatted | — | OK |
| `mypy .` (api) / `mypy -p apps.standards -p apps.directory -p apps.payments` | brak błędów (348 / 74 plików) | — | OK |
| `manage.py makemigrations --check --dry-run` | No changes detected | — | OK |
| `tools/checks/rules_gate.py` | kod 0; BR-STD-019, BR-STD-023 bez testu i spoza manifestu; BR-DIR-022 | wynik identyczny (diff pusty) | OK, stan zastany |
| `python -m tools.checks` (część `make review`) | 9 blokujących zielonych, 1459 ostrzeżeń | 1457 ostrzeżeń | OK: +2 ostrzeżenia W5 to dwa nowe polskie teksty powodu; R1 bez zmian (231) |

Pierwsze podejście do pełnego zestawu przerwał import `MySQLdb` w testach wdrożenia. Doinstalowałem `dep[dev]` w venv testowym, a nie w kodzie, i drugie przejście poszło jak u programisty.

Z `make review` nie uruchamiałem `mutation.py ratchet` ani `duplication`, bo potrzebują osobnych narzędzi. Kod 0 bramki reguł i rejestru kontroli jest jak na bazie.

## 2. Migracja danych 0008

Test `test_migration_0008_backfill_and_reverse` w pliku `02-tester-testy.py`, uruchomiony przez `MigrationExecutor` w teście transakcyjnym.

Przebieg:
1. Na pełnym schemacie założyłem cztery wersje:
   - „w ocenie” z zadaniem przypisanym do dwóch opiekunów;
   - szkic nigdy niezgłoszony;
   - opublikowaną;
   - odesłaną i zgłoszoną drugi raz (klucz zadania `…-v1-2`).
2. Cofnąłem migracje do `0007`. Tabela rozstrzygających zniknęła, dane wersji zostały.
3. Migrowałem do `0008`.

Wynik po migracji:

| Wersja | Lista rozstrzygających |
|---|---|
| w ocenie | obaj opiekunowie z zadania |
| zgłoszona drugi raz | obaj opiekunowie z zadania drugiego zgłoszenia |
| szkic | pusta |
| opublikowana | pusta |

Ponowne cofnięcie do 0007 i migracja do 0008 przechodzą, a lista odtwarza się tak samo. **OK.**

Stan 0007 uzyskałem cofnięciem migracji po założeniu danych, a nie zakładaniem wierszy na modelach historycznych. Dla migracji to ten sam stan wejściowy.

## 3. Scenariusze widoczności (API)

Plik `02-tester-testy.py`: 11 testów, wszystkie zielone. Adresy: `/api/v1/app/standards/versions/<id>/`, `/procedures/<id>/`, `/procedures/`.

| Scenariusz | Oczekiwane | Faktyczne | Wynik |
|---|---|---|---|
| Przełożony INVITED / SUSPENDED / DISABLED (3 przebiegi) | zadanie i lista u opiekuna; opiekun 200 na wersji i procedurze; obcy 404; nieaktywny przełożony nie widzi; autor nie przejmie zadania, nie opublikuje ani nie odeśle | jak oczekiwane; przed zgłoszeniem opiekun dostaje 404 na szkicu, jak dawniej | OK |
| Aktywny przełożony | zadanie u przełożonego, bez powodu; przełożony 200; opiekun 404 na wersji i procedurze, a procedury nie ma na jego liście | jak oczekiwane | OK |
| Brak przełożonego, jeden podwładny aktywny i jeden zaproszony | zadanie tylko u aktywnego podwładnego, powód dotychczasowy; podwładny 200, opiekun 404 | jak oczekiwane | OK |
| Opiekun odsyła, przełożony aktywuje konto, autor poprawia i zgłasza ponownie | nowe zadanie u przełożonego, lista nadpisana na samego przełożonego, przełożony 200, poprzedni opiekun 404 na wersji i procedurze; po zatwierdzeniu wersja obowiązuje | jak oczekiwane | OK |
| Przełożony M1 odsyła, kadry zmieniają przełożonego na M2, autor zgłasza ponownie | zadanie u M2, M2 200, M1 404 | jak oczekiwane | OK |
| Dwóch opiekunów; procedura z wersją obowiązującą i drugą w ocenie oraz druga procedura w ocenie | każda procedura raz na liście u obu opiekunów, bez powtórzonych wersji w szczegółach, `visible_to(...).count() == 1` | jak oczekiwane | OK |

## 4. Teksty powodu na zadaniu

Sprawdzone dokładnym porównaniem:

| Przypadek | Tekst | Wynik |
|---|---|---|
| nieaktywny przełożony | „Przełożony autora nie ma aktywnego konta — rozstrzyga opiekun biblioteki.” | OK |
| brak przełożonego, podwładni tylko nieaktywni | „Podwładni autora nie mają aktywnych kont — rozstrzyga opiekun biblioteki.” | OK |
| linia pusta w obie strony | „Linia podległości autora jest pusta w obie strony — rozstrzyga opiekun biblioteki.” | OK |
| aktywni podwładni (bez zmian) | „Autor nie ma przełożonego — rozstrzyga pierwszy z jego bezpośrednich podwładnych.” | OK |
| aktywny przełożony | brak klucza „powód skierowania” | OK |

## 5. Przegląd według review-checklist

- **Warstwa 1:** kontrole automatyczne są zielone (punkt 1).
- **Odczyty przez `visible_to`:** widoki Standardów czytają wersję i procedurę przez `visible_to`. Złączenie M2M ma `.distinct()` w obu querysetach. Lista bez duplikatów jest potwierdzona testem.
- **Separacja obowiązków:** `separation_required=True` zostaje, a `_not_the_author` blokuje autora w publish i reject. Sprawdzone testem.
- **Czy test sprawdza regułę:** testy programisty dla BR-STD-025 (a) i (b) naprawdę otwierają adresy API jako opiekun i jako obcy, więc sprawdzają regułę, a nie tylko zapis.
- **N+1:** zapis listy idzie przez tabelę łączącą i strażnik N+1 go przepuszcza.
- **Spójność dokumentów:** opisy BR-STD-007 i BR-STD-025 w RULES.md (wersja 3.8) zgadzają się z kodem. Manifest ma nowe brzmienie BR-STD-025, a progress.md jest zaktualizowany na main.
- **Warstwa 3, czego nie pokrywa żaden test w repozytorium:**
  - migracja danych 0008;
  - nadpisanie listy przy ponownym zgłoszeniu;
  - brak duplikatów na liście przy kilku opiekunach.

  Wszystkie trzy sprawdziłem testami w `02-tester-testy.py`. Proponuję dopisać je do `test_rules.py` (patrz D1).

## Błędy i uwagi

Błędów blokujących: **0**.

| Nr | Waga | Opis | Kroki odtworzenia / skąd |
|---|---|---|---|
| U1 | uwaga | Po odesłaniu szkicu zapisana lista nadal decyduje o widoczności aż do ponownego zgłoszenia. Odsyłający widzi poprawiany szkic, ale nowy albo właśnie aktywowany przełożony go nie widzi (404), dopóki autor nie zgłosi wersji ponownie. Przedtem o widoczności po odesłaniu decydowała bieżąca linia. Zachowanie zgadza się z RULES.md 3.8 („przed pierwszym zgłoszeniem rozstrzyga linia podległości”), więc to świadoma zmiana, nie błąd. Warto, żeby człowiek ją potwierdził. | Opiekun odsyła szkic, przełożony aktywuje konto, przełożony otwiera `/versions/<id>/` i dostaje 404. Wariant: przełożony M1 odsyła, kadry ustawiają M2; M1 dostaje 200, M2 404. Testy `test_reject_then_manager_*` drukują te wartości. |
| U2 | uwaga | Migracja bierze do listy wszystkie bieżące przypisania zadania, także dopisane później (`delegate`, `assign`), a zgłoszenie zapisuje tylko pierwotnych rozstrzygających. Osoba dopisana do zadania po zgłoszeniu nie trafia na listę i nie otworzy szkicu. Przedtem też by nie otworzyła, więc to nie regresja. Ścieżkę zwykłego przekazania zawęża już powłoka: przekazanie zwykłemu pracownikowi odmawia („Nie można przekazać zadania komuś, kto nie ma do niego dostępu”). | Sprawdzone testem `test_delegated_task_holder_can_read` oraz przeglądem `core/tasks/reading.py` (`delegate`, `assign`). |
| U3 | uwaga | PR #203 został scalony z main przed zakończeniem tego sprawdzenia. Wynik jest pozytywny, więc nie ma skutków, ale kolejność „tester, potem scalenie” nie została zachowana. | `git log origin/main`: `ecd3a5f Merge pull request #203`. |
| D1 | drobny (propozycja) | Do repozytorium warto dopisać trzy testy z `02-tester-testy.py`: migrację 0008 (wypełnienie i cofnięcie), nadpisanie listy przy ponownym zgłoszeniu oraz listę bez duplikatów przy dwóch opiekunach. To jedyne zachowania tej zmiany bez testu w repozytorium. | — |

## Pliki

- Ten raport: `zespol/wyniki/02-tester.md`.
- Jednorazowe testy (11, zielone na lab): `zespol/wyniki/02-tester-testy.py`. Nie ma ich w repozytorium. Uruchamia się je jak `test_rules.py`, po skopiowaniu do katalogu `tests/` aplikacji Standardy.
