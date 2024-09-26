+++
title = "Zakończenie"
date = 2024-09-25T13:13:55+02:00
draft = false
weight = 4
+++
# Zakończenie

## Aktualizacja danych w tabeli

Aby zmienić dane wyświetlane w tabeli, możesz wykorzystać poniższe opcje.

- **Edycja pliku `table.txt`.** Zaktualizuj zawartość pliku, dodając, usuwając lub modyfikując wiersze tabeli.
- **Dynamiczne pobieranie danych.** Możesz zmodyfikować kod, aby pobierać dane z pliku JSON lub bezpośrednio z bazy danych.

## Przykład pobierania danych z pliku JSON

### 1. **Utwórz plik `data.json`** z danymi w formacie JSON

   ```json
   [
       {
           "Name": "Tiger Nixon",
           "Position": "System Architect",
           "Office": "Edinburgh",
           "Age": "61",
           "Start date": "2011-04-25",
           "Salary": "$320,800"
       },
       {
           "Name": "Garrett Winters",
           "Position": "Accountant",
           "Office": "Tokyo",
           "Age": "63",
           "Start date": "2011-07-25",
           "Salary": "$170,750"
       }
       // ...pozostałe rekordy
   ]
   ```

### 2. Zmień kod w function.php
Dodaj lub zastąp istniejącą funkcję display_datatable_shortcode

```php
function display_datatable_shortcode() {
    // Generowanie pustej tabeli z nagłówkami
    $table_content = '<table id="myTable" class="display">
        <thead>
            <tr>
                <th>Name</th>
                <th>Position</th>
                <th>Office</th>
                <th>Age</th>
                <th>Start date</th>
                <th>Salary</th>
            </tr>
        </thead>
    </table>';

    // Skrypt inicjalizujący DataTables z pobieraniem danych z pliku JSON
    $script = '<script>
        document.addEventListener("DOMContentLoaded", function() {
            new DataTable("#myTable", {
                ajax: {
                    url: "' . get_stylesheet_directory_uri() . '/data.json",
                    dataSrc: ""
                },
                columns: [
                    { data: "Name" },
                    { data: "Position" },
                    { data: "Office" },
                    { data: "Age" },
                    { data: "Start date" },
                    { data: "Salary" }
                ],
                language: {
                    url: "//cdn.datatables.net/plug-ins/2.1.7/i18n/pl.json"
                }
            });
        });
    </script>';

    // Zwrócenie tabeli wraz ze skryptem
    return $table_content . $script;
}
add_shortcode('datatable', 'display_datatable_shortcode');

```
Opis.

Generowanie tabeli. Tworzymy tabelę z nagłówkami kolumn w HTML.
Inicjalizacja DataTables:

- ajax: ustawiamy ścieżkę do pliku `data.json` za pomocą `get_stylesheet_directory_uri()`,
- `dataSrc`: ustawione na "", ponieważ dane są bezpośrednio w tablicy na najwyższym poziomie,
- `columns`: mapujemy kolumny tabeli do odpowiednich kluczy w danych JSON,
- `language`: ustawiamy język interfejsu na polski.

Zwracanie treści: funkcja zwraca wygenerowany kod HTML tabeli wraz ze skryptem inicjalizującym.


## Dostosowanie wyglądu tabeli

- Modyfikacja CSS. Edytuj pliki CSS biblioteki DataTables lub dodaj własne style w pliku style.css Twojego motywu.
- Motywy i szablony. Wykorzystaj dostępne motywy DataTables lub integracje z frameworkami CSS (np. Bootstrap, Material Design).
- Opcje DataTables. Dostosuj ustawienia w funkcji DataTable(), aby zmienić elementy interfejsu użytkownika.

## Podsumowanie

Wdrożenie biblioteki DataTables na stronie WordPress pozwala na interaktywne wyświetlanie danych w formie tabelarycznej z funkcjami takimi jak sortowanie, filtrowanie i paginacja. Dzięki zastosowaniu czystego JavaScript i najnowszych wersji bibliotek, zapewniamy wydajność i kompatybilność z nowoczesnymi przeglądarkami.

Pamiętaj o regularnej aktualizacji bibliotek oraz dostosowywaniu ich konfiguracji do zmieniających się potrzeb projektu.

**Od Krzyśka**

Nie miałem czasu na testy :innocent::v:

**Dziękuję za wytrwałość i dotarcie do końca!**