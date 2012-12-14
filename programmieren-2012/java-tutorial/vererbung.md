#Vererbung

Sinn: Gleiche Eigenschaften und Verhaltensweisen gruppieren.

So nicht:
```java
class Student {
  String name;
  int age;
  Tutor tutor;
}
class Tutor {
  String name;
  int age;
  Student[] students;
}
```

Vielleicht so?
```java
class Person {
  String name;
  int age;
}
class Student extends Person {
  Tutor tutor;
}
class Tutor extends Person {
  Student[] students;
}
```

- Mit `extends` wird die Vererbungshierarchie aufgebaut

- Oberklasse wird auch oft Vaterklasse genannt, die Unterklasse dagegen als Kindklasse

- In Java kann nur von einer Klasse geerbt werden

Warnung: Vererbung wird __oft__ falsch eingesetzt.
```java
class Point {
  int x, y;
}
class Linie extends Point {
  int x2, y2; // huh?
}

// =>

class Linie {
  Point p1, p2;
}
```
Im Zweifelsfall: [Prefer composition over inheritance](http://stackoverflow.com/questions/49002/prefer-composition-over-inheritance)

Merkregel: Vererbung ⇔ Ist-Beziehung, Komposition ⇔ Hat-Beziehung

##Vererbung von Methoden

- Ihr habt es schon benutzt bei `toString()`, `hashCode()` und `equals(Object)`

- In Java erbt jedes Objekt _implizit_ von `java.lang.Object`

```java
package java.lang;

public class Object {

  // method implemented by the JVM
  public native int hashCode();
  
  public boolean equals(Object obj) {
    return (this == obj);
  }
  
  public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
  }
}
```
- Diser Vorgang wird als `Überschreiben` bezeichnet

- Es gibt noch `Überladen`:
  ```java
  class Class {
      void doSomething(int i) {}
      void doSomething(String s) {}
  }
  ```

- Konstruktoren werden mitvererbt und müssen in der Kindklasse __immer__ implementiert werden

####Übung
Erstelle eine Vererbungshierarchie (in Form eines Klassendiagramms) und finde mindestens ein Attribut und eine Methode für folgende Klassen:

- Mensch
- Sportler
- Fußballer
- Tier
- Haus
- Torwart
- Säugetier
- Insekt
- Stürmer
- Hund
- Büroangestellter

##Statische Typen + Casts

Der statische Typ kann sich vom Laufzeit-Typ unterscheiden.
```java
class Person {
  String name;
  int age;
  
  public Person(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public String toString() {
    return "Person";
  }
}
class Student extends Person {
  Tutor tutor;
  
  public Student(String name, int age, Tutor tutor) {
    super(name, age);
    this.tutor = tutor;
  }

  public String toString() {
    return "Student";
  }
}
class Tutor extends Person {
  Student[] students;
  
  public Tutor(String name, int age, Student[] students) {
    super(name, age);
    this.students = students;
  }

  public String toString() {
    return "Tutor";
  }
}
```
Anwendungsfall:

```java
// static type: Tutor, runtime type: Tutor
Tutor tutor = new Tutor("simon", 22, new Student[] {});
// static type: Student, runtime type: Student
Student student = new Student("felix", 20, tutor);

System.out.println(tutor);
System.out.println(student);

// Upcast - no explicit cast needed
// static type: Person, runtime type: Tutor
Person tutorPerson = tutor;

System.out.println(tutorPerson);

// Downcast - explicit cast needed
// static type: Tutor, runtime type: Tutor
Tutor tutor2 = (Tutor) tutorPerson;

System.out.println(tutor2);
```

##Vererbung + Sichtbarkeiten

```java
class MySuperClass {

  public int i = 1;

  public String print() {
    return "Superklasse: " + i;
  }
}

class MySubClass extends MySuperClass {

  public String print() {
    return "Subklasse: " + i;
  }
}

class MySubSubClass extends MySubClass {

  public int i = 2;
}
```

Was ist die Ausgabe folgenden Codestücks?
```java
MySuperClass object1 = new MySuperClass();
System.out.println(object1.i);
System.out.println(object1.print());

MySubClass object2 = new MySubClass();
System.out.println(object2.i);
System.out.println(object2.print());

MySubSubClass object3 = new MySubSubClass();
System.out.println(object3.i);
System.out.println(object3.print());
```

Noch ein Beispiel:

```java
class Animal {

  private String sound;

  public void roar() {
    System.out.println(sound);
  }

  public void jump() {
    System.out.println("jumping with a '" + sound + "'");
  }
}

class Tiger extends Animal {

  String sound = "ROAR";
}

class Cat extends Animal {

  public String sound;

  public Cat() {
    String sound = "Maunz";
  }

  public void roar() {
    System.out.println(sound);
  }
}
```

Was ist nun die Ausgabe:
```java
Animal tigger = new Tiger();
Animal felix = new Cat();
Cat ninja = new Cat();
Tiger diego = new Tiger();

tigger.roar();
felix.roar();
ninja.roar();
diego.roar();
ninja.sound = "Hust hust";
ninja.roar();
ninja.jump();
((Animal) ninja).jump();
```

- `protected` erlaubt den Zugriff von der definierenden Klasse selbst und all seiner Kinder

##Abstrakte Klassen

- An Klassen: Klasse kann nicht instanziiert werden

- An Methoden: Muss in Subklasse implementiert werden

- Abstrakte Methoden sind nur erlaubt wenn die Klasse auch abstrakt ist.

```java
abstract class MySuperClass {
  
  protected String s = "string";
  
  abstract public void print();
}

class MySubClass extends MySuperClass {

  public void print() {
    System.out.print(s);
  }
}
```

####Beispiel: Iterator
Vorher:
```java
class IntIterator {

  public boolean hasNext() {
    throw new UnsupportedOperationException(
        "Iterator#hasNext doesn't have an implementation yet");
  }

  public int next() {
    throw new UnsupportedOperationException(
        "Iterator#next doesn't have an implementation yet");
  }
}
```
Nachher:
```java
abstract class IntIterator {

  abstract public boolean hasNext();

  abstract public int next();
}
```

##Die Annotation Override
Was ist das Problem in folgendem Code?
```java
class Super {
  void hannanamanamana() {
  }
}
class Sub extends Super {
  // overrides super method
  void hannanananamana() {
  }
}
```

Der Name der überschriebenen Methode in Klasse `Sub` ist nicht korrekt. `Override` erlaubt eine Prüfung zur Compilezeit:
```java
class Sub extends Super {
  @Override
  void hannanananamana() {
  }
  // error in Eclipse: The method hannanananamana() of type Sub must override or implement a supertype method
  // → spelling mistake
}
```