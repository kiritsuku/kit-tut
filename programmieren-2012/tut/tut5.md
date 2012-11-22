#Tutorium 5 (23.11.12)

##Einführung in Eclipse
Eine IDE verfügt über eine Menge Features, die das Leben eines Entwicklers stark vereinfachen können.

- Auto-Building

- Auto-Completion

- Code-Generation

- Debugger

- Erweiterungen über Plugins nachinstallieren
  - Help → Install New Software → Add
 
###Installation

1. [Herunterladen](http://www.eclipse.org/downloads/)

2. Entpacken und ausführen

3. Workspace anlegen

4. Projekt anlegen

5. Coden ;)

##Checkstyle

- [Eclipse Plugin](http://eclipse-cs.sf.net/update/)

- [JavaDoc Regelsatz](http://baldur.iti.uka.de/programmieren/javadoc-checks.xml)

- [Whitespace Regelsatz](http://baldur.iti.uka.de/programmieren/whitespace-checks.xml)

##Levenshtein-Distanz

Beschreibt die minimale Editierdistanz zwischen zwei Strings, also die minimale Anzahl von Einfüge-, Lösch- und Ersetz-Operationen um die erste Zeichenkette in die zweite umzuwandelen.

Beispiel: Tier → Tor ⇒ 2 Operationen (löschen von i, e mit o ersetzen)

Der Algorithmus:

![lev](http://cloud.github.com/downloads/sschaef/kit-tut/lev_1.png)

Beispiel anhand von Tier -> Tor:

```
  ε T o r
ε 0 1 2 3
T 1 0 1 2
i 2 1 1 2
e 3 2 2 2
r 4 3 3 2
```

Der Abstand der beiden Strings ist 2.

##JUnit

Ermöglicht die automatische Ausführung von Tests

In Eclipse: Rechtsklick auf Projekt → Build Path → Add Libraries → JUnit → Finish (JUnit 4 auswählen)

Beispiel:

```java
import org.junit.Test;
import static org.junit.Assert.*;

public class AnyTestClassName {
  @Test
  public void testEquals() {
    assertEquals(100, 101);
  }
}
```

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://www.sandraandwoo.com/woode/comics/2012-11-19-0430-softwareentwicklung-jetzt-mit-katzen.png)
