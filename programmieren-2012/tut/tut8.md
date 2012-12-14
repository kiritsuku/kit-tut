#Tutorium 8 (14.12.12)

Quiz am Anfang: Was ist die Ausgabe des folgenden Programms?

```java
public class JTest2 {

  public static void main(String... args) {
    Class c1 = new Class();
    Class c2 = new Class();
    
    c1.inc();
    c2.inc();
    System.out.println(c1.i);
    System.out.println(c1.j);
    System.out.println(c2.i);
    System.out.println(c2.j);
  }
}

class Class {
  static int i = 0;
  int j = 0;
  
  void inc() {
    ++i;
    j = i;
  }
}
```

##Häufige Fehler auf Übungsblatt 3

####Sichtbarkeiten
Variablen nur public wenn diese `final` sind:
```java
class ImmutableIntBox {
  final int value;
  Box(int value) {
    this.value = value;
  }
}
```
Allerdings nur wenn ihr euch sicher seid, dass ihr später keine Getter mehr benötigt.

####Warum einfach wenn es auch umständlich geht?
```java
// matrix containing integers
String[][] matrix;
// WHY?

// =>
int[][] matrix;
```

```java
char c = str.substring(x, x + 1)[0];

// =>

char c = str.charAt(x);
```

Don't repeat yourself (DRY) einhalten:
```java
void a(char c) {
  boolean isNumber = c >= '0' && c <= '9';
  // ...
}

void b(char c) {
  boolean isNumber = c >= '0' && c <= '9';
  // ...
}

// =>

boolean isNumber(char c) {
  return c >= '0' && c <= '9';
}
```

Minimum von drei Zahlen:
```java
int min = Math.min(a, Math.min(b, c));
```

####Überprüfung von Parametern

- Die Korrekt von Parametern muss immer sichergestellt werden (Fahrt ihr Auto ohne euch anzuschnallen?)

- Ausnahme: Prüfung auf `null` (meistens) nicht notwendig

Null-Prüfung zwingend notwendig beim Einfügen eines Elements in eine Collection:
```java
StringList list = new StringList();

void add(String s) {
  list.add(s);
}

boolean containsEmptyString() {
  StringIterator iter = list.iterator();
  while (iter.hasNext()) {
    String elem = iter.next();
    // potential NPE here
    if (elem.isEmpty()) {
      return true;
    }
  }
  return false;
}
```
Besser:
```java
void add(String s) {
  list.add(s == null ? "" : s);
}
```
Aber: Unerwartetes Verhalten

Noch besser:
```java
/**
 * ...
 * @return false if s is null, true otherwise
 */
boolean add(String s) {
  if (s == null) {
    return false;
  }
  list.add(s);
  return true;
}
```

Eine Null-Prüfung wird oft nicht erwartet:
```java
String s = ...; // string containing only digits

// (1)
int i = Integer.valueOf(s);

// (2)
int i = s == null ? 0 : Integer.valueOf(s);
```
Wenn `s == null` wird bei (1) eine `java.lang.NumberFormatException` geworfen, bei (2) wird 0 zurückgegeben (default value). Was ist besser?

- Unterscheidung zwischen 0 und `null` (ungültiger Wert) nicht mehr möglich

- Kann Berechnungen verfälschen → Schwer zu findende Fehler
 
- Daher: Offensichtlicher Programmfehler → Muss zur Entwicklungszeit und nicht zur Laufzeit behoben werden

- Empfehlung: nicht auf `null` prüfen → Programm sterben lassen

- Bei Prüfung auf `null` in jedem Fall dokumentieren

####Javadoc

- Hat mir sehr gefallen!

- Dokumentieren WAS gemacht wird und nicht WIE!

- Geometrie-Aufgabe (Punkt an Gerade spiegeln): Ausnahmesituation, da die Vorgehensweise des Algo nicht sofort ersichtlich ist → Beschreibung der Idee des Algorithmus wäre erlaubt (meist jedoch nur als interner Kommentar)

##Vererbung
[Link zum Tutorial](../java-tutorial/vererbung.md)

##Das Wort zum Schluss

- Parameter überprüfen! ⇒ Ansonsten 2 Punkte Abzug

- Gebt euch nach wie vor mit Javadoc Mühe!

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://24.media.tumblr.com/tumblr_lv8i3tDJlk1qig5tho1_1280.jpg)
![Comic am Ende 2](http://wumocomicstrip.com/img/strip/-WM_strip_DK_20121207.jpg)