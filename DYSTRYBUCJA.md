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
  wartość kontraktu), **albo** znalazł etykietę w D65, ale ktoś wstawił obok
  dodatkową kolumnę (np. z przeliczeniem na EUR) i właściwa wartość w PLN
  przesunęła się do innej litery (np. E65). Kolumna „Źródło” pokazuje wtedy
  `⚠ D66` albo `⚠ E65` zamiast `D65`. Warto zerknąć do pliku i upewnić się,
  że to naprawdę właściwa liczba w PLN, a nie przeliczenie walutowe.
- **Spadki ▼** — wartość w ofercie jest niższa niż to, co już jest wpisane w
  ClickUp. Zdarza się (renegocjacja, korekta), ale warto rzucić okiem, czy to
  nie pomyłka w pliku.
- **Wymagające decyzji** (patrz niżej) — te wiersze same się nie zaznaczą,
  dopóki nie rozstrzygniesz, o które zadanie/wariant/plik chodzi.

### 4. Rozstrzyganie niejednoznacznych, wariantów i kilku plików

Trzy stany wymagają Twojej decyzji, zanim wiersz w ogóle będzie można wysłać:

- **niejednoznaczne** — ten sam numer oferty pasuje do kilku zadań w
  ClickUp (typowo różni klienci pod tym samym numerem) i program nie wie,
  które wybrać.
- **kilka wariantów** — plik Excela ma kilka arkuszy zaczynających się od
  „ZESTAWIENIE” (np. dla różnych wersji cenowych) zamiast jednego.
- **kilka plików** — w folderze leżą dwa (albo więcej) pliki o dokładnie tej
  samej nazwie oferty i wersji (np. `Robot.xlsm` obok `Robot.xlsx`, albo plik
  z dopiskiem „- kopia”) i program nie wie, z którego wziąć wartość.

Wiersz wymagający decyzji ma w kolumnie **Stan** dopisek „— wybierz ▸”, żeby
było od razu widać, że coś trzeba rozstrzygnąć. Sposoby otwarcia okna wyboru:

- **Podwójny klik** na wierszu (albo `Enter`, gdy jest zaznaczony w tabeli).
- Przycisk **☰ Rozstrzygnij (N)** w pasku narzędzi — otwiera okno decyzji dla
  zaznaczonego wiersza (albo pierwszego nierozstrzygniętego, gdy nic nie jest
  zaznaczone) i po każdym wyborze **od razu przechodzi do kolejnego** — dzięki
  temu można rozstrzygnąć wszystkie decyzje jedna po drugiej bez szukania ich
  w tabeli. „Anuluj” w oknie przerywa serię.
- **Prawy przycisk myszy** na wierszu → pozycja „Wybierz zadanie…” / „Wybierz
  wariant…” / „Wybierz plik…” (nazwa zależy od stanu wiersza).

W oknie kliknij właściwą pozycję i **Wybierz** (albo podwójny klik na
pozycji). Wiersz od razu przelicza się na normalny stan (do aktualizacji /
bez zmian / brak ceny). Pod tabelą główną jest podpowiedź przypominająca te
skróty.

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

Jeśli po wysyłce w tabeli zostały wiersze **plik bez zadania**, program od
razu zapyta, czy utworzyć dla nich zadania w ClickUp (patrz punkt 7 niżej).

### 6. Zapamiętane wybory (📌)

Gdy raz rozstrzygniesz „niejednoznaczne”, „kilka wariantów” albo „kilka
plików”, program **zapamiętuje Twój wybór** (który plik/arkusz/zadanie —
**nigdy kwotę**) i przy kolejnym skanie stosuje go sam, bez pytania. Taki
wiersz dostaje w kolumnie „Źródło” znacznik **📌** i wchodzi do licznika „📌 z
zapamiętanego wyboru” nad tabelą. Kwota jest zawsze czytana na nowo z pliku
Excela/z ClickUp — jeśli cena w pliku się zmieni, zobaczysz to od razu, mimo
że sam wybór (np. „ten arkusz”) został zapamiętany.

Uwaga: wybór jest zapamiętywany pod **dokładną nazwą pliku** — jeśli plik
dostanie nową wersję (inna nazwa, np. `.6` zamiast `.5`), trzeba będzie
zdecydować ponownie. To celowe zabezpieczenie: program nie zgaduje za Ciebie
przy nowej wersji oferty.

Co zrobić z zapamiętanym wyborem:
- **Prawy przycisk myszy** na wierszu z 📌 → **„Zmień wybór…”** (otwiera okno
  wyboru jeszcze raz, z tymi samymi opcjami) albo **„Zapomnij wybór”**
  (usuwa zapamiętaną decyzję — wiersz wraca do stanu wymagającego decyzji
  przy następnym skanie).
- W ustawieniach (⚙) → sekcja **„Zapamiętane wybory”** widać, ile decyzji
  jest zapisanych, i można je **wszystkie naraz wyczyścić** przyciskiem
  „Wyczyść zapamiętane wybory” (z potwierdzeniem) — przydatne np. po dużym
  porządkowaniu folderu ofert.

### 7. Tworzenie zadań dla plików bez zadania

Gdy w folderze jest plik oferty, dla którego nikt jeszcze nie założył zadania
w ClickUp (stan **plik bez zadania**), program może założyć je za Ciebie.

Kliknij przycisk **➕ Utwórz zadania (N)** w pasku narzędzi (N = liczba takich
plików) — albo poczekaj na automatyczne pytanie po wysyłce (patrz punkt 5).
Otworzy się okno z listą plików:

- Domyślnie zaznaczone są tylko pliki, dla których udało się odczytać cenę —
  pliki bez ceny są widoczne, ale odznaczone. Możesz je zaznaczyć ręcznie:
  zadanie powstanie wtedy bez wypełnionego pola OFFER VALUE.
- **Status dla zaznaczonych** — rozwijana lista pokazuje statusy dostępne na
  liście ClickUp. Domyślnie ustawiony jest status „sent to customer” (albo
  podobny, jeśli lista nazywa go inaczej pisownią), bo z założenia te pliki
  są już gotowe i wysłane do klienta. Jeśli chcesz nadać innym plikom inny
  status (np. część zostaje w statusie roboczym), zaznacz je, wybierz status
  z listy i kliknij **„Ustaw”**. Jeśli lista w ogóle nie ma statusu „sent to
  customer”, program pokaże ostrzeżenie i użyje pierwszego statusu otwartego
  — sprawdź wtedy, czy to na pewno właściwy status.
- Nazwa nowego zadania będzie **dokładnie taka jak nazwa pliku** (bez
  rozszerzenia `.xlsm`/`.xlsx`) — dzięki temu kolejny skan od razu rozpozna
  nowe zadanie i sparuje je z tym samym plikiem.
- Kliknij **➕ Utwórz zaznaczone (N)** i potwierdź. Zadania powstają po
  kolei, z logiem na bieżąco (✓ utworzone / ✗ błąd z przyczyną) — operację
  można wstrzymać albo anulować tak samo jak wysyłkę.
- Po zakończeniu wiersz w głównej tabeli dostaje stan „✓ utworzono zadanie”
  z linkiem do nowego zadania. Jeśli coś się nie udało, wiersz wraca do
  „plik bez zadania” z przyczyną błędu — możesz spróbować ponownie bez
  ponownego skanowania całego folderu.

### 8. Raport

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
zadania). Te wiersze nie da się zaznaczyć do wysyłki. „Plik bez zadania”
najprościej rozwiązać przyciskiem **➕ Utwórz zadania** (patrz punkt 7 wyżej)
— program założy zadanie za Ciebie. Jeśli to się nie zgadza (zadanie
powinno już istnieć), sprawdź, czy plik ma poprawną nazwę (wzorzec
`OFFERnnnnn.w - KLIENT - opis`), czy leży bezpośrednio w skonfigurowanym
folderze (nie w podfolderze), i czy nazwa zadania w ClickUp zawiera ten sam
numer OFFER — numer w tytule zadania może stać w dowolnym miejscu (np.
„Aktualizacja - OFFER19725.1 - ENGEL UK…” albo „FICOMIRRORS - … -
OFFER10524.2”), ale musi być zapisany **wielkimi literami** „OFFER”.

**Wiersz ma stan „kilka plików”.**
W folderze są dwa (albo więcej) pliki tej samej oferty i wersji — np.
`Robot.xlsm` obok `Robot.xlsx`, albo plik z dopiskiem „- kopia”. Program nie
zgaduje, który jest właściwy: podwójny klik (albo przycisk **☰ Rozstrzygnij**)
otwiera okno, w którym wybierasz jeden plik. Jeśli to się nie zgadza, warto
posprzątać folder i usunąć/przenieść zbędną kopię, żeby uniknąć pomyłki przy
kolejnych skanach.

**„Nie rozpoznano nazwy”.**
Nazwa pliku albo zadania nie pasuje do wzorca `OFFERnnnnn.w - KLIENT - opis`
(np. brakuje cyfry w numerze albo numer nie ma dokładnie 5 cyfr) — w nazwie
zadania numer może stać w dowolnym miejscu, ale musi mieć dokładnie 5 cyfr
i literę „OFFER” wielkimi literami. Popraw nazwę pliku albo tytuł zadania w
ClickUp i uruchom skan ponownie.

**Program czeka i pisze coś o limicie.**
ClickUp ogranicza liczbę żądań na minutę. Program sam odczekuje i ponawia —
nie przerywaj, nie klikaj ponownie.
