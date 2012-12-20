#Interfaces

Interfaces ähneln abstrakten Klassen:
```java
interface IntIterator {

  boolean hasNext();
  
  int next();
}
```

- Interfaces können keine Attribute und Konstruktoren definieren

- Alle Methoden sind automatisch `abstract` und `public`

- Eine Klasse kann mehrere Interfaces implementieren

```java
interface Swimable {

  void swim();
}

interface Flyable {

  void fly();
}

abstract class Duck {

  public final String name;

  public Duck(String name) {
    this.name = name;
  }
}

class Donald extends Duck implements Swimable, Flyable {

  public Donald(String name) {
    super(name);
  }

  public void fly() {
    System.out.println(name + "can fly");
  }

  public void swim() {
    System.out.println(name + "can swim");
  }
}
```

- Vorgegeben wird nur eine Schnittstelle über die eine Klasse angesprochen werden kann

- Abstrakte Klassen sind also konkreter als Interfaces

- Können als Gruppierung von Eigenschaften angesehen werden

```java
Flyable f1 = new Duck();
Flyable f2 = new Plane();
Flyable f3 = new Helicopter();
Flyable f4 = new Cannonball();
```