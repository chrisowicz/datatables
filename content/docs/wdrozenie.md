+++
title = "Wdrożenie"
date = 2024-09-25T13:13:25+02:00
draft = false
weight = 2
+++

# Wdrożenie

## Krok 1: Dodanie biblioteki DataTables do strony

Aby rozpocząć, należy dodać do strony wymagane pliki biblioteki DataTables w najnowszej wersji oraz skrypt inicjalizacyjny w JS.

W pliku `functions.php` Twojego motywu dodaj następującą funkcję, która załaduje skrypty i style DataTables:

```php
function enqueue_datatables_scripts() {
    // Załadowanie stylów DataTables
    wp_enqueue_style('datatables-css', 'https://cdn.datatables.net/2.1.7/css/dataTables.dataTables.min.css');

    // Załadowanie skryptu DataTables w js. Ostatni parametr true oznacza, że skrypt zostanie załadowany w stopce strony.
    wp_enqueue_script('datatables-js', 'https://cdn.datatables.net/2.1.7/js/dataTables.min.js', array(), null, true);
}
add_action('wp_enqueue_scripts', 'enqueue_datatables_scripts');
```

Jeśli nie chcesz korzystać z CND, mozesz zapisać pliki w swoim szablonie. Pamiętaj o zmianie drugiego argumentu (URL) na lokalny. 

## Krok 2: Umieszczenie pliku `table.txt` z danymi tabeli

- Umieść plik `table.txt` zawierający kod HTML tabeli w katalogu motywu (`wp-content/themes/nazwa-twojego-motywu/`).
- Upewnij się, że plik jest dostępny przez URL, np. `https://twoja-domena.pl/wp-content/themes/nazwa-twojego-motywu/table.txt`.
- Zawartość pliku powinna być dokładnie taka, jak w dostarczonym pliku z zadania.

## Krok 3: Stworzenie shortcode do wyświetlania tabeli

Utwórz shortcode, który będzie wczytywał dane z pliku table.txt i wyświetlał tabelę na stronie.

Dodaj poniższą funkcję do pliku `functions.php` w Twoim motywie:

```php
function display_datatable_shortcode() {
    // URL do pliku txt z tabelą. Funkcja get_stylesheet_directory_uri zwraca url do wybranego szablonu, nawet jeśli używasz szablonu potomnego
    $file_url = get_stylesheet_directory_uri() . '/table.txt';

    // Pobranie zawartości pliku z URL
    $response = wp_remote_get($file_url);

    // Sprawdzenie, czy odpowiedź jest poprawna
    if (is_array($response) && !is_wp_error($response)) {
        $table_content = $response['body'];

        // Dodanie identyfikatora do tabeli, jeśli go brakuje
        if (strpos($table_content, 'id="myTable"') === false) {
            $table_content = str_replace('<table', '<table id="myTable"', $table_content);
        }

        // Skrypt inicjalizujący DataTables
        $script = '<script>
            document.addEventListener("DOMContentLoaded", function() {
                var table = document.getElementById("myTable");
                new DataTable(table, {
                    paging: true,
                    searching: true,
                    ordering: true,
                    info: true,
                    language: {
                        url: "//cdn.datatables.net/plug-ins/2.1.7/i18n/pl.json"
                    }
                });
            });
        </script>';

        // Zwrócenie tabeli wraz ze skryptem
        return $table_content . $script;
    } else {
        return '<p>Nie udało się pobrać danych z pliku tabeli.</p>';
    }
}
add_shortcode('datatable', 'display_datatable_shortcode');
```
Opis.

- Funkcja display_datatable_shortcode() generuje zawartość shortcode.
- get_stylesheet_directory_uri() zwraca URL do katalogu aktywnego motywu.
- wp_remote_get() pobiera zawartość pliku table.txt.
- Dodajemy identyfikator id="myTable" do tabeli, jeśli go brakuje.
- Inicjalizujemy DataTables w czystym JavaScript, bez użycia jQuery.
- Ustawiamy opcje DataTables, takie jak paginacja, wyszukiwanie, sortowanie i informacje.
- Ustawiamy język na polski, korzystając z pliku językowego.

## Krok 4: Wstawienie shortcode
### Na stronę lub do wpisu
Aby wyświetlić tabelę na stronie lub we wpisie, użyj shortcode: `[datatable]`.
### W kodzie szablonu
Aby wyświetlić shortcode w kodzie szablonu strony w miejscu, w którym chcesz, użyj: 
```php
echo do_shortcode('[datatable]');
```

## Krok 5: Testowanie
- Odśwież stronę i sprawdź, czy tabela jest poprawnie wyświetlana.
- Upewnij się, że funkcje DataTables, takie jak sortowanie, filtrowanie i paginacja, działają poprawnie.

## Krok 6: Dostosowanie wyglądu (opcjonalnie)

Jeśli chcesz dostosować wygląd tabeli:

- Edytuj plik CSS motywu (`style.css`) lub dodaj własne style.
- Możesz również skorzystać z dostępnych motywów DataTables, np. [integracji z Bootstrapem](https://datatables.net/manual/styling/bootstrap5).