1. Napisz generyczną metodę isEqual, która przyjmuje dwa dowolne obiekty tego samego
   typu i zwraca true, jeśli są one równe, w przeciwnym razie false
2.  Zdefiniuj generyczny interfejs Stack<T> z metodami push(T item), T pop(), T peek()
    i boolean isEmpty(). Stwórz klasę implementującą ten interfejs, która będzie reprezentować stos przechowujący elementy dowolnego typu.
3. Stwórz klasę generyczną Storage<T>, która przechowuje pojedynczy obiekt dowolnego
   typu. Klasa powinna mieć metody store(T item), która zapisuje obiekt, oraz T
   retrieve(), która zwraca przechowywany obiekt.
4.  Stwórz generyczną klasę GenericQueue<T>, która implementuje prostą kolejkę. Klasa po
    winna mieć metody enqueue(T element), która dodaje element do kolejki, i dequeue(),
    która usuwa i zwraca element z początku kolejki.
5.  Utwórz statyczną metodę generyczną reverseArray, która przyjmuje tablicę elementów
    typu T i odwraca kolejność jej elementów. Metoda powinna modyfikować przekazaną
    tablicę, nie zwracając nowej. Sprawdź działanie metody na tablicach różnych typów,
    takich jak Character, Boolean oraz własnych typów obiektowych. Zabezpiecz metodę
    tak, aby nie można było jej wywołać z tablicą null.
6.  Utwórz statyczną metodę generyczną sortAndReturnFirst, która przyjmuje tablicę ele
    mentów typu generycznego T, gdzie T rozszerza Comparable<T>. Metoda powinna sorto
    wać tablicę i zwracać jej pierwszy element. Zabezpiecz metodę przed wywołaniem z pustą
    tablicą (o zerowej liczbie elementów). Przetestuj tę metodę na różnych typach danych,
    w tym na nowo utworzonej klasie Book z polami title i author. Klasa Book powinna
    implementować generyczny interfejs Comparable na podstawie pola title zgodnie z
    porządkiem naturalnym.
7. Utwórz statyczną metodę generyczną sumElements, która przyjmuje kolekcję elementów
   typu generycznego T i zwraca ich sumę. Przetestuj tę metodę na kolekcji typu Integer
   oraz Float. Następnie spróbuj użyć tej metody z kolekcją typów prostych, np. int lub
   float, i zobacz, jakie błędy kompilacji się pojawią. Wykorzystaj to do wyjaśnienia róż
   nicy między typami prostymi a obiektowymi w Javie
8. Zdefiniuj klasy Car (Samochód) i ElectricCar (Samochód Elektryczny), gdzie
   ElectricCar dziedziczy po Car. Napisz statyczną metodę generyczną compareObjects,
   która przyjmuje dwa argumenty: object1 i object2 typu extends Car. Metoda ma
   zwracać wartość true, jeśli obiekty są tego samego typu, w przeciwnym wypadku false.
   Użyj metody getClass() do porównania klas obiektów.
9.  Stwórz klasę generyczną ElementPair<T>, która przechowuje dwa obiekty tego samego
    typu. Utwórz dwie klasy: Shape i Circle, gdzie Circle dziedziczy po Shape. Następ
    nie napisz statyczną metodę generyczną findLargest, która przyjmuje ElementPair<?
    extends Shape>. Metoda powinna zwracać element większy według własnie zdefiniowa
    nego kryterium porównania.
10.  Stwórz klasę generyczną Pair<T> która przechowuje dwa obiekty tego samego typu.
     Utwórz dwie klasy: Animal (Zwierzę) i Dog (Pies), gdzie Dog dziedziczy po Animal. Klasa
     Dog ma posiadać prywatne pole age, które nie posiada klasa Animal. Następnie napisz
     statyczną metodę generyczną findMinMaxAge, która przyjmuje jako argument tablicę
     obiektów typu Dog oraz Pair<?super Dog> result. Metoda powinna ma zapisać (jako
     obiekty typu Dog) najmniejszy i najmniejszy (pod kątem wieku) psa z tablicy w dru
     gim argumencie metody. Zrób to bezpośrednio bez używania interfejsów Comparable
     czy Comparator.