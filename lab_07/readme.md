# **Programowanie Obiektowe – Java (2025Z)**

# Lab 07: Rekordy, związki między klasami, obsługa wyjątków

## **I. Rekordy w Javie (Java Records)**

**Wprowadzone w Java 14 (preview), stabilne od Java 16.**

Rekord to **specjalny rodzaj klasy**, który służy do reprezentowania **niezmiennych (immutable) danych**.
Jest to odpowiedź Javy na częsty problem:

> „Potrzebuję klasy, która tylko *przechowuje dane*, ale nie chcę pisać 50 linii boilerplate’u.”

Rekord = **dane + automatyczne metody**.

Funkcjonalność ta wprowadzona została jako sposób na uproszczenie definicji klas, które głównie służą jako pojemniki danych. Rekordy są szczególnym rodzajem klas, które automatycznie dostarczają implementacji metod takich jak equals(), hashCode() i toString() na podstawie zadeklarowanych pól. Są one szczególnie przydatne w przypadkach, gdy klasa jest używana do modelowania prostych struktur danych.

---

### **1. Co automatycznie generuje rekord?**

Dla deklaracji:

```java
public record Person(String name, int age) {}
```

Java *automatycznie* tworzy:

| Co generuje?               | Opis                                  |
| -------------------------- | ------------------------------------- |
| **pola** (final)           | `private final String name;`          |
| **konstruktor kanoniczny** | `public Person(String name, int age)` |
| **gettery**                | w formie metod: `name()` i `age()`    |
| **equals()**               | porównuje dane!                       |
| **hashCode()**             | obliczany na podstawie danych         |
| **toString()**             | np. `Person[name=Jan, age=20]`        |

Wszystko to — bez pisania ani jednej dodatkowej linijki.

---

### **2. Rekordy są niemutowalne**

Rekordy to **immutable objects**.
Pola w rekordach są automatycznie **final**.

Nie możesz zrobić:

```java
person.name = "Nowe imię"; // ❌ błąd
```

Jeśli potrzebujesz innego obiektu, tworzysz nowy rekord.

---

### **3. Tworzenie rekordu – przykład**

```java
public record Point(int x, int y) {}

public class Main {
    public static void main(String[] args) {
        Point p = new Point(10, 20);

        System.out.println(p.x());  // 10
        System.out.println(p.y());  // 20
    }
}
```

Gettery w rekordach *nie zaczynają się od `get`*.
Nazywają się tak samo jak pola.

---

### **4. Rekord może mieć dodatkowe metody**

```java
public record Circle(double radius) {
    public double area() {
        return Math.PI * radius * radius;
    }
}
```

---

### **5. Nadpisywanie konstruktora (konstruktor kanoniczny)**

Możesz przejąć kontrolę nad walidacją:

```java
public record User(String login, String email) {
    public User {
        if (login == null || login.isBlank()) {
            throw new IllegalArgumentException("Login nie może być pusty");
        }
    }
}
```

Uwaga:
Nie piszesz nawiasów po nazwach parametrów — *to konstruktor kanoniczny*.

---

### **6. Rekord z prywatnym konstruktorem i metodą fabrykującą**

Tak, nawet rekord może mieć metodę fabrykującą:

```java
public record Temperature(double celsius) {

    public static Temperature fromFahrenheit(double f) {
        return new Temperature((f - 32) * 5.0/9.0);
    }
}
```

---

### **7. Rekordy w strukturach danych**

Idealne do **DTO**, **konfiguracji**, **transportu danych**:

```java
public record LoginRequest(String username, String password) {}
```

---

### **8. Kiedy używać rekordów?**

#### ✔️ Używaj rekordu, gdy:

* klasa reprezentuje **wyłącznie dane**
* obiekt ma być **niemutowalny**
* potrzebujesz **czytelnego, krótkiego kodu**
* tworzysz **DTO**, **klasę konfiguracyjną**, **event**, **request**, **response**

#### ❌ Nie używaj rekordu, gdy:

* obiekt musi być **mutowalny**
* klasa ma skomplikowaną logikę
* chcesz korzystać z dziedziczenia (rekord nie może rozszerzać klasy)

---

### **9. Rozbudowany przykład rekordu — mini aplikacja**

```java
public record Product(String name, double price) {

    // Konstruktor kanoniczny z walidacją
    public Product {
        if (price < 0) {
            throw new IllegalArgumentException("Cena nie może być ujemna");
        }
    }

    // Metoda pomocnicza
    public double priceWithVat(double vat) {
        return price * (1 + vat);
    }

    // Metoda fabrykująca
    public static Product ofNet(String name, double net, double vat) {
        return new Product(name, net * (1 + vat));
    }
}

public class Main {
    public static void main(String[] args) {
        Product p = Product.ofNet("Laptop", 3000, 0.23);

        System.out.println(p); 
        System.out.println("Cena brutto: " + p.price());
        System.out.println("Cena z VAT 8%: " + p.priceWithVat(0.08));
    }
}
```

---

## **II. Związki pomiędzy klasami w Java**

W programowaniu obiektowym klasy zazwyczaj **nie istnieją w izolacji** — współpracują, komunikują się i często zależą od siebie.
To właśnie relacje między klasami pozwalają budować rozbudowane systemy.

W Javie wyróżniamy cztery podstawowe typy związków:

1. **Asocjacja** (Association)
2. **Agregacja** (Aggregation)
3. **Kompozycja** (Composition)
4. **Dziedziczenie** (Inheritance)
   *(dziedziczenie już omówiliśmy wcześniej, więc potraktujemy je tu skrótowo)*

---

### 1. **Asocjacja (Association)**

Jest to **najluźniejszy typ relacji**.
Mówi jedynie, że jedna klasa *zna* drugą i może z niej korzystać.

Nie ma zależności życiowej — obiekty mogą istnieć niezależnie.

#### Przykład: Student ma Laptopa (ale oboje mogą istnieć osobno)

```java
class Laptop {
    private String model;

    public Laptop(String model) {
        this.model = model;
    }

    public String getModel() { return model; }
}

class Student {
    private Laptop laptop; // asocjacja: Student -> Laptop

    public Student(Laptop laptop) {
        this.laptop = laptop;
    }

    public void showLaptop() {
        System.out.println("Mój laptop to: " + laptop.getModel());
    }
}

public class Main {
    public static void main(String[] args) {
        Laptop dell = new Laptop("Dell XPS");
        Student s = new Student(dell);

        s.showLaptop();
    }
}
```

#### Cechy:

* obiekty żyją niezależnie,
* relacja może być dwukierunkowa lub jednokierunkowa,
* nie ma własności.

---

### 2. **Agregacja (Aggregation)**

Agregacja to **specjalny typ asocjacji**:

> A zawiera B, ale B może istnieć bez A.

Agregacja to specjalny rodzaj asocjacji, w której obie klasy mogą istnieć niezależnie, ale jedna z nich (całość) zawiera drugą (część).

Przykład: **Klasa School ma Students**, ale student może istnieć sam.

```java
class Student {
    private String name;
    public Student(String name) { this.name = name; }
    public String getName() { return name; }
}

class School {
    private List<Student> students = new ArrayList<>(); // agregacja

    public void addStudent(Student s) {
        students.add(s);
    }

    public void showStudents() {
        students.forEach(s -> System.out.println(s.getName()));
    }
}
```

#### Cechy agregacji:

* obiekt jest „częścią” innego obiektu,
* ale **może być współdzielony**,
* i istnieje niezależnie od właściciela.

---

### 3. **Kompozycja (Composition)**

To **najsilniejszy rodzaj relacji**:

> A tworzy B i jest właścicielem jego cyklu życia.
> Jeśli A przestanie istnieć, B również.

Kompozycja to bardziej rygorystyczna forma agregacji, gdzie część nie może istnieć bez całości.

Przykład: **Dom składa się z pokoi**, które nie mogą istnieć bez domu.

```java
class Room {
    private String name;

    public Room(String name) {
        this.name = name;
    }
}

class House {
    private List<Room> rooms = new ArrayList<>();

    public House() {
        rooms.add(new Room("Living Room"));
        rooms.add(new Room("Kitchen"));
    }
}
```

#### Cechy kompozycji:

* obiekt część nie jest współdzielony,
* istnieje wyłącznie jako element całości,
* niszczenie całości niszczy części.

---

### 4. **Dziedziczenie (Inheritance)**

Relacja „IS-A”
`Student extends Person`

Tego tematu już nie rozwijam, bo był omawiany wcześniej — tutaj tylko wspominam jako część rodziny związków.

---

## **III. Wyjątki i obsługa błędów**

W Javie błędy **nie są tylko kodami zwracanymi przez funkcje**, jak w językach typu C.
Java posiada **system wyjątków**, który pozwala reagować na niebezpieczne sytuacje w sposób czytelny i kontrolowany.

Wyjątki są obiektami reprezentującymi **nieprawidłowy stan w czasie działania programu**, a ich obsługa odbywa się poprzez mechanizm `try–catch–finally`.

---

### **1. Hierarchia wyjątków w Javie**

Java posiada ustandaryzowaną hierarchię wyjątków:

```
                Throwable
                /       \
       Exception         Error
         /     \
RuntimeException  (checked exceptions)
```

#### **🔹 Error**

* wskazuje na poważne błędy środowiska JVM (np. `OutOfMemoryError`)
* *nie powinny być łapane i obsługiwane*

#### **🔹 Exception**

To wyjątki, które można i trzeba obsługiwać.

Dzielą się na:

#### ✔ **Checked exceptions**

Muszą być albo obsłużone (`try–catch`), albo zadeklarowane (`throws`).
Np.:

* `IOException`
* `SQLException`
* `FileNotFoundException`

#### ✔ **Unchecked exceptions (RuntimeException)**

Nie wymagają jawnej obsługi, bo wynikają z błędów programistycznych.

Np.:

* `NullPointerException`
* `IndexOutOfBoundsException`
* `ArithmeticException`
* `NumberFormatException`

---

### **2. Blok try–catch – podstawowy mechanizm**

```java
try {
    // kod, który może wygenerować wyjątek
} catch (ExceptionType e) {
    // obsługa wyjątku
}
```

#### Przykład:

```java
try {
    int x = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Nie można dzielić przez zero!");
}
```

---

### **3. Blok finally – zawsze się wykona**

Używany w sytuacjach, gdy musisz wykonać operację niezależnie od tego, czy wystąpi wyjątek.

Np. zamykanie plików, zamykanie połączeń.

```java
try {
    FileInputStream f = new FileInputStream("test.txt");
} catch (IOException e) {
    System.out.println("Błąd pliku!");
} finally {
    System.out.println("Wykonuję cleanup!");
}
```

---

### **4. Obsługa wielu wyjątków**

#### **a) Kilka catchów**

```java
try {
    String s = null;
    System.out.println(s.length());
} catch (NullPointerException e) {
    System.out.println("Pusty string!");
} catch (Exception e) {
    System.out.println("Inny błąd");
}
```

#### **b) Łączenie wyjątków w jeden catch (Java 7+)**

```java
catch (IOException | SQLException e) {
    e.printStackTrace();
}
```

---

### **5. Rzucanie wyjątków ręcznie (throw)**

Możesz "rzucić" wyjątek, czyli świadomie przerwać normalny tok programu.

```java
if (age < 0) {
    throw new IllegalArgumentException("Wiek nie może być ujemny");
}
```

---

### **6. Deklarowanie wyjątków (throws)**

Jeżeli metoda **może** wyrzucić checked exception, musi to zadeklarować:

```java
public void readFile(String path) throws IOException {
    FileInputStream f = new FileInputStream(path);
}
```

---

### **7. Tworzenie własnych wyjątków**

Możemy tworzyć wyjątki, które lepiej opisują błąd domenowy w programie.

```java
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}
```

Użycie:

```java
public void setAge(int age) throws InvalidAgeException {
    if (age < 0) throw new InvalidAgeException("Wiek < 0");
}
```

---

### **8. Try-with-resources – najlepszy sposób obsługi zasobów**

Ten mechanizm automatycznie zamyka zasoby (np. pliki, połączenia sieciowe), jeśli implementują `AutoCloseable`.

```java
try (FileInputStream file = new FileInputStream("data.txt")) {
    int x = file.read();
} catch (IOException e) {
    e.printStackTrace();
}
```

**Nie trzeba używać finally!**

---

### **9. Dobre praktyki obsługi wyjątków**

#### ✔ Łap tylko te wyjątki, które jesteś w stanie obsłużyć

Zła praktyka:

```java
catch (Exception e) { }
```

#### ✔ Unikaj pustych catchy

One zabijają debugowanie.

#### ✔ Używaj wyjątków tam, gdzie występuje wyjście z normalnego toku programu

#### ✔ Twórz własne wyjątki tylko wtedy, gdy mają sens semantyczny

Np. `InsufficientFundsException` w bankowości.

#### ✔ Nigdy nie używaj wyjątków jako logiki sterującej

To nie Python.

---

### **10. Zaawansowane: Ponowne rzucanie wyjątku**

```java
try {
    risky();
} catch (IOException e) {
    System.out.println("Log błędu");
    throw e;  // ponowne rzucenie
}
```

---

### **11. Zaawansowane: wyjątki łańcuchowe (cause)**

```java
try {
    parseConfig();
} catch (IOException e) {
    throw new RuntimeException("Błąd parsowania konfiguracji", e);
}
```

Można wtedy odczytać źródłowy błąd przez:

```java
e.getCause();
```

---

### **12. Przykład pełnego systemu obsługi wyjątków**

```java
class UjemnaKwotaException extends RuntimeException {
    public UjemnaKwotaException(String message) {
        super(message);
    }
}

class BankAccount {
    private double balance;

    public void withdraw(double amount) throws Exception {
        if (amount < 0)
            throw new UjemnaKwotaException("Kwota nie może być ujemna");

        if (amount > balance)
            throw new Exception("Brak środków!");

        balance -= amount;
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount();

        try {
            acc.withdraw(100);
            //acc.withdraw(-100);
        } catch (UjemnaKwotaException e) {
            System.out.println("Niepoprawna kwota!");
        } catch (Exception e) {
            System.out.println("Błąd wypłaty: " + e.getMessage());
        }
    }
}
```

---

## Zadania

1.  Stwórz rekord BookDTO, który reprezentuje książkę w sklepie internetowym. Powinien
    zawierać takie informacje jak title, author, price i yearOfPublication. Następnie
    stwórz kilka instancji tego rekordu, reprezentujących różne książki.
2.  Stwórz rekord BankAccount, który zawiera numer konta oraz saldo. Dodaj konstruktor,
    który pozwala na tworzenie konta tylko z numerem, przy czym domyślne saldo wynosi 0.
    Dodaj niestandardową metodę withInterest(double percentage), która zwraca nowy rekord,
    dodając odsetki. Stwórz przypadek testowy.
3. Przećwicz związki między klasami, projektując system obsługi pacjentów w szpitalu, 
   uwzględniając, że:
   * klasa Patient (z dodatkowymi polami `firstName` typu String, `lastName` typu String, 
   `birthDate` typu LocalDate) ma kartę pacjenta (klasa PatientCard z polami `medicalHistory`
   typu *ArrayList\<String\>*, `allergies` typu *ArrayList\<String\>*, `medications` typu 
   *ArrayList\<String\>*) – kompozycja (karta nie istnieje bez pacjenta).
   * klasa Doctor (z dodatkowymi polami `firstName` typu String, `lastName` typu String, 
   `specialization` typu String) może mieć listę (tablicową) klasy Pacjent – agregacja
   (pacjent nie znika, gdy lekarz przestaje pracować).
4.  Napisz program, który definiuje statyczną metodę checkAge(int age). Metoda ta powinna rzucić
    wyjątek IllegalArgumentException z odpowiednim komunikatem, jeśli podany wiek
    jest mniejszy niż 18. W głównej metodzie programu (main) wywołaj checkAge z różnymi
    wartościami i obsłuż wyjątek, wyświetlając stosowny komunikat dla użytkownika.
5.  Zaprojektuj i zaimplementuj klasę wyjątku NiepoprawnyFormatDanychException, która
    będzie rozszerzać klasę Exception. Następnie napisz metodę statyczną sprawdzFormatDanych(String
    dane), która rzuci wyjątek NiepoprawnyFormatDanychException, jeśli podany ciąg
    znaków nie odpowiada określonemu wzorcowi (np. nie jest adresem e-mail). W metodzie
    main przetestuj działanie tej metody, obsługując wyjątek i informując użytkownika o
    błędzie.
