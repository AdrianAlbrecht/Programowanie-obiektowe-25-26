# **Programowanie Obiektowe – Java (2025Z)**

## **Lab 2: Wprowadzenie do Javy – Part II**

---

## 1. Metody statyczne

W języku **Java** słowo kluczowe `static` oznacza, że **element należy do klasy, a nie do konkretnego obiektu** tej klasy.
Oznacza to, że **metoda statyczna działa bez konieczności tworzenia obiektu**.

Innymi słowy:

> Metoda statyczna to metoda, która należy do klasy, a nie do jej instancji.

---

### Deklaracja metody statycznej

Składnia jest bardzo prosta:

```java
[modyfikator dostępu] static [typ_zwracany] nazwa_metody([parametry]) {
    // ciało metody
}
```

Przykład:

```java
public static void sayHello() {
    System.out.println("Cześć, świecie!");
}
```

---

### Jak wywołać metodę statyczną?

Metodę statyczną wywołujemy **bez tworzenia obiektu**, używając **nazwy klasy**:

```java
NazwaKlasy.nazwaMetody();
```

Przykład:

```java
public class Example {

    public static void sayHello() {
        System.out.println("Witaj w metodzie statycznej!");
    }

    public static void main(String[] args) {
        // wywołanie metody statycznej bez tworzenia obiektu
        Example.sayHello();
    }
}
```

**Wynik:**

```
Witaj w metodzie statycznej!
```

---

### Dlaczego metoda `main` jest statyczna?

Każdy program w Javie zaczyna się od metody:

```java
public static void main(String[] args)
```

Jest **statyczna**, ponieważ:

* program musi wystartować **bez tworzenia obiektu klasy**,
* Java uruchamia tę metodę **bezpośrednio przez nazwę klasy**.

---

### Metody statyczne w praktyce – przykłady

#### Przykład – metoda statyczna jako narzędzie pomocnicze

Często tworzymy tzw. **klasy pomocnicze (utility classes)** — zawierające tylko metody statyczne.

```java
public class StringHelper {

    // metoda sprawdzająca, czy tekst jest pusty
    public static boolean isEmpty(String text) {
        return text == null || text.trim().isEmpty();
    }

    public static void main(String[] args) {
        System.out.println(StringHelper.isEmpty(""));       // true
        System.out.println(StringHelper.isEmpty("Java"));   // false
    }
}
```

Takie klasy są użyteczne np. w bibliotekach (`Math`, `Arrays`, `Collections`).

---

#### Przykład – porównanie metod statycznych i niestatycznych

```java
public class Counter {

    // pole statyczne (wspólne dla wszystkich obiektów)
    static int count = 0;

    // metoda statyczna - operuje na danych klasy
    public static void increment() {
        count++;
    }

    // metoda niestatyczna - działa na obiekcie
    public void showCount() {
        System.out.println("Aktualna wartość: " + count);
    }

    public static void main(String[] args) {
        Counter.increment();
        Counter.increment();

        Counter c = new Counter();
        c.showCount(); // wywołanie metody niestatycznej
    }
}
```

**Wynik:**

```
Aktualna wartość: 2
```

**Wyjaśnienie:**

* `increment()` jest **statyczna**, więc można ją wywołać bez obiektu (`Counter.increment()`).
* `showCount()` jest **niestatyczna**, więc trzeba utworzyć obiekt (`new Counter()`).

---

### Ograniczenia metod statycznych

Metody statyczne **nie mają dostępu** do:

* pól **niestatycznych** klasy (czyli należących do obiektu),
* metod **niestatycznych** klasy,
* słowa kluczowego `this` (bo nie istnieje żaden obiekt).

Przykład błędu:

```java
public class Demo {

    int x = 10;

    public static void showX() {
        // ❌ Błąd! Nie można użyć 'x' w metodzie statycznej
        // System.out.println(x);
    }
}
```

> ❗ Kompilator zgłosi błąd:
> `non-static variable x cannot be referenced from a static context`

---

### Kiedy warto używać metod statycznych?

Gdy:

* metoda **nie zależy od stanu obiektu** (czyli nie używa pól niestatycznych),
* chcemy mieć **globalny punkt dostępu** (np. `Math`, `System.out`),
* piszemy **funkcję pomocniczą** (np. `parseInt`, `isEmpty`, `max`, `min`).

Unikaj metod statycznych, gdy:

* metoda ma działać na **konkretnych danych obiektu**,
* chcesz używać **dziedziczenia lub polimorfizmu** (statyczne tego nie wspierają).

---


## 2. Moduł "Math"

> `Math` to **klasa wbudowana w Javę**, która zawiera **statyczne metody i stałe matematyczne** — np. `Math.PI`, `Math.sqrt()`, `Math.pow()`.

Oznacza to, że nie trzeba jej importować — jest częścią pakietu `java.lang`, który ładuje się automatycznie.

---

### Jak korzystać?

Wszystkie metody są **statyczne**, więc wywołujemy je bez tworzenia obiektu:

```java
Math.nazwaMetody(argumenty);
```

Przykład:

```java
double pierwiastek = Math.sqrt(25);
System.out.println(pierwiastek); // 5.0
```

---

### Najważniejsze stałe w klasie `Math`

| Nazwa     | Wartość           | Opis                                     |
| --------- | ----------------- | ---------------------------------------- |
| `Math.PI` | 3.141592653589793 | Stała π                                  |
| `Math.E`  | 2.718281828459045 | Stała e (podstawa logarytmu naturalnego) |

Przykład:

```java
public class StaleMath {
    public static void main(String[] args) {
        System.out.println("PI = " + Math.PI);
        System.out.println("E  = " + Math.E);
    }
}
```
Wynik:

```
PI = 3.141592653589793
E  = 2.718281828459045
```

---

### Podstawowe operacje matematyczne

#### a) Pierwiastki

```java
double wynik = Math.sqrt(16); // pierwiastek kwadratowy
System.out.println(wynik); // 4.0
```

---

#### b) Potęgowanie

```java
double potega = Math.pow(2, 3); // 2^3 = 8
System.out.println(potega);
```

---

#### c) Wartość bezwzględna

```java
int wartosc = Math.abs(-10);
System.out.println(wartosc); // 10
```

---

#### d) Zaokrąglanie

| Metoda          | Działanie                                  | Przykład                |
| --------------- | ------------------------------------------ | ----------------------- |
| `Math.round(x)` | Zaokrągla do najbliższej liczby całkowitej | `Math.round(3.6)` → `4` |
| `Math.floor(x)` | Zaokrągla **w dół**                        | `Math.floor(3.9)` → `3` |
| `Math.ceil(x)`  | Zaokrągla **w górę**                       | `Math.ceil(3.1)` → `4`  |

Przykład:

```java
System.out.println(Math.round(3.6)); // 4
System.out.println(Math.floor(3.6)); // 3
System.out.println(Math.ceil(3.6));  // 4
```

---

### Funkcje trygonometryczne

Klasa `Math` obsługuje wszystkie podstawowe funkcje trygonometryczne,
które przyjmują argumenty w **radianach** (nie w stopniach!).

| Metoda         | Opis          |
| -------------- | ------------- |
| `Math.sin(x)`  | Sinus         |
| `Math.cos(x)`  | Cosinus       |
| `Math.tan(x)`  | Tangens       |
| `Math.asin(x)` | Arcus sinus   |
| `Math.acos(x)` | Arcus cosinus |
| `Math.atan(x)` | Arcus tangens |

Przykład:

```java
public class Trygonometria {
    public static void main(String[] args) {
        double kat = Math.toRadians(30); // zamiana stopni na radiany
        double sin = Math.sin(kat);
        double cos = Math.cos(kat);

        System.out.println("sin(30°) = " + sin);
        System.out.println("cos(30°) = " + cos);
    }
}
```

Wynik:

```
sin(30°) = 0.49999999999999994
cos(30°) = 0.8660254037844387
```

---

### Konwersja między stopniami a radianami

```java
double radiany = Math.toRadians(180); // 3.14159...
double stopnie = Math.toDegrees(Math.PI); // 180.0

System.out.println("180° = " + radiany + " rad");
System.out.println("π rad = " + stopnie + "°");
```

Wynik:

```
180° = 3.141592653589793 rad
π rad = 180.0°
```

---

### Logarytmy i eksponenty

| Metoda          | Opis                    | Przykład                   |
| --------------- | ----------------------- | -------------------------- |
| `Math.log(x)`   | logarytm naturalny (ln) | `Math.log(Math.E)` → `1.0` |
| `Math.log10(x)` | logarytm dziesiętny     | `Math.log10(100)` → `2.0`  |
| `Math.exp(x)`   | e^x                     | `Math.exp(1)` → `2.71828`  |

Przykład:

```java
System.out.println("ln(e) = " + Math.log(Math.E));
System.out.println("log10(1000) = " + Math.log10(1000));
System.out.println("e^2 = " + Math.exp(2));
```

Wynik:

```
ln(e) = 1.0
log10(1000) = 3.0
e^2 = 7.38905609893065
```

---

### Losowe liczby

`Math.random()` zwraca **liczbę losową z zakresu [0.0, 1.0)**.

Przykład:

```java
double los = Math.random();
System.out.println(los);
```

Wynik (np.):

```
0.483729104771
```

#### Liczby losowe w konkretnym zakresie

Aby uzyskać liczbę z przedziału np. 1–10:

```java
int losowa = (int) (Math.random() * 10) + 1;
System.out.println(losowa);
```

Wynik:

```
7
```

---

### Funkcje pomocnicze

| Metoda             | Opis                          | Przykład                   |
| ------------------ | ----------------------------- | -------------------------- |
| `Math.max(a, b)`   | Zwraca większą wartość        | `Math.max(10, 20)` → `20`  |
| `Math.min(a, b)`   | Zwraca mniejszą wartość       | `Math.min(10, 20)` → `10`  |
| `Math.signum(x)`   | Zwraca znak liczby (-1, 0, 1) | `Math.signum(-5)` → `-1`   |
| `Math.cbrt(x)`     | Pierwiastek sześcienny        | `Math.cbrt(27)` → `3.0`    |
| `Math.hypot(a, b)` | Oblicza √(a² + b²)            | `Math.hypot(3, 4)` → `5.0` |

Przykład:

```java
System.out.println(Math.max(7, 12));     // 12
System.out.println(Math.min(-5, 3));     // -5
System.out.println(Math.hypot(3, 4));    // 5.0
System.out.println(Math.signum(-8));     // -1.0
```

---

### Przykład – generator liczb losowych i statystyka

```java
public class Losowanie {
    public static void main(String[] args) {
        int suma = 0;

        for (int i = 0; i < 10; i++) {
            int liczba = (int) (Math.random() * 100) + 1;
            System.out.println("Liczba " + (i + 1) + ": " + liczba);
            suma += liczba;
        }

        double srednia = (double) suma / 10;
        System.out.println("Średnia: " + Math.round(srednia));
    }
}
```

---

## 3. Operatory logiczne

Używamy ich do pracy z wartościami logicznymi (`boolean`), najczęściej w instrukcjach warunkowych (`if`, `while`).

| Operator        | Znaczenie     | Przykład          | Wynik    |
|-----------------|---------------|-------------------|----------|
| `&&`            | AND (i)       | `true && false`   | `false`  |
| `        \|\| ` | OR (lub)      | `true \|\| false` | `true`   |
| `!`             | NOT (negacja) | `!true`           | `false`  |

**Ważne:**

* `&&` zwraca `true` tylko wtedy, gdy **oba warunki** są prawdziwe.
* `||` zwraca `true`, jeśli **choć jeden** warunek jest prawdziwy.
* `!` odwraca wartość logiczną.

---

#### Przykład – operatory logiczne

```java
public class OperatoryLogiczne {
    public static void main(String[] args) {
        boolean a = true;
        boolean b = false;

        System.out.println("a && b: " + (a && b)); // false
        System.out.println("a || b: " + (a || b)); // true
        System.out.println("!a: " + (!a));         // false
        System.out.println("!b: " + (!b));         // true

        // Przykład praktyczny:
        int wiek = 20;
        boolean maPrawoJazdy = true;

        if (wiek >= 18 && maPrawoJazdy) {
            System.out.println("Możesz prowadzić samochód.");
        } else {
            System.out.println("Nie możesz prowadzić samochodu.");
        }
    }
}
```

---


## 4. Metody o zmiennej liczbie argumentów

Zwykle, gdy tworzymy metodę, musimy określić dokładną liczbę parametrów.
Na przykład:

```java
void pokaz(int a, int b) { 
    // ciało metody 
}
```

Ta metoda **zawsze** wymaga dokładnie dwóch argumentów.
Ale co, jeśli chcemy przyjąć np. **dowolną liczbę liczb** — np. 2, 3 lub 10?
Do tego właśnie służy **varargs** (*variable arguments*).

---

### Składnia metody varargs

Deklaracja wygląda tak:

```java
typ_zwracany nazwaMetody(typ... nazwaParametru)
```

Trzy kropki (`...`) oznaczają, że metoda może przyjąć **zero lub więcej** argumentów danego typu.

---

#### Przykład — metoda przyjmująca dowolną liczbę liczb całkowitych:

```java
public class MetodyVarargs {
    // Metoda przyjmuje dowolną liczbę argumentów typu int
    static void wyswietlLiczby(int... liczby) {
        System.out.println("Liczba argumentów: " + liczby.length);

        for (int liczba : liczby) {
            System.out.println("Argument: " + liczba);
        }
    }

    public static void main(String[] args) {
        wyswietlLiczby();             // bez argumentów
        wyswietlLiczby(5);            // jeden argument
        wyswietlLiczby(10, 20, 30);   // trzy argumenty
    }
}
```

**Co się tu dzieje:**

* `int... liczby` oznacza, że w środku metody `liczby` traktowane są jak **tablica typu `int[]`**.
* Możesz więc korzystać z nich jak z tablicy — np. pętlą `for`.

---

### Jak działa mechanizm varargs w Javie?

Pod spodem, kompilator zamienia wywołanie:

```java
wyswietlLiczby(1, 2, 3);
```

na:

```java
wyswietlLiczby(new int[]{1, 2, 3});
```

Czyli `int...` to tak naprawdę **skrótowa forma przekazania tablicy**.

---

### Ograniczenia varargs

1. **Tylko jeden parametr varargs na metodę.**
   Java nie pozwala mieć dwóch parametrów `...` w jednej metodzie.

   Niedozwolone:

   ```java
   void metoda(int... a, double... b) { } // błąd kompilacji
   ```

2. **Parametr varargs musi być na końcu listy argumentów.**

   Poprawne:

   ```java
   void test(String nazwa, int... liczby) { }
   ```

   Niepoprawne:

   ```java
   void test(int... liczby, String nazwa) { } // błąd!
   ```

---

## 5. Przeciążanie metod

W języku **Java** można w jednej klasie utworzyć **wiele metod o tej samej nazwie**, pod warunkiem, że **różnią się listą parametrów**.
To właśnie nazywamy **przeciążaniem metod (method overloading)**.

Dzięki temu możemy wywoływać **tę samą metodę z różnymi zestawami argumentów**, a kompilator **sam wybierze właściwą wersję** w zależności od tego, jakich argumentów użyto.


### Zasady przeciążania metod

Metody można przeciążać, jeśli różnią się:

1. liczbą parametrów,
2. typami parametrów,
3. kolejnością typów parametrów.

Natomiast **nie można** przeciążać metod tylko poprzez:

* zmianę typu zwracanego (return type),
* zmianę nazw parametrów (ich nazw, nie typów!).

---

### Przykład 1 – różna liczba parametrów

```java
public class Calculator {

    // Metoda dodająca dwa liczby całkowite
    public int add(int a, int b) {
        return a + b;
    }

    // Przeciążona metoda dodająca trzy liczby całkowite
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    public static void main(String[] args) {
        Calculator calc = new Calculator();

        System.out.println("Suma dwóch liczb: " + calc.add(2, 3));      // wywoła pierwszą metodę
        System.out.println("Suma trzech liczb: " + calc.add(2, 3, 4));  // wywoła przeciążoną metodę
    }
}
```

Wyjaśnienie:

* Obie metody mają **taką samą nazwę (`add`)**, ale różnią się **liczbą parametrów**.
* Kompilator **automatycznie wybiera** tę, która pasuje do liczby argumentów podanych w wywołaniu.

---

### Przykład 2 – różne typy parametrów

```java
public class Display {

    public void show(int number) {
        System.out.println("Wyświetlam liczbę całkowitą: " + number);
    }

    public void show(String text) {
        System.out.println("Wyświetlam tekst: " + text);
    }

    public static void main(String[] args) {
        Display d = new Display();

        d.show(10);          // wywoła wersję z int
        d.show("Hello!");    // wywoła wersję z String
    }
}
```

Wyjaśnienie:

Tutaj metoda `show()` została przeciążona poprzez zmianę **typu parametru** — raz przyjmuje `int`, a raz `String`.

---

### Przykład 3 – różna kolejność typów parametrów

```java
public class Printer {

    public void print(String text, int copies) {
        System.out.println("Drukuję " + copies + " kopii tekstu: " + text);
    }

    public void print(int copies, String text) {
        System.out.println("Drukuję " + copies + " kopii tekstu (inna kolejność): " + text);
    }

    public static void main(String[] args) {
        Printer p = new Printer();

        p.print("Raport", 3);
        p.print(5, "Zestawienie");
    }
}
```

Wyjaśnienie:

Metody mają **takie same typy parametrów (`String`, `int`)**, ale w **odwrotnej kolejności**, więc są traktowane jako różne metody.

---

### Czego NIE można zrobić

```java
public class Test {

    // Pierwsza metoda
    public int multiply(int a, int b) {
        return a * b;
    }

    // Błąd! Różni się tylko typem zwracanym!
    public double multiply(int a, int b) {
        return (double) (a * b);
    }
}
```

> ❌ Kompilator zgłosi błąd:
> **"method multiply(int,int) is already defined"**

Metody różniące się **tylko typem zwracanym** nie mogą współistnieć, ponieważ kompilator **nie potrafiłby odróżnić**, której z nich użyć.

---

### Przykład 4 – przeciążanie + typy zmiennoprzecinkowe

```java
public class AreaCalculator {

    // Pole kwadratu
    public double area(double side) {
        return side * side;
    }

    // Pole prostokąta
    public double area(double width, double height) {
        return width * height;
    }

    // Pole koła
    public double area(float radius) {
        return Math.PI * radius * radius;
    }

    public static void main(String[] args) {
        AreaCalculator ac = new AreaCalculator();

        System.out.println("Pole kwadratu: " + ac.area(4.0));
        System.out.println("Pole prostokąta: " + ac.area(4.0, 5.0));
        System.out.println("Pole koła: " + ac.area(3.5f));
    }
}
```

---

### Dlaczego przeciążanie metod jest użyteczne?

* **Ułatwia czytelność kodu** – nie trzeba wymyślać wielu nazw jak `addInt()`, `addDouble()`, `addThreeNumbers()`.
* **Ułatwia ponowne wykorzystanie kodu** – jedna nazwa metody obsługuje różne przypadki.
* **Pomaga przy pisaniu bibliotek i API**, gdzie ta sama operacja może działać dla różnych typów danych.

---

### Dodatkowa ciekawostka – przeciążanie metod statycznych

Tak, metody **statyczne** również można przeciążać:

```java
public class MathUtils {

    public static int square(int x) {
        return x * x;
    }

    public static double square(double x) {
        return x * x;
    }

    public static void main(String[] args) {
        System.out.println(MathUtils.square(4));     // int
        System.out.println(MathUtils.square(4.5));   // double
    }
}
```

---

## 6. Liczby pseudolosowe

Java (i większość języków programowania) nie generuje naprawdę losowych liczb — tylko **pseudolosowe**.
To oznacza, że:
- liczby wyglądają na losowe,
- ale są tworzone przez **algorytm matematyczny**,
- który startuje od tzw. **ziarna (seed)** – czyli liczby początkowej.

Jeśli dwa generatory wystartują z tym samym **seedem**, wygenerują **identyczny ciąg liczb**.


### Sposoby generowania liczb pseudolosowych w Javie

Java oferuje kilka sposobów:

1. Klasa `java.util.Random`
2. Metody `Math.random()`
3. Klasa `java.util.concurrent.ThreadLocalRandom` (do programów wielowątkowych)
4. Klasa `java.security.SecureRandom` (do zastosowań kryptograficznych)

### Klasa `java.util.Random`

Klasa `Random` daje **więcej możliwości** i pozwala generować różne typy liczb.

```java
import java.util.Random;

public class RandomDemo2 {
    public static void main(String[] args) {
        Random rand = new Random();

        System.out.println("Losowy int: " + rand.nextInt());         // dowolna liczba całkowita
        System.out.println("Losowy int 0–99: " + rand.nextInt(100)); // <0;99>
        System.out.println("Losowy double: " + rand.nextDouble(2.0));   // (0.0;2.0)
        System.out.println("Losowy boolean: " + rand.nextBoolean()); // true / false
    }
}
```

**Przykładowy wynik:**

```
Losowy int: -174837451
Losowy int 0–99: 56
Losowy double: 0.3274623145748931
Losowy boolean: true
```

---

### Ustawianie ziarna (`seed`)

Jak wspomnieliśmy, jeśli generator ma ten sam `seed`, wygeneruje **identyczną sekwencję** liczb.

```java
import java.util.Random;

public class RandomSeed {
    public static void main(String[] args) {
        Random rand1 = new Random(123);
        Random rand2 = new Random(123);

        System.out.println(rand1.nextInt(100)); // 45
        System.out.println(rand2.nextInt(100)); // 45
    }
}
```

Obie liczby będą **identyczne**, ponieważ ziarno (`123`) było takie samo.
To przydatne np. w testach — gdy chcesz uzyskać powtarzalne wyniki.

---

## 8. Tablice jednowymiarowe

**Tablica** to **zbiór elementów tego samego typu**, przechowywanych **pod jednym wspólnym identyfikatorem**.
Każdy element ma **numer (indeks)**, który pozwala go odczytać lub zmienić.

> W Javie indeksowanie zaczyna się od **0**!

Przykład:
Tablica 5 liczb wygląda w pamięci tak:

| Indeks  | 0  | 1  | 2  | 3  | 4  |
| :------ | :- | :- | :- | :- | :- |
| Wartość | 10 | 20 | 30 | 40 | 50 |

---

### Deklaracja tablicy

W Javie możemy zadeklarować tablicę na dwa sposoby:

```java
int[] numbers;   // preferowany zapis
// lub
int numbers[];   // też poprawny, ale rzadziej używany
```

Na tym etapie **tablica jeszcze nie istnieje** — jest tylko zmienna, która *może wskazywać* na tablicę.

---

### Tworzenie tablicy w pamięci

Używamy słowa kluczowego `new`:

```java
int[] numbers = new int[5];
```

Tworzy tablicę 5-elementową (indeksy: 0–4).
Każdy element ma **wartość domyślną**:

| Typ danych                         | Wartość domyślna            |
| ---------------------------------- | --------------------------- |
| `int`, `byte`, `short`, `long`     | 0                           |
| `double`, `float`                  | 0.0                         |
| `boolean`                          | false                       |
| `char`                             | '\u0000' (czyli pusty znak) |
| obiekty (`String`, `Integer` itd.) | null                        |

---

### Przypisywanie wartości

```java
public class ArrayExample1 {
    public static void main(String[] args) {
        int[] numbers = new int[5];

        numbers[0] = 10;
        numbers[1] = 20;
        numbers[2] = 30;
        numbers[3] = 40;
        numbers[4] = 50;

        System.out.println("Pierwszy element: " + numbers[0]);
        System.out.println("Ostatni element: " + numbers[4]);
    }
}
```

**Wynik:**

```
Pierwszy element: 10
Ostatni element: 50
```

> Uwaga: próba dostępu do indeksu spoza zakresu (`numbers[5]`) spowoduje błąd:
> `ArrayIndexOutOfBoundsException`.

---

### Inicjalizacja tablicy przy deklaracji

Jeśli znamy od razu wartości, możemy skrócić zapis:

```java
public class ArrayExample2 {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30, 40, 50};

        System.out.println("Trzeci element: " + numbers[2]);
    }
}
```

**Wynik:**

```
Trzeci element: 30
```

---

### Iteracja po tablicy (pętle)

Najczęściej używamy **pętli `for`**:

```java
public class ArrayExample3 {
    public static void main(String[] args) {
        int[] numbers = {5, 10, 15, 20, 25};

        for (int i = 0; i < numbers.length; i++) {
            System.out.println("Element " + i + ": " + numbers[i]);
        }
    }
}
```

**Wynik:**

```
Element 0: 5
Element 1: 10
Element 2: 15
Element 3: 20
Element 4: 25
```

> `numbers.length` — to właściwość mówiąca o długości tablicy (liczbie elementów).

---

### Pętla for-each (ulepszona pętla for)

Jeśli nie potrzebujemy indeksu, można użyć krótszej wersji:

```java
public class ArrayExample4 {
    public static void main(String[] args) {
        String[] fruits = {"Jabłko", "Banan", "Gruszka"};

        for (String fruit : fruits) {
            System.out.println("Owoc: " + fruit);
        }
    }
}
```

**Wynik:**

```
Owoc: Jabłko
Owoc: Banan
Owoc: Gruszka
```

---

### Dostęp do długości tablicy

Tablica **ma stałą długość** — po utworzeniu nie można jej zmienić.
Używamy właściwości `.length`:

```java
int[] arr = {1, 2, 3, 4};
System.out.println("Długość tablicy: " + arr.length);
```

Wynik:

```
Długość tablicy: 4
```

---

### Najczęstsze błędy

| Błąd                              | Przykład                             | Skutek                           |
| --------------------------------- | ------------------------------------ | -------------------------------- |
| Dostęp do nieistniejącego indeksu | `numbers[5]` w tablicy 5-elementowej | `ArrayIndexOutOfBoundsException` |
| Niezainicjalizowana tablica       | `int[] arr; arr[0] = 10;`            | `NullPointerException`           |
| Próba zmiany długości tablicy     | brak możliwości                      | trzeba stworzyć nową tablicę     |

---

### Przykład praktyczny – średnia ocen

```java
import java.util.Scanner;

public class GradesAverage {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        double[] grades = new double[5];
        double sum = 0;

        for (int i = 0; i < grades.length; i++) {
            System.out.print("Podaj ocenę " + (i + 1) + ": ");
            grades[i] = sc.nextDouble();
            sum += grades[i];
        }

        double average = sum / grades.length;
        System.out.println("Średnia ocen: " + average);
    }
}
```

---

### Metody “systemowe” do obsługi tablic

#### Sortowanie tablicy — `Arrays.sort()`

```java
import java.util.Arrays;

public class ArraysSortExample {
    public static void main(String[] args) {
        int[] numbers = {42, 7, 19, 3, 25};
        Arrays.sort(numbers);
        System.out.println("Posortowana tablica: " + Arrays.toString(numbers));
    }
}
```

**Wynik:**

```
Posortowana tablica: [3, 7, 19, 25, 42]
```

> Dla typów prostych (`int`, `double`, `char`, itp.) używa algorytmu **Dual-Pivot QuickSort**,
> a dla obiektów — **TimSort** (mieszanka MergeSort i InsertionSort).

---

#### Szukanie elementu — `Arrays.binarySearch()`

Działa **tylko na posortowanej tablicy**!

```java
import java.util.Arrays;

public class ArraysBinarySearchExample {
    public static void main(String[] args) {
        int[] numbers = {3, 7, 19, 25, 42};
        int index = Arrays.binarySearch(numbers, 25);

        if (index >= 0)
            System.out.println("Element 25 znaleziony na pozycji: " + index);
        else
            System.out.println("Element nie istnieje w tablicy.");
    }
}
```

**Wynik:**

```
Element 25 znaleziony na pozycji: 3
```

> Jeśli elementu nie ma, metoda zwraca **ujemny indeks** (pozycję, gdzie można by go wstawić).

---

#### Porównywanie tablic — `Arrays.equals()`

Zwykłe `==` porównuje **adresy pamięci**, a nie zawartość!
Dlatego do porównania zawartości tablic używamy:

```java
import java.util.Arrays;

public class ArraysEqualsExample {
    public static void main(String[] args) {
        int[] a = {1, 2, 3};
        int[] b = {1, 2, 3};
        int[] c = {1, 3, 2};

        System.out.println(Arrays.equals(a, b)); // true
        System.out.println(Arrays.equals(a, c)); // false
    }
}
```

**Wynik:**

```
true
false
```

---

#### Wypełnianie tablicy — `Arrays.fill()`

```java
import java.util.Arrays;

public class ArraysFillExample {
    public static void main(String[] args) {
        int[] scores = new int[5];
        Arrays.fill(scores, 10); // wszystkie elementy = 10

        System.out.println(Arrays.toString(scores));
    }
}
```

**Wynik:**

```
[10, 10, 10, 10, 10]
```

> Przydatne np. do inicjalizacji wartości startowych lub „zerowania” danych.

---

#### Kopiowanie tablicy — `Arrays.copyOf()` i `Arrays.copyOfRange()`

```java
import java.util.Arrays;

public class ArraysCopyExample {
    public static void main(String[] args) {
        int[] original = {1, 2, 3, 4, 5};

        int[] copy = Arrays.copyOf(original, original.length);
        int[] partial = Arrays.copyOfRange(original, 1, 4); // indeksy 1..3

        System.out.println("Kopia: " + Arrays.toString(copy));
        System.out.println("Fragment: " + Arrays.toString(partial));
    }
}
```

**Wynik:**

```
Kopia: [1, 2, 3, 4, 5]
Fragment: [2, 3, 4]
```

---

#### Porównywanie głębokie (dla tablic zagnieżdżonych) — `Arrays.deepEquals()`

Działa na **tablicach tablic** (np. `int[][]`):

```java
import java.util.Arrays;

public class ArraysDeepEqualsExample {
    public static void main(String[] args) {
        int[][] a = {{1, 2}, {3, 4}};
        int[][] b = {{1, 2}, {3, 4}};
        int[][] c = {{1, 3}, {2, 4}};

        System.out.println(Arrays.deepEquals(a, b)); // true
        System.out.println(Arrays.deepEquals(a, c)); // false
    }
}
```

---

#### Konwersja do listy — `Arrays.asList()`

Czasem potrzebujemy zamienić tablicę na listę (np. by używać kolekcji):

```java
import java.util.Arrays;
import java.util.List;

public class ArraysAsListExample {
    public static void main(String[] args) {
        String[] fruits = {"Jabłko", "Banan", "Gruszka"};
        List<String> list = Arrays.asList(fruits);

        System.out.println(list);
    }
}
```

**Wynik:**

```
[Jabłko, Banan, Gruszka]
```

>  Uwaga: ta lista jest **stałej długości** — nie można dodawać/usuwać elementów.

---

#### Przykład praktyczny – połączenie wszystkiego

```java
import java.util.Arrays;

public class ArraysAllInOne {
    public static void main(String[] args) {
        int[] numbers = {9, 3, 7, 1, 5};

        System.out.println("Oryginalna tablica: " + Arrays.toString(numbers));

        Arrays.sort(numbers);
        System.out.println("Posortowana: " + Arrays.toString(numbers));

        int index = Arrays.binarySearch(numbers, 5);
        System.out.println("Indeks liczby 5: " + index);

        int[] copy = Arrays.copyOf(numbers, numbers.length);
        System.out.println("Kopia: " + Arrays.toString(copy));

        Arrays.fill(copy, 0);
        System.out.println("Po wypełnieniu zerami: " + Arrays.toString(copy));
    }
}
```

---

## Lista Tablicowa `ArrayList`

W języku Java klasa **`ArrayList`** należy do pakietu **`java.util`** i jest **dynamiczną listą tablicową**.
W przeciwieństwie do zwykłej tablicy (`int[]` lub `String[]`), która ma **stały rozmiar**, `ArrayList` może się **automatycznie rozszerzać i kurczyć** w zależności od liczby elementów.

 **Podsumowanie różnic:**

| Cecha               | Tablica (`[]`)                    | ArrayList                                 |
| ------------------- | --------------------------------- | ----------------------------------------- |
| Rozmiar             | Stały                             | Dynamiczny                                |
| Typ danych          | Może być prymitywny lub obiektowy | Tylko obiektowy (np. `Integer`, `String`) |
| Dodawanie elementów | Trudne (tworzenie nowej tablicy)  | Proste – metoda `add()`                   |
| Iteracja            | Pętla `for`                       | Pętla `for-each` lub `forEach()`          |
| Dostęp do elementu  | `tab[i]`                          | `list.get(i)`                             |

---

### Jak używać `ArrayList`?

Najpierw trzeba **zaimportować klasę**:

```java
import java.util.ArrayList;
```

Następnie można **utworzyć listę**:

```java
ArrayList<String> fruits = new ArrayList<>();
```

Zauważ `<String>` — to **typ generyczny**, czyli określamy, jakie elementy przechowuje lista.

#### Najważniejsze metody `ArrayList`

| Metoda                      | Opis                                       |
| --------------------------- | ------------------------------------------ |
| `add(E element)`            | Dodaje element do końca listy              |
| `add(int index, E element)` | Wstawia element w określone miejsce        |
| `get(int index)`            | Pobiera element                            |
| `set(int index, E element)` | Nadpisuje element                          |
| `remove(Object o)`          | Usuwa pierwszy wystąpienie danego elementu |
| `remove(int index)`         | Usuwa element o danym indeksie             |
| `clear()`                   | Czyści całą listę                          |
| `size()`                    | Zwraca liczbę elementów                    |
| `contains(Object o)`        | Sprawdza, czy element istnieje             |
| `isEmpty()`                 | Sprawdza, czy lista jest pusta             |

---

---

#### `ArrayList` z liczbami i pętlami

> W `ArrayList` nie można przechowywać prymitywnych typów (`int`, `double`, itp.),
> ale można użyć ich **obiektowych odpowiedników**: `Integer`, `Double`, `Character`, itd.

```java
import java.util.ArrayList;
import java.util.Random;

public class NumberListExample {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<>();
        Random rand = new Random();

        // Dodajemy 5 losowych liczb
        for (int i = 0; i < 5; i++) {
            numbers.add(rand.nextInt(100)); // liczby od 0 do 99
        }

        System.out.println("Wylosowane liczby: " + numbers);

        // Obliczanie sumy
        int sum = 0;
        for (int n : numbers) {
            sum += n;
        }

        System.out.println("Suma liczb: " + sum);
    }
}
```

---

#### Przykład — zarządzanie listą studentów

```java
import java.util.ArrayList;
import java.util.Scanner;

public class StudentList {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ArrayList<String> students = new ArrayList<>();

        System.out.println("Dodaj 3 studentów do listy:");
        for (int i = 0; i < 3; i++) {
            System.out.print("Student " + (i + 1) + ": ");
            String name = scanner.nextLine();
            students.add(name);
        }

        System.out.println("\nLista studentów: " + students);

        System.out.print("Podaj imię studenta do usunięcia: ");
        String removeName = scanner.nextLine();

        if (students.remove(removeName)) {
            System.out.println(removeName + " został usunięty.");
        } else {
            System.out.println(removeName + " nie znaleziono na liście.");
        }

        System.out.println("Aktualna lista: " + students);
    }
}
```

---

## String, StringBuilder i StringBuffer

`String` w Javie reprezentuje **ciąg znaków**.
Ważna cecha: **`String` jest niemodyfikowalny (immutable)**.
Oznacza to, że każda operacja „modyfikująca” tak naprawdę tworzy **nowy obiekt**.

### Przykład:

```java
public class StringExample {
    public static void main(String[] args) {
        String text = "Hello";
        System.out.println("Pierwotny tekst: " + text);

        // Próba zmiany
        text.concat(" World"); // nie przypisujemy wyniku
        System.out.println("Po concat (bez przypisania): " + text);

        // Poprawny sposób
        text = text.concat(" World");
        System.out.println("Po concat (z przypisaniem): " + text);
    }
}
```

**Wynik:**

```
Pierwotny tekst: Hello
Po concat (bez przypisania): Hello
Po concat (z przypisaniem): Hello World
```

> 💡 Każda „modyfikacja” `String` tworzy nowy obiekt.
> Dla wielu operacji tekstowych może to być **nieefektywne**.

---

#### Operacje na `String`

| Metoda                                                   | Co robi                                               | Parametry                | Zwraca        | Przykład                                                 |
| -------------------------------------------------------- | ----------------------------------------------------- | ------------------------ | ------------- | -------------------------------------------------------- |
| `length()`                                               | Zwraca długość ciągu znaków                           | brak                     | `int`         | `"Java".length()` → 4                                    |
| `charAt(int index)`                                      | Zwraca znak znajdujący się na podanym indeksie        | `index` (0-based)        | `char`        | `"Java".charAt(1)` → `'a'`                               |
| `concat(String str)`                                     | Łączy (dokleja) podany tekst do końca                 | `str`                    | nowy `String` | `"Hello".concat(" World")` → `"Hello World"`             |
| `substring(int beginIndex)`                              | Zwraca podciąg od podanego indeksu do końca           | `beginIndex`             | nowy `String` | `"Hello".substring(2)` → `"llo"`                         |
| `substring(int beginIndex, int endIndex)`                | Zwraca podciąg od `beginIndex` do `endIndex-1`        | `beginIndex`, `endIndex` | nowy `String` | `"Hello".substring(1,4)` → `"ell"`                       |
| `contains(CharSequence s)`                               | Sprawdza, czy ciąg zawiera podany fragment            | `s`                      | `boolean`     | `"Java".contains("av")` → `true`                         |
| `equals(Object obj)`                                     | Porównuje zawartość dwóch Stringów                    | `obj`                    | `boolean`     | `"Java".equals("java")` → `false`                        |
| `equalsIgnoreCase(String anotherString)`                 | Porównuje zawartość ignorując wielkość liter          | `anotherString`          | `boolean`     | `"Java".equalsIgnoreCase("java")` → `true`               |
| `isEmpty()`                                              | Sprawdza, czy String jest pusty (`""`)                | brak                     | `boolean`     | `"".isEmpty()` → `true`                                  |
| `indexOf(int ch)`                                        | Zwraca indeks pierwszego wystąpienia znaku            | `ch` (char/int)          | `int`         | `"Java".indexOf('a')` → 1                                |
| `indexOf(String str)`                                    | Zwraca indeks pierwszego wystąpienia podciągu         | `str`                    | `int`         | `"Hello Java".indexOf("Java")` → 6                       |
| `lastIndexOf(int ch)`                                    | Zwraca indeks ostatniego wystąpienia znaku            | `ch`                     | `int`         | `"banana".lastIndexOf('a')` → 5                          |
| `lastIndexOf(String str)`                                | Zwraca indeks ostatniego wystąpienia podciągu         | `str`                    | `int`         | `"Java Java".lastIndexOf("Java")` → 5                    |
| `replace(char oldChar, char newChar)`                    | Zamienia wszystkie wystąpienia znaku                  | `oldChar`, `newChar`     | nowy `String` | `"Java".replace('a','o')` → `"Jovo"`                     |
| `replace(CharSequence target, CharSequence replacement)` | Zamienia wszystkie wystąpienia podciągu               | `target`, `replacement`  | nowy `String` | `"Java is fun".replace("fun","cool")` → `"Java is cool"` |
| `trim()`                                                 | Usuwa białe znaki z początku i końca Stringa          | brak                     | nowy `String` | `"  hello  ".trim()` → `"hello"`                         |
| `toUpperCase()`                                          | Zamienia wszystkie litery na wielkie                  | brak                     | nowy `String` | `"java".toUpperCase()` → `"JAVA"`                        |
| `toLowerCase()`                                          | Zamienia wszystkie litery na małe                     | brak                     | nowy `String` | `"JAVA".toLowerCase()` → `"java"`                        |
| `startsWith(String prefix)`                              | Sprawdza, czy String zaczyna się od podanego prefiksu | `prefix`                 | `boolean`     | `"Java".startsWith("Ja")` → `true`                       |
| `endsWith(String suffix)`                                | Sprawdza, czy String kończy się podanym sufiksem      | `suffix`                 | `boolean`     | `"Java".endsWith("va")` → `true`                         |
| `split(String regex)`                                    | Dzieli String na tablicę według wyrażenia regularnego | `regex`                  | `String[]`    | `"a,b,c".split(",")` → `["a","b","c"]`                   |
| `toCharArray()`                                          | Zwraca tablicę znaków                                 | brak                     | `char[]`      | `"Java".toCharArray()` → `['J','a','v','a']`             |
| `matches(String regex)`                                  | Sprawdza, czy String pasuje do wyrażenia regularnego  | `regex`                  | `boolean`     | `"12345".matches("\\d+")` → `true`                       |
| `repeat(int count)`                                      | Powtarza String `count` razy                          | `count`                  | nowy `String` | `"ha".repeat(3)` → `"hahaha"`                            |
| `compareTo(String anotherString)`                        | Porównuje dwa Stringi leksykograficznie               | `anotherString`          | `int`         | `"Java".compareTo("Python")` → ujemna liczba             |
| `compareToIgnoreCase(String str)`                        | Porównuje dwa Stringi ignorując wielkość liter        | `str`                    | `int`         | `"Java".compareToIgnoreCase("java")` → 0                 |


```java
String s = "Java";

System.out.println("Długość: " + s.length());        // 4
System.out.println("Znak na pozycji 1: " + s.charAt(1)); // 'a'
System.out.println("Czy zawiera 'av'? " + s.contains("av")); // true
System.out.println("Zamiana: " + s.replace("J", "j")); // 'java'
System.out.println("Duże litery: " + s.toUpperCase()); // 'JAVA'
```

---

### `StringBuffer` – modyfikowalny tekst (synchronizowany)

`StringBuffer` pozwala **modyfikować tekst bez tworzenia nowego obiektu**.
 Wątkowo bezpieczny (synchronizowany) → można używać w aplikacjach wielowątkowych.

#### Tworzenie i modyfikacja:

```java
public class StringBufferExample {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer("Hello");

        sb.append(" World"); // dodawanie na końcu
        System.out.println("Po append: " + sb);

        sb.insert(5, ","); // wstawienie przecinka na pozycji 5
        System.out.println("Po insert: " + sb);

        sb.replace(0, 5, "Hi"); // zamiana od indeksu 0 do 4
        System.out.println("Po replace: " + sb);

        sb.delete(2, 7); // usunięcie fragmentu od 2 do 6
        System.out.println("Po delete: " + sb);

        sb.reverse(); // odwrócenie zawartości
        System.out.println("Po reverse: " + sb);
    }
}
```

**Wynik przykładowy:**

```
Po append: Hello World
Po insert: Hello, World
Po replace: Hi, World
Po delete: Hiorld
Po reverse: dlro iH
```

---

### `StringBuilder` – szybka alternatywa

`StringBuilder` jest **niemal identyczny jak `StringBuffer`**, ale:

* **nie jest synchronizowany** → szybszy w aplikacjach jednowątkowych.
* Posiada te same metody (`append`, `insert`, `delete`, `replace`, `reverse`).

##### Przykład:

```java
public class StringBuilderExample {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Java");

        sb.append(" Rocks!");
        sb.insert(4, ",");
        sb.replace(0, 4, "Kotlin");
        sb.delete(6, 12);
        sb.reverse();

        System.out.println(sb);
    }
}
```

**Wynik przykładowy:**

```
,KtiloK
```

> W praktyce, gdy potrzebujesz **częstych modyfikacji tekstu**, używaj **`StringBuilder`** zamiast `String`.

---

### Porównanie klas

| Cecha                         | String                                  | StringBuffer                            | StringBuilder                           |
| ----------------------------- |-----------------------------------------| --------------------------------------- | --------------------------------------- |
| Modyfikowalność               | nie                                     | tak                                     | tak                                     |
| Synchronizacja                | nie                                     | tak                                     | nie                                     |
| Wydajność przy wielu zmianach | niska                                   | średnia                                 | wysoka                                  |
| Typowe metody                 | `concat`, `replace`, `substring`, itd.. | `append`, `insert`, `delete`, `reverse` | `append`, `insert`, `delete`, `reverse` |

---

#### Przykład praktyczny – budowanie zdania

```java
public class SentenceBuilder {
    public static void main(String[] args) {
        StringBuilder sentence = new StringBuilder();

        sentence.append("Java");
        sentence.append(" is");
        sentence.append(" powerful");
        sentence.append("!");

        System.out.println(sentence); // "Java is powerful!"

        // Zamiana 'powerful' na 'awesome'
        int start = sentence.indexOf("powerful");
        int end = start + "powerful".length();
        sentence.replace(start, end, "awesome");

        System.out.println(sentence); // "Java is awesome!"
    }
}
```

---

## Zadania
1.  Napisz statyczną metodę, której argumentem jest dodatnia liczba całkowita 𝑛 (𝑛 > 2).
    Metoda ma zwrócić największą liczbę pierwszą mniejszą niż 𝑛. Stwórz przypadek testowy
    dla tej metody.
2. Napisz statyczną metodę, która jako argument otrzymuje dodatnią liczbę całkowitą 𝑛
   i zwraca liczbę 7<sup>(−𝑛)</sup>. Nie korzystaj z żadnych gotowych funkcji bibliotecznych ani wbudowanych wewnątrz tej funkcji poza instrukcjami wejścia/wyjścia. Stwórz przypadek
   testowy.
   Podpowiedź: 7<sup>−𝑛</sup> = 1/(7<sup>𝑛</sup>).
3.  Napisz metodę generateRandomIntInRange, która przyjmuje dwie liczby całkowite jako
    argumenty i zwraca losową liczbę całkowitą z tego zakresu (włącznie z granicami). Na
    przykład, dla argumentów 5 i 10, metoda powinna zwracać liczbę z zakresu od 5 do 10.
    Stwórz przypadek testowy.
4.  Utwórz program, który tworzy jednowymiarową tablicę 20 liczb losowych z przedziału
    od 1 do 100, a następnie oblicza i wyświetla ich średnią wartość.
5.  Utwórz program, który tworzy jednowymiarową tablicę 30 liczb całkowitych. Następnie
    program powinien obliczyć i wyświetlić ilość liczb, które są kwadratami innej liczby
    całkowitej.
6. Napisz statyczną metodę minimumValue, która przyjmuje listę tablicową liczb całkowi
   tych jako argument i zwraca najmniejszą liczbę w liście tablicowej. Przyjmij, że lista
   tablicowa zawsze będzie miała co najmniej jeden element. Stwórz przypadek testowy.
7.  Napisz statyczną metodę reverseArray, która przyjmuje listę tablicową liczb całkowi
    tych jako argument i zwraca nową listę tablicową, ale z odwróconym porządkiem elementów. Na przykład, dla [1, 2, 3, 4, 5], funkcja powinna zwrócić [5, 4, 3, 2, 1]. Stwórz
    przypadek testowy.
8.  Napisz statyczną metodę, która przyjmuje napis jako argument i zwraca ten napis z
    zamienioną pierwszą i ostatnią literą. Stwórz przypadek testowy.
9. Napisz program, który przyjmuje jako wejście pojedynczy znak oraz liczbę całkowitą n.
    Używając klasy StringBuilder, zbuduj i wypisz piramidę o wysokości n, gdzie każdy
    poziom piramidy składa się z podanego znaku. Na przykład dla znaku * i n=3, oczekiwany
    wynik to:
```text
*
***
*****
```
10.  Napisz metodę statyczną capitalizeEverySecond, która przyjmuje jako argument
     obiekt typu StringBuffer. Metoda ma zmienić każdą drugą literę na wielką. Stwórz
     przypadek testowy.






