# **Programowanie Obiektowe – Java (2025Z)**

## **Klasa, obiekty, konstruktory**

### 1. Co to jest klasa?

> **Klasa** to **szablon (wzorzec)**, który opisuje, **jakie właściwości (pola)** i **zachowania (metody)** ma mieć obiekt.

Można to porównać do **projektu domu** — klasa opisuje, **co dom zawiera**, ale sam dom (czyli **obiekt**) trzeba jeszcze **zbudować**.

Przykład z życia:

* Klasa → `Samochód`
* Obiekt → *konkretne auto, np. jakieś audi sąsiada*, np. `Samochód audiSasiada = new Samochód();`

---

### 2. Składnia klasy w Javie

```java
public class NazwaKlasy {
    // Pola (atrybuty, właściwości)
    typ nazwaPola;

    // Konstruktor
    public NazwaKlasy() {
        // kod wykonywany przy tworzeniu obiektu
    }

    // Metody (zachowania)
    public void nazwaMetody() {
        // ciało metody
    }
}
```

---

### 3. Przykład – Klasa `Samochod`

```java
public class Samochod {
    // 🔸 Pola (cechy samochodu)
    String marka;
    String model;
    int rokProdukcji;
    double przebieg;

    // 🔸 Konstruktor (tworzy nowy obiekt)
    public Samochod(String marka, String model, int rokProdukcji, double przebieg) {
        this.marka = marka;
        this.model = model;
        this.rokProdukcji = rokProdukcji;
        this.przebieg = przebieg;
    }

    // 🔸 Metoda – zachowanie samochodu
    public void uruchom() {
        System.out.println(marka + " " + model + " został uruchomiony!");
    }

    public void wyswietlInformacje() {
        System.out.println("Marka: " + marka);
        System.out.println("Model: " + model);
        System.out.println("Rok: " + rokProdukcji);
        System.out.println("Przebieg: " + przebieg + " km");
    }
}
```

---

### 4. Tworzenie obiektów z klasy

Każdy obiekt tworzy się słowem kluczowym `new`.

```java
public class Main {
    public static void main(String[] args) {
        // Tworzymy dwa obiekty klasy Samochod
        Samochod auto1 = new Samochod("Toyota", "Corolla", 2018, 85000);
        Samochod auto2 = new Samochod("Audi", "A4", 2020, 42000);

        // Wywołanie metod obiektów
        auto1.uruchom();
        auto1.wyswietlInformacje();

        System.out.println();

        auto2.uruchom();
        auto2.wyswietlInformacje();
    }
}
```

**Wynik:**

```
Toyota Corolla został uruchomiony!
Marka: Toyota
Model: Corolla
Rok: 2018
Przebieg: 85000.0 km

Audi A4 został uruchomiony!
Marka: Audi
Model: A4
Rok: 2020
Przebieg: 42000.0 km
```

---

### 5. Kluczowe pojęcia

| Pojęcie              | Znaczenie                                                           |
| -------------------- | ------------------------------------------------------------------- |
| **Klasa**            | Szablon (wzór) do tworzenia obiektów. Określa pola i metody.        |
| **Obiekt**           | Konkretna instancja klasy (czyli rzeczywisty "egzemplarz").         |
| **Pola (fields)**    | Zmienne wewnątrz klasy opisujące stan obiektu.                      |
| **Metody (methods)** | Funkcje wewnątrz klasy opisujące zachowanie obiektu.                |
| **Konstruktor**      | Specjalna metoda wywoływana przy tworzeniu obiektu (`new`).         |
| **this**             | Słowo kluczowe wskazujące na aktualny obiekt (np. w konstruktorze). |

---

### 6. Przykład z wykorzystaniem metod i logiki

```java
public class KontoBankowe {
    String wlasciciel;
    double saldo;

    public KontoBankowe(String wlasciciel, double saldoPoczatkowe) {
        this.wlasciciel = wlasciciel;
        this.saldo = saldoPoczatkowe;
    }

    public void wplata(double kwota) {
        saldo += kwota;
        System.out.println("Wpłacono " + kwota + " zł. Nowe saldo: " + saldo);
    }

    public void wyplata(double kwota) {
        if (kwota <= saldo) {
            saldo -= kwota;
            System.out.println("Wypłacono " + kwota + " zł. Nowe saldo: " + saldo);
        } else {
            System.out.println("Brak środków na koncie!");
        }
    }

    public void pokazSaldo() {
        System.out.println("Saldo konta " + wlasciciel + ": " + saldo + " zł");
    }
}
```

#### Użycie w `main`:

```java
public class Main {
    public static void main(String[] args) {
        KontoBankowe konto = new KontoBankowe("Adrian", 1000);

        konto.pokazSaldo();
        konto.wplata(500);
        konto.wyplata(300);
        konto.wyplata(1500); // za duża kwota
    }
}
```

**Wynik:**

```
Saldo konta Adrian: 1000.0 zł
Wpłacono 500.0 zł. Nowe saldo: 1500.0 zł
Wypłacono 300.0 zł. Nowe saldo: 1200.0 zł
Brak środków na koncie!
```

---

#### Słowa kluczowe w podstawach

| Element       | Co robi                                   |
| ------------- | ----------------------------------------- |
| `class`       | Definiuje nowy typ obiektowy (szablon).   |
| `object`      | Egzemplarz klasy tworzony przez `new`.    |
| `fields`      | Dane wewnętrzne obiektu.                  |
| `methods`     | Zachowania obiektu.                       |
| `constructor` | Inicjalizuje pola obiektu przy tworzeniu. |
| `this`        | Odwołuje się do bieżącego obiektu.        |

## **Konstruktor w Javie (domyślny, przeciążony, kopiujący)**

### 1. Co to jest konstruktor?

**Konstruktor** to **specjalna metoda** w klasie, która **służy do tworzenia i inicjalizacji obiektów**.
Uruchamia się **automatycznie** w momencie, gdy tworzysz nowy obiekt za pomocą słowa kluczowego `new`.

---

### 2. Cechy konstruktora

| Cecha                                                                | Opis                                            |
| -------------------------------------------------------------------- | ----------------------------------------------- |
| Ma **taką samą nazwę jak klasa**                                     | np. klasa `Samochod` → konstruktor `Samochod()` |
| **Nie ma typu zwracanego**                                           | nawet `void`                                    |
| Może przyjmować **parametry**                                        | aby ustawić wartości pól przy tworzeniu obiektu |
| Można mieć **wiele konstruktorów** (przeciążanie)                    |                                                 |
| Jeśli nie zdefiniujesz żadnego, Java tworzy **domyślny konstruktor** |                                                 |

---

### 3. Rodzaje konstruktorów

#### A) Konstruktor domyślny (bez parametrów)

Jeśli **nie napiszesz żadnego konstruktora**, Java automatycznie tworzy pusty konstruktor, który nic nie robi.

```java
public class Samochod {
    String marka;
    int rok;

    // Konstruktor domyślny (Java tworzy go automatycznie)
}
```

Użycie:

```java
Samochod auto = new Samochod();  // działa mimo że nie napisaliśmy konstruktora
```

Ale często chcemy **napisać go samodzielnie**, żeby nadać wartości domyślne:

```java
public class Samochod {
    String marka;
    int rok;

    // Konstruktor domyślny napisany ręcznie
    public Samochod() {
        marka = "Nieznana";
        rok = 2000;
    }

    public void info() {
        System.out.println("Marka: " + marka + ", rok: " + rok);
    }
}

public class Main {
    public static void main(String[] args) {
        Samochod auto = new Samochod();
        auto.info();
    }
}
```

**Wynik:**

```
Marka: Nieznana, rok: 2000
```

---

#### B) Konstruktor z parametrami

Pozwala ustawić **wartości pól obiektu już w momencie tworzenia**.

```java
public class Samochod {
    String marka;
    int rok;

    // Konstruktor z parametrami
    public Samochod(String marka, int rok) {
        this.marka = marka;
        this.rok = rok;
    }

    public void info() {
        System.out.println("Marka: " + marka + ", rok: " + rok);
    }
}

public class Main {
    public static void main(String[] args) {
        Samochod auto1 = new Samochod("Toyota", 2018);
        Samochod auto2 = new Samochod("BMW", 2022);

        auto1.info();
        auto2.info();
    }
}
```

**Wynik:**

```
Marka: Toyota, rok: 2018
Marka: BMW, rok: 2022
```

Tutaj słowo kluczowe **`this`** oznacza *odwołanie do bieżącego obiektu* — dzięki temu `this.marka` i `this.rok` odnoszą się do pól klasy, a nie do zmiennych lokalnych o tej samej nazwie.

---

#### C) Konstruktor przeciążony

> **Przeciążanie konstruktora** oznacza, że w jednej klasie możemy mieć **więcej niż jeden konstruktor**, różniący się **listą parametrów**.

Java automatycznie rozpoznaje, którego konstruktora użyć — w zależności od liczby i typu przekazanych argumentów.

```java
public class Samochod {
    String marka;
    int rok;
    double przebieg;

    // Konstruktor 1 – domyślny
    public Samochod() {
        marka = "Nieznana";
        rok = 2000;
        przebieg = 0.0;
    }

    // Konstruktor 2 – z dwoma parametrami
    public Samochod(String marka, int rok) {
        this.marka = marka;
        this.rok = rok;
        przebieg = 0.0;
    }

    // Konstruktor 3 – z trzema parametrami
    public Samochod(String marka, int rok, double przebieg) {
        this.marka = marka;
        this.rok = rok;
        this.przebieg = przebieg;
    }

    public void info() {
        System.out.println("Marka: " + marka + ", rok: " + rok + ", przebieg: " + przebieg + " km");
    }
}

public class Main {
    public static void main(String[] args) {
        Samochod s1 = new Samochod();
        Samochod s2 = new Samochod("Honda", 2015);
        Samochod s3 = new Samochod("Audi", 2020, 55000);

        s1.info();
        s2.info();
        s3.info();
    }
}
```

**Wynik:**

```
Marka: Nieznana, rok: 2000, przebieg: 0.0 km
Marka: Honda, rok: 2015, przebieg: 0.0 km
Marka: Audi, rok: 2020, przebieg: 55000.0 km
```

---

#### D) Konstruktor kopiujący

Nie jest wbudowany w Javę, ale można go **napisać samodzielnie**, by utworzyć nowy obiekt **na podstawie innego obiektu tej samej klasy**.

```java
public class Samochod {
    String marka;
    int rok;

    // Konstruktor podstawowy
    public Samochod(String marka, int rok) {
        this.marka = marka;
        this.rok = rok;
    }

    // Konstruktor kopiujący
    public Samochod(Samochod inny) {
        this.marka = inny.marka;
        this.rok = inny.rok;
    }

    public void info() {
        System.out.println("Marka: " + marka + ", rok: " + rok);
    }
}

public class Main {
    public static void main(String[] args) {
        Samochod oryginal = new Samochod("Mazda", 2019);
        Samochod kopia = new Samochod(oryginal); // tworzymy kopię obiektu

        oryginal.info();
        kopia.info();
    }
}
```

**Wynik:**

```
Marka: Mazda, rok: 2019
Marka: Mazda, rok: 2019
```

---

### 4. Najczęstsze błędy przy konstruktorach

| Błąd                                                 | Co oznacza                                                        |
| ---------------------------------------------------- | ----------------------------------------------------------------- |
| Brak `this` w konstruktorze                          | Wartości nie przypisują się do pól klasy (zostają `null` lub `0`) |
| Napisanie `void` przed konstruktorem                 | To już **nie konstruktor**, tylko **metoda o nazwie jak klasa**   |
| Próba zwrotu wartości z konstruktora                 | Konstruktor **nie może mieć typu zwracanego**                     |
| Brak konstruktora z parametrami, a próba jego użycia | Java zwraca błąd „constructor not found”                          |

---

## **Metody w klasie – rodzaje i zastosowania**

---

### 1. **Metody fabrykujące (Factory Methods)**

#### Definicja:

> **Metody fabrykujące** (ang. *factory methods*) to **statyczne metody**, które **zwracają gotową instancję klasy**.
> Nie tworzymy obiektu poprzez `new`, tylko przez specjalną metodę np. `Person.create()`.

Często używa się ich, aby:

* **Ukryć złożony proces tworzenia obiektu**
* **Zwracać gotowe, skonfigurowane obiekty**
* **Zarządzać liczbą tworzonych obiektów** (np. wzorzec Singleton)

---

#### Przykład:

```java
public class Samochod {
    private String marka;
    private String model;

    // Prywatny konstruktor – nie można tworzyć obiektów przez new
    private Samochod(String marka, String model) {
        this.marka = marka;
        this.model = model;
    }

    // 🔸 Metoda fabrykująca (zwraca nowy obiekt)
    public static Samochod utworz(String marka, String model) {
        return new Samochod(marka, model);
    }

    public void info() {
        System.out.println("Samochód: " + marka + " " + model);
    }
}

public class Main {
    public static void main(String[] args) {
        // Tworzymy obiekt przez metodę fabrykującą
        Samochod auto = Samochod.utworz("Audi", "A4");
        auto.info();
    }
}
```

**Wynik:**

```
Samochód: Audi A4
```

---

### 2. **Metody dostępowe – Gettery i Settery**

#### Definicja:

> Gettery i Settery to **metody służące do bezpiecznego dostępu do pól klasy**, które są **ukryte (private)**.
> Dzięki nim realizujemy zasadę **enkapsulacji (hermetyzacji)**.

---

#### Przykład:

```java
public class KontoBankowe {
    private String wlasciciel;
    private double saldo;

    public KontoBankowe(String wlasciciel, double saldoPoczatkowe) {
        this.wlasciciel = wlasciciel;
        this.saldo = saldoPoczatkowe;
    }

    // 🔹 Getter – zwraca wartość pola
    public double getSaldo() {
        return saldo;
    }

    // 🔹 Setter – pozwala zmienić wartość pola
    public void setSaldo(double noweSaldo) {
        if (noweSaldo >= 0) {
            saldo = noweSaldo;
        } else {
            System.out.println("Saldo nie może być ujemne!");
        }
    }

    public String getWlasciciel() {
        return wlasciciel;
    }
}
```

#### Użycie:

```java
public class Main {
    public static void main(String[] args) {
        KontoBankowe konto = new KontoBankowe("Adrian", 1000);
        System.out.println("Saldo początkowe: " + konto.getSaldo());
        konto.setSaldo(1500);
        System.out.println("Nowe saldo: " + konto.getSaldo());
        konto.setSaldo(-200); // niepoprawne
    }
}
```

**Wynik:**

```
Saldo początkowe: 1000.0
Nowe saldo: 1500.0
Saldo nie może być ujemne!
```

---

#### Wzorzec nazewnictwa getterów i setterów:

| Typ    | Nazwa metody                      | Przykład dla pola `imie` |
| ------ | --------------------------------- | ------------------------ |
| Getter | `get` + nazwa pola z dużej litery | `getImie()`              |
| Setter | `set` + nazwa pola z dużej litery | `setImie(String imie)`   |

---

### 3. **Metody publiczne**

#### Definicja:

> **Publiczne metody** są widoczne **dla wszystkich klas** – niezależnie od pakietu.
> To właśnie przez nie inne klasy mogą **komunikować się** z obiektem.

---

#### Przykład:

```java
public class Kalkulator {
    // Metoda publiczna – dostępna dla wszystkich
    public int dodaj(int a, int b) {
        return a + b;
    }

    public int odejmij(int a, int b) {
        return a - b;
    }
}

public class Main {
    public static void main(String[] args) {
        Kalkulator k = new Kalkulator();
        System.out.println(k.dodaj(10, 5));  // 15
        System.out.println(k.odejmij(10, 5)); // 5
    }
}
```

**Wynik:**

```
15
5
```

**Zastosowanie:**
Publiczne metody to *interfejs klasy* – czyli sposób, w jaki inne części programu mogą z nią współpracować.

---

### 4. **Metody chronione (protected)**

#### Definicja:

> Metody oznaczone jako `protected` są **widoczne tylko dla:**
>
> * klasy, w której zostały zdefiniowane,
> * **podklas** (nawet w innych pakietach),
> * **klas w tym samym pakiecie**.

---


**Zastosowanie:**
Metody `protected` są idealne, gdy chcesz pozwolić podklasom **rozszerzać zachowanie klasy nadrzędnej**, ale **nie udostępniać tego publicznie**.

---

### 5. **Metody prywatne (private)**

#### Definicja:

> **Prywatne metody** są **widoczne tylko wewnątrz tej samej klasy**.
> Nie można ich wywołać z zewnątrz ani z podklas.

---

#### Przykład:

```java
class Uzytkownik {
    private String haslo;

    public Uzytkownik(String haslo) {
        this.haslo = haslo;
    }

    // Metoda prywatna
    private void pokazHaslo() {
        System.out.println("Hasło: " + haslo);
    }

    // Metoda publiczna korzystająca z prywatnej
    public void zaloguj(String podaneHaslo) {
        System.out.println("Logowanie...");
        if( podaneHaslo.equals(this.haslo)){
            pokazHaslo(); // OK – w tej samej klasie
        }
        else {
            System.out.println("Błędne hasło...");
        }
    }
}

public class UzytkownikMain {
    public static void main(String[] args) {
        Uzytkownik u = new Uzytkownik("tajne1234");
        u.zaloguj("tajne1234");
        // u.pokazHaslo(); // Błąd: metoda prywatna
    }
}
```

**Wynik:**

```
Logowanie...
Hasło: tajne123
```

**Zastosowanie:**
Metody `private` służą do **pomocniczych obliczeń, walidacji lub konfiguracji**, których nie chcemy udostępniać innym klasom.


## Zadania
1.  Utwórz klasę Dog z polami: name, breed i age. Napisz metodę bark(), która wydrukuje
    na konsoli "Wow Wow". Stwórz przypadek testowy, aby wywołać metodę co najmniej jeden
    raz.
2.  Utwórz klasę Point z dwoma polami: x i y reprezentującymi współrzędne na płaszczyźnie.
    Napisz metodę distance(Point otherPoint), która oblicza odległość między bieżącym
    punktem a innym punktem. Stwórz przypadek testowy, aby wywołać metodę co najmniej
    jeden raz.
3.  Utwórz klasę Person z publicznym polem name oraz prywatnym polem password. Zobacz
    jak różne modyfikatory dostępu wpływają na dostęp do tych pól z innej klasy.
4. Stwórz dwie klasy w tym samym pliku: Employee i Company. Klasa Employee powinna mieć pole bez modyfikatora dostępu. Spróbuj uzyskać dostęp do tego pola z klasy
   Company.
5.  Napisz klasę Book, która będzie zawierać trzy pola: title, author, publicationYear.
    Następnie zaimplementuj dwa konstruktory- jeden domyślny, który nie przyjmuje żadnych argumentów, i drugi, który przyjmuje trzy argumenty odpowiadające polom klasy.
    W przypadku drugiego konstruktora, nazwy parametrów muszą być takie same jak nazwy
    pól. Sprawdź czy jesteś w stanie prawidłowo przypisać wartości do pól klasy używając
    słowa kluczowego this.
6.  Stwórz klasę Car, która będzie zawierać trzy pola: brand, model, productionYear. Za
    implementuj trzy konstruktory- pierwszy domyślny, drugi przyjmujący dwa argumenty
    (brand i model), trzeci przyjmujący trzy argumenty (brand, model, productionYear).
    W przypadku drugiego i trzeciego konstruktora, nazwy parametrów muszą być takie
    same jak nazwy pól. Wykorzystaj słowo kluczowe this do rozróżnienia pól klasy od
    parametrów.
7.  Stwórz klasę Television z prywatnymi polami: brand, screenDiagonal, resolution,
    isSmartTV oraz price. Dodaj konstruktor, który przyjmuje wszystkie pola jako argumenty. Dodaj metody dostępowe (gettery i settery) oraz metodę showInformation(),
    która wyświetla informacje o telewizorze.
8. Napisz klasę Osoba, która nie pozwala tworzyć obiektu przez konstruktor.
   Zamiast tego dodaj metodę fabrykującą stworzOsobe, która zwraca nowy obiekt Osoba.
    Podpowiedź: Zastanów się, co możesz zrobić z konstruktorem, aby nie mieć dostępu do konstruktora z zewnątrz klasy/obiektu.
9. Napisz klasę Kalkulator, która ma publiczną metodę obliczSume(int a, int b)
   i prywatną metodę pomocniczą sprawdzDane(int a, int b),
   która sprawdza, czy liczby są dodatnie.
   Jeśli któraś liczba jest ujemna – wypisz komunikat i zwróć false.
10. Wykonaj kolejno poniższe czynności:
      1. Stwórz klasę Person z polem name. Dodaj do klasy metodę introduceYourself,
                która wyświetli wiadomość “Hi, I’m” i imię osoby. W klasie TestPerson, utwórz
                obiekt Person i wywołaj na nim metodę introduceYourself. Czy musisz użyć
                słowo kluczowe this w implementacji metody? 
      2. Dodaj do klasy Person metodę sayHello, która jako argument przyjmuje inny
                obiekt klasy Person i wyświetla wiadomość “Hello,” i imię drugiej osoby. Przeanalizuj działanie. 
    3. Dodaj do klasy Person metodę changeName, która jako argument przyjmuje
                łańcuch znaków i przypisuje go do pola name. Utwórz obiekt Person i użyj
                metody changeName do zmiany jego imienia. Następnie wywołaj metodę
                introduceYourself. Czy imię zostało zmienione? Czy musisz użyć słowo
                kluczowe this w implementacji metody? 
    4. Dodaj do klasy Person metodę swapNames, która jako argument przyjmuje inny
                obiekt klasy Person i zamienia imionami obie osoby. Utwórz dwa obiekty Person
                i użyj metody swapNames do zamiany ich imion. Następnie wywołaj metodę
                introduceYourself na obu obiektach. Czy imiona zostały zamienione?
