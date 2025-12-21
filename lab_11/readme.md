# **Programowanie Obiektowe – Java (2025Z)**

# Lab 11: Wyrażenia lambda, strumienie, referencja do metody, klasy anonimowe

## Interfejs funkcyjny – punkt wyjścia

### Definicja

**Interfejs funkcyjny** to interfejs, który posiada **dokładnie jedną metodę abstrakcyjną**.

Przykłady wbudowane:

* `Runnable`
* `Callable<T>`
* `Comparator<T>`
* `Consumer<T>`
* `Function<T, R>`
* `Predicate<T>`

To jest **kluczowe**, bo:

> **Lambda jest tylko skróconą implementacją interfejsu funkcyjnego**

---

## Klasy anonimowe – historyczne tło

Zanim pojawiły się lambdy, Java używała **klas anonimowych**.

### Idea

Tworzymy **jednorazową implementację interfejsu lub klasy abstrakcyjnej**.

### Przykład koncepcyjny

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Wątek działa");
    }
};
```

Co się tu dzieje?
* tworzona jest nowa klasa (bez nazwy)
* klasa implementuje Runnable
* obiekt tej klasy przypisywany jest do zmiennej typu Runnable
* działa polimorfizm

### Definicja 
Klasa anonimowa to:

>jednorazowa, bezimienna klasa tworzona w miejscu użycia,
która dziedziczy po klasie lub implementuje interfejs
i nie ma własnej nazwy.

Tworzymy ją wtedy, gdy:
* klasa ma być użyta tylko raz
* nie ma sensu nadawać jej nazwy
* chcemy nadpisać metody w miejscu użycia

### Cechy

* Brak nazwy klasy
* Pełny dostęp do interfejsu / klasy abstrakcyjnej
* Dużo kodu szablonowego

### Ogólny schemat

```java
Typ referencji = new Typ() {
    // implementacja metod
};
```
Gdzie:
* Typ → interfejs albo klasa (często abstrakcyjna)
* { ... } → ciało nowej, anonimowej klasy

### Klasa anonimowa a klasa abstrakcyjna

```java
abstract class Animal{
    abstract void makeSound();
}
```

```java
Animal animal = new Animal() {
    @Override
    void makeSound() {
        System.out.println("Nieznany dźwięk");
    }
};
```

Wnioski:
* nie można utworzyć instancji klasy abstrakcyjnej
* chyba że robimy to przez klasę anonimową
* klasa anonimowa musi zaimplementować wszystkie metody abstrakcyjne

Klasa anonimowa:
* nie może mieć konstruktora
* nie może dziedziczyć po więcej niż jednej klasie
* nie może mieć modyfikatorów dostępu
* nie ma nazwy
* nie może być ponownie użyta

### Przykład praktyczny

```java
abstract class Validator {
    abstract boolean isValid(String value);

    void printResult(String value) {
        if (isValid(value)) {
            System.out.println("Poprawna wartość: " + value);
        } else {
            System.out.println("Niepoprawna wartość: " + value);
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {

        Validator passwordValidator = new Validator() {

            @Override
            boolean isValid(String value) {
                return value.length() >= 8 && value.matches(".*\\d.*");
            }
        };

        passwordValidator.printResult("haslo");
        passwordValidator.printResult("haslo123");
    }
}
```

---

## Wyrażenia lambda (lambda expressions)

### Definicja

**Wyrażenie lambda** to:

> zwięzły zapis implementacji metody interfejsu funkcyjnego

_**WAŻNE**_:
* lambda nie jest metodą
* lambda nie jest klasą
* lambda nie ma własnego typu
* typ lambdy wynika z kontekstu (interfejsu)

Lambda może istnieć tylko tam, gdzie oczekiwany jest:
* interfejs z jedną metodą abstrakcyjną

```java
@FunctionalInterface
interface Printer {
void print(String text);
}
```

### Składnia

#### Pełna postać
```java
(parametry z typem) -> { ciało metody }
```

Przykład:

```java
(String text) -> {
    System.out.println(text);
}
```

#### Skrócona (inferencja typów)

```java
parametr -> { ciało metody }
```

Przykład:

```java
text -> System.out.println(text);
```

#### Zwracanie wartości - przykład:

```java
(x, y) -> x + y
```

### Lambda jako „zachowanie” (behavior as data)

Lambda pozwala przekazywać logikę jako argument metody albo przypisywać do zmiennej.

### Przykłady

#### Runnable

```java
Runnable r = () -> System.out.println("Wątek działa");
```

#### Comparator

```java
Comparator<String> cmp = (a, b) -> a.length() - b.length();
```

#### Consumer

```java
Consumer<String> c = s -> System.out.println(s);
```
#### Praktyczny przykład

a) filtrowanie kolekcji

```java
List<String> names = new ArrayList<>(List.of("Ala", "Zosia", "Ania"));

names.sort((a, b) -> a.length() - b.length());
```

b) "zamień na wielkie litery każdy element"

```java
names.forEach(name -> System.out.println(name.toUpperCase()));
```

c) użycie interfejsu Predicate<E>

```java
Predicate<String> longName = s -> s.length() > 4;

System.out.println(longName.test("Ania"));   // false
System.out.println(longName.test("Zuzanna")); // true
```

### Lambda:

* przyjmuje → parametry metody
* wykonuje → logikę
* zwraca → to, co metoda interfejsu

---

## Lambda a polimorfizm

Lambda **nie jest typem** — jej typem jest **interfejs**, do którego ją przypisujemy.

```java
Comparator<Integer> c = (a, b) -> a - b;
```

### **polimorfizm interfejsów**

* ta sama lambda może pasować do różnych kontekstów
* zachowujemy zasadę „programuj do interfejsu”

---

## _**Ważne**_

Lambda:
* może używać zmiennych lokalnych, ale muszą być final lub efektywnie final
* nie może „rozszerzać” sygnatury wyjątków
* musi respektować wyjątki interfejsu

## Referencje do metod

### Definicja

**Referencja do metody** to jeszcze krótszy zapis lambdy, gdy:

* tylko wywołujemy istniejącą metodę
* bez dodatkowej logiki

### Rodzaje

#### 1. Metoda statyczna

```java
Integer::compare
```

#### 2. Metoda instancyjna obiektu

```java
System.out::println
```

#### 3. Metoda instancyjna klasy

```java
String::length
```

#### 4. Konstruktor

```java
ArrayList::new
```

### Porównanie

```java
list.forEach(s -> System.out.println(s));   // lambda
list.forEach(System.out::println);          // referencja do metody
```

 To **to samo semantycznie**

## Lambda vs referencja do metod vs klasa anonimowa

| Cecha                                 | Klasa anonimowa                            | Wyrażenie lambda                    | Referencja do metody                        |
| ------------------------------------- |--------------------------------------------| ----------------------------------- | ------------------------------------------- |
| Czym jest                             | Bezpośrednia definicja klasy bez nazwy     | Wyrażenie implementujące metodę     | Skrócony zapis wywołania istniejącej metody |
| Czy jest klasą                        | ✔                                          | ❌                                   | ❌                                           |
| Czy ma nazwę                          | ❌                                          | ❌                                   | ❌                                           |
| Wymaga interfejsu funkcyjnego         | ❌ (może być klasa abstrakcyjna)            | ✔                                   | ✔                                           |
| Ilość metod abstrakcyjnych            | Dowolna                                    | Dokładnie 1                         | Dokładnie 1                                 |
| Może implementować klasę abstrakcyjną | ✔                                          | ❌                                   | ❌                                           |
| Może mieć pola (stan)                 | ✔                                          | ❌                                   | ❌                                           |
| Może mieć konstruktor                 | ❌                                          | ❌                                   | ❌                                           |
| Może zawierać złożoną logikę          | ✔                                          | ✔ (ograniczoną do 1 metody)         | ❌                                           |
| Czy tworzy nowe zachowanie            | ✔                                          | ✔                                   | ❌                                           |
| Czy deleguje do istniejącej metody    | Opcjonalnie                                | Opcjonalnie                         | ✔ (zawsze)                                  |
| `this`                                | Odnosi się do klasy anonimowej             | Odnosi się do klasy zewnętrznej     | Odnosi się do klasy zewnętrznej             |
| Czytelność                            | Średnia / niska                            | Wysoka                              | Bardzo wysoka                               |
| Typowe zastosowanie                   | Klasy abstrakcyjne, listenerzy, strategia  | Strumienie, kolekcje, prosta logika | Strumienie, przekazywanie metod             |
| Zastępowalność                        | Najbardziej elastyczna                     | Zastępuje wiele klas anonimowych    | Najbardziej zwięzła forma                   |


---

## Podsumowanie: Klasy anonimowe vs lambda vs referencja

| Mechanizm       | Kiedy używać                               |
| --------------- | ------------------------------------------ |
| Klasa anonimowa | Gdy potrzeba stanu lub wielu metod         |
| Lambda          | Gdy implementujesz 1 metodę                |
| Referencja      | Gdy tylko delegujesz do istniejącej metody |

---

## Strumienie (Stream API)

### Definicja

**Strumień** to:

> sekwencja danych, na której wykonujemy operacje funkcyjne

Strumień:

* **nie przechowuje danych**
* **nie modyfikuje kolekcji**
* działa **leniwie**

---

## Tworzenie strumieni

Najczęściej z kolekcji:

```java
list.stream();
```

Z tablic:

```java
Arrays.stream(array);
```

---

## Operacje pośrednie i terminalne

### Operacje pośrednie (lazy)

* `filter`
* `map`
* `sorted`
* `distinct`
* `limit`

> Zwracają **nowy strumień**

### Operacje terminalne

* `forEach`
* `collect`
* `count`
* `findFirst`
* `anyMatch`

> **Uruchamiają strumień**

---

## Co to znaczy, że **strumień działa leniwie** (lazy evaluation)?

**Leniwość strumienia** oznacza, że:

> **Operacje na strumieniu NIE są wykonywane w momencie ich deklarowania,
> tylko dopiero wtedy, gdy pojawi się operacja terminalna.**

Innymi słowy:

* zapisujesz **plan przetwarzania danych**
* ale **nic się nie wykonuje**, dopóki nie zażądasz wyniku

---

## Jak to wygląda w praktyce (krok po kroku)

```java
Stream<Integer> stream = list.stream()
    .filter(x -> x > 10)
    .map(x -> x * 2);
```

**Na tym etapie:**

* nie wykonano `filter`
* nie wykonano `map`
* nie przetworzono żadnego elementu

To jest tylko **opis potoku operacji**.

---

## Moment uruchomienia strumienia

```java
stream.forEach(System.out::println);
```

Dopiero **`forEach`** (operacja terminalna):

* uruchamia strumień
* powoduje faktyczne przetwarzanie danych

---

## Leniwość element po elemencie

Strumień **nie działa etapami na całej kolekcji**, tylko:

> **przetwarza jeden element od początku do końca potoku, a potem następny**

Przykład:

```java
list.stream()
    .filter(x -> {
        System.out.println("filter: " + x);
        return x > 10;
    })
    .map(x -> {
        System.out.println("map: " + x);
        return x * 2;
    })
    .findFirst();
```

### Co się stanie?

* strumień przetworzy **tylko tyle elementów, ile potrzeba**
* `findFirst()` zatrzyma strumień po pierwszym pasującym elemencie

To jest **optymalizacja wynikająca z leniwości**

---

## Dlaczego to jest ważne?

### Wydajność

* brak niepotrzebnych obliczeń
* brak tworzenia pośrednich kolekcji

### Możliwość nieskończonych strumieni

```java
Stream.iterate(1, x -> x + 1)
      .filter(x -> x % 2 == 0)
      .limit(5)
      .forEach(System.out::println);
```

To działa, bo strumień:

* nie generuje wszystkiego naraz
* produkuje dane **na żądanie**

---

## Porównanie z podejściem klasycznym

### Imperatywnie (bez leniwości):

```java
List<Integer> result = new ArrayList<>();
for (int x : list) {
    if (x > 10) {
        result.add(x * 2);
    }
}
```

* przetwarzane są **wszystkie elementy**
* brak możliwości wcześniejszego zatrzymania

### Funkcyjnie (strumień):

```java
list.stream()
    .filter(x -> x > 10)
    .map(x -> x * 2)
    .findFirst();
```

* przetwarzanie kończy się **jak tylko znajdziemy wynik**

---

## Co NIE jest leniwe?

* operacje terminalne:

    * `forEach`
    * `collect`
    * `count`
    * `findFirst`
    * `anyMatch`

One:

* **uruchamiają strumień**
* **zwracają wynik**
* **kończą strumień** (nie da się go użyć ponownie)

---

## Jednozdaniowa definicja (egzaminacyjna)

> Strumień w Javie działa leniwie, co oznacza, że operacje pośrednie nie są wykonywane w momencie ich deklaracji, lecz dopiero w chwili wywołania operacji terminalnej, a przetwarzanie odbywa się element po elemencie tylko w zakresie niezbędnym do uzyskania wyniku.

## Lambda + Stream + Kolekcje

```java
list.stream()
    .filter(x -> x > 10)
    .sorted()
    .forEach(System.out::println);
```

Co tu się dzieje:

* `filter` → `Predicate<T>`
* `sorted` → `Comparator<T>`
* `forEach` → `Consumer<T>`

> **wszystko, co już omawialiśmy, spotyka się w jednym miejscu**

---

## Strumienie a generyki

Stream jest generyczny:

```java
Stream<T>
```

Metody:

* `map(Function<T, R>)` → zmiana typu
* `filter(Predicate<T>)` → zachowanie typu

To tłumaczy:

* wildcards
* bounded types
* inferencję typów (`var`)

---

# Zadania


## Klasy anonimowe 

### Zadanie 1

Stwórz **klasę abstrakcyjną `Shape`** z metodą `double area()`.
Natsępnie utwórz **klasę anonimową**, która reprezentuje **prostokąt**, i oblicz jego pole.

### Zadanie 2

Stwórz **klasę abstrakcyjną `Validator`** z metodą `boolean isValid(String value)`.
W metodzie `main` utwórz **anonimowy walidator**, który sprawdza, czy napis:

* ma co najmniej 6 znaków
* zawiera cyfrę

### Zadanie 3

Stwórz klasę `Button`, która posiada metodę:

```java
void onClick(Action action)
```

gdzie `Action` jest **interfejsem z dwiema metodami abstrakcyjnymi**.
Zaimplementuj obsługę kliknięcia **za pomocą klasy anonimowej**.


### Zadanie 4

Stwórz klasę abstrakcyjną `Logger` z metodami:

* `logInfo(String msg)`
* `logError(String msg)`

Utwórz **jednorazową implementację** loggera jako klasy anonimowej.


### Zadanie 5

Stwórz klasę `Timer`, która przyjmuje obiekt typu `Task` (klasa abstrakcyjna).
Zdefiniuj **klasę anonimową**, która przechowuje stan (licznik wywołań) i wypisuje go przy każdym uruchomieniu.

---

## Wyrażenia lambda

### Zadanie 6

Dana jest lista liczb całkowitych.
Użyj **wyrażenia lambda**, aby:

* wypisać tylko liczby parzyste

### Zadanie 7

Dana jest lista napisów.
Użyj lambdy jako `Comparator`, aby:

* posortować napisy malejąco według długości

### Zadanie 8

Zdefiniuj interfejs funkcyjny `Calculator` z metodą:

```java
int calculate(int a, int b);
```

Utwórz **dwie różne lambdy**:

* dodawanie
* mnożenie

### Zadanie 9

Dana jest lista obiektów `Person` (imię, wiek).
Użyj lambdy jako `Predicate<Person>`, aby:

* odfiltrować osoby pełnoletnie

### Zadanie 10

Dana jest metoda:

```java
void process(IntConsumer action)
```

Przekaż do niej lambdę, która:

* wypisuje kwadrat przekazanej liczby


## Referencje do metod

### Zadanie 11

Dana jest lista napisów.
Zastąp lambdę:

```java
s -> System.out.println(s)
```

**referencją do metody**.


### Zadanie 12

Dana jest klasa `MathUtils` z metodą statyczną:

```java
static int square(int x)
```

Użyj **referencji do metody statycznej** w strumieniu.

### Zadanie 13

Dana jest klasa `Person` z metodą:

```java
String getName()
```

Użyj **referencji do metody instancyjnej**, aby wypisać imiona wszystkich osób.


### Zadanie 14

Dana jest lista obiektów `Person`.
Użyj **referencji do konstruktora**, aby:

* utworzyć nową listę osób na podstawie listy imion


### Zadanie 15

Zamień istniejącą lambdę używaną jako `Comparator<Person>`
na **referencję do metody**, jeżeli sygnatura na to pozwala.


## Strumienie (Stream API)

### Zadanie 16

Dana jest lista liczb całkowitych.
Użyj strumienia, aby:

* usunąć liczby ujemne
* podnieść pozostałe do kwadratu
* wypisać wynik

### Zadanie 17

Dana jest lista napisów.
Użyj strumienia, aby:

* znaleźć pierwszy napis dłuższy niż 5 znaków

### Zadanie 18

Dana jest lista obiektów `Person`.
Użyj strumienia, aby:

* policzyć średni wiek osób


### Zadanie 19

Dana jest lista liczb.
Użyj strumienia, aby:

* sprawdzić, czy **wszystkie** liczby są dodatnie

### Zadanie 20

Dana jest lista napisów zawierająca duplikaty.
Użyj strumienia, aby:

* usunąć duplikaty
* posortować alfabetycznie
* zapisać wynik do nowej listy


