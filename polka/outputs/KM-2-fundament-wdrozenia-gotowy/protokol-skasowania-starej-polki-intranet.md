<!--
opis: Protokół skasowania starej półki intranetu: próg próby, kopia, co skasowano, zamknięte uwagi kontroli #64 i #75.
rodzaj: wynik
zadanie: ZAD-002
kamien: KM-2 Fundament wdrożenia gotowy
rozmowa: zad002SkasowanieStarejPolkiIntranetu
zmiana: 24.11.2026, zad002SkasowanieStarejPolkiIntranetu
status: do akceptacji
slowa: półki, stara półka, context_files, skasowanie, kopia, intranet, kontrola półki
-->
# Protokół skasowania starej półki: intranet (Półki C)

Wydanie 1 · 24.11.2026 · rozmowa zad002SkasowanieStarejPolkiIntranetu (ZAD-002). Uzupełnia listę z ZAD-001
(`outputs/bez-kamienia/lista-uzupelniajaca-intranet.md`, rozdz. 4). Ustalenia z wywiadu z Pawłem 24.11.2026.

## 1. Próg skasowania (lista migracji helpDesk, rozdz. 1 pkt 4)

| Warunek | Stan | Źródło |
|---|---|---|
| 7 dni pracy tylko na nowej półce | spełniony: zamrożenie 14.10.2026, skasowanie 24.11.2026 (ok. 6 tygodni) | `_WIKI.md`, historia ZAD-002 |
| Ani razu nie brakuje pliku | spełniony: nikt nie sięgał do starej półki ani nie zgłaszał braku | Paweł, 24.11.2026 |
| Kontrola poranna czysta | spełniony po zamknięciu uwag #64 i #75 (rozdz. 4): 0 uwag | `pmo:uwagi sprawdz`, 24.11.2026 |
| Akceptacja Pawła | zgoda w rozmowie; akceptacja zadania przez DO AKCEPTACJI | ten protokół |
| Kopia przed kasowaniem | zrobiona i sprawdzona (rozdz. 2) | `storage/backups/ZAD-002-20261124/` |

## 2. Kopia przed kasowaniem

`storage/backups/ZAD-002-20261124/`:

- `context/`: 6 plików starej półki w stanie zamrożonym (z linią „Kopia zamrożona…”), zgodne z oryginałem (porównanie katalogów).
- `context_files-10-15.sql`: zrzut 6 wpisów bazy (id 10–15). Przywrócenie nie kasuje innych wpisów
  (bez `DROP TABLE`, `CREATE TABLE IF NOT EXISTS`, `INSERT IGNORE`).

Wersje sprzed zamrożenia są w kopii z ZAD-001: `storage/backups/ZAD-001-20261014/context/`.

## 3. Co skasowano

| # | Co | Gdzie | Uwagi |
|---|---|---|---|
| K1 | katalog `context/` (6 plików: PRD, CLIENT_BRD, RULES, ARCHITECTURE, CLAUDE, json systemu wizualnego) | `storage/projects/intranet/context/` | katalog `storage/projects/intranet/` zostaje (miejsce na kopię główną kodu) |
| K2 | wpisy `context_files` id 10–15 (projekt intranet) | baza portalu | projekt nie ma już wpisów; rozmowy projektu przestają dostawać listę „Pliki projektu” ze starymi ścieżkami |

Bieżące wersje tych plików: `zalozenia/` (PRD, CLIENT_BRD, RULES, ARCHITECTURE, json) i `dev/CLAUDE-intranet.md`.
Tabela przekierowań zostaje w `_WIKI.md` projektu (dla starych odwołań w rozmowach).

## 4. Uwagi kontroli zamknięte przy okazji

| Uwaga | Co było | Co zrobione |
|---|---|---|
| #64 `dokument_w_rozmowie` | `Intranet_Canexpol_zalozenia_produkcyjne.docx` w rozmowie „Intranet — czat 04.08 17:38” | usunięty z rozmowy bez kopii (decyzja Pawła); treść identyczna z md na półce (`zalozenia/Intranet_Canexpol_zalozenia_produkcyjne.md`, porównanie słów 1471/1471, brak różnic) |
| #75 instrukcja bez „Repozytoria projektu” | opis repozytorium w `dev/` bez sekcji w instrukcji | sekcja dopisana do `_INSTRUKCJA-PROJEKTU.md` według wzoru z procedury: repozytorium produktu tylko do odczytu z `lab`, własne repozytorium Canexpol zakłada ZAD-004; kto scala PR, do ustalenia w ZAD-004 |

## 5. Uwagi

- W bazie zostaje wpis id 162 (automatyczny import DOCX z rozmowy 04.08 do plików kontekstowych użytkownika,
  poza projektem). Nie należy do starej półki i kontrola go nie zgłasza; nie ruszany.
- `_WIKI.md` projektu: akapit „Przekierowanie” mówi o skasowaniu i wskazuje kopię.
