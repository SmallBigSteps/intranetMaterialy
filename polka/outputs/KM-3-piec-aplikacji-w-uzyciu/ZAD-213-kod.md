<!--
opis: Notka zadania kodu ZAD-213: https://github.com/SmallBigSteps/intranetFebo/pull/231
rodzaj: wynik
zadanie: ZAD-213
kamien: KM-3 Pięć aplikacji w użyciu
rozmowa: ZAD-213 Szyfrowanie archiwum audytu
zmiana: 02.11.2026, ZAD-213 Szyfrowanie archiwum audytu
status: do akceptacji
-->
# ZAD-213: Szyfrowanie archiwum audytu (zmiana w kodzie)

- PR: https://github.com/SmallBigSteps/intranetFebo/pull/231
- Co zmieniono: Archiwum dziennika audytu szyfrowane programem age do kluczy publicznych z AUDIT_ARCHIVE_RECIPIENTS: osobna część na partię, jawna lista identyfikatorów, odmowa bez kluczy; kontrakt core-audit 3.8, decyzja i krok 8a instrukcji wdrożenia.
- Jak sprawdzić: Testy tests/test_audit_archive_encryption.py i tests/test_audit_archive_batches.py; na dev po wpisaniu klucza publicznego pary testowej nocne audit.archive tworzy pliki .jsonl.age, które otwiera klucz prywatny z Dostępów.
