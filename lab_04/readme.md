# **Programowanie Obiektowe – Java (2025Z)**

## Pakiety, hermetyzacja, statyczność

---

Jedno słowo przed startem - jeżeli chcą państwo zacząć dokumentować swój kod profesjonalnie warto zajrzeć 
jak powinno się pisać tzw. `Doc Comments`, co jest odpowiednikiem *String Doc* w innych językach. Przydatny link: https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html .

## **Pakiety (Packages) w języku Java**

### 1. Co to jest pakiet?

> **Pakiet (package)** to sposób **grupowania klas, interfejsów i podpakietów** w logiczne moduły.

Pakiety pomagają:

* uporządkować kod w dużych projektach,
* unikać konfliktów nazw klas,
* kontrolować dostęp do klas i metod,
* lepiej zarządzać hermetyzacją.

---

### 2. Struktura i koncepcja pakietów

Każdy plik `.java` może należeć do **jednego pakietu**.
Pakiety odpowiadają strukturze katalogów w projekcie.

Przykład struktury projektu:

```
src/
 └── pl/
     └── uczelnia/
         ├── model/
         │    └── Student.java
         └── main/
              └── Main.java
```

#### W pliku `Student.java`:

```java
package pl.uczelnia.model;  // deklaracja pakietu

public class Student {
    private String imie;
    private int indeks;

    public Student(String imie, int indeks) {
        this.imie = imie;
        this.indeks = indeks;
    }

    public void przedstawSie() {
        System.out.println("Jestem " + imie + ", mój numer indeksu to " + indeks);
    }
}
```

#### W pliku `Main.java`:

```java
package pl.uczelnia.main;

import pl.uczelnia.model.Student; // import klasy z innego pakietu

public class Main {
    public static void main(String[] args) {
        Student s = new Student("Adrian", 12345);
        s.przedstawSie();
    }
}
```

**Efekt działania:**

```
Jestem Adrian, mój numer indeksu to 12345
```

---

### 3. Dlaczego warto używać pakietów?

| Zaleta                        | Opis                                                            |
| ----------------------------- | --------------------------------------------------------------- |
|  **Organizacja kodu**         | Dzielisz projekt na moduły (np. `model`, `service`, `utils`).   |
|  **Unikanie konfliktów nazw** | Możesz mieć różne klasy o tej samej nazwie w różnych pakietach. |
|  **Kontrola dostępu**         | Klasy bez `public` są widoczne tylko w swoim pakiecie.          |
|  **Łatwiejsza nawigacja**     | IDE (np. IntelliJ, Eclipse) grupują klasy według pakietów.      |
|  **Reużywalność kodu**        | Możesz importować pakiety do innych projektów.                  |

---

### 4. Jak używać pakietów w praktyce

#### **Deklaracja pakietu**

Na początku każdego pliku `.java`, który jest w pakiecie:

```java
package nazwa.pakietu;
```

#### **Importowanie klas**

Aby użyć klasy z innego pakietu:

```java
import nazwa.pakietu.NazwaKlasy;
```

lub z użyciem *Wild Card* (co nie jest zalecan, gdyż nie zawsze potrzebujemy wszystkiego z danego pakietu):

```java
import nazwa.pakietu.*; // importuje wszystkie klasy z pakietu
```

#### **Brak deklaracji package**

Jeśli nie podasz `package`, klasa trafi do **domyślnego pakietu** (co nie jest zalecane w większych projektach).

---

### 5. Dostępność między pakietami

| Modyfikator                   | W tym samym pakiecie | W innym pakiecie       |
| ----------------------------- | -------------------- | ---------------------- |
| `public`                      | ✅                    | ✅                      |
| `protected`                   | ✅                    | ✅ (tylko w podklasach) |
| `default` (brak modyfikatora) | ✅                    | ❌                      |
| `private`                     | ❌                    | ❌                      |

*To pokazuje, że pakiety są ściśle związane z hermetyzacją.*

---

### 6. Pakiety z podpakietami

Java pozwala tworzyć **hierarchię pakietów**, np.:

```
pl.uczelnia
├── model
│   ├── Student.java
│   └── Pracownik.java
└── utils
    └── Walidator.java
```

Pakiety **nie dziedziczą** klas z podpakietów.
`pl.uczelnia.model` nie ma automatycznego dostępu do `pl.uczelnia.utils` — trzeba je **importować**.

---


### 7. Dobre praktyki przy pracy z pakietami

| Zasada                                                                     | Dlaczego warto                       |
| -------------------------------------------------------------------------- | ------------------------------------ |
| Nazwy pakietów pisz **małymi literami**                                    | Konwencja Javy                       |
| Używaj pełnych nazw domenowych odwróconych (np. `pl.uczelnia`, `com.firma`) | Zapobiega kolizjom nazw              |
| Dziel pakiety tematycznie                                                  | np. `model`, `service`, `controller` |
| Nie mieszaj klas niepowiązanych funkcjonalnie                             | Trudniejsza konserwacja kodu         |
| Używaj importów jawnych, nie `*` w dużych projektach                     | Zwiększa czytelność i bezpieczeństwo |

---

## **Hermetyzacja (Encapsulation) w Javie**

---

### 1. Definicja – co to jest hermetyzacja?

> **Hermetyzacja (enkapsulacja)** to **ukrywanie szczegółów wewnętrznych obiektu** przed światem zewnętrznym
> i **kontrolowanie dostępu** do danych za pomocą **metod** (getterów i setterów).

Czyli:

* Dane obiektu są **prywatne (private)**,
* Dostęp do nich jest możliwy **tylko przez publiczne metody**.

Hermetyzacja zapewnia **bezpieczeństwo danych**, **spójność stanu obiektu** i **łatwiejsze utrzymanie kodu**.

---

### 2. Dlaczego hermetyzacja jest ważna?

Wyobraź sobie klasę `KontoBankowe`.
Jeśli pole `saldo` byłoby publiczne, ktoś mógłby zrobić:

```java
konto.saldo = 10000000; //10 mln na koncie :)
```

Hermetyzacja temu zapobiega – bo **nie można zmienić pola bez kontroli**.

---

### 3. Zasada hermetyzacji krok po kroku

| Krok | Działanie                                                       | Cel                                                                                           |
| ---- |-----------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| 1️⃣  | Pola klasy ustawiamy jako **`private`**                         | Ukrywamy dane                                                                                 |
| 2️⃣  | Tworzymy **gettery i settery**                                  | Kontrolujemy dostęp. Czy zawsze Setter będzie na miejscu? Czy zawsze Getter będzie potrzebny? |
| 3️⃣  | Powinniśmy dodać **walidację danych**, tam gdzie jest potrzebna | Chronimy obiekt przed błędnymi wartościami                                                    |

---

### 4. Przykład – hermetyzacja w praktyce

#### Deklaracja klasy

```java
public class KontoBankowe {
    // Pola prywatne – niedostępne z zewnątrz
    private String wlasciciel;
    private double saldo;

    // Konstruktor
    public KontoBankowe(String wlasciciel, double saldoPoczatkowe) {
        this.wlasciciel = wlasciciel;
        this.saldo = saldoPoczatkowe;
    }

    // 🔹 Getter – tylko odczyt
    public double getSaldo() {
        return saldo;
    }

    // 🔹 Setter z walidacją – nie pozwala na ujemne saldo
    public void setSaldo(double noweSaldo) {
        if (noweSaldo >= 0) {
            saldo = noweSaldo;
        } else {
            System.out.println("❌ Nie można ustawić ujemnego salda!");
        }
    }

    // Metoda publiczna do wpłaty
    public void wplata(double kwota) {
        saldo += kwota;
        System.out.println("💰 Wpłacono " + kwota + " zł. Nowe saldo: " + saldo);
    }

    // Metoda publiczna do wypłaty
    public void wyplata(double kwota) {
        if (kwota <= saldo) {
            saldo -= kwota;
            System.out.println("🏦 Wypłacono " + kwota + " zł. Pozostało: " + saldo);
        } else {
            System.out.println("❌ Brak środków!");
        }
    }
}
```

#### Użycie klasy w main:

```java
public class Main {
    public static void main(String[] args) {
        KontoBankowe konto = new KontoBankowe("Adrian", 1000);

        konto.wplata(500);
        konto.wyplata(200);

        System.out.println("Saldo: " + konto.getSaldo());

        konto.setSaldo(-100); // próba ustawienia niepoprawnej wartości
    }
}
```

**Wynik:**

```
💰 Wpłacono 500.0 zł. Nowe saldo: 1500.0
🏦 Wypłacono 200.0 zł. Pozostało: 1300.0
Saldo: 1300.0
❌ Nie można ustawić ujemnego salda!
```

---

Ale czy aby na pewno to jest dobre? No nie! Możemy sobie ustawić saldo tak o na dowolną kwotę. Przeanalizujmy poniższy przykład:

#### Deklaracja poprawionej klasy

```java
public class KontoBankowe {
    private String wlasciciel;
    private double saldo;

    public KontoBankowe(String wlasciciel, double saldoPoczatkowe) {
        this.wlasciciel = wlasciciel;
        this.saldo = saldoPoczatkowe;
    }

    public double getSaldo() {
        return saldo;
    }

    public void wplata(double wplata){
        if (wplata > 0){
            this.saldo += wplata;
            System.out.println("Wpłacono na konto " + wplata + "PLN. Twój stan konta: " + this.saldo + "PLN.");
        }
        else{
            System.out.println("Błędna kwota wpłaty!");
        }
    }

    public void wyplata(double wyplata){
        if (wyplata > 0 && wyplata <= this.saldo){
            this.saldo -= wyplata;
            System.out.println("Wypłacono z konta " + wyplata + "PLN. Twój stan konta: " + this.saldo + "PLN.");
        }
        else {
            System.out.println("Błędna kwota wypłaty lub niewystarczające środki na koncie!");
        }
    }
}
```

#### Użycie klasy:

```java
public class Main {
    public static void main(String[] args) {
        KontoBankowe konto = new KontoBankowe("Adrian", 1000);

        konto.wplata(500);
        konto.wyplata(200);

        System.out.println("Saldo: " + konto.getSaldo());
    }
}
```

### 5. Jak rozpoznać dobrze zhermetyzowaną klasę?

| Cechy dobrej hermetyzacji                                        | Opis                                            |
|------------------------------------------------------------------| ----------------------------------------------- |
| Wszystkie pola są `private`(chyba, że są statyczne i niezmienne) | Dane są ukryte przed światem zewnętrznym        |
| Dostęp przez gettery/settery                                     | Klasa sama decyduje, kto i jak zmienia jej dane |
| Zawiera walidację                                                | Nie pozwala na błędne stany obiektu             |
| Nie ma „gołych” pól publicznych                                  | Brak możliwości bezpośredniej manipulacji       |

---

### 6. Korzyści hermetyzacji

| Korzyść                           | Opis                                                 |
| --------------------------------- | ---------------------------------------------------- |
| **Bezpieczeństwo danych**      | Chroni pola przed niekontrolowanym dostępem          |
| **Łatwiejsze utrzymanie kodu** | Można zmienić implementację bez wpływu na inne klasy |
| **Kontrola wartości pól**      | Walidacja w setterach                                |
| **Czytelność i spójność**      | Obiekt ma jasne reguły użycia                        |
| **Łatwość testowania**         | Każda metoda ma jasno określony cel                  |

---

### 7. Hermetyzacja a inne filary OOP

| Filar             | Związek z hermetyzacją                                       |
| ----------------- | ------------------------------------------------------------ |
| **Abstrakcja**    | Hermetyzacja ukrywa szczegóły implementacji.                 |
| **Dziedziczenie** | Chronione pola (`protected`) mogą być dziedziczone.          |
| **Polimorfizm**   | Hermetyzacja wspiera elastyczne rozszerzanie zachowań metod. |

---

### 8. Szybkie porównanie dostępu do pól (przypomnienie)

| Modyfikator | Dostęp wewnątrz klasy | Dostęp z podklas | Dostęp z innej klasy |
| ----------- | --------------------- | ---------------- | -------------------- |
| `private`   | ✅                     | ❌                | ❌                    |
| `protected` | ✅                     | ✅                | ❌                    |
| `public`    | ✅                     | ✅                | ✅                    |

---

## **Statyczność w języku Java (`static`)**

---

### 1. Co oznacza słowo kluczowe `static`?

> Słowo kluczowe **`static`** oznacza, że dany **element należy do klasy**, a nie do konkretnego obiektu tej klasy.

Innymi słowy:

* **Pola statyczne** – są wspólne dla wszystkich obiektów klasy.
* **Metody statyczne** – można je wywołać bez tworzenia obiektu.
* **Bloki statyczne** – wykonują się tylko raz, przy pierwszym załadowaniu klasy.

---

### 2. Kluczowa różnica: klasa vs obiekt

| Element                          | Opis                                  |
| -------------------------------- | ------------------------------------- |
| **Pole instancyjne (bez static)** | Każdy obiekt ma swoją kopię pola      |
| **Pole statyczne (z static)**    | Wspólne dla wszystkich obiektów klasy |

---

#### Przykład porównawczy

```java
public class Student {
    String imie;              // pole instancyjne
    static String uczelnia;   // pole statyczne (wspólne)

    public Student(String imie) {
        this.imie = imie;
    }
}
```

#### Użycie:

```java
public class Main {
    public static void main(String[] args) {
        Student.uczelnia = "Politechnika Warszawska"; // dostęp bez obiektu

        Student s1 = new Student("Adrianna");
        Student s2 = new Student("Tomasz");

        System.out.println(s1.imie + " studiuje w " + Student.uczelnia);
        System.out.println(s2.imie + " studiuje w " + Student.uczelnia);

        Student.uczelnia = "Uniwersytet Warszawski"; // zmiana wspólnej wartości

        System.out.println(s1.imie + " studiuje teraz w " + Student.uczelnia);
        System.out.println(s2.imie + " studiuje teraz w " + Student.uczelnia);
    }
}
```

**Wynik:**

```
Adrianna studiuje w Politechnika Warszawska
Tomasz studiuje w Politechnika Warszawska
Adrianna studiuje teraz w Uniwersytet Warszawski
Tomasz studiuje teraz w Uniwersytet Warszawski
```

*Widzisz? Zmieniliśmy jedną wartość – i zmieniło się dla wszystkich obiektów.*

---

### 3. Statyczne metody

> **Metoda statyczna** to taka, którą można wywołać **bez tworzenia obiektu**.

```java
public class Matematyka {
    public static int dodaj(int a, int b) {
        return a + b;
    }
}
```

#### Użycie:

```java
public class Main {
    public static void main(String[] args) {
        int wynik = Matematyka.dodaj(5, 3);
        System.out.println("Wynik dodawania: " + wynik);
    }
}
```

**Wynik:**

```
Wynik dodawania: 8
```

Nie musimy pisać `new Matematyka()`, bo metoda należy do klasy, a nie do obiektu.

---

### 4. Ograniczenia metod statycznych

| Czego nie wolno w metodzie `static`                   | Dlaczego                                           |
| ----------------------------------------------------- | -------------------------------------------------- |
| ❌ Nie można używać `this`                             | Nie ma obiektu, więc nie ma odniesienia do `this`  |
| ❌ Nie można odwoływać się do pól/metod niestatycznych | Bo nie wiadomo, do którego obiektu miałyby należeć |
| ✅ Można używać tylko pól/metod statycznych            | Bo są wspólne dla wszystkich                       |

---

### 5. Bloki statyczne

> **Blok statyczny** wykonuje się tylko **raz**, przy pierwszym załadowaniu klasy do pamięci.

```java
public class Aplikacja {
    static {
        System.out.println("Ładowanie aplikacji...");
    }

    public static void main(String[] args) {
        System.out.println("Start programu");
    }
}
```

**Wynik:**

```
Ładowanie aplikacji...
Start programu
```

Bloki statyczne często służą do **inicjalizacji pól statycznych**, np. ładowania konfiguracji.

---

### 6. Statyczne pola i metody w praktyce

#### Przykład klasy pomocniczej:

```java
public class Konwerter {
    public static double mileNaKilometry(double mile) {
        return mile * 1.60934;
    }

    public static double kilometryNaMile(double km) {
        return km / 1.60934;
    }
}

public class Main {
    public static void main(String[] args) {
        System.out.println("10 mil to " + Konwerter.mileNaKilometry(10) + " km");
        System.out.println("5 km to " + Konwerter.kilometryNaMile(5) + " mil");
    }
}
```

**Wynik:**

```
10 mil to 16.0934 km
5 km to 3.1068559611866697 mil
```

Takie klasy to tzw. **utility classes** – z samymi metodami statycznymi.

---

### 7. Statyczne zmienne i liczniki

Statyczne pola mogą też służyć do zliczania utworzonych obiektów:

```java
public class Pracownik {
    private String imie;
    private static int liczbaPracownikow = 0;

    public Pracownik(String imie) {
        this.imie = imie;
        liczbaPracownikow++;
    }

    public static int getLiczbaPracownikow() {
        return liczbaPracownikow;
    }
}

public class Main {
    public static void main(String[] args) {
        new Pracownik("Adrianna");
        new Pracownik("Tomasz");
        new Pracownik("Ola");

        System.out.println("Liczba pracowników: " + Pracownik.getLiczbaPracownikow());
    }
}
```

**Wynik:**

```
Liczba pracowników: 3
```

---

### 8. Statyczne klasy wewnętrzne

> W Javie możesz mieć **klasy zagnieżdżone** – a niektóre z nich mogą być `static`.

```java
public class Zewnetrzna {
    static class Wewnetrzna {
        static void witaj() {
            System.out.println("Witaj z klasy wewnętrznej!");
        }
    }

    public static void main(String[] args) {
        Wewnetrzna.witaj();
    }
}
```

**Wynik:**

```
Witaj z klasy wewnętrznej!
```

### 9. Import statyczny w języku Java

Normalnie, kiedy chcemy użyć klasy lub metody z innego pakietu, piszemy np.:

```java
import java.lang.Math;
```

a potem wywołujemy metody w ten sposób:

```java
double wynik = Math.pow(2, 3);
```

Ale jeśli użyjemy **importu statycznego**, możemy **pominąć nazwę klasy** przed metodą lub polem.
Dzięki temu kod staje się **krótszy i bardziej czytelny**.

---

#### Składnia importu statycznego

```java
import static <pełna_nazwa_klasy>.<nazwa_metody_lub_pola>;
```

albo, jeśli chcemy zaimportować **wszystkie** metody i pola statyczne z danej klasy:

```java
import static <pełna_nazwa_klasy>.*;
```

---

##### Przykład 1: Import pojedynczej metody

```java
import static java.lang.Math.sqrt;

public class Main {
    public static void main(String[] args) {
        double x = 16;
        System.out.println(sqrt(x)); // nie musimy pisać Math.sqrt()
    }
}
```

---

##### Przykład 2: Import wszystkich metod z klasy Math

```java
import static java.lang.Math.*;

public class Main {
    public static void main(String[] args) {
        System.out.println(pow(2, 5));   // zamiast Math.pow(2, 5)
        System.out.println(sqrt(81));   // zamiast Math.sqrt(81)
        System.out.println(abs(-12));   // zamiast Math.abs(-12)
    }
}
```

---

##### Przykład 3: Import statycznych stałych

```java
import static java.lang.Math.PI;

public class Main {
    public static void main(String[] args) {
        double r = 3;
        double pole = PI * r * r;
        System.out.println("Pole koła: " + pole);
    }
}
```

---

#### Dlaczego warto używać importu statycznego?

| Zaleta                               | Opis                                                                                                   |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| ✅ **Czytelniejszy kod**              | Nie trzeba pisać `Math.` przed każdą metodą.                                                           |
| ✅ **Mniej powtórzeń**                | W kodzie, który często używa metod z jednej klasy (np. `Math`, `Collections`, `Assertions` w testach). |
| ✅ **Wygoda w testach jednostkowych** | W testach JUnit często importuje się statycznie metody `assertEquals`, `assertTrue`, itp.              |

---

#### Uwaga – kiedy **nie** warto używać?

| Problem                                | Dlaczego to zły pomysł                                                                        |
| -------------------------------------- | --------------------------------------------------------------------------------------------- |
| ❌ Kod traci kontekst                   | Gdy używasz wielu importów statycznych, trudno zgadnąć, skąd pochodzi dana metoda.            |
| ❌ Konflikty nazw                       | Jeśli różne klasy mają metody o tych samych nazwach (np. `max()`), Java nie wie, której użyć. |
| ❌ Niezrozumiały kod dla początkujących | Studenci widzą `pow(2,3)` i nie wiedzą, z jakiej klasy to pochodzi.                           |

---

##### Przykład konfliktu nazw:

```java
import static java.lang.Math.*;
import static java.lang.Integer.*;

public class Main {
    public static void main(String[] args) {
        System.out.println(max(10, 20));  // ❌ Błąd kompilacji — która metoda max()?
    }
}
```

Kompilator nie wie, czy użyć `Math.max()` czy `Integer.max()`.

---

### 10. Podsumowanie

| Element              | Co oznacza `static`                       | Dostęp                  | Przykład użycia            |
| -------------------- | ----------------------------------------- | ----------------------- | -------------------------- |
| **Pola**             | Wspólne dla wszystkich obiektów           | Przez `NazwaKlasy.pole` | `Student.uczelnia`         |
| **Metody**           | Wywoływane bez tworzenia obiektu          | `NazwaKlasy.metoda()`   | `Math.pow(2, 3)`           |
| **Bloki**            | Kod wykonywany raz przy starcie           | automatycznie           | `static { ... }`           |
| **Klasy wewnętrzne** | Zagnieżdżone, ale niezależne od instancji | `Zewnetrzna.Wewnetrzna` | Wzorce projektowe, helpery |

---

### 11. Dobre praktyki przy użyciu `static`

| Zasada                                                                     | Dlaczego                          |
| -------------------------------------------------------------------------- | --------------------------------- |
| Używaj `static` dla **narzędzi i funkcji pomocniczych**                    | np. `Math`, `Collections`         |
| Nie nadużywaj `static`                                                    | Utrudnia testowanie i obiektowość |
| Używaj `static` do **liczników i współdzielonych danych**                | np. liczba obiektów               |
| Nie używaj `static` w miejscach, gdzie każdy obiekt powinien mieć swoje dane | np. `saldo`, `wiek`, `imie`       |

---

## Zadania

1.  Stwórz pakiet o nazwie animals. Wewnątrz tego pakietu utwórz dwie klasy: Dog i
    Cat. Oba powinny zawierać pola name i age. Każda z klas powinna posiadać metodę
    makeSound(). Dla klasy Dog metoda ta powinna drukować “Woof, woof!”, a dla klasy
    Cat- “Meow!”. W klasie testującej, poza tym pakietem, stwórz obiekty obu klas, nadaj
    im wartości i wywołaj ich metody makeSound().
2.  Stwórz dwa pakiety: books i library. W pakiecie books stwórz klasę Book z polami
    title, author i publicationYear. W pakiecie library stwórz klasę Shelf zawierającą
    listę książek oraz metody umożliwiające umożliwiające dodawanie i usuwanie książek.
    Aby korzystać z klasy Book w pakiecie library, musisz zaimportować odpowiedni pakiet.
    W klasie testującej, stwórz kilka książek, dodaj je do półki i wydrukuj zawartość półki.
3.  Stwórz klasę Product, która zawiera pole statyczne numberOfProducts oraz pole sta
    tyczne MAX_PRODUCTS. Pole numberOfProducts będzie służyć do zliczania ilości utworzą
    nych produktów, a MAX_PRODUCTS do ograniczenia ich liczby. Oznacz tylko jedno z tych
    pól słowem kluczowym final i zastanów się nad konsekwencjami tego wyboru.
4. Napisz klasę KalkulatorStatyczny, która ma:
   * 3 metody statyczne: dodaj, odejmij, pomnoz
   * statyczne pole liczbaOperacji zliczające ile razy użyto jakiejkolwiek metody.
   Każda metoda powinna zwiększać licznik o 1 i zwracać wynik.
5. Napisz klasę Szkoła, która ma prywatne pole uczniowie przyjmujące listę uczniów 
w formie tekstowej (imię i nazwisko). Dodaj odpowiednie metody, aby móc dodać ucznia i 
zwrócić listę uczniów. Utwórz przypadek testowy. Spróbuj przypisać listę zwróconą przez obiekt
testowy do zmiennej, a następnie zmienić tą listę (np. usunąć jeden element). Czy wpłynęło 
to na zawartość oryginalnej listy obiektu? Czy jest to zgodne z ideą hermetyzacji? 
Dokonaj potrzebnych zmian, aby nie dało się tego zmienić.
