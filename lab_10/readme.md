# **Programowanie Obiektowe – Java (2025Z)**

# Lab 10: Kolekcje

W Javie kolekcje to zestaw wyspecjalizowanych struktur danych, zaprojektowanych tak, aby umożliwiać łatwe i efektywne przechowywanie, przetwarzanie oraz organizowanie obiektów.

Cały system kolekcji znajduje się w pakiecie:

```java
java.util
```

I jest zorganizowany wokół hierarchii interfejsów.

---

## **1. Hierarchia kolekcji – najważniejsze interfejsy**

Java Collections Framework opiera się na kilku kluczowych interfejsach:

```
                   Iterable
                       |
                  Collection
          _________/   |   \_________
         |             |             |
        List          Set          Queue
         |             |             |
      ArrayList     HashSet      ArrayDeque
     LinkedList     TreeSet     PriorityQueue
```

Dodatkowo mamy oddzielną gałąź:

```
Map (nie dziedziczy po Collection)
   |
HashMap, TreeMap, LinkedHashMap
```

---

## **2. Interfejs `Collection<E>` – fundament (prawie) wszystkich kolekcji**

Najważniejsze metody (każda kolekcja je dziedziczy):

| Metoda                       | Co robi                  |
| ---------------------------- | ------------------------ |
| `boolean add(E e)`           | dodaje element           |
| `boolean remove(Object o)`   | usuwa element            |
| `int size()`                 | liczba elementów         |
| `boolean isEmpty()`          | czy kolekcja jest pusta  |
| `void clear()`               | usuwa wszystkie elementy |
| `boolean contains(Object o)` | czy zawiera element      |
| `Iterator<E> iterator()`     | zwraca iterator          |
| `Object[] toArray()`         | konwersja na tablicę     |

Przykład:

```java
Collection<String> col = new ArrayList<>();
col.add("A");
col.add("B");
System.out.println(col.size()); // 2
```

---

## **3. List – kolekcja uporządkowana (indeksowana)**

`List<E>` pozwala na:

* przechowywanie duplikatów
* dostęp przez indeks
* zachowuje kolejność dodawania

Implementacje:

### **3.1. ArrayList**

Najpopularniejsza lista. Szybka w odczycie, wolniejsza w wstawianiu w środek.

```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
System.out.println(list.get(0)); // A
```

### **3.2. LinkedList**

`LinkedList<E>` to implementacja interfejsu **List** oraz **Deque**.
Oznacza to, że:

* Jest **dwukierunkową listą** — każdy element zna swojego poprzednika i następnika.
* Może działać zarówno jako **lista**, jak i **kolejka/stos**.
* Umożliwia **wstawianie i usuwanie elementów w dowolnym miejscu listy w czasie O(1)** (jeśli mamy referencję do węzła).

---

#### Struktura wewnętrzna

Każdy element to **węzeł** (`Node`) zawierający:

* `E item` – wartość elementu
* `Node<E> next` – referencja do następnego węzła
* `Node<E> prev` – referencja do poprzedniego węzła

Dlatego:

* dostęp po indeksie wymaga **przeszukiwania od początku lub końca** → O(n)
* dodawanie/usuwanie w środku listy jest **szybsze niż w ArrayList** (nie trzeba przesuwać pozostałych elementów)

---

#### Konstrukcja i podstawowe metody

```java
LinkedList<String> list = new LinkedList<>();

// Dodawanie elementów
list.add("A");          // na końcu
list.addFirst("Start"); // na początku
list.addLast("End");    // na końcu

// Pobieranie elementów
System.out.println(list.get(0));    // indeks 0
System.out.println(list.getFirst()); // pierwszy
System.out.println(list.getLast());  // ostatni

// Usuwanie elementów
list.remove();      // usuwa pierwszy element
list.removeFirst(); // usuwa pierwszy
list.removeLast();  // usuwa ostatni

// Iteracja
for (String s : list) {
    System.out.println(s);
}
```

---

#### Metody przydatne w LinkedList

| Metoda          | Opis                                    |
| --------------- | --------------------------------------- |
| `addFirst(E e)` | dodaje element na początku              |
| `addLast(E e)`  | dodaje element na końcu                 |
| `getFirst()`    | zwraca pierwszy element                 |
| `getLast()`     | zwraca ostatni element                  |
| `removeFirst()` | usuwa pierwszy element                  |
| `removeLast()`  | usuwa ostatni element                   |
| `offer(E e)`    | dodaje element na końcu (Queue)         |
| `poll()`        | usuwa pierwszy element (Queue)          |
| `peek()`        | odczytuje pierwszy element bez usuwania |
| `push(E e)`     | dodaje element na początku (Deque/Stos) |
| `pop()`         | usuwa pierwszy element (Deque/Stos)     |

---

## **4. Set – zbiór unikalnych elementów**

`Set<E>`:

* nie pozwala na duplikaty
* najczęściej nie zachowuje kolejności

### **4.1 HashSet**

* działa na bazie hashCode()
* brak uporządkowania
* szybkie operacje

### **4.2 LinkedHashSet**

* zachowuje kolejność dodawania
* też unikalne elementy

### **4.3 TreeSet**

* automatycznie sortuje elementy
* wymaga Comparable lub Comparator

```java
Set<Integer> numbers = new TreeSet<>();
numbers.add(3);
numbers.add(1);
numbers.add(2);
System.out.println(numbers); // [1, 2, 3]
```

---

## **5. Queue – kolejka (FIFO, priorytety)**

### Unikalne metody

| Metoda               | Co robi                                                                                    |
|----------------------| ------------------------------------------------------------------------------------------ |
| `boolean offer(E e)` | Dodaje element na koniec kolejki; różnica w zachowaniu przy pełnej kolejce w bounded Queue |
| `E poll()`           | Pobiera i usuwa pierwszy element kolejki; zwraca null jeśli kolejka pusta                  |
| `E remove()`         | Pobiera i usuwa pierwszy element; wyrzuca wyjątek jeśli pusta                              |
| `E peek()`           | Pobiera pierwszy element, nie usuwając go; zwraca null jeśli pusta                         |
| `E element()`        | Pobiera pierwszy element bez usuwania; wyrzuca wyjątek jeśli pusta                         |

### **5.1. Queue<E>**

* dodawanie: `offer()`
* pobieranie: `poll()`

### **5.2. PriorityQueue**

* elementy same ustawiają się wobec siebie (Comparable)
* nie musi być FIFO

### **5.3. ArrayDeque**

* działa jako stos *lub* kolejka
* szybszy od LinkedList

Przykład:

```java
Queue<Integer> pq = new PriorityQueue<>();
pq.offer(30);
pq.offer(10);
pq.offer(20);

while (!pq.isEmpty()) {
    System.out.println(pq.poll());
}
```

```text
10
20
30
```

---

## **6. Map – słownik (klucz → wartość)**

`Map` to interfejs w **Java Collections Framework**, który:

* **nie dziedziczy po Collection<E>**
* przechowuje dane jako **pary klucz → wartość**
* każdy klucz jest unikalny, wartość może się powtarzać
* najczęstsze implementacje: `HashMap`, `TreeMap`, `LinkedHashMap`, `ConcurrentHashMap`

### Metody unikalne Map

| Metoda                                         | Co robi                                 |
|------------------------------------------------|-----------------------------------------|
| `V put(K key, V value)`                        | Dodaje lub nadpisuje wartość dla klucza |
| `V get(Object key)`                            | Pobiera wartość dla klucza              |
| `V remove(Object key)`                         | Usuwa wpis o danym kluczu               |
| `boolean containsKey(Object key)`              | Sprawdza, czy klucz istnieje            |
| `boolean containsValue(Object value)`          | Sprawdza, czy wartość istnieje          |
| `Set<K> keySet()`                              | Zwraca zbiór wszystkich kluczy          |
| `Collection<V> values()`                       | Zwraca kolekcję wszystkich wartości     |
| `Set<Map.Entry<K,V>> entrySet()`               | Zwraca zbiór par klucz → wartość        |
| `void putAll(Map<? extends K, ? extends V> m)` | Dodaje wszystkie wpisy z innej mapy     |
| `void clear()`                                 | Usuwa wszystkie wpisy                   |
| `boolean isEmpty()`                            | Sprawdza, czy mapa jest pusta           |
| `int size()`                                   | Liczba wpisów                           |

### **6.1 HashMap**

* najszybsza
* brak kolejności

### **6.2 LinkedHashMap**

* zachowuje kolejność dodawania

### **6.3 TreeMap**

* sortuje klucze

Przykład:

```java
Map<String, Integer> ages = new HashMap<>();
ages.put("Ala", 10);
ages.put("Ola", 12);
System.out.println(ages.get("Ola")); // 12
```

---

## **7. Najważniejsze różnice między strukturami**

### List vs Set

| Cecha               | List   | Set                     |
| ------------------- |--------|-------------------------|
| Duplikaty           | Tak    | Nie                     |
| Kolejność           | Tak    | Zależy od implementacji |
| Dostęp przez indeks | Tak    | Nie                     |

### HashSet vs TreeSet

| Cecha              | HashSet | TreeSet    |
| ------------------ | ------- | ---------- |
| Kolejność          | brak    | sortowanie |
| Wydajność          | szybszy | wolniejszy |
| Wymaga Comparable? | nie     | tak        |

### HashMap vs TreeMap

| Cecha     | HashMap    | TreeMap           |
| --------- | ---------- | ----------------- |
| Kolejność | brak       | sortowanie kluczy |
| Wydajność | szybsza    | wolniejsza        |
| Null key  | 1 null key | brak null         |

---

## **8. Iterowanie po kolekcjach**

### **8.1. For-each**

```java
for (String s : list) {
    System.out.println(s);
}
```

### **8.2. Iterator**

```java
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}
```

### **8.3. Java 8 – forEach**

Metoda `forEach()` jest domyślną metodą interfejsu `Iterable<E>` i występuje też w interfejsach `Collection`, `Map` (dla `Map` jest trochę inaczej).

Jej zadaniem jest iteracja po każdym elemencie kolekcji i wykonanie określonej akcji.

```java
default void forEach(Consumer<? super E> action);
```
, gdzie:
* action – obiekt typu Consumer, czyli funkcjonalny interfejs przyjmujący jeden argument i nic nie zwracający.
* ? super E → oznacza, że action może przyjmować E lub jego nadtyp.
* zwraca void.

---

#### Wymagania dla argumentu

Musi być _**wyrażeniem lambda**_, _**referencją do metody**_ lub instancją klasy implementującej Consumer<E>.

**Nie może zwracać wartości.**

Przykład:

```java
list.forEach(System.out::println);
```

---

## **9. Narzędzia pomocnicze – Collections**

Klasa `java.util.Collections` zawiera metody:

* sortowanie: `Collections.sort(list)`
* minimum, maksimum
* wypełnianie list
* tworzenie kolekcji niemodyfikowalnych
* synchronizowanie kolekcji

Przykład:

```java
Collections.sort(list);
```

Najważniejsze metody:

| Metoda                                                            | Sygnatura / Typ                               | Co robi                                    |
| ----------------------------------------------------------------- | --------------------------------------------- | ------------------------------------------ |
| `sort(List<T> list)`                                              | static <T extends Comparable<? super T>> void | Sortuje listę naturalnie (Comparable)      |
| `sort(List<T> list, Comparator<? super T> c)`                     | static <T> void                               | Sortuje listę według Comparator            |
| `reverse(List<?> list)`                                           | static void                                   | Odwraca kolejność elementów w liście       |
| `shuffle(List<?> list)`                                           | static void                                   | Miesza elementy losowo                     |
| `swap(List<?> list, int i, int j)`                                | static void                                   | Zamienia elementy o indeksach i i j        |
| `min(Collection<? extends T> coll)`                               | static <T extends Comparable<? super T>> T    | Zwraca najmniejszy element kolekcji        |
| `max(Collection<? extends T> coll)`                               | static <T extends Comparable<? super T>> T    | Zwraca największy element kolekcji         |
| `frequency(Collection<?> c, Object o)`                            | static int                                    | Liczy wystąpienia elementu w kolekcji      |
| `fill(List<? super T> list, T obj)`                               | static <T> void                               | Wypełnia listę tym samym elementem         |
| `copy(List<? super T> dest, List<? extends T> src)`               | static <T> void                               | Kopiuje elementy z jednej listy do drugiej |
| `binarySearch(List<? extends Comparable<? super T>> list, T key)` | static <T> int                                | Wyszukiwanie binarne w posortowanej liście |
| `synchronizedList(List<T> list)`                                  | static <T> List<T>                            | Tworzy listę synchroniczną (thread-safe)   |
| `synchronizedSet(Set<T> s)`                                       | static <T> Set<T>                             | Tworzy zbiór synchroniczny                 |
| `synchronizedMap(Map<K,V> m)`                                     | static <K,V> Map<K,V>                         | Tworzy mapę synchroniczną                  |
| `unmodifiableList(List<? extends T> list)`                        | static <T> List<T>                            | Zwraca niemodyfikowalną listę              |
| `unmodifiableSet(Set<? extends T> s)`                             | static <T> Set<T>                             | Zwraca niemodyfikowalny zbiór              |
| `unmodifiableMap(Map<? extends K, ? extends V> m)`                | static <K,V> Map<K,V>                         | Zwraca niemodyfikowalną mapę               |
| `emptyList()`                                                     | static <T> List<T>                            | Zwraca pustą listę                         |
| `emptySet()`                                                      | static <T> Set<T>                             | Zwraca pusty zbiór                         |
| `emptyMap()`                                                      | static <K,V> Map<K,V>                         | Zwraca pustą mapę                          |


---

## **10. Kolekcje a Typy Generyczne**

Każda kolekcja w Javie jest generyczna:

```java
List<String> names = new ArrayList<>();
Map<Integer, String> ids = new HashMap<>();
```

Umożliwia to:

* bezpieczeństwo typów
* brak rzutowania
* wykrywanie błędów w czasie kompilacji

---

## **11. Kolekcje niemodyfikowalne**

Niemodyfikowalne kolekcje to takie, do których **nie można dodawać, usuwać ani zmieniać elementów** po ich utworzeniu.

* Jeśli spróbujesz zmodyfikować kolekcję, zostanie wyrzucony wyjątek **`UnsupportedOperationException`**.
* Są przydatne do **bezpiecznego udostępniania danych**, np. w API, w wielowątkowości lub w stałych danych konfiguracyjnych.

### Tworzenie niemodyfikowalnych kolekcji

#### a) Klasa `Collections`:

```java
public class UnmodifiableExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>(List.of("A", "B", "C"));
        List<String> unmodifiableList = Collections.unmodifiableList(list);

        Set<Integer> set = new HashSet<>(Set.of(1,2,3));
        Set<Integer> unmodifiableSet = Collections.unmodifiableSet(set);

        Map<String, Integer> map = new HashMap<>();
        map.put("Ala", 10);
        Map<String, Integer> unmodifiableMap = Collections.unmodifiableMap(map);
    }
}
```

**Cechy:**

* Kolekcja bazowa (`list`, `set`, `map`) nadal może być zmieniana — zmiany będą widoczne w niemodyfikowalnej wersji.
* Sama niemodyfikowalna kolekcja nie pozwala na operacje `add`, `remove`, `put` itp.

#### b) Statyczne metody tworzące niemodyfikowalne kolekcje w Java 9+:

```java
List<String> list = List.of("A", "B", "C");
Set<Integer> set = Set.of(1, 2, 3);
Map<String, Integer> map = Map.of("Ala", 10, "Ola", 12);
```

**Cechy:**

* W pełni niemodyfikowalne, nie można ich zmienić ani poprzez dodawanie, ani usuwanie.
* Tworzenie krótkie i wygodne.
* Dobrze nadają się do stałych danych.

---

### Przykład użycia i wyjątek

```java
List<String> names = List.of("Ala", "Ola");

try {
    names.add("Jan"); // UnsupportedOperationException
} catch (UnsupportedOperationException e) {
    System.out.println("Nie można modyfikować tej listy!");
}
```

Wyjście:

```
Nie można modyfikować tej listy!
```

---

## **12. Kolekcje specjalne (zaawansowane)**

* **EnumSet** → szybki zbiór oparty o enumy
* **EnumMap** → klucze tylko enum
* **WeakHashMap** → klucze z weak references
* **IdentityHashMap** → porównywanie przez `==`, nie `equals`
* **PriorityBlockingQueue** → wersja wielowątkowa
* **ConcurrentHashMap** → thread-safe

---

# Zadania
1.  Napisz statyczną metodę printUnique(Collection<T> items), która przyjmuje generyczny
    interfejs Collection jako argument i wyświetla na ekranie każdy unikalny element z tej
    kolekcji dokładnie raz.
2.  Napisz metodę reversePrint(Iterable items), która przyjmuje generyczny interfejs
    Iterable jako argument i wyświetla na ekranie elementy tej sekwencji w odwrotnej kolejności,
    niż zostały one przekazane.
3. Stwórz metodę mergeLists, która przyjmuje dwie generyczne ArrayList<T> i zwraca
   nową ArrayList<T>, będącą połączeniem elementów z obu list. Upewnij się, że kolejność elementów z oryginalnych list jest zachowana w wynikowej liście
4.  Napisz metodę isPalindrome, która przyjmuje generyczną listę powiązaną (LinkedList<T>
    list) i zwraca true, jeśli lista jest palindromem, a false w przeciwnym przypadku. Lista
    jest palindromem, gdy czytana od przodu i od tyłu jest taka sama. Metoda powinna być jak
    najbardziej wydajna.
5.  Napisz metodę findUniqueElements, która przyjmuje generyczną listę (List<T> list) i
    zwraca HashSet<T>, który zawiera tylko unikalne elementy z tej listy. Metoda powinna skutecznie eliminować duplikaty.
6.  Napisz metodę findElementsInRange, która przyjmuje TreeSet<T> oraz dwie wartości
    graniczne T lowerBound i T upperBound. Metoda powinna zwracać TreeSet<T>, który
    zawiera wszystkie elementy z pierwotnego zbioru, które mieszczą się w przedziale między
    lowerBound a upperBound (włącznie z obiema granicami).
7.  Napisz metodę reverseQueue, która przyjmuje generyczną kolejkę (Queue<T> queue) i
    odwraca kolejność jej elementów. Metoda powinna wykonywać operacje odwracania w miejscu,
    nie używając dodatkowych kolekcji.
8.  Napisz metodę isSymmetric, która przyjmuje generyczny Deque (Deque<T> deque) i
    zwraca true, jeśli elementy w Deque są ułożone symetrycznie (tj. kolejność elementów jest
    taka sama, gdy czytamy je od przodu do tyłu i od tyłu do przodu). W przeciwnym przypadku
    metoda zwraca false.
9.  Napisz metodę mergePriorityQueues, która przyjmuje dwie generyczne PriorityQueues
    (PriorityQueue<T> queue1 i PriorityQueue<T> queue2). Metoda powinna zwrócić nową
    PriorityQueue zawierającą wszystkie elementy z obu wejściowych kolejek. Zakładamy, że elementy w obu kolejnych mogą być porównywane między sobą.
10.  Napisz metodę reverseMap, która przyjmuje generyczną mapę (Map<K, V> map) i zwraca
     nową mapę (Map<V, K>), gdzie każdy klucz staje się wartością, a każda wartość kluczem. Jeśli
     oryginalna mapa zawiera powtarzające się wartości, zachowaj tylko ostatnią parę klucz-wartość
     odwracając mapę.
11. Napisz metodę countValueOccurrences, która przyjmuje generyczną HashMap
    (HashMap<K, V> map) i zwraca nową HashMap (HashMap<V, Integer>), gdzie każdy klucz to
    wartość z oryginalnej mapy, a wartość to liczba wystąpień tej wartości w oryginalnej mapie.
12.  Napisz metodę subMapInRange, która przyjmuje generyczną TreeMap (TreeMap<K, V>
     map), a także dwie wartości kluczy K startKey i K endKey. Metoda powinna zwrócić nową
     TreeMap, która zawiera wszystkie pary klucz-wartość z oryginalnej mapy, których klucze mieszczą się w zakresie od startKey do endKey, włącznie.