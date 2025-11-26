# **Programowanie Obiektowe – Java (2025Z)**

# Lab 08: Interfejsy

## **1. Czym jest interfejs w Java?**

Interfejs to **kontrakt**, który określa *jakie operacje* klasa musi udostępniać, ale **nie mówi, jak** ma je implementować.

* Zawiera **nagłówki metod** (domyślnie publiczna, abstrakcyjna).
* Może zawierać:

    * pola `public static final`
    * metody `default`
    * metody `static`
    * metody `private` (od Java 9)
* **Nie może mieć stanu obiektu** (nieprzypisane pola instancyjne — wyjątek: prywatne stałe).

Nazwy interfejsów według konwencji:

* **powinny wyrażać zachowanie**, np.:

    * `Comparable`
    * `Runnable`
    * `Serializable`
    * `Validator`
    * `Encryptor`

**Często są to nazwy odczasownikowe:**

* `Callable`
* `Closable`
* `Iterable`
* `Printable`
* `Storable`

... ale nie zawsze musi tak być...

---

## **2. Dlaczego używamy interfejsów?**

* Aby rozdzielić **co** klasa robi od **jak** to robi.

* Aby umożliwić **wielodziedziczenie zachowań** — klasy nie mogą dziedziczyć po wielu klasach, ale mogą implementować wiele interfejsów.

* Aby pisać **kod bardziej elastyczny i testowalny**:

    * Możemy podmienić implementację interfejsu np. `DatabaseConnector` na wersję testową.

* Do definiowania **standardowych “zdolności”** — np. możliwość sortowania, iterowania, zamykania.

---

## **3. Przykład własnego interfejsu (zgodnie z konwencją)**

```java
public interface Encryptable {
    String encrypt(String input);
    String decrypt(String input);
}
```

Implementacja:

```java
public class SimpleEncryptor implements Encryptable {

    @Override
    public String encrypt(String input) {
        return new StringBuilder(input).reverse().toString();
    }

    @Override
    public String decrypt(String input) {
        return new StringBuilder(input).reverse().toString();
    }
}
```

---

## **4. Metody default i static w interfejsach**

### **Metody default**

Pozwalają dostarczać **domyślną implementację**, którą klasa może nadpisać lub nie.

```java
public interface Validable {
    boolean isValid(String input);

    default void printRule() {
        System.out.println("Validator rule is not specified.");
    }
}
```

### **Metody static**

To metody narzędziowe związane z interfejsem:

```java
public interface Addable {
    static int add(int a, int b) {
        return a + b;
    }
}
```

---

## **5. Najważniejsze wbudowane interfejsy w Java**

### **5.1. `Comparable<T>`**

Opis: naturalny porządek obiektów danej klasy — implementacja pozwala obiektom porównywać się nawzajem (np. do sortowania).

**Metoda kluczowa**:
```java
int compareTo(T other)
```
* przyjmuje: obiekt other tego samego typu T.
* zwraca: liczbę całkowitą:
  * <0 jeżeli this < other,
  * 0 jeżeli równe,
  * \>0 jeżeli this > other.
* zachowanie: powinno być spójne z equals (jeżeli compareTo zwraca 0, to obiekty traktujemy jako równe w sortowaniu).

Przykład:
```java
public class Person implements Comparable<Person> {
private final String name;
private final int age;
public Person(String name, int age) { this.name = name; this.age = age; }

    @Override
    public int compareTo(Person other) {
        return Integer.compare(this.age, other.age); // porządek wg wieku
    }

    // gettery, toString...
}

// użycie:
List<Person> people = List.of(new Person("A",30), new Person("B",25));
List<Person> sorted = new ArrayList<>(people);
Collections.sort(sorted); // używa compareTo
```
---

### **5.2. `Comparator<T>`**

Alternatywny sposób porównywania obiektów.

`Comparator<T>` służy do tworzenia zewnętrznych reguł porównywania obiektów, niezależnych od samej klasy.
Różni się od `Comparable` tym, że:
* `Comparable` → logika sortowania jest **wbudowana w klasę**
* `Comparator` → logika sortowania jest **tworzona poza klasą**, może być wiele różnych komparatorów

**Metoda kluczowa:**
```java
int compare(T o1, T o2);
```

Co przyjmuje?
* o1 — pierwszy obiekt do porównania
* o2 — drugi obiekt do porównania

Co zwraca?
* wartości ujemne → o1 < o2
* 0 → o1 == o2
* wartości dodatnie → o1 > o2

Przykład:
```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public int getAge() { return age; }
}
```

```java
public class PersonAgeComparator implements Comparator<Person> {

    @Override
    public int compare(Person p1, Person p2) {
        return Integer.compare(p1.getAge(), p2.getAge());
    }
}
```
---

### **5.3. `Iterable<T>`**

Umożliwia korzystanie z `foreach`.

`Iterable<T>` to najbardziej podstawowy interfejs dla kolekcji, który umożliwia:
* użycie pętli for-each,
* uzyskanie iteratora,
* tworzenie niestandardowych struktur danych kompatybilnych z Javą

**Metoda kluczowa**:
```java
Iterator<T> iterator();
```

Co przyjmuje?

Nic.

Co zwraca?

Obiekt typu Iterator<T>, który potrafi:
* pobierać kolejne elementy,
* sprawdzić, czy są kolejne elementy,
* opcjonalnie usuwać elementy.

Co robi?

Dostarcza mechanizm iteracji przez obiekt.


Przykład:
```java
public class StringBox implements Iterable<String> {

    private String[] items;
    private int size;

    public StringBox(int capacity) {
        items = new String[capacity];
        size = 0;
    }

    public void add(String value) {
        if (size < items.length) items[size++] = value;
    }

    @Override
    public Iterator<String> iterator() {
        return new Iterator<String>() {

            private int index = 0;

            @Override
            public boolean hasNext() {
                return index < size;
            }

            @Override
            public String next() {
                return items[index++];
            }
        };
    }
}

```

---

### **5.4. `Iterator`**

`Iterator<E>` to fundamentalny interfejs w Javie odpowiedzialny za iterację po elementach kolekcji.
Każdy obiekt, który ma być "iterowalny", dostarcza iterator — zwykle przez metodę `iterator()` z `Iterable<E>` lub `Collection<E>`.

**Metody kluczowe**:
1. hasNext()
```java
boolean hasNext();
```

Przyjmuje:

Nic.

Robi:
* Sprawdza, czy istnieje kolejny element do pobrania.

Zwraca:
* true — jeśli jest następny element
* false — jeśli iteracja się kończy

2. next()
```java
E next();
```

Przyjmuje:

Nic.

Robi:
* Zwraca następny element z kolekcji i przesuwa wewnętrzny wskaźnik o 1.

Zwraca:
* Element typu E(generyczny).

Ważne:
* Jeśli wywołasz `next()` bez `hasNext()`, możesz dostać:
`NoSuchElementException`.

3. remove()
```java
default void remove();
```

Przyjmuje:

Nic.

Robi:
* Usuwa ostatni zwrócony element przez `next()`.

Zwraca:
* Nic (void).

Ograniczenia:
* Nie można wywołać dwa razy pod rząd bez `next()`.
* Rzuca `UnsupportedOperationException`, jeśli iterator nie obsługuje usuwania (np. niektóre kolekcje niemodyfikowalne).

Przykład:
```java
public class StringBox implements Iterable<String> {

    private String[] items;
    private int size;

    public StringBox(int capacity) {
        items = new String[capacity];
        size = 0;
    }

    public void add(String value) {
        if (size < items.length) items[size++] = value;
    }

    @Override
    public Iterator<String> iterator() {
        return new Iterator<String>() {

            private int index = 0;

            @Override
            public boolean hasNext() {
                return index < size;
            }

            @Override
            public String next() {
                return items[index++];
            }
        };
    }
}
```
---

### **5.5. `AutoCloseable`**

`AutoCloseable` to bardzo mały, ale fundamentalny interfejs w Javie.
Umożliwia automatyczne zamykanie zasobów przez konstrukcję `try-with-resources`
i jest podstawą bezpiecznej obsługi zasobów, takich jak:
* pliki (FileInputStream, BufferedWriter, …)
* połączenia sieciowe
* połączenia bazodanowe
* strumienie
* locki
* własne klasy, które czegoś „pilnują”

Wiele obiektów wymaga „sprzątania”, np.:
* zamknięcie pliku
* zwolnienie gniazda TCP
* oddanie połączenia do puli
* zwolnienie zasobu systemowego

Java nie może tego zrobić sama („GC nie zamknie Ci pliku”).
Dlatego klasy, które zarządzają zasobami, implementują `AutoCloseable`.

**Metoda kluczowa**:
```java
void close() throws Exception;
```

Przyjmuje:

Nic.

Robi:
* Zamyka zasób, sprząta, uwalnia pamięć natywną, kończy połączenia itp.

Zwraca:
* Nic(void).

Może wyrzucać:
* `Exception` — celowo szeroko, w przeciwieństwie do `Closeable`, które wyrzuca `IOException`.

Przykład:
```java
public class FileWrapper implements AutoCloseable {

    private final BufferedReader reader;

    public FileWrapper(String path) throws IOException {
        reader = new BufferedReader(new FileReader(path));
    }

    public String readLine() throws IOException {
        return reader.readLine();
    }

    @Override
    public void close() throws IOException {
        System.out.println("Zamykam plik...");
        reader.close();
    }
}
```
---

### **5.6. `Runnable`**

`Runnable` to funkcjonalny interfejs (**SAM – Single Abstract Method**), który reprezentuje fragment kodu, który ma zostać wykonany w osobnym wątku lub przekazany jako zadanie do uruchomienia.

Jest fundamentem programowania współbieżnego w Javie.

Definicja interfejsu Runnable:
```java
@FunctionalInterface
public interface Runnable {
void run();
}
```
Co oznacza?
* posiada jedną metodę abstrakcyjną `run()` → dlatego można go używać jako **_lambdy_**,
* metoda nie przyjmuje argumentów,
* nie zwraca wartości (void),
* kod w `run()` to instrukcje, które mają wykonać się w osobnym wątku lub jednostce pracy.
---

### **5.7. `Callable<V>`**

`Callable` to funkcjonalny interfejs wprowadzony w Java 5 wraz z pakietem `java.util.concurrent`.
Służy do reprezentowania zadań, które mają:
* zwracać wynik (V),
* zgłaszać wyjątki typu checked,
* wykonywać się w osobnym wątku (głównie przez `ExecutorService`).

Definicja interfejsu Callable:
```java
@FunctionalInterface
public interface Callable<V> {
V call() throws Exception;
}
```

Znaczenie:
* metoda `call()` zwraca wartość,
* metoda może zgłaszać wyjątki checked,
* parametr `V` oznacza typ wyniku zadania (może być dowolny typ: `Integer`, `String`, własna klasa itd.).

Używasz Callable, gdy potrzebujesz:
* zwrócić wynik,
* obsłużyć wyjątek,
* śledzić postęp pracy za pomocą Future,
* przetwarzać dane w tle i pobrać rezultat.

Używasz Runnable, gdy:
* nie potrzebujesz wyniku,
* nie chcesz rzucać checked exceptions,
* zadanie ma tylko coś wykonać.
---

### **5.8. `Serializable`**

`Serializable` to marker interface (interfejs znacznikowy), który sygnalizuje JVM, że obiekt klasy może zostać:
* zapisany do strumienia bajtów (serializacja),
* odtworzony z tych bajtów (deserializacja).

Nie posiada żadnych metod.
Jego rola polega tylko i wyłącznie na oznaczeniu klasy.
---

### **5.9. `Cloneable`**

`Cloneable` to marker interface (znacznikowy), który mówi JVM, że **_obiekt klasy może być legalnie klonowany metodą `clone()` z klasy Object_**.

Nie zawiera żadnych metod.
Cała logika klonowania jest wbudowana w Object (jako nadpisywanie tej metody `clone()`)
---

## **6. Porównanie interfejsów z klasami abstrakcyjnymi**

| Cecha            | Interfejs                            | Klasa abstrakcyjna                           |
| ---------------- | ------------------------------------ | -------------------------------------------- |
| Dziedziczenie    | **Wiele interfejsów naraz**          | Tylko jedna klasa abstrakcyjna               |
| Pola instancyjne | ❌ (tylko final static)               | ✔ normalne pola                              |
| Konstruktor      | ❌ brak                               | ✔ może mieć                                  |
| Metody           | abstrakcyjne, default, static        | abstrakcyjne i normalne                      |
| Stan obiektu     | brak                                 | jest                                         |
| Zastosowanie     | definiowanie **zdolności, zachowań** | tworzenie **częściowej implementacji klasy** |
| Dziedziczenie    | klasy implementują interfejs         | klasy dziedziczą po klasach abstr.           |

### **Kiedy używać interfejsów?**

* Gdy klasa ma jakąś zdolność:
  `Comparable`, `Runnable`, `Encryptor`, `Validator`, `Storable`.

### **Kiedy używać klasy abstrakcyjnej?**

* Gdy potrzebujesz wspólnego **stanu** i częściowej implementacji.
* Gdy podklasy są blisko powiązane (np. hierarchia zwierząt).

---

## **7. Ważne konwencje nazewnictwa**

### Dobre:

* `Encryptor`
* `Validator`
* `Repository`
* `Notifier`

### Niedobre:

* `EncryptionInterface`
* `IValidator`
* `SomethingDo`
* `Doer`

## **8. Dwa interfejsy do jednej klasy**

W Javie można podpiąć (implementować) kilka interfejsów do jednej klasy. Jest to jedna z fundamentalnych cech języka Java, która pozwala na wielokrotną implementację interfejsów, co umożliwia wielokrotne dziedziczenie zachowań. W przeciwieństwie do dziedziczenia klas, gdzie Java pozwala na dziedziczenie tylko z jednej klasy bazowej, klasa może implementować dowolną liczbę interfejsów.
```java
public interface InterfaceA {
    void methodA();
}

public interface InterfaceB {
    void methodB();
}

public class MyClass implements InterfaceA, InterfaceB {
    @Override
    public void methodA() {
        // Implementacja metody z InterfaceA
    }

    @Override
    public void methodB() {
        // Implementacja metody z InterfaceB
    }
}
```

## **9. “Dziedziczenie” interfejsów**

W Javie interfejs może rozszerzać jeden lub więcej innych interfejsów.
Oznacza to, że nowy interfejs dziedziczy wszystkie abstrakcyjne metody swoich interfejsów bazowych i może:
* dodawać nowe metody abstrakcyjne,
* definiować metody domyślne (default) lub statyczne (static).

Interfejs dziedziczy tylko deklaracje metod i stałe (public static final), nie pola instancji ani implementacje z klas.

Przykład:
```java
public interface A {
    void methodA();
}

public interface B extends A {
    void methodB();
}
```

Java nie ogranicza liczby dziedziczonych interfejsów, w przeciwieństwie do klas (gdzie mamy pojedyncze dziedziczenie).

```java
// reszta kodu jak wyżej:>
public interface C {
    void methodC();
}

public interface D extends A, C {
    void methodD();
}
```

Klasa implementująca D musi dostarczyć implementację wszystkich trzech metod (A, C, D).

---

1.  Napisz klasę Student, która zawiera pola: name (typu String), averageGrade (typu
    double) i yearOfBirth (typu int). Zaimplementuj interfejs Comparable w taki sposób,
    aby obiekty klasy Student były sortowane malejąco według średniej ocen. Stwórz listę
    tablicową 5 obiektów klasy Student i posortuj ją według sprecyzowanego kryterium.
2.  Napisz klasę Product, która zawiera pola: name (typu String), price (typu double) i
    expirationDate (typu LocalDate). Zaimplementuj interfejs Comparable w taki sposób,
    aby obiekty klasy Product były sortowane według jednego kryterium: malejąco według
    daty ważności, a przy równości sortowane były rosnąco według ceny. Stwórz listę obiek
    tów klasy Product i posortuj ją według sprecyzowanego kryterium. Następnie wyświetl
    posortowaną listę na ekranie.
3.  Napisz klasę Osoba z polami imie (String), wiek (int) i wzrost (double). Napisz
    klasę implementującą interfejs Comparator, która porównuje osoby na podstawie wieku.
    Stwórz tablicę 5 osób i posortuj ją według wieku
4.  Napisz klasę Order z polami id (typu int), customerName (typu String) oraz
    orderDate (typu LocalDate). Zaimplementuj dwie klasy implementujące generyczny
    interfejs Comparator: OrderDateComparator do porównywania obiektów po polu
    orderDate (od najwcześniejszej do najpóźniejszej daty) oraz CustomerNameComparator
    do porównywania obiektów po polu customerName (alfabetycznie od A do Z). Stwórz
    listę 5 obiektów klasy Order i posortuj ją zgodnie z oboma kryteriami (najpierw po
    dacie zamówienia, a następnie przy równości po nazwie klienta).
5.  Napisz interfejs o nazwie LoudAnimal, który będzie miał jedną metodę o nazwie
    makeNoise(). Następnie stwórz dwie klasy: Dog i Cat, które będą implementować ten
    interfejs. Dla każdej klasy zaimplementuj metodę makeNoise(), tak aby wydrukowała
    ona odpowiedni dźwięk zwierzęcia
6. Wykonaj poniższe czynności:
   * Stwórz interfejs Drawable z metodą draw().
   * Rozszerz ten interfejs, tworząc ColorDrawable, który dodaje metodę setColor(String
   color).
   * Stwórz klasę Circle, implementującą ColorDrawable. Metoda draw() powinna ryso
   wać koło, a setColor()- zmieniać kolor koła (metody mają wyświetlać odpowiednie
   komunikaty).
   * W osobnej klasie testującej (TestDrawing), utwórz obiekt Circle, ustaw kolor i narysuj
   koło za pomocą obu metod

