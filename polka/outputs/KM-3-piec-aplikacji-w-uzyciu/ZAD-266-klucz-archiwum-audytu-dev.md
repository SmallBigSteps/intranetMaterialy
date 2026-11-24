<!--
opis: Klucz age archiwum audytu wpisany na dev i sprawdzony; usterka wdrożenia (obraz migrate) opisana i przeniesiona do ZAD-294.
rodzaj: wynik
zadanie: ZAD-266
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-266 Klucz archiwum audytu na dev
zmiana: 13.11.2026, ZAD-266 Klucz archiwum audytu na dev
status: do akceptacji
slowa: audyt, archiwum, age, dev, AUDIT_ARCHIVE_RECIPIENTS, migrate, martwa kolejka
-->
# ZAD-266 — klucz archiwum audytu na dev

## Co zrobiono (13.11.2026)

| Krok | Wynik |
|---|---|
| Para age | wygenerowana przez P. Mazka; prywatny w Bitwardenie i w Dostępach projektu (`age-archiwum-audytu-dev`) |
| Zgodność pary | klucz publiczny wyliczony z klucza w Dostępach = klucz wpisany na dev |
| Ustawienie dev | `AUDIT_ARCHIVE_RECIPIENTS` w `.env` dev (kopia `.env.bak-20261113-zad266`); api i worker widzą klucz |
| Wdrożenie | main @ fa77d0a na dev, wszystkie usługi zdrowe, strona dev 200 |
| Ręczne `audit.archive` | sukces, `archived = {}` — na dev nie ma jeszcze wpisów po terminie retencji |
| Próba szyfrowania | `_encrypt` kluczem z ustawień → odszyfrowanie kluczem z Dostępów: treść identyczna (sha256), plik testowy usunięty |

Dziennik audytu nie był ruszany; pierwsze prawdziwe pliki `.jsonl.age` na dev powstaną dopiero, gdy wpisy przekroczą retencję.

## Usterka wykryta po drodze → ZAD-294

`make deploy` buduje obrazy nginx, api i worker, ale nie jednorazowej usługi `migrate`. Każde `docker compose up`
uruchamia ją na starym obrazie, a jej `post_migrate` → `registry.sync()` oznacza nowsze zadania jako wycofane.
Na dev dotyczyło to 12 zadań (m.in. `audit.archive`, `audit.purge_security`, `files.apply_retention`,
`accounts.refresh_substitutions`); nocnej archiwizacji nie było od 30.10.
Na dev: obraz `migrate` przebudowany z main, rejestr zsynchronizowany (wycofane zostało tylko `logtms.sync_dictionaries`).
Trwała poprawka w procedurze wdrożenia: ZAD-294 (inaczej problem wróci na test i prod).

## Zostaje

- 14.11 rano: kontrola nocnego `audit.archive` z 01:30 (przebieg `schedule`, wynik success).
- Martwa kolejka: 2 oczekujące wpisy `audit.archive` z 29.10 (`ArchiveUnavailableError` sprzed klucza).
  Wznowić może tylko `shell.admin_tech` z potwierdzonym drugim składnikiem (panel → martwa kolejka → Wznów).
