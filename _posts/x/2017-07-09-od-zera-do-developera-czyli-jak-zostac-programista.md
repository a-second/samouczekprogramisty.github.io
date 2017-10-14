---
layout: default
title: Od zera do developera, czyli jak zostać programistą
date: '2017-07-09 22:54:16 +0200'
categories:
- Programowanie
- Początkujący programista
excerpt_separator: "<!--more-->"
permalink: "/od-zera-do-developera,-czyli-jak-zostac-programista/"
---
Zdecydowanie pytania tego typu dostaję od Was najczęściej. Wiedza związana z programowaniem jest bardzo rozległa. Początkujący mogą czuć się zagubieni. Wcale się im nie dziwię. Postaram się wypunktować kroki, które moim zdaniem powinieneś przejść aby zostać programistą. W ogłoszeniach profil tego typu określany jest jako “Full stack Java developer”. Zapraszam do lektury.

# Nie ma dróg na skróty
  
Napiszę to w pierwszym akapicie. Moim zdaniem jest to bardzo ważny punkt, który wielu pomija. Programowanie nie jest łatwe. Nauka programowania tym bardziej nie jest łatwa. Wymaga od Ciebie zaangażowania, często również sporo poświęcenia. Jeśli aktualnie pracujesz i chcesz zmienić swój profil jesteś w jeszcze cięższej sytuacji.

Jak to mówią, “nikt nie powiedział, że będzie łatwo i słowa dotrzymał”. Z nauką programowania jest jak z nauką języków obcych. Są ludzie, którzy mają swego rodzaju dar i przychodzi im to łatwiej. Znakomita większość (w tym ja) musi spędzić mnóstwo czasu żeby przyswoić tę wiedzę. Jeśli liczysz na to, że kupisz kurs, przeczytasz dwie książki i zostaniesz programistą muszę wyprowadzić Cię z błędu. Tak nie będzie. Tylko Twoja ciężka praca może doprowadzić Cię do celu. Materiały w internecie mogą Ci w tym pomóc, nakierować na pewien tor. Na pewno nie zastąpią Twojej pracy. Ona po prostu jest niezbędna.

Jeśli masz słomiany zapał (jak ja) to też nie jesteś na uprzywilejowanej pozycji. Będzie ciężko, ale będzie też mnóstwo satysfakcji jeśli osiągniesz cel, który sobie założyłeś. Jeśli nie masz zamiaru ciężko pracować żeby zostać programistą zamknij tę zakładkę przeglądarki. Jeśli tak jest to szkoda Twojego czasu, nie ma sensu czytać dalej.

# Full stack Java developer
  
Określenie “programista” czy “developer” jest bardzo szerokie. Kryje w sobie mnóstwo technologii i ścieżek, które możesz wybrać. A może “programista Java”? Programista Java niestety także nie jest wystarczająco specyficzne. Skupię się zatem na profilu, który moim zdaniem aktualnie jest jednym z najczęściej poszukiwanych na rynku pracy “full stack Java developer”.

Czym zajmuje się szablonowy “full stack Java devloper”? Jest to programista, który rozwija aplikacje webowe. Zajmuje się zarówno warstwą interfejsu użytkownika jak i kodem wykonywanym po stronie serwera. Dalsza część artykułu opisuje właśnie ten typ stanowiska wraz z zakresami wiedzy, które powinieneś opanować.

Rzeczy, które opiszę poniżej dotyczą profilu idealnego kandydata. Jest tego dużo, jednak nie zrażaj się. Firmy bardzo często patrzą na potencjał nie na aktualny zestaw umiejętności.

# Wiedza ogólna

## Składnia języka Java
  
Podstawą jest znajomość składni języka Java. Obsługa wyjątków, pętle, instrukcje warunkowe, adnotacje czy “lambdy” - wszystko co związane jest z samym językiem programowania. Wiedzę z tego zakresu możesz zdobyć przerabiając artykuły zebrane w ramach [kursu języka Java](http://www.samouczekprogramisty.pl/kurs-programowania-java/).
## Biblioteka standardowa
  
Bez znajomości biblioteki standardowej nie będziesz mógł efektywnie pracować z kodem źródłowym. Znajomość kolekcji czy wyrażeń regularnych świetnie pasuje do tej kategorii. Dobrym sposobem na ćwiczenie tej umiejętności jest rozwiązywanie zadań. Sporo [zadań z przykładowymi rozwiązaniami](http://www.samouczekprogramisty.pl/strefa-zadaniowa/) znajdziesz na blogu.
## Biblioteki narzędziowe
  
Tutaj mam na myśli najczęściej używane biblioteki, które rozszerzają możliwości biblioteki standardowej na przykład [Guava](https://github.com/google/guava), [Apache Commons](https://commons.apache.org) czy [Joda](http://www.joda.org/). Nie chodzi mi tu o zapamiętanie całego API dostępnego w tych bibliotekach, chodzi o samą świadomość ich istnienia i możliwości użycia w projektach. Jako przykład mogę podać Ci jeden z projektów, który realizowałem. [Pogodynka](http://www.samouczekprogramisty.pl/projekty/pogodynka/) także dostępna jest na githubie gdzie możesz przejrzeć kod źródłowy tego projektu.
## IDE
  
Do efektywnej pracy z językiem Java niezbędne jest IDE. Czy to będzie Idea, Eclipse, Netbeans czy cokolwiek innego - nie ma to znaczenia. Istotne jest abyś swobodnie pracował z IDE. Znajomość skrótów klawiaturowych czy możliwości IDE są bardzo przydatne. Tę umiejętność także rozwiniesz przez praktykę. W większości IDE są wtyczki, które wspierają pracę ze skrótami klawiaturowymi. Pracując z Eclipse używałem wtyczki [mousefeed](https://marketplace.eclipse.org/content/mousefeed). Teraz z Ideą bardzo często używam funkcjonalności [Search Everywhere](https://www.jetbrains.com/help/idea/searching-everywhere.html#d392899e17) (`Shift + Shift`) lub [Navigate to Action](https://www.jetbrains.com/help/idea/2017.1/navigating-to-action.html) (`Ctrl + Shift + A`).
## System kontroli wersji
  
System kontroli wersji to mechanizm ułatwiający pracę z kodem. W systemie tym zapisuje się historyczne wersje plików, dzięki temu w łatwy sposób możemy śledzić zmiany. System ten pomaga także przy pracy zespołowej nad jednym fragmentem kodu. Standardem w większości firm jest Git, zdarzają się też firmy, które używają SVN czy innych bardziej “egzotycznych” systemów. Proponowałbym zacząć do Git’a. Jeśli jesteś początkującym zachęcam do zajrzenia na jeden z interaktywnych kursów gita:
- [http://gitreal.codeschool.com](http://gitreal.codeschool.com),
- [https://try.github.io/levels/1/challenges/1](https://try.github.io/levels/1/challenges/1).
  

## Testy jednostkowe
  
Umiejętność pisania testów jednostkowych jest bardzo przydatna. Jeśli będziesz mógł się pochwalić znajomością bibliotek takich jak [JUnit](http://junit.org/), [Mockito](http://site.mockito.org/) czy [Hamcrest](http://hamcrest.org/JavaHamcrest/) będzie bardzo dobrze. Sam używam tej trójki przy pisaniu testów. Jeśli ta tematyka Cię interesuje napisałem artykuł [wprowadzający do testów jednostkowych](http://www.samouczekprogramisty.pl/testy-jednostkowe-z-junit/) i [metodyki TDD](http://www.samouczekprogramisty.pl/test-driven-development-na-przykladzie/).
# Biblioteki, specyfikacje i technologie

## Interfejs użytkownika
  
Aplikacje webowe posiadają interfejs użytkownika. W zależności od projektu, w którym będziesz pracował do jego tworzenia mogą wykorzystywane być różne technologie. Jest jednak pewien zestaw minimum, który zawsze powinieneś znać. Chodzi mi tu o podstawy HTML, CSS i JavaScript. Jako początkujący wystarczy, że znasz podstawy tych technologii. Im jesteś bardziej doświadczony poznasz kilka bibliotek JavaScript używanych przy pracy z interfejsem użytkownika. Osobiście bardzo zaniedbuję ten obszar. Sam musisz zobaczyć co Ci bardziej odpowiada :).
## Kod wykonywany na serwerze
  
Pracując z aplikacjami webowymi w Javie będziesz miał styczność z servletami. Powinieneś poznać mechanizm ich działania wraz z ich ograniczeniami. Przechodząc przez [serię artykułów na temat programowania webowego w Javie](http://www.samouczekprogramisty.pl/kurs-aplikacji-webowych/) poznasz specyfikację serwletów.
## Spring MVC
  
Spring MVC jest bardzo popularny. Używany jest w dużej liczbie projektów. Dokumentacja tylko do tej biblioteki to kilkaset stron tekstu. Nikt od początkującego nie będzie wymagał cudów. Jeśli będziesz potrafił stworzyć prostą aplikację webową opartą o kilka kontrolerów jesteś na dobrej drodze. Jako przykład może posłużyć Ci [Pogodynka](http://www.samouczekprogramisty.pl/projekty/pogodynka/), którą realizowałem na blogu.
## Bazy danych
  
Większość aplikacji webowych zapisuje dane w bazach danych. Powinieneś wiedzieć czym są relacyjne bazy danych. Takie rzeczy jak klucz główny, klucz obcy, indeks czy rodzaje złączeń nie powinny Cię zaskakiwać. Powinieneś także konstruować proste zapytania z użyciem `GROUP BY`, `HAVING`, `ORDER BY` i `JOIN`.
## ORM (ang. _Object-Relational Mapping_)
  
Po Spring MVC mamy tu kolejnego mocnego gracza. Chodzi o Hibernate. Jest to _de facto_ standard w branży. Także i tutaj dokumentacja to kilkaset stron. Moim zdaniem na początek umiejętność zmapowania relacji wiele do wielu powinna wystarczyć. Resztę przeczytasz w dokumentacji jak będziesz tego potrzebował.
# Programowanie to nie tylko technologia

## Język angielski
  
Większość materiałów pomagających przy nauce programowania napisana jest w języku angielskim. Jeśli nie wiesz od czego zacząć podszlifuj angielski do poziomu gdzie czytanie dokumentacji technicznej nie jest problemem.
## Praca zespołowa
  
Rzadko zdarzają się projekty, które możesz zrealizować samodzielnie. Szczególnie jeśli jesteś początkującym programistą. Programowanie to praca zespołowa. Musisz umieć pracować w zespole. Jak się tego nauczyć? Jak zwykle praktyka jest najlepsza, wcale nie trzeba ćwiczyć tej umiejętności w projekcie programistycznym. Równie dobrze można pracować zespołowo na aplikacją webową jak i innym projektem spoza działki IT.
## Przyjmowanie krytyki
  
Programiści czasami potrafią być straszni. Część osób (w tym ja) ma problem z przyjmowaniem krytyki. Niestety jest to spora wada. Musisz koniecznie się tego oduczyć! Jeśli ktoś krytykuje Twoją pracę postaraj wyciągnąć od tej osoby jak najwięcej informacji. Szczególnie sposobu naprawienia danego błędu. Pamiętaj też, że krytykujące nie zawsze musi mieć rację. Niemniej jednak zawsze warto go wysłuchać aby poznać inny punkt widzenia.
## Sprzedaż
  
No tak... W końcu nadchodzi czas na pierwszą rozmowę kwalifikacyjną. Rozmowa to nic innego jak sprzedawanie swojego czasu przyszłemu pracodawcy. Jeśli potrafisz ten czas sprzedać i pasujesz do zespołu dostaniesz pracę.W tym punkcie można też zawrzeć CV i jego przygotowanie. Tutaj odsyłam Cię do materiałów w internecie - są ludzie, którzy poradzą Ci jak to zrobić dużo lepiej ode mnie ;).
# Podsumowanie
  
Jak wspomniałem na początku artykułu programowanie nie jest łatwe. Jego nauka także nie. Szczególnie, że materiału do opanowania jest sporo. W artykule tym starałem się zebrać dla Ciebie listę zagadnień, które są istotne w pracy jako “junior full-stack Java developer”. A może uważasz, że pominąłem coś istotnego? Daj znać w komentarzach co sądzisz o tej liście.

Mam nadzieję, że artykuł przypadł Ci do gustu. Proszę podziel się nim ze swoimi znajomymi, może ktoś z nich zechce nauczyć się programowania?

Jeśli nie chcesz pominąć kolejnych artykułów dopisz się do samouczkowego newsletera i polub stronę na facebooku. Do następnego razu!

[FM\_form id="3"]

Zdjęcie dzięki uprzejmości&nbsp;https://www.flickr.com/photos/goddess-arts/4442889445/sizes/o
