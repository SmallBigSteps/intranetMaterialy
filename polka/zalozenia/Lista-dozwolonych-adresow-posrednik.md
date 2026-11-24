<!--
opis: Lista adresów, które Intranet musi osiągać przez pośrednika (proxy).
rodzaj: zalozenie
rozmowa: resources
zmiana: 16.10.2026, resources
status: roboczy
slowa: siec,proxy,adresy
-->
# Lista dozwolonych adresów na pośredniku — Intranet FEBO

**Wersja:** 1.0 · **Data:** 31.08.2026 · **Dotyczy:** pośrednika ruchu wychodzącego (Squid)
**Przygotował:** redAi / Small Big Steps · **Prowadzi listę:** Mateusz Majchrzak (FEBO)

Maszyny Intranetu nie mają dostępu do internetu wprost. Każde połączenie na zewnątrz idzie przez
pośrednika, który przepuszcza wyłącznie adresy z poniższej listy i zapisuje ślad każdego zapytania.
Wszystkie pozycje działają po porcie **443** (HTTPS), protokół **TCP**.

---

## 1. Lista startowa

| Lp. | Adres | Do czego służy | `febo-test-app` | `febo-prod-app` | Maszyny baz |
|---|---|---|---|---|---|
| 1 | `github.com` | Pobieranie kodu Intranetu z repozytorium | tak | nie | nie |
| 2 | `api.github.com` | Obsługa repozytorium przy budowaniu | tak | nie | nie |
| 3 | `codeload.github.com` | Pobieranie paczek z kodem | tak | nie | nie |
| 4 | `objects.githubusercontent.com` | Pliki wydań i zasobów repozytorium | tak | nie | nie |
| 5 | `raw.githubusercontent.com` | Pojedyncze pliki z repozytorium | tak | nie | nie |
| 6 | `pypi.org` | Katalog bibliotek Pythona | tak | nie | nie |
| 7 | `files.pythonhosted.org` | Pobieranie bibliotek Pythona | tak | nie | nie |
| 8 | `registry.npmjs.org` | Biblioteki JavaScriptu (część przeglądarkowa) | tak | nie | nie |
| 9 | `registry-1.docker.io` | Obrazy bazowe kontenerów | tak | nie | nie |
| 10 | `auth.docker.io` | Uwierzytelnianie przy pobieraniu obrazów | tak | nie | nie |
| 11 | `index.docker.io` | Katalog obrazów | tak | nie | nie |
| 12 | `production.cloudflare.docker.com` | Właściwe pobranie warstw obrazu | tak | nie | nie |
| 13 | `deb.debian.org` | Aktualizacje systemu Debian | tak | tak | tak |
| 14 | `security.debian.org` | Poprawki bezpieczeństwa systemu | tak | tak | tak |
| 15 | `login.microsoftonline.com` | Logowanie do Microsoft 365 przy wysyłce poczty | tak | tak | nie |
| 16 | `graph.microsoft.com` | Wysyłka poczty z dedykowanej skrzynki Intranetu | tak | tak | nie |
| 17 | `api.nbp.pl` | Kursy walut Narodowego Banku Polskiego | tak | tak | nie |

**Pozycje 1–12 są potrzebne wyłącznie maszynie testowej**, bo to na niej budujemy wersje. Produkcja
pobiera gotowe, ponumerowane wydania z rejestru obrazów stojącego wewnątrz sieci FEBO i nie sięga
po nic z internetu poza aktualizacjami systemu, pocztą i danymi zewnętrznymi.

**Pozycje 13–14 dotyczą także maszyn baz danych** — o ile nie są one łatane w inny sposób. Decyzja
w tej sprawie należy do administratora infrastruktury (pozycja FW-032 w zestawieniu przepustów).

**Pozycje 15–16** obsługują wysyłkę poczty przez Microsoft 365. Skrzynka jest dedykowana Intranetowi,
osobne dane dostępowe dla produkcji i dla testu. Środowisko testowe domyślnie **nie wysyła poczty
do pracowników**.

## 2. Jak lista będzie rosła

Intranet składa się z małych aplikacji dziedzinowych. Każda kolejna może przynieść własne zewnętrzne
źródło danych — kurs, rejestr, słownik, usługę mapową. Zmiany będą przychodzić **po jednym adresie**,
a nie po kilkanaście naraz.

Dlatego prowadzenie listy jest po stronie Mateusza Majchrzaka: dopisanie jednego adresu ma być
kwestią minut, a nie zgłoszenia oczekującego na wolne ręce administratora. Zamknięty charakter listy
i tak pozostaje zachowany — nic spoza niej nie przechodzi.

Przy dopisywaniu obowiązują trzy zasady:

1. **Adres, nie zakres.** Wpisujemy konkretną nazwę, nie całą domenę z gwiazdką.
2. **Wskazanie maszyny.** Każdy wpis mówi, która maszyna go potrzebuje — produkcja, test, czy obie.
3. **Powód w opisie.** Po roku nikt nie pamięta, po co wpuszczono dany adres; brak powodu oznacza,
   że przy przeglądzie wpis zostanie usunięty.

## 3. Czego na liście nie ma i nie będzie

- **Dostępu do internetu dla maszyn baz danych** poza aktualizacjami systemu.
- **Ruchu przychodzącego z internetu do sieci FEBO** — pośrednik obsługuje wyłącznie połączenia
  wychodzące, inicjowane przez maszyny Intranetu.
- **Usług poczty typu SMTP** — poczta wychodzi przez interfejs Microsoft 365 po HTTPS, nie przez
  własny serwer pocztowy.
- **Ogólnego dostępu do internetu dla użytkowników** — pośrednik obsługuje maszyny Intranetu,
  nie stacje pracowników.
