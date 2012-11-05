#Javadoc
Code sollte dokumentiert werden, indem beschrieben wird was er macht und nicht wie er funktioniert. Ein Beispiel:

```java
/**
 * Create a new Rational.
 * 
 * @param numerator
 *        the numerator
 * @param denominator
 *        the denominator
 * @return the created Rational
 */
public static Rational valueOf(int numerator, int denominator) {
  return new Rational(numerator, denominator);
}
```

Man könnte noch mehr dokumentieren, z.B. was `numerator` und `denominator` sind. Da dies im gewählten Beispiel aber sebstverständlich ist, ist es nicht unbedingt notwendig die Dokumentation dahingehend anzupassen.

#Zukünftige Erweiterungen

Das vorliegende Beispiel hat noch einige Schwachstellen:

* `Rational` wird nicht gekürzt, dies führt zu umständlichen Darstellungen (z.B. 9/9 anstatt 1/1 oder nur 1). Kann mit Abfragen/Schleifen behoben werden -> Berechnung des GGT und anschließende Kürzung von Nenner und Zähler

* Es ist noch nicht möglich verschiedene Instanzen von `Rational` miteinander zu vergleichen.

* Es ist möglch 1/0 als gültiges `Rational` zu erstellen -> kann z.B. durch den Einsatz von Exceptions behoben werden

* Negative Nenner sind darstellbar -> Behebung durch Abfragen

* Über `Rational#valueOf` kann Caching eingefügt werden, das verhindert, dass unnötigerweise mehrere Instanzen eines unveränderlichen `Rational` existieren.

Berechnung des GGT:

```java
// iterative
int gcd(int a, int b) {
  while (b != 0) {
    int t = b;
    b = a % b;
    a = t;
  }
  return a;
}

// recursive
int gcd(int a, int b) {
  if (b == 0) {
    return a;
  } else {
    return gcd(b, a % b);
  }
}

// recursive (elegant)
int gcd(int a, int b) {
  return b == 0 ? a : gcd(b, a % b);
}
```

Benutzung des GGT:

```java
// in class Rational
Rational(int numerator, int denominator) {
  int g = gcd(Math.abs(numerator), Math.abs(denominator));
  this.numerator = numerator / g;
  this.denominator = denominator / g;
}
```

Bessere Implementierung von `toString`

```java
// in class Rational
public String toString() {
  return denominator == 1
      ? String.valueOf(numerator)
      : numerator + "/" + denominator;
}
```

Bearbeitung von negativen Nennern:

```java
// in class Rational
public static Rational valueOf(int numerator, int denominator) {
  int g = gcd(Math.abs(numerator), Math.abs(denominator));
  return denominator > 0
      ? new Rational(numerator / g, denominator / g)
      : new Rational(-numerator, -denominator);
}

// set ctor of Rational private to forbid creation by new
private Rational(int numerator, int denominator) {
  this.numerator = numerator;
  this.denominator = denominator;
}
```

Prüfung ob Nenner gleich 0:

```java
class Requirements {
  public static void require(boolean requirement) {
    if (!requirement) {
      throw new AssertionError("requirement failed");
    }
  }
  
  public static void require(boolean requirement, String message) {
    if (!requirement) {
      throw new AssertionError("requirement failed:" + message);
    }
  }
}

// in class Rational
public static Rational valueOf(int numerator, int denominator) {
  Requirements.require(denominator != 0, "denominator is zero");
  // rest as before
}
```

Implementierung von `Comparable`:

```java
class Rational implements Comparable<Rational> {
  // rest as before
  public int compareTo(Rational r) {
    return numerator * r.denominator - denominator * r.numerator;
  }
}
```

Implementierung von `equals` und `hashCode`:

```java
// in class Rational
public boolean equals(Object obj) {
  return obj instanceof Rational ? compareTo((Rational) obj) == 0 : false;
}

public int hashCode() {
  return toString().hashCode();
}
```

Simples caching für `Rational` einfügen:

```java
// in class Rational
private static final Map<String, Rational> cache = new HashMap<String, Rational>();

/**
 * Create a new Rational. Internally instances of Rational are cached, thus
 * there is no new one created if one already exists.
 * 
 * @param numerator
 *        the numerator
 * @param denominator
 *        the denominator
 * @return the created Rational
 */
public static Rational valueOf(int numerator, int denominator) {
  Requirements.require(denominator != 0, "denominator is zero");

  int g = gcd(Math.abs(numerator), Math.abs(denominator));
  int n = denominator > 0 ? numerator / g : -numerator;
  int d = denominator > 0 ? denominator / g : -denominator;

  String key = n + "/" + d;
  if (cache.containsKey(key)) {
    return cache.get(key);
  }

  Rational r = new Rational(n, d);
  cache.put(r.toString(), r);
  return r;
}
```

---

Der vollständige Code mit allen Erweiterungen:

```java
import java.util.HashMap;
import java.util.Map;


public class RationalTest {

  public static void main(String[] args) {
    Rational oneThird = Rational.valueOf(1, 3);
    Rational twoThird = Rational.valueOf(2, 3);
    System.out.println(oneThird);

    Rational one = oneThird.add(twoThird);
    System.out.println(one);

    Rational oneThirdNegative = oneThird.sub(twoThird);
    System.out.println(oneThirdNegative);

    Rational sevenThird = oneThird.add(Rational.valueOf(2));
    System.out.println(sevenThird);

    Rational fourThird = oneThird.add(1);
    System.out.println(fourThird);

    System.out.println(Rational.valueOf(5, 3));
  }
}

class Requirements {

  public static void require(boolean requirement) {
    if (!requirement) {
      throw new IllegalArgumentException("requirement failed");
    }
  }

  public static void require(boolean requirement, String message) {
    if (!requirement) {
      throw new IllegalArgumentException("requirement failed:" + message);
    }
  }
}

class Rational implements Comparable<Rational> {

  private final int numerator;
  private final int denominator;

  private static final Map<String, Rational> cache = new HashMap<String, Rational>();

  /**
   * Create a Rational.valueOf. Internally instances of Rational are cached,
   * thus there is no new one created if one already exists.
   * 
   * @param numerator
   *        the numerator
   * @param denominator
   *        the denominator
   * @return the created Rational
   */
  public static Rational valueOf(int numerator, int denominator) {
    Requirements.require(denominator != 0, "denominator is zero");

    int g = gcd(Math.abs(numerator), Math.abs(denominator));
    int n = denominator > 0 ? numerator / g : -numerator;
    int d = denominator > 0 ? denominator / g : -denominator;

    String key = n + "/" + d;
    if (cache.containsKey(key)) {
      return cache.get(key);
    }

    Rational r = new Rational(n, d);
    cache.put(r.toString(), r);
    return r;
  }
  
  /*
   * I'm too lazy to add further documentation to remaining members. ;)
   */

  public static Rational valueOf(int numerator) {
    return valueOf(numerator, 1);
  }
  
  private Rational(int numerator, int denominator) {
    this.numerator = numerator;
    this.denominator = denominator;
  }

  public int getNumerator() {
    return numerator;
  }

  public int getDenominator() {
    return denominator;
  }

  public Rational add(int i) {
    return Rational.valueOf(numerator + i * denominator, denominator);
  }

  public Rational add(Rational r) {
    return Rational.valueOf(
        numerator * r.denominator + r.numerator * denominator,
        denominator * r.denominator);
  }

  public Rational sub(int i) {
    return Rational.valueOf(numerator - i * denominator, denominator);
  }

  public Rational sub(Rational r) {
    return Rational.valueOf(
        numerator * r.denominator - r.numerator * denominator,
        denominator * r.denominator);
  }

  public String toString() {
    return denominator == 1
        ? String.valueOf(numerator)
        : numerator + "/" + denominator;
  }

  public boolean equals(Object obj) {
    return obj instanceof Rational ? compareTo((Rational) obj) == 0 : false;
  }

  public int hashCode() {
    return toString().hashCode();
  }

  public int compareTo(Rational r) {
    return numerator * r.denominator - denominator * r.numerator;
  }

  private static int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
  }
}
```