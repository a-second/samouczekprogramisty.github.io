---
title: Funkcje analityczne w SQL
last_modified_at: 2019-09-19 10:11:50 +0200
categories:
- Bazy danych
- Kurs SQL
permalink: /funkcje-analityczne-sql/
header:
    teaser: /assets/images/2021/window_sql.jpg
    overlay_image: /assets/images/2021/window_sql.jpg
    caption: "[&copy; Rob Wingate](https://unsplash.com/photos/Fd9tUmRBJzk)"
excerpt: W tym artykule opisuję funkcje analityczne w SQL. Po lekturze tego artykułu będziesz wiedzieć czym są funkcje analityczne i czym różnią się od funkcji agregujących. Na licznych przykładach poznasz składnię funkcji analitycznych. Te przykłady pozwolą Ci przetestować zapytania analityczne samodzielnie. Na końcu artykułu czeka na Ciebie zestaw zadań, które pomogą Ci utrwalić zdobytą wiedzę.
---

{% include kurs-sql-notice.md %}

W tym artykule używam funkcji SQLite, które zostały dodane w wersji 3.28.0. Jeśli używasz SQLite do eksperymentowania upewnij się, że korzystasz z wersji 3.28.0 bądź nowszej. Możesz to zrobić używając polecenia `sqlite3 --version`.
{:.notice--info}

## Czym są funckcje analityczne w SQL

W jednym zdaniu można powiedzieć, że funkcje analityczne (ang. _analytic functions_) zwracają wartość na podstawie grupy wierszy powiązanych z aktualnym wierszem. Tę grupę nazywa się partycją. Sam opis może być skomplikowany, więc proszę spójrz na przykład poniżej:

```sql
  SELECT customerid
        ,invoiceid
        ,total
        ,SUM(total) OVER (PARTITION BY customerid) AS customer_total_sum
    FROM invoice
ORDER BY customerid
   LIMIT 10;
```

Poza funkcją analityczną użyłem tu [aliasu kolumny]({% post_url 2018-09-04-sortowanie-aliasy-ograniczanie-wynikow-i-zwracanie-unikalnych-wartosci %}#aliasy-dla-kolumn), [sortowania]({% post_url 2018-09-04-sortowanie-aliasy-ograniczanie-wynikow-i-zwracanie-unikalnych-wartosci %}#sortowanie-wyników) i [ograniczenia liczby zwracanych wierszy]({% post_url 2018-09-04-sortowanie-aliasy-ograniczanie-wynikow-i-zwracanie-unikalnych-wartosci %}#ograniczanie-liczby-wyników). W wyniu tego zapytania otrzymasz dziesięć wierszy:

    CustomerId  InvoiceId  Total  customer_total_sum
    ----------  ---------  -----  ------------------
    1           98         3.98   39.62
    1           121        3.96   39.62
    1           143        5.94   39.62
    1           195        0.99   39.62
    1           316        1.98   39.62
    1           327        13.86  39.62
    1           382        8.91   39.62
    2           1          1.98   37.62
    2           12         13.86  37.62
    2           67         8.91   37.62

To zapytanie zwraca cztery różne kolumny. Ostatnia z nich jest wynikiem działania funkcji analitycznej. Spróbuję rozłożyć ją na części pierwsze:

```sql
SUM(total)
OVER (PARTITION BY customerid)
```

W pierwszej linijce widzisz funkcję `SUM`. Możesz ją pamiętać z [artykułu o funkcjach w SQL]({% post_url 2018-10-20-funkcje-i-grupowanie-danych-w-sql %}#funkcje-grupujące). W poprzednim przypadku była ona użyta jako funkcja agregująca. Użycie słowa kluczowego `OVER` sprawia, że jej zachowanie nieznacznie się zmienia, staje się ona funkcją analityczną. W tym przypadku `SUM` nadal zwraca sumę, jednak w przypadku funkcji analitycznej pod uwagę brana jest partycja a nie cała tabela[^edgecase].

[^edgecase]: Partycją może być także cała tabela.

W nawiasie znajduje się definicja partycji wierszy, która zostanie użyta do obliczenia wartości funkcji analitycznej. W tym przypadku do partycji należą wiersze zawierające taką samą wartość kolumny `customerid`.

Zatem ta funkcja:
* oblicza sumę kolumny `total` (`SUM(total)`),
* dla wszystkich wierszy, które mają taką samą wartość kolumny `customerid` (`PARTITION BY customerid`).

Proszę spójrz na pierwszych siedem wierszy, które mają taką samą wartość kolumny `customerid`. Suma wartości kolumny `total` dla tych wierszy to wynik działania funkcji analitycznej dla każdego z tych wierszy:`3.98 + 3.96 + 5.94 + 0.99 + 1.98 + 13.86 + 8.91 = 39.62`.

Można powiedzieć, że funkcje analityczne są podobne do standardowego grupowania przy użyciu kauzuli `GROUP BY`. Funkcje agregujące zwracają jeden wiersz dla grupy, funkcje analityczne zwracają wiele wierszy.

#### Pozostałe funkcje agregujące

Poniżej znajdziesz listę funkcji agregujących, których możesz użyć przed słowem klucozwym `OVER`:

 * `AVG` – zwraca średnią wartość,
 * `COUNT` – zwraca liczbę wierszy,
 * `MAX` — zwraca maksymalną wartość,
 * `MIN` – zwraca minimalną wartość,
 * `SUM` – zwraca sumę wartości.

### Klauzula `PARTITION BY`

W przykładzie wyżej wszystkie wiersze w tabeli `invoice` zostały podzielone na osobne partycje. Do podziału na partycji użyłem wyłącznie jednej kolumny. W klauzuli `PARTITION BY` możesz użyć wielu wyrażeń:

```sql
SUM(total) OVER
OVER (PARTITION BY customerid, billingcountry)
```

W tym przypadku tabela zostanie podzielona na więcej partycji. Do jednej partycji trafią wszytkie wiersze, które mają taką samą wartość kolumn `customerid` i `billingcountry`

Istnieje też możliwość pominięcia klauzuli `PARTITION BY`:

```sql
SUM(total) OVER ()
```

W takim przypadku partycja równoznaczna jest z całą tabelą[^virtualtable]. Dla każdego wiersza w wyniku zapytania `SUM(total) OVER ()` zwróci sumę kolumny `total` we wszystkich wierszach.

[^virtualtable]: Właściwie to nie z całą tabelą, a relacją powstałą po filtrowaniu i grupowaniu.

### Kiedy obliczana jest wartość funkcji analitycznej

Funkcje analityczne mogą być użyte wyłącznie w klauzuli `SELECT` i `ORDER BY`. Wynika to z faktu, że funkcje analityczne operują na „wirtualnej tabeli” (w [modelu relacyjnym]({% post_url 2018-03-06-wstep-do-relacyjnych-baz-danych %}#model-relacyjny) można mówić o relacji), która powstanie po filtrowaniu i grupowaniu wierszy.

Można powiedzieć, że zapytanie wykonywane jest w następującej kolejności[^queryengine]:

[^queryengine]: Wszystko tu zależy od silnika zapytań. Taki podział ułatwia zrozumienie funkcji analitycznych.

1. Wykonanie klauzuli `WHERE`,
1. Wykonanie klauzuli `GROUP BY`,
1. Wykonanie klauzuli `HAVING`,
1. Wykonanie funkcji analitycznych,
1. Wykonanie kauzuli `ORDER BY`,
1. Wykonanie kauzuli `LIMIT`.

### Ćwiczenia do samodzielnego wykonania

Teraz czas na Twoje eksperymenty. Spróbuj samodzielnie uruchomić przykładowe zapytanie. Możesz je także zmodyfikować:

* zmień limit zwracanych wierszy, 
* zwróć wyłącznie wiersze z parzystą wartością kolumny `customerid`.

## Sortowanie w funkcjach analitycznych

Nieznacznie zmodyfikuję definicję partycji z pierwszego zapytania. Przykład poniżej używa dwóch funkcji agregujących. Druga z nich używa `ORDER BY invoiceid` w definicji partycji:

```sql
  SELECT customerid
        ,invoiceid
        ,total
        ,SUM(total) OVER (PARTITION BY customerid) AS customer_total_sum
        ,SUM(total) OVER (PARTITION BY customerid
                              ORDER BY invoiceid) AS customer_total_increasing_sum
    FROM invoice
ORDER BY customerid, invoiceid
   LIMIT 10;
```

Proszę spójrz na wynik zapytania. Zwróć uwagę na wartości kolumn `customer_total_sum` i `customer_total_increasing_sum`:

    CustomerId  InvoiceId  Total  customer_total_sum  customer_total_increasing_sum
    ----------  ---------  -----  ------------------  -----------------------------
    1           98         3.98   39.62               3.98
    1           121        3.96   39.62               7.94
    1           143        5.94   39.62               13.88
    1           195        0.99   39.62               14.87
    1           316        1.98   39.62               16.85
    1           327        13.86  39.62               30.71
    1           382        8.91   39.62               39.62
    2           1          1.98   37.62               1.98
    2           12         13.86  37.62               15.84
    2           67         8.91   37.62               24.75

Użycie `ORDER BY` w definicji funkcji analitycznej powoduje utworzenie „narastających” grup dla każdego kolejnego wiersza:

* grupa dla pierwszego wiersza to wyłącznie pierwszy wiersz (`3.98 = 3.98`),
* grupa dla drugiego wiersza to dwa pierwsze wiersze (`3.98 + 3.96 = 7.94`),
* grupa dla trzeciego wiersza to trzy pierwsze wiersze (`3.98 + 3.96 + 5.94 = 13.88`),
* itd.

Zauważ, że w tym przykładzie użyłem dwóch klauzul `ORDER BY`. Pierwsza z nich służy do określenia partycji dla funkcji analitycznej, druga służy do sortowania wyników całego zapytania.

### Partycje a sortowanie

To zachowanie może być różne w przypadku róznych silników baz danych.
{:.notice--info}

Zapytanie używające partycji zwraca dane posortowane zgodnie z definicją partycji. Na przykład wyniki poniższego zapytania będą posortowane używając kolumny `customerid`:

```sql
SELECT customerid
      ,SUM(total) OVER (PARTITION BY customerid) AS customer_total_sum
  FROM invoice
```

Chociaż dane będą zwrócone w ten sposób nie polegałbym na tym zachowaniu. Jeśli zależy Ci na uzyskaniu posortowanych danych określ to jasno używając [klauzuli `ORDER BY`]({% post_url 2018-09-04-sortowanie-aliasy-ograniczanie-wynikow-i-zwracanie-unikalnych-wartosci %}). W ten sposób jasno określasz intencje, a nie bazujesz na „przypadkowym”[^implementationdetail] zachowaniu:

[^implementationdetail]: Odsyłam Cię tutaj do dokumentacji Twojego silnika bazy danych. Możliwe, że w Twoim przypadku nie jest to „przypadkowe” zachowanie a jasno określone przez specyfikację.

```sql
  SELECT customerid
        ,SUM(total) OVER (PARTITION BY customerid) AS customer_total_sum
    FROM invoice
ORDER BY customerid;
```

We wszystkich przykładach w artykule staram się dodawać klauzule `ORDER BY` właśnie po to żeby pokazać intencję.

### Ćwiczenia do samodzielnego wykonania

Teraz czas na Twoje eksperymenty. Spróbuj samodzielnie uruchomić to zapytanie. Możesz je także zmodyfikować:

* sprawdź jak na wynik zapytania wpływają różne kolumny użyte do sortowania, 
* uzyj kilku kolumn do sortowania wyników/wierszy w partycji,
* użyj `DESC`/`ASC` do z zmiany wyniku sortowania.

## Unikanie duplikacji – nazwane partycje

Wyobraź sobie sytuację, w której chcesz zwrócić wynik różnych funkcji analitycznych, jednak używając tej samej definicji partycji. Sprójrz na przykład poniżej:

```sql
  SELECT customerid
        ,invoiceid
        ,total
        ,SUM(total) OVER (PARTITION BY customerid) AS customer_total_sum
        ,AVG(total) OVER (PARTITION BY customerid) AS customer_total_avg
    FROM invoice
ORDER BY customerid
   LIMIT 10;
```

W tym przykładzie definicja partycji jest prosta. Możesz jednak trafić na przypadek, w którym będzie ona dużo bardziej skomplikowana. Takie zapytanie zawiera duplikację definicji partycji. Duplikacja w większości przypadków jest zła. Nie inaczej jest w przypadku zapytań SQL. W takiej sytuacji z pomocą przychodzi klauzula `WINDOW`. Proszę spójrz na przykład poniżej, jest on równoznaczny z poprzednim zapytaniem:

```sql
  SELECT customerid
        ,invoiceid
        ,total
        ,SUM(total) OVER customer_window AS customer_total_sum
        ,AVG(total) OVER customer_window AS customer_total_avg
    FROM invoice
  WINDOW customer_window AS (PARTITION BY customerid)
ORDER BY customer id
   LIMIT 10;
```

Oba zapytania zwrócą ten sam wynik:

    CustomerId  InvoiceId  Total  customer_total_sum  customer_total_avg
    ----------  ---------  -----  ------------------  ------------------
    1           98         3.98   39.62               5.66
    1           121        3.96   39.62               5.66
    1           143        5.94   39.62               5.66
    1           195        0.99   39.62               5.66
    1           316        1.98   39.62               5.66
    1           327        13.86  39.62               5.66
    1           382        8.91   39.62               5.66
    2           1          1.98   37.62               5.37428571428571
    2           12         13.86  37.62               5.37428571428571
    2           67         8.91   37.62               5.37428571428571

Co więcej partycje zdefiniowane w ten sposób możesz dodatkowo rozszerzać:

```sql
  SELECT customerid
        ,invoiceid
        ,total
        ,SUM(total) OVER (customer_window ORDER BY invoiceid) AS customer_ordered_total_sum
        ,AVG(total) OVER customer_window AS customer_total_avg
    FROM invoice
  WINDOW customer_window AS (PARTITION BY customerid)
ORDER BY customerid
   LIMIT 10;
```

W tym przykładzie suma kolumy `total` jest narastająca:

    CustomerId  InvoiceId  Total  customer_ordered_total_sum  customer_total_avg
    ----------  ---------  -----  --------------------------  ------------------
    1           98         3.98   3.98                        5.66
    1           121        3.96   7.94                        5.66
    1           143        5.94   13.88                       5.66
    1           195        0.99   14.87                       5.66
    1           316        1.98   16.85                       5.66
    1           327        13.86  30.71                       5.66
    1           382        8.91   39.62                       5.66
    2           1          1.98   1.98                        5.37428571428571
    2           12         13.86  15.84                       5.37428571428571
    2           67         8.91   24.75                       5.37428571428571

## Funkcje okna

Jak już wiesz funkcje analityczne działają w oparciu o partycje. Dodatkowo funkcje te pozwalają Ci na zdefiniowanie tak zwanego okna. Domyślnie okno zawiera:

* wszystkie wiersze partycji jeśli nie użyjesz klauzuli `ORDER BY`,
* wiersze „do aktualnego wiersza” jeśli użyjesz klauzuli `ORDER BY`.

Domyślną zawartość okna możesz zmienić. Okno pozwala na dalsze ograniczenie wierszy branych pod uwagę przez funkcję. Składnię można rozszerzyć do:

```sql
<funkcja>
OVER
[ PARTITION BY … ]
[ ORDER BY … ]
<definicja okna>
```

Okno może być jednego z trzech rodzajów:

* `ROWS` – granice okna określone są przez liczbę wierszy przed/po aktualnym wierszem,
* `GROUPS` – granice okne określone są przez liczbę „grup” przed/po aktualnej „grupie”. Do grupy zalicza się te wartości, które są „równe” w trakcie sortowania przy użyciu `ORDER BY`,
* `RANGE` – granice okna określone są przez różnicę wartości względem aktualnego wiersza.

Dla uproszczenia w definicji okna będę używał wyłącznie `BETWEEN x PRECEDING AND y FOLLOWING`. Oznacza to, że okno będzie obejmowało zakres `x` przed aktualnym wierszem i `y` po aktualnym wierszu. Składania pozwala na dużo bardziej zaawansowane modyfikacje, jednak ich znajomość nie jest niezbędna do zrozumienia działania samego mechanizmu. Jeśli jesteś zainteresowany tymi szczegółami odsyłam Cię do dokumentacji silnika bazy danych, którego używasz.

Mam świadomość, że to wszystko brzmi jak łacina bez konkretnego przykładu. Postaram się to poprawić ;)

### Okno typu `ROWS`

Proszę spójrz na pierwszy z nich:

```sql
  SELECT customerid
        ,invoiceid
        ,total
        ,SUM(total) OVER (PARTITION BY customerid
                              ORDER BY invoiceid
                                  ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS rolling_sum
    FROM invoice
ORDER BY customerid
        ,invoiceid
   LIMIT 10;
```

W wyniku tego zapytania otrzymasz 10 wierszy:

    CustomerId  InvoiceId  Total  rolling_sum
    ----------  ---------  -----  -----------
    1           98         3.98   7.94
    1           121        3.96   13.88
    1           143        5.94   10.89
    1           195        0.99   8.91
    1           316        1.98   16.83
    1           327        13.86  24.75
    1           382        8.91   22.77
    2           1          1.98   15.84
    2           12         13.86  24.75
    2           67         8.91   24.75  

W tym przypadku `SUM(total)` sumuje jedynie wiersze należące do okna, a nie całej partycji. 

* dla pierwszego wiersza oknem są wiersze pierwszy i drugi: `3.98 + 3.96 = 7.94` (brak poprzedniego wiersza w partycji),
* dla drugiego wiersza oknem są wiersze pierwszy, drugi i trzeci: `3.98 + 3.96 + 5.94 = 13.88` ,
* dla siódmego wiersza oknem są wiersze szósty i siódmy: `13.86 + 8.91 = 22.77` (brak następnego wiersza w partycji).

### Okno typu `GROUPS`

Tym razem do utworzenia partycji posłużę się kolumną `billingcountry`:

```sql
  SELECT billingcountry
        ,invoiceid
        ,total
        ,SUM(total) OVER (PARTITION BY billingcountry
                              ORDER BY total
                                GROUPS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS rolling_sum
    FROM invoice
   WHERE billingcountry = 'India'
ORDER BY total;
```

W wyniku tego zapytania otrzymasz 13 wierszy:

    BillingCountry  InvoiceId  Total  rolling_sum
    --------------  ---------  -----  -----------
    India           120        1.98   9.92
    India           218        1.98   9.92
    India           315        1.98   9.92
    India           97         1.99   17.84
    India           412        1.99   17.84
    India           23         3.96   23.78
    India           338        3.96   23.78
    India           45         5.94   37.62
    India           360        5.94   37.62
    India           186        8.91   57.42
    India           284        8.91   57.42
    India           131        13.86  45.54
    India           229        13.86  45.54

Także tym przypadku `SUM(total)` sumuje jedynie wiersze należące do okna, a nie całej partycji:

* dla pierwszego wiersza oknem jest pierwszych pięć wierszy. Grupa do której należy pierwszy wiersz i następna grupa: `3 * 1.98 + 2 * 1.99 = 9.92` (brak poprzedniej grupy w partycji),
* dla piątego wiersza oknem jest pierwszych siedem wierszy. Grupa poprzedzająca, grupa do której należy piąty wiersz i następna grupa: `3 * 1.98 + 2 * 1.99 + 2 * 3.96 = 17.84` ,
* dla przedostatniego wiersza oknem są wiersze 10., 11., 12. i 13.: `2 * 8.91 + 2 * 13.86` (brak następnej grupy w partycji).

### Okno typu `RANGE`

W tym przypadku okno definiowane jest jako „odległość” 2 przed i po wartością kolumny `total`:

```sql
  SELECT customerid
        ,invoiceid
        ,total
        ,SUM(total) OVER (PARTITION BY customerid
                              ORDER BY total
                                 RANGE BETWEEN 2 PRECEDING AND 2 FOLLOWING) AS rolling_sum
    FROM invoice
ORDER BY customerid
        ,total
   LIMIT 10;
```

W wyniku tego zapytania otrzymasz dziesięć wierszy:

    CustomerId  InvoiceId  Total  rolling_sum
    ----------  ---------  -----  -----------
    1           195        0.99   2.97
    1           316        1.98   10.91
    1           121        3.96   15.86
    1           98         3.98   15.86
    1           143        5.94   13.88
    1           382        8.91   8.91
    1           327        13.86  13.86
    2           293        0.99   4.95
    2           1          1.98   8.91
    2           196        1.98   8.91

`SUM(total)` sumuje jedynie wiersze należące do okna, a nie całej partycji. 

* dla pierwszego wiersza oknem są pierwsze dwa wiersze. Dzieje się tak ponieważ wartość kolumny `total` dla tych wierszy jest w zakresie  `<0.99 - 2, 0.99 + 2>`,
* dla drugiego wiersza oknem są pierwsze cztery wiersze. Dzieje się tak ponieważ wartość kolumny `total` dla tych wierszy jest w zakresie  `<1.98 - 2, 1.98 + 2>`,
* dla trzeciego wiersza oknem są wiersze drugi, trzeci, czwarty i piąty. Dzieje się tak ponieważ wartość kolumny `total` dla tych wierszy jest w zakresie  `<3.96 - 2, 3.96 + 2>`.

### Filtrowanie okna

Jakby tego było mało do tego wszystkiego dochodzi możliwość filtrowania :). Oznacza to tyle, że możesz uzyć filtrowania jak w klauzuli `WHERE`, żeby dodatkowo ograniczyć wiersze „pasujące” do definicji okna.


sqlite> SELECT customerid, invoiceid, total, SUM(total) OVER (rows_window) AS rolling_sum, SUM(total) FILTER (WHERE invoiceid != 121) OVER rows_window FROM invoice WINDOW rows_window AS (PARTITION BY customerid)
CustomerId  InvoiceId  Total  rolling_sum  SUM(total) FILTER (WHERE invoiceid != 121) OVER rows_window
----------  ---------  -----  -----------  -----------------------------------------------------------
1           98         3.98   39.62        35.66
1           121        3.96   39.62        35.66
1           143        5.94   39.62        35.66
1           195        0.99   39.62        35.66
1           316        1.98   39.62        35.66
1           327        13.86  39.62        35.66
1           382        8.91   39.62        35.66
2           1          1.98   37.62        37.62
2           12         13.86  37.62        37.62
2           67         8.91   37.62        37.62
sqlite> otal, SUM(total) OVER (rows_window) AS rolling_sum, SUM(total) FILTER (WHERE invoiceid != 121) OVER rows_window FROM invoice WINDOW rows_window AS (PARTITION BY customerid) ORDER BY customerid LIMIT 10;



## Funkcje analityczne a filtrowanie danych

Jak już wiesz funkcje analityczne mogą być użyte wyłącznie w klauzuli `SELECT` i `ORDER BY`. Co jeśli musisz użyć wyniku funkcji analitycznej do filtrowania? Z pomocą przychodzą [podzapytania]({% post_url 2019-09-18-podzapytania-sql %}). Na przykład poniższe zapytanie zwróci wyłącznie te faktury wystawione dla klienta, których suma będzie mniejsza niż 10:

```sql
  SELECT customerid
        ,invoiceid
        ,total
    FROM invoice JOIN (SELECT invoiceid
                             ,SUM(total) OVER (PARTITION BY customerid
                                                   ORDER BY invoiceid) AS invoice_sum
                         FROM invoice)
                 USING (invoiceid)
   WHERE invoice_sum < 10
ORDER BY customerid
   LIMIT 10;
```

    CustomerId  InvoiceId  Total
    ----------  ---------  -----
    1           98         3.98 
    1           121        3.96 
    2           1          1.98 
    3           99         3.98 
    4           2          3.96 
    4           24         5.94 
    5           77         1.98 
    5           100        3.96 
    6           46         8.91 
    7           78         1.98 

Nie przejmuj się, jeśli to zapytanie będzie dla Ciebie zbyt skomplikowane. To nic dziwnego, używa ono wielu elementów składki SQL. Postaraj się przeanalizować je jeszcze raz. Spróbuj też samodzielnie ekperymentować. Zacznij od wywołania podzapytania i przeanalizowania jego wyników.
{:.notice--info}

## Dodatkowe materiały do nauki

Artykuł nie wyczerpuje tematu funkcji analitycznych. Zachęcam Cię do rzucenia okiem na dodatkowe materiały do nauki. Pamiętaj, że dokumentacja Twojego sinlika baz danych jest niezastąpiona ;) i zawiera dużo bardziej szczegółowe informacje.

* [https://www.postgresql.org/docs/current/tutorial-window.html](Tutorial dotyczący funkcji analitycznych dla PostgreSQL),
* [https://www.postgresql.org/docs/current/sql-expressions.html#SYNTAX-WINDOW-FUNCTIONS](Składnia funkcji analitycznych w PosgreSQL),
* [https://www.sqlite.org/windowfunctions.html](Składnia funkcji analitycznych w SQLite).
* [Materiały wykładowe z Politechniki Poznańskiej](http://tpd.cs.put.poznan.pl/accounts/pdf/PABD/SQL_ZaawansowaneAnalizy_Czesc2_Wyklad.pdf)

## Podsumowanie

Po przeczytaniu tego artykułu wiesz już czym są funkcje analityczne. Wiesz czym takie funkcje różnią się od zwykłego grupowania. Wiesz czym są funkcje okna i jak ich używać. Po przerobieniu ćwiczeń możesz śmiało powiedzieć, że udało Ci się sprawdzić wiedzę w praktyce. Gratulacje ;), funkcje analityczne to jedne z bardziej zaawansowanych elementów składki SQL.

Mam nadzieję, że artykuł był dla Ciebie pomocny. Proszę podziel się nim ze swoimi znajomymi. Dzięki temu pozwolisz mi dotrzeć do nowych Czytelników, za co z góry dziękuję. Jeśli nie chcesz pominąć kolejnych artykułów dopisz się do samouczkowego neslettera i polub [Samouczka Programisty na Facebooku](https://www.facebook.com/SamouczekProgramisty).

Do następnego razu!