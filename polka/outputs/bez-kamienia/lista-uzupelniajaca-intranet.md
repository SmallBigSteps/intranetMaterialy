<!--
opis: Lista uzupełniająca przenosin intranetu na półkę (Półki C): stara półka, wyniki w rozmowach (synteza założeń produkcyjnych, kod POC), liczba plików.
rodzaj: wynik
zadanie: ZAD-001
rozmowa: zad001PolkiCPrzeniesienieMaterialow
zmiana: 14.10.2026, zaakceptowane przez Pawła Mazka
status: zaakceptowany
slowa: półki, migracja, intranet, lista uzupełniająca, POC, raw
-->
# Lista uzupełniająca: intranet (Półki C)

Wydanie 1 · 14.10.2026 · rozmowa zad001PolkiCPrzeniesienieMaterialow (ZAD-001). Uzupełnia rozdz. 3 listy migracji
z helpDesk (`outputs/ETP-5-modul-pmo/lista-migracji-pozostale-projekty.md`, ZAD-047). Ustalenia z wywiadu z Pawłem
14.10.2026. Kolumna **OK**: pusta = zgoda.

## 1. Stara półka (według listy z helpDesk, bez zmian)

| # | Plik w `context/` | Dokąd | Uwagi | OK |
|---|---|---|---|---|
| S1 | `xmxLfh_PRD.md` | `zalozenia/PRD.md` | przedrostek zdjęty, nagłówek, status zaakceptowany | |
| S2 | `TWwAPM_CLIENT_BRD.md` | `zalozenia/CLIENT_BRD.md` | nagłówek, status zaakceptowany | |
| S3 | `MXXuA6_RULES.md` | `zalozenia/RULES.md` | nagłówek, status zaakceptowany | |
| S4 | `FS0FGd_ARCHITECTURE.md` | `zalozenia/ARCHITECTURE.md` | nagłówek, status roboczy (dokument ma status POC) | |
| S5 | `LbhCPz_CLAUDE.md` | `dev/CLAUDE-intranet.md` | nazwa zmieniona, nagłówek, status roboczy (część konwencji to propozycje) | |
| S6 | `xG1KjF_canexpol_intranet_visual_assumptions.json` | `zalozenia/canexpol_intranet_visual_assumptions.json` | bez nagłówka, opis w spisie folderu | |

Treść plików bez zmian (sprawdzone porównaniem z oryginałem, poza dopisanym nagłówkiem). Żaden nie przekracza 400 linii,
więc nic nie dzielę.

## 2. Wyniki w rozmowach projektu

Przejrzane 3 rozmowy: intranet-czat-0408-1738, intranet-czat-1307-0018, intranet-ustalenia-projektu (ta ostatnia bez plików).
Backlog nie miał zadań poza ZAD-001, więc wyniki nie są przypisane do zadań.

| # | Plik w rozmowie | Rozmowa | Dokąd | Wersja | Uwagi | OK |
|---|---|---|---|---|---|---|
| R1 | `Intranet_Canexpol_zalozenia_produkcyjne.md` | intranet-czat-0408-1738 | `zalozenia/Intranet_Canexpol_zalozenia_produkcyjne.md` | bieżąca | synteza i propozycja założeń produkcyjnych z 04.08.2026, dokument roboczy (do zatwierdzenia), więc do założeń; w rozmowie skrót (dowiązanie) do półki | |
| R2 | `Intranet_Canexpol_zalozenia_produkcyjne.docx` | intranet-czat-0408-1738 | zostaje w rozmowie | kopia R1 | decyzja Pawła; kontrola półki zgłasza go jako `dokument_w_rozmowie` (patrz rozdz. 4) | |
| R3 | `poc-zamowienia-surowcow/` (aplikacja Laravel) | intranet-czat-1307-0018 | `raw/poc-zamowienia-surowcow/` | jedyna | decyzja Pawła: kod przeniesiony i usunięty z rozmowy, aplikacja docelowa powstaje od zera. Na półce 112 plików kodu źródłowego (ok. 1,2 MB) z `README.md` i `POC_NOTES.md`; bez `vendor/`, `node_modules/`, `storage/`, `bootstrap/cache/`, `public/build/`, `.env` i bazy sqlite z danymi demo. W rozmowie skrót (dowiązanie) do `raw/`. Opis w `dev/_WIKI.md` | |

## 3. Wiki i spisy

- `_WIKI.md` projektu napisane z PRD i BRD (po co i dla kogo, co budujemy, założenia, gdzie co jest, przekierowania,
  otwarte pytania). Karta projektu jest pusta (cel, efekt, PM, kamienie), co wiki zaznacza.
- `dev/_WIKI.md`: brak repozytorium kodu, opis POC i `CLAUDE-intranet.md`. `raw/_WIKI.md`: opis folderu POC.

## 4. Uwagi

- **Kopia przed przenosinami:** `storage/backups/ZAD-001-20261014/`: cały `context/`, oryginał R1 i pełne archiwum
  aplikacji POC razem z bibliotekami, `.env` i bazą demo (`poc-zamowienia-surowcow-pelna.tar.gz`, 17 129 pozycji, zgodne z oryginałem).
- **Stara półka** `storage/projects/intranet/context/` jest zamrożona: na górze 5 plików md jest linia z bieżącą ścieżką,
  a w json pierwszy klucz `_kopia_zamrozona` z tą samą informacją (plik nadal poprawny). Kasowanie razem z wpisami
  `context_files` 10–15 robi zadanie próby.
- **Kontrola półki:** jedna uwaga `dokument_w_rozmowie` dla R2 (DOCX zostaje w rozmowie). Usunie ją usunięcie DOCX
  z rozmowy albo odłożenie go na półkę; do decyzji Pawła.
- **Repozytorium projektów:** intranet nie jest podpięty (decyzja z 25.09.2026), więc `raw/` z kodem POC nie trafia na GitHub.
- **Liczba plików po przeniesieniu** (bez spisów `_WIKI.md`):
  - `zalozenia/`: 6 plików (S1–S4, S6, R1),
  - `dev/`: 1 plik (S5),
  - `raw/`: 1 folder (R3, 112 plików),
  - `outputs/bez-etapu/`: 1 plik (ta lista).
