#Methoden und Konstruktoren

Nur Klassen zu bauen ist langweilig. Schauen wir mal ob wir sie auch zu etwas gebrauchen können.

Bauen wir uns unseren eigenen Datentyp: Rationale Zahlen.

```java
class Rational {
  int numerator;
  int denominator;
}
```

Testen wir den Code mal:

```java
public class RationalTest {
  public static void main(String[] args) {
    Rational oneThird = new Rational();
    oneThird.numerator = 1;
    oneThird.denominator = 3;
    System.out.println(oneThird);
  }
}
```

Mit der Implementierung von `toString()` können wir eine schöne Konsolenausgabe erreichen:

```java
// in class Rational
public String toString() {
  return numerator + "/" + denominator;
}
```

Mit Hilfe eines eigenen Konstruktors ist es einfacher ein Objekt zu erstellen:

```java
// in class Rational
Rational(int numerator, int denominator) {
  this.numerator = numerator;
  this.denominator = denominator;
}
```

Überprüfung ob alles funktioniert:

```java
public class RationalTest {
  public static void main(String[] args) {
    Rational oneThird = new Rational(1, 3);
    System.out.println(oneThird);
  }
}
```

Hinzufügen der ersten Methode:

```java
// in RationalTest
static void addRationals(Rational r1, Rational r2) {
  r1.numerator = r1.numerator * r2.denominator + r2.numerator * r1.denominator;
  r1.denominator = r1.denominator * r2.denominator;
}

static void subRationals(Rational r1, Rational r2) {
  r1.numerator = r1.numerator * r2.denominator - r2.numerator * r1.denominator;
  r1.denominator = r1.denominator * r2.denominator;
}
```

Die genaue Bedeutung von `static` wird später erklärt. Schauen wir uns lieber an ob der Code funktioniert:

```java
public class RationalTest {
  public static void main(String[] args) {
    Rational oneThird = new Rational(1, 3);
    Rational twoThird = new Rational(2, 3);
    System.out.println(oneThird);

    addRationals(oneThird, twoThird);
    System.out.println(oneThird);
    
    subRationals(oneThird, twoThird);
    System.out.println(oneThird);
  }
  // ...
}
```

Hier sollte auffallen, dass das Ergebnis nicht ganz korrekt bzw. die Semantik ungenau ist. Da `Rational` veränderlich ist, passt der Name unserer Variable `oneThird` nicht mehr zum Inhalt. Folgich ist die zweite Operation, bei der wir zwei Drittel von einem Drittel subtrahieren wollen, auch nicht mehr korrekt. Wir können dies ändern indem wir `Rational` unveränderlich machen:

```java
class Rational {
  final int numerator;
  final int denominator;
}
```

Nun kompiliert unser Code aber nicht mehr, wir müssen unsere Methoden anpassen, so dass sie ein neues `Rational` zurück geben:

```java
// in class RationalTest
static Rational addRationals(Rational r1, Rational r2) {
  int newNumerator = r1.numerator * r2.denominator + r2.numerator * r1.denominator;
  int newDenominator = r1.denominator * r2.denominator;
  Rational newRational = new Rational(newNumerator, newDenominator);
  return newRational;
}

static Rational subRationals(Rational r1, Rational r2) {
  int newNumerator = r1.numerator * r2.denominator - r2.numerator * r1.denominator;
  int newDenominator = r1.denominator * r2.denominator;
  Rational newRational = new Rational(newNumerator, newDenominator);
  return newRational;
}
```

Der Code bisher ist nicht sonderlich schön, machen wir die bisherigen statischen Methoden zu Objekt-Methoden:

```java
// in class Rational
Rational add(Rational r) {
  int newNumerator = numerator * r.denominator + r.numerator * denominator;
  int newDenominator = denominator * r.denominator;
  Rational newRational = new Rational(newNumerator, newDenominator);
  return newRational;
}

Rational sub(Rational r) {
  int newNumerator = numerator * r.denominator - r.numerator * denominator;
  int newDenominator = denominator * r.denominator;
  Rational newRational = new Rational(newNumerator, newDenominator);
  return newRational;
}
```

Da der Inhalt unserer Ausgangsvariablen nun nicht mehr verändert wird müssen wir neue anlegen:

```java
public class RationalTest {
  public static void main(String[] args) {
    Rational oneThird = new Rational(1, 3);
    Rational twoThird = new Rational(2, 3);
    System.out.println(oneThird);

    Rational one = oneThird.add(twoThird);
    System.out.println(one);

    Rational oneThirdNegative = oneThird.sub(twoThird);
    System.out.println(oneThirdNegative);
  }
}
```

Die temporären Variablen sind unnötig, wir können sie also weglassen:

```java
// in class Rational
Rational add(Rational r) {
  return new Rational(
      numerator * r.denominator + r.numerator * denominator,
      denominator * r.denominator);
}

Rational sub(Rational r) {
  return new Rational(
      numerator * r.denominator - r.numerator * denominator,
      denominator * r.denominator);
}
```

Wir wollen nun auch mit normalen Integern in `Rational` arbeiten können:

```java
// in class Rational
Rational add(int i) {
  return add(new Rational(i, 1));
}
```

Diesen Vorgang nennen wir "Überladen". Da die Erzeugung eines temporären `Rational` unnötig ist, können wir den Methodenkörper auch ein wenig verändern:

```java
// in class Rational
Rational add(int i) {
  return new Rational(numerator + i * denominator, denominator);
}
```

Eine weitere Verbesserung ist ein zusätzlicher Konstruktor, der uns Arbeit abnimmt:

```java
// in class Rational
Rational(int numerator) {
  this(numerator, 1);
}
```

Damit können wir nun ganze Zahlen komfortabel repräsentieren.

#Sichtbarkeiten

Anhand von `toString`, `main` und `RationalTest` haben wir sie schon gesehen: die Sichtbarkeits-Modifizierer. Es gibt `private`, `public`, `protected` und ein default-Wert.

Fügen wir sie hinzu:

```java
class Rational {

  private final int numerator;
  private final int denominator;
  
  // add public keyword to methods and ctors
  // ...
}
```

Um nun auf `numerator` und `denominator` zugreifen zu können bedarf es Gettern:

```java
// in class Rational
public int getNumerator() {
  return numerator;
}

public int getDenominator() {
  return denominator;
}
```

Setter benötigen wir keine, da unsere Attribute unveränderlich sind. Noch haben wir jedoch nicht sonderlich viel mit den Sichtbarkeits-Modifizierern erreicht, interessanter wird es erst wenn wir uns packages anschauen. Doch zuvor will `static` noch erklärt werden, z.B. anhand folgenden Beispiels:

```java
// in class Rational
public static Rational valueOf(int numerator, int denominator) {
  // possibly cache Rational objects
  return new Rational(numerator, denominator);
}
```

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
      throw new IllegalArgumentException("requirement failed");
    }
  }
  
  public static void require(boolean requirement, String message) {
    if (!requirement) {
      throw new IllegalArgumentException("requirement failed:" + message);
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
Zum Schluss nun noch der bisherige Code (ohne zukünftige Erweiterungen):

```java
public class RationalTest {

  public static void main(String[] args) {
    Rational oneThird = new Rational(1, 3);
    Rational twoThird = new Rational(2, 3);
    System.out.println(oneThird);

    Rational one = oneThird.add(twoThird);
    System.out.println(one);

    Rational oneThirdNegative = oneThird.sub(twoThird);
    System.out.println(oneThirdNegative);

    Rational sevenThird = oneThird.add(new Rational(2));
    System.out.println(sevenThird);

    Rational fourThird = oneThird.add(1);
    System.out.println(fourThird);
    
    System.out.println(Rational.valueOf(5, 3));
  }
}

class Rational {

  private final int numerator;
  private final int denominator;

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

  /**
   * Create a new Rational.
   * 
   * @param numerator
   *        the numerator
   * @param denominator
   *        the denominator
   */
  public Rational(int numerator, int denominator) {
    this.numerator = numerator;
    this.denominator = denominator;
  }
  
  /*
   * I'm too lazy to add further documentation to remaining members. ;) 
   */

  public Rational(int numerator) {
    this(numerator, 1);
  }

  public int getNumerator() {
    return numerator;
  }
  
  public int getDenominator() {
    return denominator;
  }

  public Rational add(int i) {
    return new Rational(numerator + i * denominator, denominator);
  }

  public Rational add(Rational r) {
    return new Rational(
        numerator * r.denominator + r.numerator * denominator,
        denominator * r.denominator);
  }

  public Rational sub(int i) {
    return new Rational(numerator - i * denominator, denominator);
  }

  public Rational sub(Rational r) {
    return new Rational(
        numerator * r.denominator - r.numerator * denominator,
        denominator * r.denominator);
  }

  public String toString() {
    return numerator + "/" + denominator;
  }
}
```

Und mit den Erweiterungen:

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