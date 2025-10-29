# **Programowanie Obiektowe – Java (2025Z)**

## **Dziedziczenie**

### Wprowadzenie do dziedziczenia

Dziedziczenie (*inheritance*) to **mechanizm programowania obiektowego**, który pozwala **tworzyć nowe klasy na bazie już istniejących**.
Nowa klasa (podklasa, *subclass*) **dziedziczy** pola i metody klasy bazowej (*superclass*), dzięki czemu nie trzeba ich powielać.

Dzięki dziedziczeniu możemy:

* ponownie wykorzystywać kod,
* rozszerzać istniejące klasy o nowe funkcje,
* tworzyć bardziej logiczną hierarchię obiektów.

---

### Relacja *IS-A* („jest rodzajem”)

W Java dziedziczenie zawsze oznacza relację typu **„X jest Y”**, a może trafniejsze jest stwierdzenie **"X jest specyficznym rodzajem Y"**.

Przykłady:

* `Pies` **jest** `Zwierzęciem`,
* `SamochódElektryczny` **jest** `Samochodem`,
* `Student` **jest** `Osobą`.

---

### Słowo kluczowe `extends`

Aby klasa dziedziczyła po innej, używamy słowa kluczowego **`extends`**:

```java
class Zwierze {
    void oddychaj() {
        System.out.println("Zwierzę oddycha");
    }
}

class Pies extends Zwierze {
    void szczekaj() {
        System.out.println("Hau hau!");
    }
}
```

W klasie `Pies` dostępne są **wszystkie metody** klasy `Zwierze`, oprócz tych oznaczonych jako `private`.

---

### Dostępność pól i metod

| Modyfikator      | Dostęp w tej samej klasie | Dostęp w podklasie           | Dostęp w innych klasach                                    |
| ---------------- | ------------------------- | ---------------------------- | ---------------------------------------------------------- |
| `public`         | ✅ Tak                     | ✅ Tak                        | ✅ Tak                                                      |
| `protected`      | ✅ Tak                     | ✅ Tak                        | ❌ Nie (tylko w tym samym pakiecie lub przez dziedziczenie) |
| `default` (brak) | ✅ Tak                     | ✅ Tylko w tym samym pakiecie | ❌ Nie                                                      |
| `private`        | ✅ Tak                     | ❌ Nie                        | ❌ Nie                                                      |

---

### Słowo kluczowe `super`

`super` odnosi się do klasy nadrzędnej (superklasy).
Używa się go w trzech kontekstach:

1. **Do wywołania konstruktora klasy nadrzędnej:**

```java
class Osoba {
    String imie;
    Osoba(String imie) {
        this.imie = imie;
    }
}

class Student extends Osoba {
    int indeks;
    
    Student(String imie, int indeks) {
        super(imie);      // wywołanie konstruktora z klasy Osoba
        this.indeks = indeks;
    }
}
```

2. **Do wywołania metody klasy nadrzędnej:**

```java
class Zwierze {
    void wydajDzwiek() {
        System.out.println("Zwierzę wydaje dźwięk");
    }
}

class Pies extends Zwierze {
    @Override
    void wydajDzwiek() {
        super.wydajDzwiek(); // wywołanie oryginalnej metody
        System.out.println("Hau hau!");
    }
}
```

3. **Do uzyskania dostępu do pola klasy nadrzędnej:**

```java
class Rodzic {
    String nazwisko = "Kowalski";
}

class Dziecko extends Rodzic {
    String nazwisko = "Nowak";
    
    void pokazNazwiska() {
        System.out.println(nazwisko);        // Nowak
        System.out.println(super.nazwisko);  // Kowalski
    }
}
```

---

### Nadpisywanie metod (`@Override`)

Podklasa może **zmienić zachowanie** metody z klasy nadrzędnej, używając adnotacji `@Override`.

```java
class Zwierze {
    void wydajDzwiek() {
        System.out.println("Zwierzę wydaje dźwięk");
    }
}

class Kot extends Zwierze {
    @Override
    void wydajDzwiek() {
        System.out.println("Miau!");
    }
}
```

**Zasady:**

* Sygnatura metody (nazwa i parametry) musi być taka sama.
* Typ zwracany może być taki sam lub bardziej szczegółowy (*covariant return*).
* Modyfikator dostępu nie może być bardziej restrykcyjny.

---

### Dziedziczenie wielopoziomowe

W Javie można tworzyć **łańcuch dziedziczenia** — klasa może dziedziczyć po klasie, która sama jest podklasą innej.

```java
class Pojazd {
    void rusz() { System.out.println("Pojazd rusza"); }
}

class Samochod extends Pojazd {
    void zatrzymaj() { System.out.println("Samochód zatrzymuje się"); }
}

class ElektrycznySamochod extends Samochod {
    void ladowanie() { System.out.println("Ładowanie baterii"); }
}
```

Klasa `ElektrycznySamochod` ma dostęp do metod `rusz()` i `zatrzymaj()`.

---

### Słowo kluczowe `final` i dziedziczenie

Słowo `final` ma szczególne znaczenie w kontekście dziedziczenia:

| Co oznacza `final` | Przykład                 | Efekt                          |
| ------------------ | ------------------------ | ------------------------------ |
| **Finalna klasa**  | `final class Utils {}`   | Nie można po niej dziedziczyć  |
| **Finalna metoda** | `final void metoda() {}` | Nie można jej nadpisać         |
| **Finalne pole**   | `final int x = 5;`       | Wartość nie może być zmieniona |

---

### Hierarchia klas w praktyce

#### Przykład: System zarządzania pracownikami

```java
class Pracownik {
    protected String imie;
    protected double pensja;

    public Pracownik(String imie, double pensja) {
        this.imie = imie;
        this.pensja = pensja;
    }

    public void pokazDane() {
        System.out.println(imie + " zarabia " + pensja + " zł");
    }
}

class Kierownik extends Pracownik {
    private String dzial;

    public Kierownik(String imie, double pensja, String dzial) {
        super(imie, pensja);
        this.dzial = dzial;
    }

    @Override
    public void pokazDane() {
        System.out.println(imie + " (kierownik " + dzial + ") zarabia " + pensja + " zł");
    }
}
```

---

## **Klasa Abstrakcyjna w Java**

---

### Co to jest klasa abstrakcyjna?

**Klasa abstrakcyjna** to klasa, która:

* nie może być bezpośrednio utworzona (czyli nie można zrobić `new`),
* może zawierać **metody abstrakcyjne** (bez implementacji),
* oraz **metody konkretne** (z kodem).

> Krótko mówiąc — to **szablon** dla innych klas.
> Służy do **definiowania wspólnych cech i zachowań**, ale **pozostawia szczegóły** implementacji podklasom.

---

#### Przykład:

```java
abstract class Zwierze {
    abstract void wydajDzwiek(); // metoda abstrakcyjna (bez ciała)

    void spij() {                // metoda konkretna
        System.out.println("Zzz...");
    }
}
```

Nie możesz zrobić tego:

```java
Zwierze z = new Zwierze(); // ❌ Błąd kompilacji!
```

Ale możesz to:

```java
class Pies extends Zwierze {
    @Override
    void wydajDzwiek() {
        System.out.println("Hau hau!");
    }
}
```

Teraz:

```java
Zwierze z = new Pies();
z.wydajDzwiek(); // Hau hau!
z.spij();        // Zzz...
```

> **!!UWAGA!!**
> 
> Jeżeli klasa abstrakcyjna ma również metody abstrakcyjne muszą być one nadpisane w klasie podrzędnej.

---

### Po co używać klas abstrakcyjnych?

| Cel                                        | Opis                                                                     |
| ------------------------------------------ | ------------------------------------------------------------------------ |
| **Wspólny szkielet**                       | Ustalamy wspólne metody i pola dla grupy klas.                           |
| **Częściowa implementacja**                | Niektóre metody mogą być już gotowe, inne – tylko zadeklarowane.         |
| **Polimorfizm**                            | Umożliwiają wywołanie metod w sposób ogólny dla wielu typów.             |
| **Bezpośrednia współpraca z interfejsami** | Często klasa abstrakcyjna implementuje interfejs i dostarcza część kodu. |

---

### Składnia

```java
abstract class NazwaKlasy {
    // pola (mogą być prywatne, protected, public)
    
    // konstruktor (tak, klasy abstrakcyjne mogą mieć konstruktor!)
    
    // metody konkretne

    // metody abstrakcyjne
}
```

---

### Przykład praktyczny – pojazdy

```java
abstract class Pojazd {
    abstract void uruchomSilnik();
    
    void zatrzymaj() {             
        System.out.println("Pojazd się zatrzymał.");
    }
}

class Samochod extends Pojazd {
    @Override
    void uruchomSilnik() {
        System.out.println("Silnik samochodu: wroom!");
    }
}

class Motocykl extends Pojazd {
    @Override
    void uruchomSilnik() {
        System.out.println("Motocykl: brum brum!");
    }
}

public class Main {
    static void main(String[] args) {
        Pojazd p1 = new Samochod();
        Pojazd p2 = new Motocykl();

        p1.uruchomSilnik(); // wroom!
        p2.uruchomSilnik(); // brum brum!
        p1.zatrzymaj();     // wspólna metoda
    }
}
```

---

### Klasa abstrakcyjna a konstruktor

Tak – **klasa abstrakcyjna może mieć konstruktor**, który jest wywoływany przy tworzeniu obiektu podklasy.

```java
abstract class Zwierze {
    Zwierze() {
        System.out.println("Tworzę zwierzę");
    }
}

class Pies extends Zwierze {
    Pies() {
        System.out.println("Tworzę psa");
    }
}

public class Main {
    static void main(String[] args) {
        new Pies();
    }
}
```

Wynik:

```
Tworzę zwierzę
Tworzę psa
```

---

### Kiedy używać klasy abstrakcyjnej?

Użyj klasy abstrakcyjnej, gdy:

- kilka klas **dzieli wspólne zachowanie**, ale nie powinno się ich tworzyć samodzielnie,
- chcesz **wymusić implementację** pewnych metod w klasach pochodnych,
- potrzebujesz **pól lub metod z kodem**, których nie mogą mieć interfejsy (np. prywatne zmienne instancji).

---

### Kiedy NIE używać klasy abstrakcyjnej?

- jeśli nie planujesz dziedziczenia,
- jeśli klasa ma tylko metody abstrakcyjne – wtedy lepszy jest **interfejs**,
- jeśli chcesz łączyć różne, niezależne typy zachowań – interfejsy są elastyczniejsze (bo można ich wiele implementować).

---

## Zadania
1. Wykonaj kolejno poniższe czynności:
   1. Stwórz klasę bazową Person z prywatnym polem firstName oraz chronionym polem
   lastName. Następnie stwórz klasę Employee, która dziedziczy po klasie Person. W
   klasie Employee próbuj odnieść się do obu pól i zauważ, które z nich są dostępne.
   2. Na bazie klasy Person z poprzedniego podpunktu, stwórz metody dostępowe (gettery) dla obu pól. W klasie Employee stwórz metodę displayData, która korzysta
   z tych metod dostępowych, aby wypisać informacje o pracowniku. Zastanów się,
   dlaczego metody dostępowe są używane do dostępu do prywatnych pól.
   3. Przenieś teraz obydwie klasy do innych pakietów. Sprawdź dostęp do pola chronionego z i bez metod dostępowych. Czy jest jakakolwiek różnica? Stwórz klasy z metodą startową (*public static void main*) w różnych pakietach. Zastanów się nad wynikiem.
2.  Stwórz klasę bazową o nazwie Vehicle z polami: brand i model. Klasa ta powinna
    posiadać konstruktor przyjmujący oba te parametry. Następnie stwórz klasę potomną o
    nazwie Car, która dziedziczy po klasie Vehicle. Klasa Car powinna posiadać dodatkowe
    pole numberOfDoors. Stwórz konstruktor dla klasy Car, który przyjmuje wszystkie trzy
    parametry i korzysta z konstruktora klasy bazowej. Stwórz przypadek testowy.
3.  Stwórz klasę Game, która w swoim konstruktorze ma metodę finalną initialize (inicjalizującą pewne dane). Utwórz klasę potomną RPG, która próbuje dostosować konstruktor
    klasy bazowej. Upewnij się, że metoda initialize działa poprawnie, mimo że jest oznaczona jako final.
4.  Zdefiniuj abstrakcyjną klasę WorkTool z polami name typu String oraz productionYear
    typu int. Dodaj metodę abstrakcyjną use(), która będzie symulować użycie narzędzia. Następnie zdefiniuj klasy Hammer, Screwdriver i Saw, które dziedziczą po klasie
    WorkTool i implementują metodę use(). Stwórz przypadki testowe dla obiektów każdej z klas dziedziczących
    i wywołaj dla nich napisaną metodę.
5. Napisz klasę abstrakcyjną Item z polem name oraz metodą abstrakcyjną use(). Następnie stwórz dwie klasy *Weapon* i *Armor* dziedziczące po klasie Item. Klasa *Weapon*
   powinna posiadać dodatkowe pole damage, a klasa *Armor* dodatkowe pole defense.
   1. Utwórz klasę Fighter, która będzie posiadać pola:
      - (opcjonalne) `armor` typu złożonego *Armor*,
      - listę broni `weapons` typu złożonego *ArrayList<Weapon>*.
   
       Dodatkowo do tej klasy utwórz gettery i settery dla jego pól. Stwórz przypadek testowy przynajmniej z dwoma brońmi. Sprawdź czy zachowane są zasady hermetyzacji.
       Co trzeba wykonać, aby były spełnione?
   2. Dodaj do klasy Fighter metodę useEquipment(), która wykona metody use dla pól klasy Fighter. Sprawdź jej działanie.