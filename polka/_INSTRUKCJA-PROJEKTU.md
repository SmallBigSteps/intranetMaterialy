<!--
opis: Nowy adres repozytorium kodu i założeń: GitHub przeniósł SmallBigSteps/intranetFebo na SmallBigSteps/intranet (stary adres tylko przekierowuje).
rodzaj: instrukcja
rozmowa: ZAD-261 Umiejscowienie: uprawnienie przed danymi
zmiana: 13.11.2026, ZAD-261 Umiejscowienie: uprawnienie przed danymi
status: zaakceptowany
-->
# Instrukcja projektu intranet2.0

Zmiany tej instrukcji przechodzą przez skrzynkę zmian PMO (odkładanie z `--do instrukcja`).

## Co gdzie trafia

- Wynik zadania: `outputs/` według etapu zadania (odkładanie wyniku).
- Materiał źródłowy od ludzi: `raw/`.
- Założenie albo decyzja dotycząca całego projektu: `zalozenia/`.
- Opis repozytorium kodu: `dev/`.

## Zasady tego projektu

1. Założenia żyją w repozytorium intranetFebo: `CLAUDE.md`, kontrakty w `docs/contract/`, decyzje
   w `docs/decisions/`, karty zadań, `docs/progress.md`, `.claude/`. Każda zmiana idzie tego samego dnia
   commitem i pushem na `main`, z podpisem `redAi <redai@febologistic.com>`. Na półce nie trzymamy ich kopii.
2. Na półkę trafia to, czego nie ma w repozytorium: dokumenty dla ludzi (Word, PDF), zrzuty, arkusze,
   materiały od ludzi. Rozmowa szuka najpierw w repozytorium i na półce, a nie w innych czatach.
3. Płatności prowadzimy w projekcie IntranetPlatnosci; materiały o nich odkładamy na tamtą półkę
   (`--do-projektu=intranetplatnosci`).
4. Danych osobowych (wypełnione listy pracowników i podobne) nie kładziemy na półkę.
5. Materiałów roboczych z diagnozy usterek (logi, zrzuty błędów, wklejki) nie odkładamy.

## Repozytoria projektu

- **intranetFebo** `git@github.com:SmallBigSteps/intranet.git` (dawniej `intranetFebo`; katalogi kopii zachowują starą nazwę), gałąź `main`: kod powłoki i lokatorów
  oraz założenia. Kopia główna: `storage/terminals/intranet20-czat-1008-1132/intranetFebo`
  (przed pracą `git pull`, po zmianach `git commit` i `git push`).
- **febo-intranet-wdrozenie**: konfiguracja wdrożenia FEBO (kopia: `storage/worktrees/febo-intranet-wdrozenie`).
- **SmallBigSteps/febo** `projekty/intranet20/`: wspólne repozytorium projektów portalu; portal wysyła tam
  opis, zadania i dziennik projektu.
- **Repozytorium materiałów**: osobne prywatne repozytorium na półkę bez `raw/` (do założenia w ZAD-176);
  wysyłka rusza po zgodzie PM w sekcji Repozytorium na stronie projektu.
- Kod: gałąź `zad-<nr>-<opis>` w drzewie roboczym `storage/worktrees/intranetFebo-zad<nr>`, pull request;
  scala Paweł Mazek. Zadanie kodu oddaje się adresem PR (`pmo:odloz --pr=…`).
- Założenia: commit wprost na `main` tego samego dnia (zasada 1).
- Przejście na praktykę ze standardu PMO (kopia poza katalogiem rozmowy, blokada wypychania na `main`)
  rozstrzyga ZAD-175; do tego czasu obowiązuje praktyka opisana wyżej.
- Pełny opis (stos, struktura, polecenia, środowiska dev/test/prod): `dev/Repozytoria-i-srodowisko.md`.
