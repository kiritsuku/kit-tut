#Tutorium 10 (11.01.13)

Quiz am Anfang (Kompiliert der Code? Wird er korrekt ausgeführt? Was wird ausgegeben?):

```java
public class Test {
  public static void main(final String... args) {
    final String[] a1 = { "abc" };
    final Object[] a2 = a1;
    a2[0] = 17;
    System.out.println(a1[0]);
  }
}
```

##Wiederholung

Zuletzt wurden `Interfaces` und `Generics` behandelt. In Kombination kann man mit ihnen z.B. eine Klasse schreiben, deren Objekte sich ordnen lassen:

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;


public class Test {

  public static void main(final String... args) {
    final List<IntBox> list = Arrays.asList(
        new IntBox(30), new IntBox(17), new IntBox(3), new IntBox(49));

    System.out.println(list);
    Collections.shuffle(list);
    System.out.println(list);
    Collections.sort(list);
    System.out.println(list);
  }
}

class IntBox implements Comparable<IntBox> {

  public final int value;

  public IntBox(final int value) {
    this.value = value;
  }

  public int compareTo(final IntBox o) {
    return value - o.value;
  }

  @Override
  public String toString() {
    return "IntBox(" + value + ")";
  }
}
```

`IntBox` kann selbst generisch gemacht werden:

```java
import java.util.ArrayList;
import java.util.Collections;


public class Test {

  public static void main(final String... args) {
    final ArrayList<Box<Integer>> list = new ArrayList<Box<Integer>>();
    list.add(new Box<Integer>(30));
    list.add(new Box<Integer>(17));
    list.add(new Box<Integer>(3));
    list.add(new Box<Integer>(49));

    System.out.println(list);
    Collections.shuffle(list);
    System.out.println(list);
    Collections.sort(list);
    System.out.println(list);
  }
}

class Box<T extends Comparable<T>> implements Comparable<Box<T>> {

  public final T value;

  public Box(final T value) {
    this.value = value;
  }

  public int compareTo(final Box<T> o) {
    return value.compareTo(o.value);
  }

  @Override
  public String toString() {
    return "Box(" + value + ")";
  }
}
```

##Know your API

Neben Listen bietet die Java API auch noch andere Collection-Arten wie z.B. `Sets` an:

```java
public class Test {

  public static void main(final String... args) {
    final HashSet<Box<Integer>> set = new HashSet<Box<Integer>>();
    set.add(new Box<Integer>(30));
    set.add(new Box<Integer>(17));
    set.add(new Box<Integer>(3));
    set.add(new Box<Integer>(49));
    set.add(new Box<Integer>(17));

    System.out.println(set);
  }
}
```

Ein `Set` speichert keine Duplikate. Wenn wir obigen Code ausführen erhalten wir aber folgende Ausgabe:

```
[Box(17), Box(17), Box(3), Box(49), Box(30)]
```

Die `Box(17)` ist zwei Mal vorhanden, da wir `equals` und `hashCode` nicht überschrieben haben:

```java
// in class Box

@Override
public int hashCode() {
  return value.hashCode();
}

@Override
public boolean equals(final Object obj) {
  if (obj instanceof Box) {
    @SuppressWarnings("unchecked")
    final Box<T> o = (Box<T>) obj;
    return value.equals(o.value);
  }
  return false;
}
```

Empfehlung: `equals` und `hashCode` immer überschreiben wenn die entsprechenden Objekte in einer Collection abgelegt werden sollen.

Ebenfalls sehr nützlich sind `Maps`:

```java
import java.util.HashMap;
import java.util.Map;


public class Test {

  public static void main(final String... args) {
    final HashMap<String, Box<Integer>> map = new HashMap<String, Box<Integer>>();
    map.put("a", new Box<Integer>(30));
    map.put("b", new Box<Integer>(17));
    map.put("c", new Box<Integer>(3));
    map.put("d", new Box<Integer>(49));

    System.out.println(map);
    for (final Map.Entry<String, Box<Integer>> entry : map.entrySet()) {
      System.out.println("element '" + entry.getKey() + "' is '" + entry.getValue() + "'");
    }
  }
}
```

`Map` speichert Key-Value-Paare. Mit dem Schlüssel kann nach einem Wert gesucht werden.

`Map` ist eigentlich keine `Collection` (es implementiert die Schnittstellen `Collection` und `Iterable` nicht), deswegen kann auch nicht mit der normalen foreach-loop über die Elemente iteriert werden.

###Laufzeitverhalten

- List
  - Einfügen: O(1)
  - Suchen: O(n)
  - Besonderheit: indiziert

- Set
  - Einfügen: O(log n)
  - Suchen: O(1) bzw. O(log n)
  - Besonderheit: nicht indiziert

- Map
  - Einfügen: O(log n)
  - Suchen: O(1) bzw. O(log n)
  - Besonderheit: nicht indiziert

##Entwicklung einer Shell
[Link zum Tutorial](../java-tutorial/entwicklung-shell.md)

##Rekursion

Rekursion bezeichnet den Selbstaufruf einer Funktion:

```java
public class Test {

  public static void main(final String... args) {
    for (int i = 0; i < 10; ++i) {
      System.out.println(fib(i));
    }
  }

  public static long fib(final int n) {
    if (n <= 1) {
      return 1;
    }
    return fib(n - 1) + fib(n - 2);
  }
}
```

Das genannte Beispiel hat jedoch eine sehr schlechte Laufzeit. Zum Testen ein kleiner Benchmark:

```java
class Benchmark {

  static <T> T time(final boolean warmup, final TimeTest<T> f) {
    if (warmup) {
      System.out.println("warmup...");
      for (int i = 0; i < 1000; ++i) {
        f.execute();
      }
    }

    System.out.println("starting benchmark...");
    final long start = System.nanoTime();
    final T t = f.execute();
    System.out.println("time: " + (System.nanoTime() - start) / 1e6 + "ms");
    return t;
  }
}

interface TimeTest<T> {

  T execute();
}
```

Shon bei `n = 45` benötigt der Code mehrere Sekunden um zu terminieren:

```java
public class Test {

  public static void main(final String... args) {
    Benchmark.time(false, new TimeTest<Long>() {

      public Long execute() {
        return fib(45);
      }
    });
  }

  public static long fib(final int n) {
    if (n <= 1) {
      return 1;
    }
    return fib(n - 1) + fib(n - 2);
  }
}
```

Ausgabe:

```
starting benchmark...
time: 8966.878365ms
```

##Das Wort zum Schluss

- Das verwenden der Pakete `java.lang` und `java.util` ist erlaubt

- Die Prüfungsanmeldung ist freigeschaltet

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](https://d2o7bfz2il9cb7.cloudfront.net/main-qimg-47f5765a1021678861138034755a0a7c)

Coded Movie:
```java
import disney.pocahontas.plot;

public class Avatar extends Pocahontas {
  public void plot() {
    setTimeEra("Future");
    setPlanet("NotEarth");
  }
}
```