#Tutorium 3 (09.11.12)

##Häufige Fehler auf Übungsblatt 1 / Neue Regeln

#### Fehler: Variablennamen/Parameter der Konstruktoren zu kurz/bedeutungslos
- Was könnte `boolean light;` wohl alles bedeuten? Besser ist `boolean hasLight;`

#### Fehler: Preis in Euro statt in Cent an die Konstruktoren übergeben
- Umrechnung in Cent

#### Fehler: Fehlende Methoden, falsche Methodensignaturen oder Methoden in falscher Klasse
- Rückgabewert spielt in Java keine Rolle bei der Methodensignatur

#### Fehler: Unschöne Formatierung von Kommentaren
- Sie kommen über die Zeile, die dokumentiert werden soll

- Code sollte im besten Fall keine Kommentare (außer Dokumentationskommentare) beinhalten

#### Ab sofort: Jede Klasse bekommt ihre eigene Datei
- Mehrere Klassen pro Datei nicht mehr erlaubt (Ausnahme: innere Klassen)

#### Checkstyle wird euch in Zukunft foltern!
- Wird automatisch vom Praktomat überprüft

- Lösung oft einreichen, wenn ihr euch mit der Formatierung nicht sicher seit

- Kann auch lokal auf eurem Rechner überprüft werden → [Zukünftige Regelsätze](http://baldur.iti.uka.de/programmieren/)

#### [Java Code Conventions](http://www.oracle.com/technetwork/java/codeconv-138413.html) beachten
- Wird auf den folgenden Übungsblättern auch von Checkstyle erwartet

####Bedingungen vereinfachen!
Unschön:
```java
boolean isAvailable() {
  boolean ret = false;
  if (isAvailable == true && isBusy == false) {
    ret = true;
  }
  return ret;
}
```
Besser:
```java
boolean isAvailable() {
  return isAvailable && !isBusy;
}
```

####Erzeugung von Strings
```java
// schlecht
String str = new String("hello");
// besser
String str = "hello";
```
Die JVM verfügt über einen String-Cache. String-Literale kommen auf diesen Cache, mit `new` erzeugte Strings aber nicht.

####Mehrfachdeklarationen
```java
// schlecht
int a, b, c;
// besser (+ mit besseren Variablennamen)
int a;
int b;
int c;
```

####Logische Verknüpfungen benutzen
- `&&` und `||` statt `&` und `|` → Letztere nennen sich binäre Verknüpfungen

##Javadoc

Normale Kommentare:

```java

// 
// Dear maintainer:
// 
// Once you are done trying to 'optimize' this routine,
// and have realized what a terrible mistake that was,
// please increment the following counter as a warning
// to the next guy:
// 
// total_hours_wasted_here = 42
// 
void method() {
  //When I wrote this, only God and I understood what I was doing
  //Now, God only knows
  System.out.println("This method does something cool");
}

/*
 * Always returns true.
 */
public boolean isAvailable() {
    return false;
}
```
[Source of above comments](http://stackoverflow.com/questions/184618/what-is-the-best-comment-in-source-code-you-have-ever-encountered)

Javadoc:
```java



/**
 * Returns the days of a given month and a given year. The month can be given
 * as its name regardless if it contains upper or lower case. Thus, the names
 * "january", "January" or "jaNuARy" are equivalent.
 * 
 * @param month
 *        the name of a month
 * @param year
 *        the year
 * @return the days of a month or {@code 0} if the name does not exist or the
 *         year is negative.
 */
int getDaysOfMonth(String month, int year) {
  // needs to be implemented
  return 0;
}

/**
 * Drunk, fix later.
 * 
 * @author My humble self
 * @version 0.1
 * @since JDK1.6, Nov 5, 2012
 */
class Class {}
```

##Kontrollstrukturen
[Link zum Tutorial](../java-tutorial/kontrollstrukturen.md)

##Übungsaufgabe zu Kontrollstrukturen
[Link zur Übung](../java-tutorial/kontrollstrukturen-uebung.md)

##Das Wort zum Schluss
- Checkstyle beachten!
- Abgabetermin des 2. Übungsblattes: 19.11.12 13:00

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://geekandpoke.typepad.com/.a/6a00d8341d3df553ef017617629460970c-pi)