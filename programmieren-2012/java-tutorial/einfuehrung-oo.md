#Einführung in die Objektorientierung

##Wozu Klassen und Objekte?

* Sie sollen uns helfen von der Funktionsweise der Maschine zu abstrahieren, indem die Funktionsweise der Realen Welt modelliert wird.

* Vorteile von OO nicht sofort ersichtlich → Geduld

##Beispiel: Modellierung einer Person
###Fragen, denen wir uns zu Beginn stellen müssen:
* Welche Informationen benötigen wir für eine Person?

* Welche Informationen benötigen wir _nicht_?

Codebeispiel:

```java
class Person {
  int age;
  String name;
  String faveColor;
  double money;

  String town;
  String street;
  int streetNumber;
  int zip;
}
```

Es bleibt uns überlassen, bis zu welcher Tiefe wir Objekte modellieren:

```java
class Person {
  int age;
  String name;
  Color faveColor;
  BankAccount account;
  Address address;
}

class BankAccount {
  double money;
}

class Address {
  String town;
  String street;
  int streetNumber;
  int zip;
}

enum Color {
  RED, BLUE, GREEN;
}
```

###Übungsaufgabe: Modellierung eines Autos
* Welche Klassen und Attribute könnten für ein Auto benötigt werden?

* Ist es sinnvoll, wirklich alle möglichen Autos modellieren zu können oder verzichten wir zugunsten geringerer Komplexität auf gewisse Spezifikationen?