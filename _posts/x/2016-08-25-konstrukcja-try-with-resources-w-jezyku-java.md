---
layout: default
title: Konstrukcja try-with-resources w języku Java
date: '2016-08-25 18:45:08 +0200'
categories:
- Kurs programowania Java
excerpt_separator: "<!--more-->"
permalink: "/konstrukcja-try-with-resources-w-jezyku-java/"
---
Artykuł opisuje konstrukcję try-with-resources występującą w języku Java. Na przykładowym programie będziesz mógł dowiedzieć się jak ona działa. W artykule przeczytasz też o tym czym jest „cukier syntaktyczny”. Całość zakończymy jak zwykle zadaniem, które pozwoli Ci przećwiczyć materiał opisany w artykule. Zapraszam!

{% include kurs-java-notice.md %}

# Try-with-resources
  
Pamiętasz w jaki sposób musieliśmy [obchodzić się ze strumieniami danych](http://www.samouczekprogramisty.pl/operacje-na-plikach-w-jezyku-java/)? Trzeba było koniecznie pamiętać o ich zamknięciu. Generowało to sporą masę powtarzalnego kodu (ang. _boilerplate code_). Dobrą praktyką jest unikanie, powtórzeń – mniej kodu źródłowego, mniej miejsc do popełniania potencjalnego błędu :). Tutaj z pomocą przyszli twórcy Javy, wprowadzając konstrukcję „try-with-resources”.

Tego typu konstrukcję możemy nazwać cukrem syntaktycznym (ang. _syntactic sugar_). Takie konstrukcje upraszczają kod, jednak nie są niezbędne dla języka programowania. Innymi słowy programista mógłby sobie bez takich konstrukcji poradzić, jednak ich obecność zmniejsza ilość kodu, która jest potrzebna do wykonania tego samego zadania.

Najlepiej będzie jeśli zobaczymy to na przykładzie. Poniższy fragment kodu prezentuje odczytanie linijki z pliku tekstowego. W tym przykładzie także obsługujemy wyjątki, nie są one wspomniane w sygnaturze metody po klauzuli `throws` więc odpowiednie bloki `catch` muszą się znaleźć aby kompilator pozwolił nam ten kod skompilować (pamiętasz [wyjątki typu checked i unchecked](http://www.samouczekprogramisty.pl/wyjatki-w-jezyku-java/)?)

    BufferedReader fileReader = null;try { fileReader = new BufferedReader(new FileReader(inputPath)); fileReader.readLine();} catch (FileNotFoundException e) { e.printStackTrace();} catch (IOException e) { e.printStackTrace();} finally { if (fileReader != null) { try { fileReader.close(); } catch (IOException e) { e.printStackTrace(); } }}

  
Prawda, że dużo tego? I tu z pomocą przychodzi konstrukcja try-with-resources, zobacz proszę jak ona wygląda:

    try(BufferedReader fileReader = new BufferedReader(new FileReader(inputPath))) { fileReader.readLine();} catch (FileNotFoundException e) { e.printStackTrace();} catch (IOException e) { e.printStackTrace();}

  
Konstrukcja wygląda jak `try`/`catch` z tym, że przez blokiem objętym `try` możemy zainicjalizować zmienne, które zostaną automatycznie zamknięte. Kompilator widząc konstrukcję try-with-resources wygeneruje za nas kod zbliżony do tego z pierwszego przykładu (w podobny sposób generuje np. domyślne konstruktory).

Konstrukcję try-with-resources możemy nazwać także „menadżerem kontekstu”, automatycznie zarządza ona za nas kontekstem, w ramach którego dostępne są zmienne zdefiniowane wewnątrz nawiasów `()`. Co więcej, wewnątrz tych nawiasów możemy zainicjalizować więcej zmiennych, każda z nich zostanie poprawnie zamknięta (zostanie na nich wywołana metoda `close`). W przykładzie poniżej odczytujemy linijkę z pliku wejściowego i zapisujemy ją do pliku wyjściowego (tutaj pominąłem obsługę wyjątków):

    try( BufferedReader fileReader = new BufferedReader(new FileReader(inputPath)); BufferedWriter fileWriter = new BufferedWriter(new FileWriter(outputPath))) { String line = fileReader.readLine(); fileWriter.write(line);}

  
Taka struktura tłumaczona jest przez kompilator na sekwencję zagnieżdżonych bloków `try`/`catch`/`finally`. W każdym z tych zagnieżdżonych bloków zarządza kolejną zmienną. Zmienne zamykane są w odwrotnej kolejności niż są deklarowane, zatem w naszym przykładzie pierwszy zostanie zamknięty `fileWriter` po nim `fileReader`.
# Własne menadżery kontekstu
  
Całość działa, ponieważ klasy takie jak `BufferedFileWriter` czy `BufferedFileReader` implementują interfejs [`java.lang.AutoCloseable`](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html). Właściwie to wszystkie znane mi klasy do obsługi strumieni wejściowych/wyjściowych w standardowej bibliotece Javy implementują ten interfejs. To jest właśnie główne wymaganie, które musi być spełnione przez zmienne definiowane wewnątrz `()` gdy używamy konstrukcji try-with-resources.

Nasunąć się zatem może pytanie, czy&nbsp; możemy stworzyć własną klasę, która będzie działała z tą konstrukcją? Oczywiście! :) Poniżej przykład:

    public class MyContextManager implements AutoCloseable { public MyContextManager() { System.out.println("Wlasnie sie tworze!"); } public void doSomething() { System.out.println("Wlasnie cos robie :)"); } public void close() { System.out.println("Ktos mnie teraz zamyka!"); }}public class MyContextManagerMain { public static void main(String[] args) { try (MyContextManager manager = new MyContextManager()) { manager.doSomething(); } }}

  
`MyContextManager` implementuje interfejs `AutoCloseable` dzięki czemu możemy użyć go wewnątrz konstrukcji try-with-resources. Jeśli uruchomiłbyś metodę main z powyższego przykładu na konsoli pojawiłyby się następujące linijki:

    Wlasnie sie tworze!Wlasnie cos robie :)Ktos mnie teraz zamyka!

  
Widzisz? Metody `close` nie wywoływałem nigdzie a mimo to została automatycznie wykonana podczas działania programu. Stało się tak właśnie dzięki kompilatorowi, który przetłumaczył konstrukcję try-with-resources na kod, który poprawnie zamyka zmienne.
# Zadanie
  
Teraz nadszedł czas na zadanie dla Ciebie. Strony w internecie tworzone są przy pomocy `HTML`, jest to język znaczników, który jest&nbsp; interpretowany przez przeglądarki internetowe wyświetlające strony www. Przykładem znaczników jest para ``, jak widzisz jest tu znacznik otwierający `` oraz zamykający `` (akurat ta para odpowiedzialna jest za wyróżnianie tekstu). Wewnątrz znaczników znajduje się treść np. `tekst`. Istnieje wiele innych par znaczników np. ``, ``, `` itd.,

Zaimplementuj klasę `HtmlTag`, która będzie menadżerem kontekstu. W konstruktorze powinna przyjmować nazwę znacznika np. `em`. Klasa powinna mieć metodę `body`, która przyjmie parametr typu `String` i wypisze go na konsoli. Uruchamiając kod

    try ( HtmlTag h1 = new HtmlTag("h1"); HtmlTag em = new HtmlTag("em")) { em.body("moj tekst");}

  
Na konsoli powinien znaleźć się tekst

    moj tekst

  
Jak zwykle zachęcam do samodzielnego rozwiązania zadania, jeśli jednak utknąłbyś przygotowałem [przykładowe rozwiązanie](https://github.com/SamouczekProgramisty/KursJava/blob/master/17_manager_kontekstu/src/main/java/pl/samouczekprogramisty/kursjava/context/exercise/HtmlTag.java), które może Ci pomóc.
# Materiały dodatkowe

- [dokumentacja interfejsu AutoCloseable](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html)
- [sekcja z Java Language Specification opisująca konstrukcję try-with-resources](http://docs.oracle.com/javase/specs/jls/se8/html/jls-14.html#jls-14.20.3)
- [kod źródłowy przykładów i rozwiązanie zadania](https://github.com/SamouczekProgramisty/KursJava/tree/master/17_manager_kontekstu/src/main/java/pl/samouczekprogramisty/kursjava/context)
  

# Podsumowanie
  
Po przeczytaniu artykułu wiesz już jak używać konstrukcji try-with-resources, wiesz czym jest cukier syntaktyczny. Mam nadzieję, że spędziłeś trochę czasu rozwiązując zadanie, które pozwoliło Ci przećwiczyć wiedzę w praktyce. Na koniec mam do Ciebie wielką prośbę, zależy mi na dotarciu do jak największej liczby czytelników, możesz mi w tym pomóc polecając bloga znajomym i udostępniając link na facebooku, z góry dziękuję i do następnego razu.

[FM\_form id="3"]

Zdjęcie dzięki uprzejmości https://www.flickr.com/photos/livinginmonrovia
