#Generics

- Verdammt cool - man benötigt aber lange um sie vollkommen zu verstehen

- Erlauben Codeduplizierungen zu beseitigen indem gleiche Dinge zusammengefasst und unterschiedliche Dinge austauschbar gemacht werden

```java
class IntList { ... }
class DoubleList { ... }
class PersonList { ... }
class StringList { ... }
```

- Die Listen sind identisch, sie unterscheiden sich nur in den Elementen, die sie beinhalten

- Über einen Typparameter kann der Typ der Elemente austauschbar gemacht werden

```java
class List<T> { ... }
```

`T` ist der Name eines beliebigen Typs und kann auch wie jeder andere Typ benutzt werden:

```java
class Box<T> {

  final T value;

  public Box(final T value) {
    this.value = value;
  }
}
```

Benutzung:

```java
Box<Integer> boxOfInt = new Box<Integer>(5);
Box<String> boxOfString = new Box<String>("hello world");
Box<Person> boxOfPerson = new Box<Person>(new Person("simon", 22));

// in Java 7

Box<Integer> boxOfInt = new Box<>(5);
Box<String> boxOfString = new Box<>("hello world");
Box<Person> boxOfPerson = new Box<>(new Person("simon", 22));
```

- Das Konzept wird als Generics bezeichnet

- Ein Typ, der einen generischen Typ erwartet, wird als parametrisierter Typ bezeichnet

Wir können IntIterator damit generisch machen:
```java
interface IntIterator<T> {

  boolean hasNext();
  
  T next();
}
```

Einschränkungen:

- Primitve Datentypen können nicht als Typparameter benutzt werden

- Man kann anstatt `Box<String` auch nur `Box` schreiben, dann hat man aber keine Typsicherheit mehr → nicht zu empfehlen

- Generics werden vom Java Compiler zur Compilezeit entfernt → Zur Laufzeit existieren sie nicht mehr

Typparameter können nicht nur an Klassen, sondern auch an Methoden gesetzt werden:

```java
<T> String mkString(List<T> list) {
  return list.mkString("List(", ", ", ")");
}
```

`T` ist hier nur innerhalb der Methode gültig.

##Iterable und die foreach-loop

Iterable ist in Java ein Interface, das einen Iterator zurück gibt:

```java
interface Iterable<T> {

  /**
   * Returns an iterator over a set of elements of type T.
   * 
   * @return an Iterator.
   */
  Iterator<T> iterator();
}
```

Es erlaubt uns die foreach-loop zu benutzen:

```java
// for Iterable
Iterable<Integer> list = List.of(1, 2, 3, 4, 5);
for (Integer i : list) {
  System.out.println(i);
}

// for arrays
int[] array = { 1, 2, 3, 4, 5 };
for (int i : array) {
  System.out.println(i);
}
```

Generelle Form der foreach-loop:
```
for (<type> <name> : <iterable_or_array>) {
  <body>
}
```

Jetzt gibt es noch viel mehr ...

- Wildcard-Types: `Box<?> box = new Box<>(something);`
- Upper-Type-Bounds: `Box<? extends Person> box = new Box<>(something);`
- Lower-Type-Bounds: `Box<? super Person> box = new Box<>(something);`
- Varianzen: `<C extends Comparable<? super C>> C maximum(List<? extends C> list) { ... }`

... was aber eher für das Selbststudium geeignet ist.