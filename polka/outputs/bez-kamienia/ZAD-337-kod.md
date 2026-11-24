<!--
opis: Notka zadania kodu ZAD-337: https://github.com/SmallBigSteps/intranet/compare/main...zad-337-test-rol-kadr?expand=1
rodzaj: wynik
zadanie: ZAD-337
rozmowa: ZAD-337 Test ról Kadr
zmiana: 24.11.2026, ZAD-337 Test ról Kadr
status: do akceptacji
-->
# ZAD-337: Test ról Kadr (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranet/compare/main...zad-337-test-rol-kadr?expand=1
- Co zmieniono: Test ról Kadr (test_hr_administers_accounts_but_directors_administer_permissions) czyta tylko role powłoki shell.* i dalej wymaga dokładnie {shell.user_admin}; rola aplikacji directory.admin z wdrożenia FEBO (ZAD-016) go nie łamie. Gałąź zad-337-test-rol-kadr, commit c59d997.
- Jak sprawdzić: make test z INTRANET_DEPLOYMENT=febo_deployment: test przechodzi (na lab 2328 passed; stara wersja pada na directory.admin). Bez wdrożenia: 1614 passed. Uwaga: wariant bez wdrożenia na main nie startuje w czystym środowisku (nplusone.py wymienia standards.Procedure, test_aud_transaction importuje apps.directory) — osobna sprawa.
