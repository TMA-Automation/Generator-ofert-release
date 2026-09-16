# Generator ofert — instrukcja

Aplikacja pomaga aktualizować wartości ofert w ClickUp. Moduł **Update offer
value** skanuje folder z wycenami (pliki Excel) i porównuje wartość końcową
kontraktu z tym, co jest wpisane w polu `OFFER VALUE` zadania ClickUp — a
potem, dopiero po Twojej zgodzie, wysyła różnice. Program niczego nie zmienia
w Excelu ani w ClickUp bez Twojego kliknięcia „Wyślij”.

## Instalacja

1. Pobierz `GeneratorOfert.exe` z najnowszego wydania (zakładka **Releases**
   repozytorium `TMA-Automation/Generator-ofert-release`).
2. Skopiuj plik do **lokalnego** folderu, np. `C:\GeneratorOfert\`. Na pulpicie
   zostaw tylko skrót.
   > Nie uruchamiaj programu z folderu OneDrive — synchronizacja blokuje plik
   > i aktualizacja się nie powiedzie.
3. Uruchom `GeneratorOfert.exe`. Instalacja nie jest potrzebna.

Aktualizacje zgłaszają się same żółtym paskiem u góry okna — wystarczy kliknąć
**⬇ Pobierz i zainstaluj**.

## Pierwsze ustawienia (⚙)

Kliknij ikonę koła zębatego w prawym górnym rogu.

1. **Token API ClickUp** — w ClickUp: kliknij awatar w lewym dolnym rogu →
   **Settings** → **Apps** → sekcja **API Token** → **Generate**/**Copy**.
   Wklej skopiowany token w polu „Token API”.
2. Kliknij **Wykryj** przy „Team ID” — rozwinie się lista zespołów, do których
   token ma dostęp. Wybierz właściwy z listy.
3. **Lista zadań** — wklej **link do listy** z ClickUp (np.
   `https://app.clickup.com/1553698/v/l/6-200608428-1`) albo samo ID listy.
   Domyślnie wpisana jest lista `TMA MAIN`.
4. **Pole wartości** — nazwa pola w ClickUp, które ma być aktualizowane.
   Domyślnie `OFFER VALUE` — zmień tylko, jeśli w Twojej liście pole nazywa
   się inaczej.
5. **Folder** (sekcja „Oferty (Excel)”) — wskaż folder z plikami ofert
   (`…`), albo wpisz ścieżkę ręcznie. Program czyta tylko pliki
   `OFFER*.xlsm`/`OFFER*.xlsx` leżące **bezpośrednio** w tym folderze (nie
   przeszukuje podfolderów, np. archiwum poprzedniego roku).
6. **Zapisz** (zobaczysz „✓ Zapisano”).

## Praca z modułem — krok po kroku

Po wejściu do modułu **Update offer value** skan startuje **sam**, zaraz po
otwarciu — pasek na dole pokazuje, co program aktualnie robi („Pobieram
zadania z ClickUp…”, „Czytam oferty 34/187…”). Jeśli coś się zmieniło w
folderze lub w ClickUp, kliknij **⟳ Skanuj ponownie**.

### 1. Liczniki nad tabelą

Pierwsza linia: ile pozycji jest **do aktualizacji** (z rozbiciem na wzrost
▲, spadek ▼, nowe ●) i ile **bez zmian**. Druga podkreśla **⚠ ile wartości
pochodzi z innej komórki niż D65** (patrz niżej — te warto sprawdzić ręcznie).
Trzecia: ile wymaga Twojej decyzji, ile zadań nie ma pliku, ile plików nie ma
zadania, ile jest błędów.

### 2. Filtrowanie i szukanie

Pole **Szukaj** filtruje po numerze oferty, kliencie, nazwie zadania albo
pliku — możesz wpisać kilka słów naraz, wielkość liter i polskie znaki nie
mają znaczenia. Lista **Pokaż** zawęża widok do konkretnego stanu (np. „Wzrost
▲”, „Wymagające decyzji”, „Zadania bez pliku”). Filtr **Status CU** pokazuje
tylko zadania w danym statusie ClickUp. Klik w nagłówek kolumny sortuje tabelę,
drugi klik odwraca kolejność.

### 3. Co sprawdzić przed wysyłką

- **⚠ Inna komórka niż D65** — program znalazł etykietę „Wartość końcowa
  kontraktu” nie w standardowym miejscu (D65), tylko gdzie indziej w tym
  samym arkuszu (np. D66 — bywa tak, gdy w D65 jest marża przedstawiciela, nie
  wartość kontraktu). Kolumna „Źródło” pokazuje wtedy `⚠ D66` zamiast `D65`.
  Warto zerknąć do pliku i upewnić się, że to naprawdę właściwa liczba.
- **Spadki ▼** — wartość w ofercie jest niższa niż to, co już jest wpisane w
  ClickUp. Zdarza się (renegocjacja, korekta), ale warto rzucić okiem, czy to
  nie pomyłka w pliku.
- **Wymagające decyzji** (patrz niżej) — te wiersze same się nie zaznaczą,
  dopóki nie rozstrzygniesz, o które zadanie/wariant chodzi.

### 4. Rozstrzyganie niejednoznacznych i wariantów

Dwa stany wymagają Twojej decyzji, zanim wiersz w ogóle będzie można wysłać:

- **niejednoznaczne** — ten sam numer oferty pasuje do kilku zadań w
  ClickUp (typowo różni klienci pod tym samym numerem) i program nie wie,
  które wybrać.
- **kilka wariantów** — plik Excela ma kilka arkuszy zaczynających się od
  „ZESTAWIENIE” (np. dla różnych wersji cenowych) zamiast jednego.

**Podwójny klik** na takim wierszu (albo `Enter`, gdy jest zaznaczony)
otwiera okno wyboru — kliknij właściwą pozycję i **Wybierz**. Wiersz od razu
przelicza się na normalny stan (do aktualizacji / bez zmian / brak ceny).

### 5. Zaznaczanie i wysyłka

Klik w kolumnę **☑** przełącza pojedynczy wiersz. **Zaznacz widoczne** /
**Odznacz widoczne** działają na tym, co aktualnie widać po filtrach — wygodne,
żeby np. zaznaczyć tylko przefiltrowane „Wzrost ▲”. Domyślnie zaznaczone są
wszystkie wiersze „do aktualizacji”.

Kliknij **⬆ Wyślij zaznaczone (N)** — pokaże się okno potwierdzenia z
podsumowaniem (ile ▲, ▼, ●, ile z innej komórki niż D65) i pełną listą
pozycji. Sprawdź jeszcze raz i kliknij **⬆ Wyślij**. Zapisy idą po kolei;
log na dole pokazuje na bieżąco, co się udało (✓) i co nie (✗, z przyczyną).
Wysyłkę można **wstrzymać** albo **anulować** przyciskami na dole okna — to,
co już wysłano, zostaje w ClickUp.

Po wysyłce wiersz dostaje status „✓ wysłano” i znika z „do aktualizacji”.
Jeśli coś się nie udało, wiersz zostaje zaznaczalny ze statusem „✗ błąd
wysyłki” — możesz spróbować wysłać go ponownie.

### 6. Raport

**⤓ Zapisz raport (CSV)** zapisuje bieżący stan tabeli do pliku — otwiera się
poprawnie w Excelu (polskie znaki, przecinek dziesiętny) i zawiera link do
każdego zadania ClickUp.

## Najczęstsze problemy

**„Na liście nie ma pola OFFER VALUE”.**
Sprawdź w ustawieniach (⚙), czy nazwa pola dokładnie zgadza się z nazwą pola
w ClickUp (wielkość liter nie ma znaczenia, ale literówka już tak).

**Plik oferty pokazuje „błąd odczytu”.**
Najczęściej plik jest otwarty w Excelu (zablokowany) albo leży na OneDrive z
włączonymi „Plikami na żądanie” i nie jest jeszcze pobrany na dysk — kliknij
plik prawym przyciskiem w Eksploratorze i wybierz „Zawsze zachowuj na tym
urządzeniu”, albo po prostu zamknij plik w Excelu i kliknij „⟳ Skanuj
ponownie”.

**Wiersz ma stan „brak ceny”.**
Program nie znalazł etykiety „Wartość końcowa kontraktu” w arkuszu
ZESTAWIENIE, albo wartość jest pusta, zerowa, albo to błąd formuły
(`#DIV/0!` itp.). Otwórz plik i sprawdź arkusz ręcznie — kolumna „Przyczyna”
(prawy przycisk myszy → „Pokaż przyczynę”) mówi dokładnie, co się nie zgadza.

**Zadanie bez pliku / plik bez zadania — co robić?**
To nie jest błąd programu — po prostu w folderze nie ma jeszcze pliku dla
zadania z ClickUp (albo odwrotnie: plik jest, ale nikt nie założył jeszcze
zadania). Te wiersze nie da się zaznaczyć do wysyłki — jeśli to się nie
zgadza, sprawdź, czy plik ma poprawną nazwę (wzorzec `OFFERnnnnn.w - KLIENT -
opis`) i czy leży bezpośrednio w skonfigurowanym folderze, nie w podfolderze.

**„Nie rozpoznano nazwy”.**
Nazwa pliku albo zadania nie pasuje do wzorca `OFFERnnnnn.w - KLIENT - opis`
(np. brakuje cyfry w numerze albo numer nie ma dokładnie 5 cyfr). Popraw
nazwę pliku albo tytuł zadania w ClickUp i uruchom skan ponownie.

**Program czeka i pisze coś o limicie.**
ClickUp ogranicza liczbę żądań na minutę. Program sam odczekuje i ponawia —
nie przerywaj, nie klikaj ponownie.
