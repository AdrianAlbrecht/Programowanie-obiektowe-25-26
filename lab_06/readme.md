# **Programowanie Obiektowe – Java (2025Z)**

## **Polimorfizm**

### Co to jest polimorfizm?

**Polimorfizm** (z greckiego *poli* = wiele, *morphe* = formy) oznacza dosłownie **wiele form**.

W kontekście Javy oznacza to, że:

> **ten sam fragment kodu może zachowywać się inaczej w zależności od typu obiektu, z którym pracuje.**

---

#### Przykład w języku naturalnym:

Wyobraź sobie metodę `wydajDzwiek()`, która jest zdefiniowana np. w abstrakcyjnej klasie Zwierze.
Istnieją 3 klasy dziedziczące po tej klasie - Pies, Kot i Krowa. Metoda ta działa następująco:

* dla obiektu klasy `Pies` – zwróci *"Hau hau!"*,
* dla obiektu klasy `Kot` – zwróci *"Miau!"*,
* dla obiektu klasy `Krowa` – *"Muu!"*.

Ale w kodzie wywołanie jest **identyczne**:

```java
pies.wydajDzwiek();
kot.wydajDzwiek();
krowa.wydajDzwiek();
```

... albo jeżeli użyjemy przypisywania obiektów szczegółowych do typu ogólnego (to znaczy obiekt klasy podrzędnej pod typ klasy nadrzędnej) to może wyglądać następująco:

```java
zwierze.wydajDzwiek();
```

To właśnie **polimorfizm** — *jedno wywołanie, różne zachowanie.*

---

### Rodzaje polimorfizmu

| Typ polimorfizmu                             | Opis                                                             | Przykład                                   |
| -------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------ |
| **Polimorfizm czasu kompilacji** (statyczny) | Realizowany przez **przeciążanie metod** (*method overloading*). | `sum(int, int)` i `sum(double, double)`    |
| **Polimorfizm czasu wykonania** (dynamiczny) | Realizowany przez **nadpisywanie metod** (*method overriding*).  | `Zwierze z = new Pies(); z.wydajDzwiek();` |

---

### Polimorfizm dynamiczny (runtime)

To najczęściej omawiany rodzaj — ten, który działa dzięki dziedziczeniu i nadpisywaniu metod.

---

#### Przykład:

```java
class Zwierze {
    void wydajDzwiek() {
        System.out.println("Zwierzę wydaje dźwięk");
    }
}

class Pies extends Zwierze {
    @Override
    void wydajDzwiek() {
        System.out.println("Hau hau!");
    }
    
    public void kopDziure() {
        System.out.println("Pies kopie dziure.");
    }
}

class Kot extends Zwierze {
    @Override
    void wydajDzwiek() {
        System.out.println("Miau!");
    }
    
    public void wdrapSie() {
        System.out.println("Kot wdrapuje się wysoko.");
    }
}

public class Main {
    public static void main(String[] args) {
        Zwierze zwierze = new Zwierze();
        Zwierze pies = new Pies();
        Zwierze kot = new Kot();

        zwierze.wydajDzwiek();
        pies.wydajDzwiek();
        //pies.kopDziure(); //błąd, ponieważ określiliśmy typ jako Zwierze,
        // a ta klasa nie ma metody kopDziure()
        kot.wydajDzwiek();
        // kot.wdrapSie(); //błąd, to samo co przy kopDziure()

        Pies pies2 = new Pies();
        pies2.kopDziure();
    }
}
```

---

#### Jak to działa „pod spodem”?

* **Typ referencji** (`Zwierze`) określa, **jakie metody możesz wywołać**.
* **Typ obiektu** (`Pies` lub `Kot`) określa, **która implementacja zostanie wykonana**.

Java podczas działania programu sprawdza, do jakiego *rzeczywistego typu* odnosi się referencja i uruchamia odpowiednią wersję metody.

To zjawisko nazywamy **dynamicznym wiązaniem metod** (*dynamic method dispatch*).

---

#### Przykład z użyciem tablicy obiektów

Polimorfizm pozwala przechowywać różne typy obiektów w jednej kolekcji, o ile dziedziczą po wspólnej klasie.

```java
public class Main {
    public static void main(String[] args) {
        Zwierze[] zwierzeta = {
            new Pies(),
            new Kot(),
            new Zwierze()
        };

        for (Zwierze z : zwierzeta) {
            z.wydajDzwiek(); // każdy obiekt zachowa się inaczej
        }
    }
}
```

**Wynik:**

```
Hau hau!
Miau!
Hau hau!
```

---

### Polimorfizm z klasami abstrakcyjnymi

Polimorfizm działa również, gdy klasa bazowa jest **abstrakcyjna**.

```java
abstract class Figura {
    abstract double pole();
}

class Kolo extends Figura {
    double r;
    Kolo(double r) { this.r = r; }

    @Override
    double pole() {
        return Math.PI * r * r;
    }
}

class Prostokat extends Figura {
    double a, b;
    Prostokat(double a, double b) { this.a = a; this.b = b; }

    @Override
    double pole() {
        return a * b;
    }
}

public class Main {
    public static void main(String[] args) {
        Figura[] figury = {
            new Kolo(3),
            new Prostokat(4, 5)
        };

        for (Figura f : figury) {
            System.out.println("Pole: " + f.pole());
        }
    }
}
```

**Wynik:**

```
Pole: 28.27
Pole: 20.0
```

---

### Zalety polimorfizmu

| Zaleta                     | Opis                                                                                     |
| -------------------------- | ---------------------------------------------------------------------------------------- |
| **Elastyczność**           | Możemy pisać kod ogólny, niezależny od konkretnych klas.                                 |
| **Rozszerzalność**         | Łatwo dodawać nowe klasy bez zmiany istniejącego kodu.                                   |
| **Czytelność**             | Kod działa na poziomie pojęć (np. `Zwierze`, `Figura`), a nie konkretnych implementacji. |
| **Polimorficzne kolekcje** | Można przechowywać różne obiekty w jednej liście, tablicy, itp.                          |

---

### Ograniczenia i pułapki

| Problem                                       | Opis                                                             |
| --------------------------------------------- |------------------------------------------------------------------|
| **Brak dostępu do metod spoza klasy bazowej** | Referencja `Zwierze` nie widzi metod `kopDziure()` klasy `Pies`. |
| **Rzutowanie w dół (downcasting)**            | Możliwe, ale niebezpieczne — może wywołać `ClassCastException`.  |
| **Nadmierna abstrakcja**                      | Zbyt wiele poziomów dziedziczenia utrudnia zrozumienie kodu.     |

---

#### Przykład downcastingu:

```java
Zwierze z = new Pies();
((Pies) z).machajOgonem(); // poprawne, bo z rzeczywiście jest Psem
```

Ale jeśli:

```java
Zwierze z = new Kot();
((Pies) z).machajOgonem(); // Błąd w czasie działania!
```

otrzymamy:

```
Exception in thread "main" java.lang.ClassCastException
```

---

### Polimorfizm w praktyce

#### Przykład: System płatności

```java
abstract class Platnosc {
    abstract void zaplac(double kwota);
}

class Karta extends Platnosc {
    void zaplac(double kwota) {
        System.out.println("Płacenie kartą: " + kwota + " zł");
    }
}

class Blik extends Platnosc {
    void zaplac(double kwota) {
        System.out.println("Płacenie BLIKIEM: " + kwota + " zł");
    }
}

public class Main {
    public static void main(String[] args) {
        Platnosc[] metody = { new Karta(), new Blik() };
        for (Platnosc p : metody) {
            p.zaplac(99.99);
        }
    }
}
```

**Wynik:**

```
Płacenie kartą: 99.99 zł
Płacenie BLIKIEM: 99.99 zł
```

---

### Podsumowanie

| Pojęcie                      | Znaczenie                                                    |
| ---------------------------- | ------------------------------------------------------------ |
| **Polimorfizm**              | Różne zachowania tej samej metody dla różnych typów obiektów |
| **Statyczny (compile-time)** | Przeciążanie metod                                           |
| **Dynamiczny (runtime)**     | Nadpisywanie metod                                           |
| **Cel**                      | Elastyczność, uniwersalność, czysty kod                      |
| **Zastosowanie**             | Interfejsy, klasy abstrakcyjne, wzorce projektowe            |

---

## Metody `equals()`, `hashCode()`, `toString()` i inne w kontekście dziedziczenia i polimorfizmu

---

### Klasa `Object` – wspólny przodek wszystkich klas

W języku Java **każda klasa automatycznie dziedziczy po klasie `Object`**, nawet jeśli tego nie napiszesz.

```java
class Osoba {
    String imie;
}
```

jest równoważne z:

```java
class Osoba extends Object {
    String imie;
}
```

To oznacza, że **każdy obiekt w Javie ma zestaw metod**, które pochodzą właśnie z klasy `Object` — a najważniejsze z nich to:

| Metoda               | Opis                                                                              |
| -------------------- |-----------------------------------------------------------------------------------|
| `equals(Object obj)` | Porównuje dwa obiekty pod względem zawartości (domyślnie – referencji)            |
| `hashCode()`         | Zwraca liczbę całkowitą używaną w strukturach typu `HashSet`, `HashMap`           |
| `toString()`         | Zwraca tekstową reprezentację obiektu                                             |
| `clone()`            | Tworzy kopię obiektu                                                              |
| `getClass()`         | Zwraca klasę, z której pochodzi obiekt                                            |
| `finalize()`         | (Przestarzała) – wywoływana przed usunięciem obiektu przez GC (Garbage Collector) |

---

### Metoda `toString()`

#### Domyślnie:

`toString()` zwraca nazwę klasy + adres w pamięci (technicznie: hash kod obiektu w formie szesnastkowej).

```java
class Osoba {
    String imie;
}

public class Main {
    public static void main(String[] args) {
        Osoba o = new Osoba();
        o.imie = "Adrian";
        System.out.println(o.toString());
    }
}
```

Wynik (przykładowy):

```
Osoba@5e91993f
```

---

#### Nadpisywanie `toString()`

W praktyce **zawsze nadpisujemy `toString()`**, żeby obiekt wypisywał się w czytelny sposób:

```java
class Osoba {
    String imie;
    int wiek;

    @Override
    public String toString() {
        return "Osoba: " + imie + ", wiek: " + wiek;
    }
}
```

Teraz:

```java
Osoba o = new Osoba();
o.imie = "Adrian";
o.wiek = 30;
System.out.println(o);
```

Wynik:

```
Osoba: Adrian, wiek: 30
```

---

### Metoda `equals(Object obj)`

Domyślna implementacja `equals()` w klasie `Object` porównuje **adresy w pamięci** (czyli, czy to ten sam obiekt).

```java
Osoba o1 = new Osoba();
Osoba o2 = new Osoba();
System.out.println(o1.equals(o2)); // false, bo to dwa różne obiekty
```

---

#### Nadpisywanie `equals()`

Aby porównywać **wartości pól**, należy metodę `equals()` **nadpisać**.

```java
class Osoba {
    String imie;
    int wiek;

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;           // ten sam obiekt
        if (obj == null || getClass() != obj.getClass()) return false; //jeżeli inne klasy

        Osoba o = (Osoba) obj;                  // rzutowanie. Potrzebne, bo parametr jest klasy Object, czyli nadrzędnej
        return wiek == o.wiek && imie.equals(o.imie);
    }
}
```

Teraz:

```java
Osoba o1 = new Osoba();
o1.imie = "Adrian";
o1.wiek = 30;

Osoba o2 = new Osoba();
o2.imie = "Adrian";
o2.wiek = 30;

System.out.println(o1.equals(o2)); // true
```

---

### Metoda `hashCode()`

W strukturach takich jak `HashSet` i `HashMap`, Java używa **hashCode()** do szybkiego wyszukiwania obiektów.
Jeśli nadpiszesz `equals()`, **musisz też nadpisać `hashCode()`**, inaczej będą błędy logiczne!

> 💡 Zasada:
> Jeśli `a.equals(b) == true`, to `a.hashCode() == b.hashCode()` musi być również `true`.

---

#### Przykład poprawnego nadpisania:

```java
class Osoba {
    String imie;
    int wiek;

    @Override
    public int hashCode() {
        return java.util.Objects.hash(imie, wiek);
    }
}

public class Main {
    public static void main(String[] args) {
        java.util.HashSet<Osoba> zbior = new java.util.HashSet<>();
        zbior.add(new Osoba("Adrian", 30));
        zbior.add(new Osoba("Adrian", 30)); // duplikat logiczny

        System.out.println(zbior); // tylko jeden element
    }
}
```

---

### Inne metody klasy `Object`

| Metoda                              | Opis                                                         | Przykład                       |
| ----------------------------------- | ------------------------------------------------------------ | ------------------------------ |
| `getClass()`                        | Zwraca klasę, z której utworzono obiekt                      | `obj.getClass().getName()`     |
| `clone()`                           | Tworzy kopię obiektu (jeśli klasa implementuje `Cloneable`)  | `obiekt.clone()`               |
| `finalize()`                        | (deprecated) – wywoływana przed zniszczeniem obiektu         | –                              |
| `wait()`, `notify()`, `notifyAll()` | Używane w programowaniu współbieżnym (synchronizacja wątków) | tylko w `synchronized` blokach |

---

### Polimorfizm a `toString()`, `equals()`, `hashCode()`

Ponieważ te metody **są dziedziczone z klasy `Object`**, każda klasa może:

* korzystać z ich **domyślnej wersji**,
* lub **nadpisać je**, dostosowując do swojego typu.

Dzięki temu możliwe jest **polimorficzne wywoływanie metod**:

```java
Object o1 = new Osoba("Adrian", 30);
Object o2 = new Osoba("Adrian", 30);

System.out.println(o1.equals(o2)); // wywoła equals() z klasy Osoba
System.out.println(o1.toString()); // wywoła toString() z klasy Osoba
```

Choć zmienne są typu `Object`, **Java automatycznie** wywoła właściwą implementację metod z klasy `Osoba`.
To właśnie **polimorfizm w praktyce**.

---

### Dobre praktyki

| Zasada                                          | Dlaczego                                                          |
| ----------------------------------------------- |-------------------------------------------------------------------|
| Zawsze nadpisuj `toString()`                    | Ułatwia debugowanie i logowanie                                   |
| Zawsze nadpisuj `equals()` i `hashCode()` razem | Zapewnia spójność w strukturach danych                            |
| Używaj `Objects.equals()` i `Objects.hash()`    | Unikasz błędów przy `null`                                        |
| Nie porównuj obiektów `==`                      | To porównuje referencje (adresy obiektów), nie zawartość (pola)   |
| Pamiętaj o polimorfizmie                        | Wywoływana jest wersja metody zgodna z rzeczywistym typem obiektu |

---

## Zadania
1.  Wykonaj poniższe czynności:
   - Zdefiniuj klasę Person, która posiada następujące pola: firstName, lastName i age. 
   - Napisz konstruktor, który przyjmuje trzy argumenty i waliduje je przed przypisaniem do
       odpowiednich pól.
     - Wiekosoby (age) nie powinien być ujemny. W przypadku podania wartości ujemnej
         dla wieku, ustaw wiek osoby na zero.
     - PolafirstName i lastName nie powinny być puste ani równać się null. W przypadku
         podania pustego napisu lub null dla tych pól, ustaw odpowiednio pusty napis.
         
   - Dodaj metodę toString(), która zwraca informacje o osobie w formacie: "Person:
       [firstName] [lastName], Age: [age].". Zwróć uwagę na wielkość liter i znaki interpunkcyjne. 
   - Dodaj metodę equals(), która porównuje dwie osoby na podstawie ich pól firstName,
       lastName i age. Dwie osoby są uważane za identyczne, jeśli wszystkie trzy pola są takie
       same. 
   - Dodaj metodę hashCode(), która generuje kod hash dla odpowiedniego obiektu. Metoda
       ta powinna być zgodna z metodą equals()
2.  Utwórz klasę Property z polami address, size i price. Utwórz klasy House i
    Apartment, które dziedziczą po klasie Property. Klasa House powinna mieć dodatkowe
    pole numberOfFloors, a klasa Apartment pole floorNumber. Dodaj konstruktory,
    metody gettery i settery (tak, aby spełnione były zasady hermetyzacji), metodę toString(), equals() oraz hashCode() dla każdej z
    klas. Napisz program testujący zdefiniowane klasy i metody.
3. Utwórz klasę ComputerGame z polami title, producer oraz ratings (jako tablica z
    elementami typu double). Dodaj metody pozwalające na dodawanie i usuwanie ocen.
    Utwórz klasę RPGGame, która dziedziczy po klasie ComputerGame. Klasa RPGGame powinna
    mieć dodatkowe pole gameWorld. Dodaj konstruktory, metody gettery i settery(tak, aby spełnione były zasady hermetyzacji), metodę
    toString(), equals() oraz hashCode() dla każdej z klas. Napisz program testujący
    zdefiniowane klasy i metody.
4.  Wykonaj poniższe czynności:
   - Stwórz klasę Gradebook z prywatnymi polami: firstName, lastName oraz grades (jako
       ArrayList typu int). Dodaj konstruktor, który przyjmuje firstName i lastName jako
       argumenty. Dodaj metody dostępowe (gettery i settery) oraz metody addGrade(int
       grade) i removeGrade(int index), które odpowiednio dodają lub usuwają ocenę z listy
       ocen. Dodaj również metodę averageGrade() do obliczania i zwracania średniej ocen. 
   - Dodaj metodę toString(), która zwraca informacje o uczniu, średniej jego ocen oraz
       wszystkich ocenach w formacie: "Gradebook for [firstName] [lastName]: Average
       Grade = [averageGrade], Grades: [grade1, grade2, ...].". Zwróć uwagę na
       wielkość liter i znaki interpunkcyjne. 
   - Dodaj metodę equals(), która porównuje dwa obiekty klasy Gradebook na podstawie
       ich pól firstName, lastName oraz zawartości listy grades. Dwa dzienniczki są uważane
       za identyczne, jeśli mają takie same imię, nazwisko i identyczny zestaw ocen (z uwzględnieniem kolejności). 
   - Dodaj metodę hashCode(), która generuje kod hash dla odpowiedniego obiektu. Metoda
       ta powinna być zgodna z metodą equals()