---
layout: default
title: Advent of Code 2016 dzień 2
date: '2016-12-04 19:53:28 +0100'
categories:
- Strefa zadaniowa
excerpt_separator: "<!--more-->"
permalink: "/advent-of-code-2016-dzien-2/"
---
Advent of Code 2016 dzień 2. Dzisiaj pomagamy Mikołajowi skorzystać z toalety w Kwaterze Głównej Króliczka Wielkanocnego. Dasz radę znaleźć prawdziwy kod?

# Wprowadzenie
  
{% include aoc-2016-link.md day="2" %}

Advent of Code to inicjatywa, w której codziennie publikowane są zadania algorytmiczne dla programistów. Ich rozwiązywanie pomaga rozwijać umiejętności nie tyko początkujących programistów. W tej serii artykułów pokazuję zadanie opublikowane w ramach Advent of Code 2016 wraz z przykładowym rozwiązaniem.

{% include aoc-2016-leaderboard.md %}

# Dzień 2 zadanie 1
  
Docierasz do Kwatery Głównej Króliczka Wielkanocnego pod osłoną nocy. Zrzut odbył się w takim pośpiechu, że zapomniałeś skorzystać z toalety! Odjechane budynki, takie jak Kwatera Główna Króliczka Wielkanocnego, wejścia do toalet przeważnie mają chronione kodem. Zaczynasz szukać recepcji z nadzieją, że tam znajdziesz kod.

Znalazłeś dokument, zaczynasz czytać "W celu poprawienia bezpieczeństwa kody do toalet nie będą już zapisywane na kartkach. Proszę je zapamiętać i podążać za instrukcjami poniżej aby dostać się do toalety.".

Dokument wyjaśnia, że każdy znak kodu, który ma być naciśnięty może być odnaleziony zaczynając od poprzedniego znaku. Ruch polega na poruszaniu się po sąsiadujących przyciskach według instrukcji:

- `U` do góry,
- `D` w dół,
- `L` w lewo,
- `R` w prawo.
  
  
Każda linijka z instrukcjami odpowiada jednemu znakowi na klawiaturze. Każda kolejna instrukcja rozpoczyna od znaku, na którym skończyłeś poprzednio. Dla pierwszej linii zakładamy, że pozycją początkową jest "5". Jeśli ruch "wychodzi" poza klawiaturę zignoruj go.

Powoli nie możesz już wytrzymać więc decydujesz się odgadnąć kod w drodze do toalety. Wyobrażasz sobie klawiaturę w postaci:

    1 2 34 5 67 8 9

  
Zakładając, że instrukcje są następujące:

    ULLRRDDDLURDLUUUUD

- Zaczysznasz na 5, idziesz do góry (2), w lewo (1) i w lewo (nie możesz wykonać tego ruchu więc zostajesz na 1), pierwszą cyfrą kodu jest 1,
- Zaczynając z poprzedniego przycisku (1), poruszasz się w prawo dwa razy (3) i trzy razy w dół (9) (zatrzymujesz się na 9 po dwóch ruchach, ignorując trzeci), drugą cyfrą kodu jest 9,
- Idąc dalej z 9 idziesz w lewo (8), do góry (5), w prawo (6), w dół (9) i w lewo (8), trzecią cyfrą kodu jest 8,
- W końcu, z 8 idziesz do góry cztery razy (zatrzymując się na 2) i raz w dół kończąc na 5.
  
  
W tym przypadku kod do toalety to 1985.&nbsp; Jaki jest kod do toalety jeśli instrukcje są jak w pliku [day02\_input](http://www.samouczekprogramisty.pl/wp-content/uploads/2016/12/day02_input.txt)?
# Podsumowanie
  
Zachęcam do dalszej zabawy z drugim zadaniem, jego treść pokaże się na stronie AoC2016 po rozwiązaniu pierwszego. Takie zadania pomagają w rozwijaniu umiejętności nie tylko początkujących programistów. Jeśli będziesz miał jakikolwiek problem z rozwiązaniem zadania możesz rzucić okiem do [przykładowego rozwiązania](https://github.com/SamouczekProgramisty/StrefaZadaniowaSamouka/tree/master/05_aoc_2016/src/main/java/pl/samouczekprogramisty/szs/aoc2016/day02), jednak zrób to raczej w ostateczności.

Na koniec mam do Ciebie prośbę - podziel się linkiem do artykułu ze znajomymi, może Oni także będą chcieli pomóc Świętemu Mikołajowi ;) ? Jeśli nie chcesz ominąć kolejnych artykułów proszę zapisz się do mojego newslettera i polub stronę na facebooku. Do następnego razu!

[FM\_form id="3"]

Zdjęcie dzięki uprzejmości https://www.flickr.com/photos/derekl/sets/72157649148835567
