#Tutorium 9 (21.12.12)

Quiz am Anfang:

```java
public class Test {
  public static void main(final String... args) {
    final Person p = new Runner("hugo");
    p.tellYourName();
    p.run();
  }
}

class Person {

  final String name;

  public Person(final String name) {
    this.name = name;
  }

  void tellYourName() {
    System.out.println("Hello, I'm " + name);
  }
}

class Runner extends Person {

  public Runner(final String name) {
    super(name);
  }

  void run() {
    System.out.println("...and I start to run now...");
  }
}
```

##Häufige Fehler auf Übungsblatt 3

####equals-Methode falsch implementiert

```java
public boolean equals(Object o) {
  return o.equals(this);
}
```
Führt zu einem `StackOverflowError`.

####HTML-Code falsch ausgegeben

Anstatt `<` und `>` wurde `&lt;` und `&gt;` benutzt.

####Parameterüberprüfung

Meine [Testklasse](blatt04-tests.md) hat die meisten Programme in die Knie gezwungen.

##Vererbung (Fortsetzung)
[Link zum Tutorial](../java-tutorial/vererbung.md)

##Interfaces
[Link zum Tutorial](../java-tutorial/interfaces.md)

##Generics
[Link zum Tutorial](../java-tutorial/generics.md)
[Listenimplementierung mit Generics und Interfaces](../java-tutorial/list-generics.md) + [Tests](../java-tutorial/list-generics-test.md)

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://imgs.xkcd.com/comics/tree.png)
![Comic am Ende](http://imgs.xkcd.com/comics/incident.png)