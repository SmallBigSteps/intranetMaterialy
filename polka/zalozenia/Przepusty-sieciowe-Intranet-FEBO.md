<!--
opis: Przepusty sieciowe potrzebne Intranetowi FEBO (porty, kierunki, systemy).
rodzaj: zalozenie
rozmowa: resources
zmiana: 16.10.2026, resources
status: roboczy
slowa: siec,przepusty,porty
-->
# Przepusty sieciowe — Intranet FEBO

**Wersja:** 1.0 · **Data:** 31.08.2026 · **Dotyczy:** wdrożenia Intranetu FEBO na serwerze lokalnym
**Przygotował:** redAi / Small Big Steps · **Adresat:** pewne.it (administracja infrastruktury)

Kolumny z adresami IP pozostawiono puste — uzupełnia je administrator infrastruktury po nadaniu
adresacji. Numeracja prowadzona z przerwami, żeby dało się dopisywać wiersze bez przenumerowania
całości.

---

## 1. Czego dotyczy zestawienie

Cztery maszyny wirtualne w jednym nowym VLAN-ie przeznaczonym dla aplikacji, poza siecią serwerową.

| Maszyna | Rola | Pamięć | Wątki | Internet |
|---|---|---|---|---|
| `febo-prod-app` | Aplikacja produkcyjna: nginx, aplikacja, worker, scheduler, Redis | 8 GB | 6 | tylko przez pośrednika |
| `febo-prod-db` | PostgreSQL 17 produkcji | 12 GB | 4 | brak (poza aktualizacjami — patrz FW-032) |
| `febo-test-app` | Aplikacja testowa oraz budowanie wersji | 8 GB | 4 | tylko przez pośrednika |
| `febo-test-db` | PostgreSQL 17 środowiska testowego | 6 GB | 2 | brak (poza aktualizacjami — patrz FW-032) |

System na wszystkich czterech maszynach: **Debian 13**. Baza: **PostgreSQL 17** wraz z rozszerzeniem
wektorowym, oba z podstawowego zestawu pakietów dystrybucji.

Środowisko deweloperskie **nie stoi w FEBO** — pracuje u dostawcy (redAi) i występuje w tym zestawieniu
wyłącznie w wierszach FW-070 i FW-071.

## 2. Zasady, z których wynika ta tabela

1. **Produkcja nie ma dostępu do internetu.** Pobiera wyłącznie gotowe, ponumerowane wersje z rejestru
   obrazów stojącego po stronie FEBO.
2. **Maszyna testowa jest jedyną, która buduje.** Pobiera kod z GitHuba oraz biblioteki i obrazy bazowe —
   w całości przez pośrednika z listą dozwolonych adresów.
3. **Cały ruch wychodzący idzie przez pośrednika.** Żadna maszyna nie ma drogi wprost do internetu.
   Listę dozwolonych adresów prowadzi Mateusz Majchrzak (FEBO).
4. **Systemy źródłowe czytamy wyłącznie przez dedykowane widoki**, kontami tylko do odczytu, osobnymi
   dla produkcji i dla testu. Nigdy z tabel źródłowych.
5. **Do sieci FEBO nie wpuszczamy ruchu z internetu.** Jedyne połączenie wychodzące poza sieć firmy
   (FW-070) zestawia strona FEBO i może je w każdej chwili wyłączyć.
6. **Czego nie ma w tabeli, tego nie potrzebujemy.** Maszyny baz nie widzą sieci biurowej, produkcja
   i test nie mają powodu się widzieć.

## 3. Tabela przepustów

| ID | Źródło / aplikacja | Source IP / subnet | Cel / aplikacja | Destination IP / FQDN | Protokół | Port docelowy | Kierunek | Cel komunikacji | Środowisko | Uwagi |
|---|---|---|---|---|---|---|---|---|---|---|
| FW-001 | Sieć biurowa (użytkownicy) |  | `febo-prod-app` |  | TCP | 443 | biuro → APP | Korzystanie z Intranetu | PROD | TLS |
| FW-002 | Sieć biurowa (użytkownicy) |  | `febo-test-app` |  | TCP | 443 | biuro → TEST | Odbiór wersji przed wdrożeniem | TEST | TLS |
| FW-003 | Stacja Mateusza (wydzielony adres) |  | `febo-prod-app` |  | TCP | 22 | admin → APP | Administracja, wdrożenia | PROD | Jedyny adres z dostępem SSH |
| FW-004 | Stacja Mateusza (wydzielony adres) |  | `febo-test-app` |  | TCP | 22 | admin → TEST | Administracja, budowanie wersji | TEST | Jedyny adres z dostępem SSH |
| FW-005 | Paweł Mazek, przez OpenVPN do sieci FEBO |  | `febo-prod-app` |  | TCP | 22 | admin → APP | Administracja zastępcza, ciągłość | PROD | Adres z puli VPN, najlepiej przypisany na stałe |
| FW-006 | Paweł Mazek, przez OpenVPN do sieci FEBO |  | `febo-test-app` |  | TCP | 22 | admin → TEST | Administracja zastępcza, ciągłość | TEST | Adres z puli VPN, najlepiej przypisany na stałe |
| FW-010 | `febo-prod-app` |  | `febo-prod-db` |  | TCP | 5432 | APP → DB | PostgreSQL: dane i kolejka zadań | PROD | Brak ruchu zwrotnego inicjowanego przez bazę |
| FW-011 | `febo-test-app` |  | `febo-test-db` |  | TCP | 5432 | TEST → DB | PostgreSQL: dane i kolejka zadań | TEST | Brak ruchu zwrotnego inicjowanego przez bazę |
| FW-020 | `febo-test-app` |  | Rejestr obrazów |  | TCP | 443 | TEST → rejestr | Publikowanie zbudowanych wersji | TEST | Zapis, z uwierzytelnianiem |
| FW-021 | `febo-prod-app` |  | Rejestr obrazów |  | TCP | 443 | APP → rejestr | Pobieranie zatwierdzonych wersji | PROD | Tylko odczyt |
| FW-030 | `febo-test-app` |  | Pośrednik (Squid) |  | TCP | 3128 | TEST → proxy | Budowanie: GitHub, biblioteki, obrazy bazowe; zewnętrzne API; aktualizacje systemu | TEST | Port do potwierdzenia |
| FW-031 | `febo-prod-app` |  | Pośrednik (Squid) |  | TCP | 3128 | APP → proxy | Zewnętrzne API lokatorów, wysyłka poczty przez Microsoft Graph, aktualizacje systemu | PROD | Bez dostępu wprost do internetu |
| FW-032 | `febo-prod-db`, `febo-test-db` |  | Pośrednik (Squid) |  | TCP | 3128 | DB → proxy | Wyłącznie aktualizacje Debiana | PROD/TEST | Wiersz odpada, jeśli maszyny baz są łatane w inny sposób |
| FW-033 | Pośrednik (Squid) |  | Internet | lista dozwolonych adresów | TCP | 443 | proxy → internet | Wyłącznie adresy z listy | PROD/TEST | Listę prowadzi Mateusz Majchrzak |
| FW-040 | `febo-prod-app` |  | Optima (MS SQL) |  | TCP | 1433 | APP → SQL | Odczyt dedykowanych widoków | PROD | Konto tylko do odczytu, bez tabel źródłowych |
| FW-041 | `febo-test-app` |  | Optima (MS SQL) |  | TCP | 1433 | TEST → SQL | Odczyt dedykowanych widoków | TEST | Osobne konto, te same zasady |
| FW-050 | `febo-prod-app` |  | LogTMS (MySQL), przez tunel |  | TCP | 3306 | APP → LogTMS | Odczyt danych spedycyjnych | PROD | Sposób zestawienia tunelu — osobne ustalenie |
| FW-051 | `febo-test-app` |  | LogTMS (MySQL), przez tunel |  | TCP | 3306 | TEST → LogTMS | Odczyt danych spedycyjnych | TEST | Osobne konto |
| FW-060 | Wszystkie cztery maszyny |  | DNS wewnętrzny |  | UDP/TCP | 53 | maszyny → DNS | Rozwiązywanie nazw | PROD/TEST | Dwa serwery |
| FW-061 | Wszystkie cztery maszyny |  | Serwer czasu |  | UDP | 123 | maszyny → NTP | Synchronizacja czasu | PROD/TEST | Warunek działania drugiego składnika logowania |
| FW-062 | Stacja Mateusza, Paweł Mazek przez OpenVPN |  | Wszystkie cztery maszyny |  | ICMP | — | admin → maszyny | Diagnostyka | PROD/TEST |  |
| FW-070 | Punkt końcowy tunelu w FEBO |  | Środowisko deweloperskie redAi |  | do uzgodnienia | do uzgodnienia | FEBO → redAi | Zestawienie tunelu, inicjowane wyłącznie od strony FEBO | DEV | Np. WireGuard; nic nie jest wpuszczane z internetu |
| FW-071 | Środowisko deweloperskie redAi (przez tunel) |  | Optima **testowa** (MS SQL) |  | TCP | 1433 | redAi → SQL test | Odczyt dedykowanych widoków | DEV | Wyłącznie instancja testowa, konto tylko do odczytu |
| FW-080 | Serwer kopii zapasowych |  | Maszyny objęte kopią |  |  |  |  | Wykonanie kopii zapasowych | PROD/TEST | Do uzupełnienia przez administratora infrastruktury |

## 4. Pozycje wymagające rozstrzygnięcia

**FW-032 — aktualizacje maszyn baz danych.** We wcześniejszej korespondencji deklarowaliśmy, że maszyny
baz nie mają ruchu wychodzącego w ogóle. Skoro cały ruch wychodzący prowadzimy przez pośrednika, trzeba
wybrać jedno z dwojga: albo maszyny baz sięgają przez niego po poprawki Debiana, albo są łatane w inny
sposób i wiersz zostaje wykreślony. Decyzja należy do administratora infrastruktury.

**FW-070 i FW-071 — droga do środowiska deweloperskiego.** To jedyne połączenie wychodzące poza sieć
FEBO. Powód: środowisko deweloperskie stoi u dostawcy i musi widzieć **testową** instancję Optimy, żeby
integracja powstawała na realnym kształcie danych. Połączenie zestawia strona FEBO; z internetu nie
otwiera się nic do środka, a wyłączenie tunelu jest jednostronną decyzją FEBO.

**FW-080 — kopie zapasowe.** Kopie pozostają w gestii administratora infrastruktury, więc kierunek
i porty uzupełnia on. Po naszej stronie: zrzut logiczny bazy wykonywany codziennie o 01:30, przed
kopią całych maszyn, oraz zasada „najpierw pliki, potem baza" przy odtwarzaniu.

**Rejestr obrazów.** Zakładamy osobną usługę z uwierzytelnianiem, dostępną po HTTPS, o pojemności około
40 GB, przechowującą kilka ostatnich wydań. Maszyna testowa ma do niej prawo zapisu, produkcyjna
wyłącznie odczytu.
