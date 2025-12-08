# **Programowanie Obiektowe – Java (2025Z)**

# Lab 09: Programowanie generyczne

Programowanie generyczne (ang. *generics*) to mechanizm Javy pozwalający:

* tworzyć klasy, interfejsy i metody działające na **nieokreślonych jeszcze typach**,
* zwiększyć bezpieczeństwo typów podczas kompilacji,
* unikać rzutowania,
* pisać kod wielokrotnego użytku (reusable).

Programowanie generyczne to koncepcja w programowaniu, która pozwala na pisanie kodu, który może być używany z różnymi typami danych, bez konieczności powtarzania tego samego kodu dla każdego typu danych. Jest to szczególnie przydatne w językach programowania silnie typowanych, takich jak Java, C# czy C++, gdzie typy danych muszą być określone podczas kompilacji.

---

## 1. **Co to są generyki?**

Generyki pozwalają parametryzować typy:

```java
List<String> names = new ArrayList<>();
```

`List<T>` jest **szablonem**, a `T` to **parametr typu**, który w czasie użycia zastępujemy konkretnym typem (tutaj `String`).

To działa także dla:

* klas (`class Box<T>`)
* interfejsów (`interface Repository<T>`)
* metod (`<T> void print(T value)`)
* konstruktorów (`<T> MyClass(T value)`)

---

## 2. **Prosty przykład klasy generycznej**

```java
public class Box<T> {
    private T value;

    public Box(T value) { this.value = value; }

    public T getValue() { return value; }

    public void setValue(T value) { this.value = value; }
}
```

Użycie:

```java
Box<Integer> box = new Box<>(10);
int number = box.getValue(); // brak rzutowania
```

---

## 3. **Typy surowe (raw types)**

```java
List list = new ArrayList();
```

To **wyłącza mechanizm generyków**, prowadząc do błędów:

Zawsze pisz:

```java
List<Object> list = new ArrayList<>();
```

---

## 4. **Parametry typu — nazwy i konwencje**

Standardowe litery:

| Litera | Znaczenie                  |
| ------ | -------------------------- |
| **T**  | Type                       |
| **E**  | Element (np. w kolekcjach) |
| **K**  | Key                        |
| **V**  | Value                      |
| **R**  | Result                     |
| **N**  | Number                     |

---

## 5. **Wielokrotne parametry typu**

```java
public class Pair<K, V> {
    private K key;
    private V value;
}
```

---

## 6. **Metody generyczne**

Metody mogą mieć własny parametr typu, niezależny od klasy:

```java
public static <T> void print(T value) {
    System.out.println(value);
}
```

---

## 7. **Ograniczenia typów (bounded types)**

### 7.1. Ograniczenie górne (extends)

\<T extends typ_graniczny\> jest składnią używaną w programowaniu generycznym do określenia górnej granicy dla typu generycznego T. Oznacza to, że T musi być podtypem (klasą pochodną) klasy określonej jako typ_graniczny lub sama być tym typem.


```java
public class MathBox<T extends Number> {
    T number;
}
```


Pozwala używać tylko podtypów `Number` → np. `Integer`, `Double`.

---

### 7.2. Ograniczenie dolne (super)

\<? super typ_graniczny\> jest składnią używaną w programowaniu generycznym do określenia dolnej granicy wildcarda `?`.
Oznacza to, że argument typu (np. List<?>) musi być nadtypem typ_graniczny lub nim samym.
Pozwala to dodawać elementy typu typ_graniczny, ale wartości odczytywane mają typ `Object`, ponieważ dokładny typ wildcarda nie jest znany.

Istnieje _*TYLKO*_ z wildcards:

```java
List<? super Integer>
```

Pozwala umieszczać `Integer`, ale zwracany typ będzie `Object`.

---

## 8. **Wildcards — ?, ? extends, ? super**

### 8.1. `?` — dowolny typ

```java
List<?> list;
```

Możesz pobrać element jako `Object`, **ale nie możesz go dodać** (oprócz `null`).

---

### 8.2. `? extends T` — tylko odczyt (PECS: Producer Extends)

```java
List<? extends Number> list;
```

* możesz czytać jako `Number`,
* **nie możesz dodawać**.

---

### 8.3. `? super T` — tylko zapis (PECS: Consumer Super)

```java
List<? super Integer> list;
```

* możesz dodawać `Integer`,
* możesz czytać tylko jako `Object`.

---

## 9. **Zasada PECS**

**Producer Extends, Consumer Super**

* Gdy kolekcja **produkuje** dane → `? extends`
* Gdy kolekcja **konsumuje** dane → `? super`

---

Dla przykładów kodu opisanych poniżej użyjemy prostego modelu danych z klasą bazową użytkownika (`User`) oraz dwoma klasami, które ją rozszerzają: `Operator` i `Customer`.

### Producer Extends

Ważne jest, aby zrozumieć, że zasada PECS musi być stosowana z punktu widzenia kolekcji. Innymi słowy, jeśli iterujemy przez listę i przetwarzamy jej elementy, lista będzie działać jako producent naszej logiki:

```java
public static void sendEmails(List<User> users) {
    for (User user : users) {
        System.out.println("sending email to " + user);
    }
}
```

Załóżmy teraz, że chcemy użyć metody `sendEmail` dla listy obiektów `Operator`. Klasa `Operator` rozszerza `User`, więc można by się spodziewać, że będzie to proste wywołanie metody. Niestety, pojawi się błąd kompilacji.

Aby rozwiązać problem, możemy zaktualizować metodę `sendEmail`, stosując się do reguły PECS. Ponieważ lista użytkowników jest producentem naszej logiki, użyjemy słowa kluczowego `extends`:

```java
public static void sendEmails(List<? extends User> users) {
    for (User user : users) {
        System.out.println("sending email to " + user);
    }
}
```

W rezultacie możemy teraz łatwo wywołać metodę dla list dowolnego typu generycznego, o ile dziedziczą one z klasy `User`:

```java
List<Operator> operators = Arrays.asList(new Operator("sam"), new Operator("daniel"));
List<Customer> customers = Arrays.asList(new Customer("john"), new Customer("arys"));

sendEmailsFixed(operators);
sendEmailsFixed(customers);
```

### Consumer Super

Gdy dodajemy elementy do kolekcji, stajemy się producentem, a lista działa jak konsument. Napiszmy metodę, która otrzymuje listę obiektów `Operator` i dodaje do niej dwa kolejne elementy:

```java
public static void addUsersFromMarketingDepartment(List<Operator> users) {
    users.add(new Operator("john doe"));
    users.add(new Operator("jane doe"));
}
```

To będzie działać idealnie, jeśli przekażemy listę obiektów `Operator`. Ale jeśli chcemy użyć go do dodania tych dwóch obiektów klasy `Operator` do listy obiektów `Users`, ponownie pojawi się błąd kompilacji.

Dlatego musimy zaktualizować metodę i sprawić, by akceptowała zbiór obiektów `Operator` lub ich podklas, używając słowa kluczowego `super`:

```java
public static void addUsersFromMarketingDepartmentFixed(List<? super Operator> users) {
    users.add(new Operator("john doe"));
    users.add(new Operator("jane doe"));
}
```

---

## 10. **Generyki a typy prymitywne**

Generyki **nie działają z typami prymitywnymi**:

```java
List<int> ints; // ❌
```

Używa się wrapperów:

```java
List<Integer> ints; // ✔
```

---

## 11. **Ograniczenia generyków w Javie**

### 11.1. Brak instancjonowania `T`

```java
T value = new T(); // ❌
```

Powód: **type erasure** (patrz niżej).

---

### 11.2. Nie można _**TWORZYĆ**_ tablic generycznych

```java
T[] arr = new T[10]; // ❌
```
, ale można przyjmować tablice generyczne jako argument metody :)

```java
public <T> void processArray(T[] array) {
    for (T element : array) {
        System.out.println(element);
    }
}
```

Deklaracja pola tego typu też jest OK:

```java
class Box<T> {
    private T[] array;
}
```

---

### 11.3. Nie ma informacji o typie w runtime (Type Erasure)

Cały system generyków jest **usuwany podczas kompilacji**.
JVM nie wie, czy masz `List<String>`, czy `List<Integer>`.

Skutki:

* brak przeciążenia metod po typie generycznym,
* brak tworzenia nowych instancji,
* brak refleksji typu T.

---

## 12. **Type Erasure, czyli Wymazywanie Typów — jak to działa?**

Kod:

```java
List<String> x = new ArrayList<>();
List<Integer> y = new ArrayList<>();
```

Po kompilacji **oba są List<Object>**.
Informacja o typach istnieje tylko na etapie kompilacji.

---

## 13. **Generyki w interfejsach**

Przykład:

```java
public interface Repository<T> {
    void save(T value);
    T findById(int id);
}
```

---

## 14. **Generyki w klasach abstrakcyjnych**

```java
public abstract class AnimalRepository<T extends Animal> {
    abstract void add(T animal);
}
```

---

## 15. **Generyki + polimorfizm**

```java
List<Dog> dogs = new ArrayList<>();
List<Animal> animals = dogs; // ❌ Nie działa!
```

Dlaczego?
Bo mogłabyś dodać `Cat` do listy psów.

Rozwiązanie?
Polimorfizm generyczny działa TYLKO przez wildcardy:
* ? extends T – PRODUCER (read - only)
* ? super T – CONSUMER (write - only)

---

## 16. **Generyki + metody statyczne**

Metoda statyczna **nie może używać typu T klasy**, ale może mieć własny:

```java
class Box<T> {
    private T value;

    public static void printValue(T x) {   // BŁĄD
        System.out.println(x);
    }
}
```
Dlaczego?

Bo parametr typu T istnieje tylko na poziomie instancji, a metoda statyczna działa bez instancji → więc T nie istnieje.

Ale metoda statyczna sama w sobie MOŻE być generyczna:

```java
class Util {
    public static <T> void print(T value) {   // OK
        System.out.println(value);
    }
}
```

---

## 17. **Generyki + konstruktor**

Możemy mieć różne przypadki:


### Klasa generyczna + zwykły konstruktor

```java
class Box<T> {
    private T value;

    public Box(T value) {  // konstruktor używa T z klasy
        this.value = value;
    }
}
```
* Konstruktor normalnie korzysta z parametru klasy

### Konstruktor generyczny z typem niezależnym od typu klasy

```java
class Box<T> {

    private T value;

    public <U> Box(U input) {
        System.out.println("Otrzymano argument typu: " + input.getClass());
    }
}
```
* T i U to całkowicie różne typy.

### Konstruktor generyczny i niegeneryczna klasa

```java
class Logger {

    public <T> Logger(T value) {
        System.out.println("Loguję: " + value);
    }
}
```
---

## 18. **Najważniejsze klasy generyczne wbudowane w Jave**

### Kolekcje:

* `List<T>`
* `Set<T>`
* `Map<K,V>`
* `Queue<T>`
* `Deque<T>`

### Narzędzia:

* `Optional<T>`
* `Comparator<T>`
* `EnumSet<E extends Enum<E>>`
* `Class<T>`

---

## 19. **Najczęstsze błędy**
* Użycie raw types
* Próba tworzenia `new T()`
* Dodawanie elementu do `List<? extends T>`
* Mylenie PECS
* Zakładanie, że generyki działają w runtime

---

## 20. **Dobre praktyki**
* Zawsze używaj generyków w kolekcjach
* Preferuj interfejsy (`List<String>` zamiast `ArrayList<String>`)
* Stosuj PECS
* Zawsze określaj typ parametru, jeśli kompilator nie potrafi wywnioskować
* Unikaj raw types
* Dokumentuj znaczenie parametrów typu

---

# Zadania

1.  Stwórz prostą klasę generyczną Box, która może przechowywać obiekt dowolnego typu.
    Klasa powinna zawierać metodę set, aby ustawić obiekt, oraz metodę get, aby go pobrać.
2.  Stwórz klasę generyczną Triple<T, U, V>, która może przechowywać trzy obiekty różnych
    typów. Zaimplementuj metody getFirst(), getSecond() i getThird() do pobierania
    odpowiednio pierwszego, drugiego i trzeciego elementu.
3. Napisz generyczną metodę max, która przyjmuje tablicę elementów typu porównywalnego
   (implementujących interfejs Comparable<T>) i zwraca element o najwyższej wartości.
   Uwzględnij obsługę przypadku pustej tablicy.
4.  Napisz statyczną metodę generyczną swap, która przyjmuje tablicę dowolnego typu i
    dwa indeksy, a następnie zamienia miejscami elementy w tej tablicy pod wskazanymi
    indeksami. Metoda powinna działać dla tablicy każdego typu. Przykładowe wywołanie
    metody: swap(myArray, 0, 2);, gdzie myArray to tablica typu Integer[] lub dowolnego
    innego typu. Zabezpiecz metodę tak, aby nie można było jej wywołać z indeksami
    spoza zakresu tablicy.
5.  Utwórz dwie klasy: Animal (Zwierzę) i Dog (Pies), gdzie Dog dziedziczy po Animal.
    Następnie napisz statyczną metodę generyczną findMax, która przyjmuje dwa argumenty:
    element1 i element2 typu extends Animal. Metoda powinna zwracać element większy
    według właśnie zdefiniowanego kryterium porównania. W implementacji porównaj
    elementy bazując na wybranym przez siebie atrybucie, na przykład wieku.
6.  Stwórz klasę generyczną Pair<T>, która przechowuje dwa obiekty tego samego typu.
    Następnie utwórz dwie klasy: Plant i Tree, gdzie Tree dziedziczy po Plant. Klasa Tree
    powinna posiadać prywatne pole height, którego nie posiada klasa Plant. Następnie
    napisz statyczną metodę generyczną findMinMaxHeight, która przyjmuje jako argument
    tablicę obiektów typu Tree oraz Pair<? super Tree> result. Metoda powinna zapisać
    (jako obiekty typu Tree) najniższe i najwyższe (pod kątem wysokości) drzewo z tablicy
    w drugim argumencie metody. Wykorzystaj też generyczny interfejs Comparable.
