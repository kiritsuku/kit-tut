#Tutorium 12 (25.01.13)

Quiz am Anfang (Kompiliert der Code? Wird er korrekt ausgeführt? Was wird ausgegeben?):

```java
```

##Häufige Fehler auf dem Übungsblatt

###Konstanten verwenden

Anstelle die Werte hartkodiert in den Quelltext zu schreiben, diese über Konstanten austauschbar machen:
```java
// not this
void test(int x) {
  if (x == 42) ...
  else ...
}
// but this
static final int ANSWER = 42;
void test(int x) {
  if (x == ANSWER) ...
  else ...
}
```

###Enum anstelle von Konstanten verwenden

Konstanten, die durch primitive Datentypen repräsentiert werden, sind fehleranfällig:

```java
public static final int DOG = 1 << 0;
public static final int CAT = 1 << 1;
public static final int MOUSE = 1 << 2;

public String sound(final int animal) {
  switch (animal) {
    case DOG:
      return "wuff";
    case CAT:
      return "miau";
    case MOUSE:
      return "meep";
    default:
      return "";
  }
}
```

- Sie haben kein Verhalten

- Man kann den Konstantennamen überhaupt nicht (`sound(1)`) oder sogar ungültige Werte (`sound(100)`) verwenden

- Falls int-Konstanten doch einmal benötigt werden, dann am besten nur Zweierpotenzen benutzen (bekommt man über `1 << n`). Das erlaubt die Konstanten zu maskieren:

  ```java
  int mask = CAT | MOUSE;
  boolean containsDog = (mask & DOG) != 0;      // false
  boolean containsCat = (mask & CAT) != 0;      // true
  boolean containsMouse = (mask & MOUSE) != 0;  // true
  ```

Besser:

```java
enum Animal {

  DOG("wuff"), CAT("miau"), MOUSE("meep");

  private final String sound;

  private Animal(final String sound) {
    this.sound = sound;
  }

  public String sound() {
    return sound;
  }
}
```

###Konsolenausgaben konkatenieren

I/O ist langsam → auf ein Minimum reduzieren

```java
void print() {
  for (int i = 0; i < x; ++i) {
    for (int j = 0; j < y; ++j) {
      System.out.print(arr[i][j]);
    }
    System.out.println();
  }
}
```

Beis jedem Aufruf von `print` oder `println` muss ein sogenannter `syscall` (Aufruf des Betriebssystems) ausgeführt werden! Dieses muss dann die Hardware aktualisieren (Bildschirme etc.). Deshalb sollte man einen Weg suchen wie man die Anzahln der aufgerufenen `syscalls` verringert.

Geht im Beispiel über einen `StringBuilder`:

```java
void print() {
  StringBuilder sb = new StringBuilder(sizeHint);
  
  for (int i = 0; i < x; ++i) {
    for (int j = 0; j < y; ++j) {
      sb.append(arr[i][j]);
    }
    sb.append("\n");
  }
  System.out.println(sb.toString());
}
```

`javac` benutzt zur Konkatenation von Strings übrigens auch einen `StringBuilder`:

```java
String welcome = "Welcome " + firstName + " " + lastName + "!"
// compiles to
String welcome = new StringBuilder()).append("Welcome ").append(firstName).append(" ").append(lastName).append("!").toString();
```

Wir können uns anschauen was für einen Code `javac` erzeugt, indem wir `javap - The Java Class File Disassembler` benutzen:

```java
public class Test {
  String firstName = "a";
  String lastName = "b";
  String welcome = "Welcome " + firstName + " " + lastName + "!";
}
```

Kompilieren: `javac Test.java`

Disassemblieren: `javap -private -s -verbose -c -l Test`

`javap` spuckt Bytecode aus, wer den nicht lesen kann, der kann sich auch wieder Java Code erzeugen lassen. Dafür wird aber ein externes Tool benötigt, wie z.B. [jad](http://www.varaneckas.com/jad/) oder [jd-gui](http://java.decompiler.free.fr/?q=jdgui). 

`jad -p Test` erzeugt:

```java
// Decompiled by Jad v1.5.8e. Copyright 2001 Pavel Kouznetsov.
// Jad home page: http://www.geocities.com/kpdus/jad.html
// Decompiler options: packimports(3) 
// Source File Name:   Test.java


public class Test
{

    public Test()
    {
        firstName = "a";
        lastName = "b";
        welcome = (new StringBuilder()).append("Welcome ").append(firstName).append(" ").append(lastName).append("!").toString();
    }

    String firstName;
    String lastName;
    final String welcome;
}
```

`jd-gui` + Auswählen des Bytecodes erzeugt:

```java
public class Test
{
  String firstName = "a";
  String lastName = "b";
  final String welcome = "Welcome " + this.firstName + " " + this.lastName + "!";
}
```

###Objekte immer initialisieren

```java
class Person {
  private String name;
  private int age;

  public String getName() {
    return name;
  }

  public void setName(final String name) {
    this.name = name;
  }

  public int getAge() {
    return age;
  }

  public void setAge(final int age) {
    this.age = age;
  }
}
```

Und dann:

```java
Person person = new Person();
person.setAge(50);
person.setName("hugo");
```

Dies ist fehleranfällig, da der Aufruf der Setter vergessen werden kann. Lieber unveränderliche Objekte benutzen:

```java
class Person {
  final String name;
  final int age;

  public Person(final String name, final int age) {
    this.name = name;
    this.age = age;
  }
}
```

###Das Spielfeld darf objektorientiert sein

- Anstelle von `int[][] board` ein `Cell[][] board` oder sogar `Map<Position, Cell> board`
 
- `Cell` kann wieder über ein enum dargestellt werden

- `Position` könnte ein Objekt mit `x` und `y` Werten sein

###DRY (Don't repeat yourself) einhalten

- Standard Vorgehensweise um ein Spielzug zu validieren: Ein Spielfeld nach allen acht Richtungen absuchen.

- Man kann nun für jede Richtung eine eigene Methode schreiben: `validateNorth()`, `validateNorthEast()`, `validateEast()`...

- Man kann aber auch nur eine `validate()` Methode schreiben und dieser die Richtung mitgeben

Beispiel:

```java
Result validateNorth(final Position playerPos) {
  Position curPos = playerPos;
  Result result = ...
  while (curPos.isValid(width, height) && isMoveValid(curPos)) {
    if (isOfPlayer(curPos, curPlayer)) {
      result = ...
    }
    curPos = new Position(curPos.x, curPos.y - 1);
  }
  return result;
}

// + 7 similar methods

// use with
Result north = validateNorth(playerPos);
Result northEast = validateNorthEast(playerPos);
Result east = validateEast(playerPos);
// + 5 similar calls
```

Dies könnte generalisiert werden zu:

```java
Result validate(final Position playerPos, final int xDir, final int yDir) {
  Position curPos = playerPos;
  Result result = ...
  while (curPos.isValid(width, height) && isMoveValid(curPos)) {
    if (isOfPlayer(curPos, curPlayer)) {
      result = ...
    }
    curPos = new Position(curPos.x + xDir, curPos.y + yDir);
  }
  return result;
}

// use with
Result north = validate(playerPos, 0, -1);
Result northEast = validate(playerPos, -1, -1);
Result east = validate(playerPos, 1, 0);
// + 5 similar calls
```

Das was sich ändert wird austauschbar gemacht. In diesem Fall wäre das die Richtungsangabe.

Aber selbst die Richtungsangabe kann noch weiter generalisiert werden:

```java
interface Direction {

  Direction NORTH = new Direction() {
    public Position move(final Position pos) {
      return new Position(pos.x, pos.y - 1);
    }
  };

  Direction NORTH_EAST = new Direction() {
    public Position move(final Position pos) {
      return new Position(pos.x + 1, pos.y - 1);
    }
  };

  // 6 similar position objects

  List<Direction> directions = Arrays.asList(NORTH, NORTH_EAST);

  Position move(Position pos);
}

Result validate(final Position playerPos, final Direction dir) {
  Position curPos = playerPos;
  Result result = ...
  while (curPos.isValid(width, height) && isMoveValid(curPos)) {
    if (isOfPlayer(curPos, curPlayer)) {
      result = ...
    }
    curPos = dir.move(curPos);
  }
  return result;
}

// use with
for (Direction dir : Direction.directions) {
  results.add(validate(playerPos, dir));
}
```

###Kleinigkeiten

Vereinfachung:
```java
// instead of
char digits = { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' };

// use the following
char digits[] = "0123456789".toCharArray()
```

Noch eine Vereinfachung:
```java
// please avoid this
void test() {
  if (isTrue1) {
    if (isTrue2) {
      if (isTrue3) {
        ...
      } else {
        ...
      }
    } else {
      ...
    }
  } else {
    ...
  }
}

// what about?
void test() {
  if (!isTrue1) {
    ...
    return;
  }
  if (!isTrue2) {
    ...
    return;
  }
  if (!isTrue3) {
    ...
    return;
  }
  ...
}
```

Statische Member auch statsich adressieren:

```java
class Static {
  static final int i = 0;
}

// not
Static s = new Static();
int i = s.i;
// but
int i = Static.i;
```

##Random

Über `java.util.Random` kann man sich Psoudo-Zufallszahlen berechnen lassen:

```java
import java.util.Random;

public class Test {

  public static void main(final String... args) {
    final String key = getKey(6);
    System.out.println(key);
  }

  static String getKey(final int size) {
    if (size < 1 || size >= 9) {
      return "";
    }
    final Random random = new Random();

    final StringBuilder sb = new StringBuilder(size);
    while (sb.length() < size) {
      sb.append(random.nextInt(9));
    }
    return sb.toString();
  }
}
```

- `StringBuilder` für eine schnelle Konkatenation

- `Random` einmal zu Beginn erzeugen und nicht in jedem Iterationsschritt aufs neue

##File I/O

Um eine Datei zu lesen und wideer zu schreiben genügt folgender Code:

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.Scanner;

public class Test {

  public static void main(final String... args) {
    try {
      final Scanner in = new Scanner(new File("<filename>")).useDelimiter("\0");
      final String code = in.next();

      final PrintWriter out = new PrintWriter(new File("<filename>"));
      out.print(code);
      out.close();
    } catch (final FileNotFoundException e) {
      System.out.println(e.getMessage());
    }
  }
}
```

- `Scanner` kann einen `InputStream` lesen

- Mit `useDelimiter("\0")` wird bis zum Ende einer Datei gelesen

- `PrintWriter` kann einen `OutputStream` schreiben

- Es muss eine `FileNotFoundException` gefangen werden

##Java Toolchain

###JRebel

- JRebel ist das geilste Tool seit es Tools gibt!

- Plugin für die JVM um Bytecode zur Laufzeit austauschen zu können → Verbessert Entwicklungs-Test-Zyklus

- Zuerst müssen wir JRebel installieren: [Installationsanleitung für Eclipse](http://zeroturnaround.com/software/jrebel/download/installing-jrebel-plugin-for-eclipse/)

- JRebel muss dann noch speziell für ein Projekt aktiviert werden: In Eclipse `Rechtsklick auf Projekt → JRebel → Add JRebel Nature`.

Beispiel zur Benutzung: Wir nehmen den bisherigen [Source Code unserer Shell](../java-tutorial/entwicklung-shell-fort.md) und führen diesen aus.

Wir sehen z.B. folgende Ausgabe:

```
<JRebel console output>
Welcome to Number Collector!
> create
create[]
Error! Invalid number of arguments! Expected format: create <name>
> create
[2013-01-23 00:31:02] JRebel: Reloading class 'edu.kit.test.Shell'.
Error! Invalid number of arguments! Expected format: create <name>
> create simon
time: 0.24993ms
```

Wie zu erkennen hat sich die Ausgabe zwischen dem ersten und zweiten Befehl geändert. Die Debug-Ausgabe des Befehls+Argumente ist verschwunden, dafür kommt eine Meldung von JRebel. Es hat die Klasse `Shell` neu geladen. Ich habe einfach nur in `Shell#handleCommand` die erste Zeile entfernt (das `System.out.println(command+args);`), gespeichert und JRebel hat die Änderungen im laufenden Program Verfügbar gemacht.

Weitere Änderung: Wir wollen einen Befehl `count <name>` hinzufügen, der die Anzahl der Zahlen eines Benutzers auf der Konsole ausgibt.

Die Änderungen in der Klasse `Shell` sind einfach:

```java
private void handleCommand(final String command, final List<String> args) {
  if (":q".equals(command)) {
    shutdown();
  } else if ("create".equals(command)) {
    createUser(args);
  } else if ("add".equals(command)) {
    addNumbers(args);
  } else if ("contains".equals(command)) {
    containsNumber(args);
  } else if ("count".equals(command)) {
    count(args);
  } else {
    System.out.println("command not found: " + command);
  }
}

private void count(final List<String> args) {
  if (args.size() != 1) {
    throw new IllegalArgumentException("Invalid number of arguments! Expected format: count <name>");
  }
  final int count = database.count(args.get(0));
  System.out.println(count);
}
```

Für `Database` müssen wir auch nicht viel tun:

```java
public int count(final String name) {
  final Set<Integer> set = datasets.get(name);
  if (set == null) {
    throw new IllegalArgumentException(String.format("username '%s' doesn't exist!", name));
  }
  return set.size();
}
```

Wenn wir jetzt noch nicht gespeichert haben, dann erhalten wir folgende Ausgabe auf der Shell:

```
> add simon 1-10
time: 0.163424ms
> count simon
command not found: count
time: 0.198247ms
```

Nun speichern wir:

```
> count simon
[2013-01-23 00:35:40] JRebel: Reloading class 'edu.kit.test.Shell'.
[2013-01-23 00:35:40] JRebel: Reloading class 'edu.kit.test.Database'.
10
time: 38.83041ms
> count simon
10
time: 0.169998ms
```

Es funktioniert, der Code wurde neu geladen! Ist das nicht geil? Sehr schön ist auch zu sehen wie sich die gemessene Zeit verändert. Beim ersten mal muss der Bytecode interpretiert werden, daher ist es sehr langsam. Beim zweiten Mal hat der JIT-Compiler der JVM den Code bereits kompiliert und kann ihn nativ ausführen.

Einziger Nachteil: Das Tool ist kommerziell. Man kann sich aber einen Free-Account sichern sofern man seinen Twitter- oder Facebook-Account (oder Zweitaccount...) angibt.

###Erstellen eines ausführbaren Programms

Geht bei _kleinen_ Programmen am einfachsten über Eclipse:

`File → Export → Java → Runnable JAR file → Launch configuration auswählen → Finish`

Nun kann die exportierte JAR über folgendes Kommande gestartet werden:

`java -jar <name>.jar`

Über `jar tf <name>.jar` kann man sich außerdem den Inhalt des JARs ansehen.

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://i.imgur.com/pG3q7.jpg)