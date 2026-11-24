<!--
opis: Standardy FEBO, część 05: ekrany aplikacji.
rodzaj: zalozenie
rozmowa: tenantStandards
zmiana: 16.10.2026, tenantStandards
status: roboczy
slowa: standardy,ekrany
-->
# Karta 5 — ekrany lokatora (T-57a)

Gałąź: `t-57a-ekrany`

## Środowisko

`ssh dev-febo`, `/home/ubuntu/febo-intranet`, `git pull` na `main`. Karty 1–4 scalone.

## Przeczytaj, zanim cokolwiek zmienisz

- `docs/contract/plan.contract.md` — karta **T-57a**
- `api/apps/standards/RULES.md` — **wszystkie trzydzieści reguł**; ekrany nie mogą złamać
  żadnej
- `docs/decisions/standardy-przeniesienie-poc.decisions.md` — rozdziały 7, 13, 16
- `docs/contract/core-ui.contract.md` — komponenty i żetony marki
- `docs/external/brand/` — makiety jako **referencja, nigdy jako komponenty do kopiowania**

## Co powstaje

Widoczna część wszystkiego, co powstało w kartach 1–4. **Nic nowego w dziedzinie**: jeśli
ekran potrzebuje reguły, której nie ma w kodzie lokatora, to karta jest źle napisana,
a odpowiedzią jest decyzja człowieka, a nie druga implementacja reguły.

- **lista** z kategoriami, tagami i wyszukiwaniem,
- **procedura pod stałym adresem** `/app/standards/procedure/<id>` — treść obowiązująca,
  metryka i historia wersji,
- **edytor autora**, otwierany na szablonie Micro-SOP,
- **zgłoszenie do zatwierdzenia**,
- **wersja wycofana**: zostaje pod swoim adresem z jednoznaczną adnotacją i odsyłaczem do
  następcy, ale znika z domyślnej listy i z wyszukiwarki,
- **plakietka „do przeglądu"** przy procedurach po terminie — nie zmienia ich stanu,
  procedura nadal obowiązuje,
- **arkusz stylów do druku**: metryka w nagłówku, **numer wersji i data zatwierdzenia
  w stopce**. Kartka bez tego wisi na ścianie trzy lata i nikt nie wie, czy jest aktualna.
  Wydruk do pliku robi systemowa drukarka PDF — karty eksportu nie ma i nie będzie.

**Zatwierdzenie nie pyta drugi raz w okienku.** Nad przyciskami stoi ostrzeżenie mówiące,
co się stanie. Dwa okna z rzędu uczą klikać „tak" bez czytania.

## Na co uważać

**Szkic to nie jest niczyja sprawa** poza autorem i osobą rozstrzygającą — lista nie może
pokazywać stanu, którego serwer i tak nie otworzy.

**Stan wersji nigdy nie jest przekazywany samym kolorem.**

Układ projektuj **najpierw pod tablet**. Wersja na telefon nie jest teraz priorytetem
(decyzja z 28.08.2026).

## Czego w tej karcie nie ma

Wyboru odbiorców zapoznania i widoku potwierdzeń — to karta 6. Asystenta — karta 7.

## Protokół

Plan w trybie tylko do odczytu, akceptacja, gałąź, `make check` i `make review`,
`docs/progress.md`, commit i push jako **redAi \<redai@febologistic.com\>**.

## Karta jest skończona, gdy

Całą drogę da się przejść klikając, na szerokości tabletu; **status lokatora w rejestrze
przechodzi z „w budowie" na „beta"**.
