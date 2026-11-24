<!--
opis: Notka zadania kodu ZAD-046: https://github.com/SmallBigSteps/intranet/pull/247
rodzaj: wynik
zadanie: ZAD-046
kamien: KM-4 Uruchomienie produkcyjne i przekazanie do utrzymania
rozmowa: zad046ModulPlikow
zmiana: 13.11.2026, zad046ModulPlikow
status: do akceptacji
-->
# ZAD-046: Moduł plików w Intranecie wyciągnięty przed WhatsApp — rejestr i magazyn plików, podpisane adresy, retencja, formaty multimedialne oraz retencja ustawiana przez właściciela procesu (karty T-49, T-50, T-82) (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/pull/247
- Co zmieniono: Przegląd modułu plików po ZAD-046 (P1–P10): wspólne usuwanie na stałe dla kosza i retencji z ponownym sprawdzeniem wstrzymań i daty pod blokadą, ekran wstrzymań retencji z informacją o skutku zdjęcia (odsłonięte pliki, aplikacja, nagrobki), poprawki po czterech scaleniach z main. Wcześniejsze kroki: PR #220, #240, #241.
- Jak sprawdzić: Na dev: Pliki › Retencja — lista wstrzymań pokazuje aplikację, skutek zdjęcia i usunięte obiekty; plik w koszu usunięty na stałe znika z magazynu i zostawia nagrobek; wstrzymany plik nie jest usuwany ani przez kosz, ani przez retencję. CI gałęzi zielone.
