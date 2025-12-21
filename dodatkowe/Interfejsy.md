1.  Napisz klasę Employee, która zawiera pola: name (typu String), salary (typu double) i
    employmentDate (typu LocalDate). Zaimplementuj interfejs Comparable w taki sposób,
    aby obiekty klasy Employee były sortowane rosnąco według pensji. Stwórz listę tablicową
    5 obiektów klasy Employee i posortuj ją według sprecyzowanego kryterium
2. Napisz klasę Book, która zawiera pola: title (typu String), numberOfPages (typu int) i
   publicationDate (typu LocalDate). Zaimplementuj interfejs Comparable w taki sposób,
   aby obiekty klasy Book były sortowane malejąco według liczby stron. Stwórz tablicę 4
   obiektów klasy Book i posortuj ją według sprecyzowanego kryterium
3. Zdefiniuj klasę Client, która będzie implementować generyczny interfejs Comparable. W
    klasie tej zadeklaruj prywatne pola lastName typu String oraz balance typu double.
    Implementując metodę compareTo interfejsu Comparable, porównuj klientów na podsta
    wie ich salda, a w przypadku takiego samego salda- na podstawie nazwiska. Następnie
    zdefiniuj klasę Company dziedziczącą po klasie Client. Klasa Company ma dodatkowo po
    siadać prywatne pole numberOfEmployees typu int. Implementując metodę compareTo
    interfejsu Comparable w klasie Company, skorzystaj z metody compareTo zdefiniowanej
    wklasie Client oraz, w razie potrzeby, uwzględnij pole numberOfEmployees. Napisz pro
    gram TestClient, w którym utwórz listę tablicową 5 klientów i firm o nazwie clientList
    posługując się klasą ArrayList. W składzie listy powinny wystąpić przynajmniej dwóch
    klientów o takim samym saldzie i różnym nazwisku oraz dwie firmy o takiej samej licz
    bie pracowników i różnym saldzie. Wyświetl zawartość listy clientList, posortuj ją za
    pomocą instancyjnej metody sort z klasy ArrayList i ponownie wyświetl zawartość tej
    listy
4.  Napisz klasę Produkt z polami nazwa (String), cena (double) i dataWaznosci
    (LocalDate). Napisz klasę implementującą interfejs Comparator, która porównuje
    produkty na podstawie daty ważności. Stwórz listę 5 produktów i posortuj ją według
    daty ważności.
5. Napisz klasę Person z polami firstName (typu String), lastName (typu String) oraz
   birthDate (typu LocalDate). Zaimplementuj dwie klasy implementujące generyczny in
   terfejs Comparator: LastNameComparator do porównywania obiektów po polu lastName
   (alfabetycznie od A do Z) oraz BirthDateComparator do porównywania obiektów po
   polu birthDate (od najstarszej do najmłodszej osoby). Stwórz tablicę 5 obiektów klasy
   Person i posortuj ją zgodnie z oboma kryteriami (najpierw po nazwisku, a następnie
   przy równości po dacie urodzenia).
6. Napisz klasę Student z polami id (typu int), name (typu String) oraz averageGrade
   (typu double). Zaimplementuj dwie klasy implementujące generyczny interfejs
   Comparator: AverageGradeComparator do porównywania obiektów po polu
   averageGrade (od najwyższej do najniższej średniej ocen) oraz IdComparator do
   porównywania obiektów po polu id (od najniższego do najwyższego identyfikatora).
   Stwórz listę 5 obiektów klasy Student i posortuj ją zgodnie z oboma kryteriami
   (najpierw po średniej ocen, a następnie przy równości po identyfikatorze)
7. Załóżmy, że mamy interfejs MusicPlayer z metodami turnOn(), turnOff() i
   nextTrack(). Stwórz klasę Radio, która będzie implementować ten interfejs. W meto
   dzie turnOn() powinien zostać wydrukowany komunikat “Radio włączone”, w metodzie
   turnOff()- “Radio wyłączone”, a w nextTrack()- “Zmieniono stację radiową”.
8.  Zaprojektuj interfejs Sensor z trzema metodami abstrakcyjnymi: readValue() zwraca
    jącą double, getStatus() zwracającą String oraz reset() zwracającą void. Stwórz
    dwie klasy TemperatureSensor i PressureSensor, które implementują ten interfejs. W
    klasie SensorTest przetestuj działanie metod dla obiektów z obu klas.
9. Wykonaj poniższe czynności:
   * Utwórz interfejs VehicleManager z dwoma metodami abstrakcyjnymi: startEngine()
   zwracającą String i getFuelLevel() zwracającą int.
   * Stwórz klasę Car, implementującą VehicleManager. W metodzie startEngine zwróć
   “Silnik samochodu uruchomiony”, a w getFuelLevel zwróć wartość 50.
   * Stwórz klasę Motorcycle, również implementującą VehicleManager. W startEngine
   zwróć “Silnik motocykla uruchomiony”, a w getFuelLevel zwróć wartość 30.
   * W klasie VehicleManagerTest stwórz obiekty obu klas i przetestuj ich metody.
10. Stwórz interfejs SoundPlayer z:
    * Metodą abstrakcyjną playSound().
    * Metodą domyślną stopSound() wyświetlającą informację “Sound Stopped”.
    * Metodą statyczną getDeviceType() zwracającą String “Sound Device”.
    
    Stwórz klasy MusicPlayer i Radio, które implementują SoundPlayer. playSound() w
    MusicPlayer powinno wyświetlać “Playing Music”, a w Radio- “Playing Radio”. Stwórz
    klasę testującą SoundTester. Utwórz obiekty MusicPlayer i Radio, wywołaj dla nich
    playSound() i stopSound(), oraz statycznie SoundPlayer.getDeviceType()