#Tutorium 5 (23.11.12)

##Einführung in Eclipse
Eine IDE verfügt über eine Menge Features, die das Leben eines Entwicklers stark vereinfachen können.

- Auto-Building

- Auto-Completion

- Code-Generation

- Debugger

- Erweiterungen über Plugins nachinstallieren
  - `Help → Install New Software → Add`
 
###Installation

1. [Herunterladen](http://www.eclipse.org/downloads/)

2. Entpacken und ausführen

3. Workspace anlegen

4. Projekt anlegen

5. Coden ;)

##Checkstyle

- Das [Eclipse Plugin](http://eclipse-cs.sf.net/update/)

- Die [JavaDoc](http://baldur.iti.uka.de/programmieren/javadoc-checks.xml) und [Whitespace](http://baldur.iti.uka.de/programmieren/whitespace-checks.xml) Regelsätze für Checkstyle
 - Regelsätze über `Window → Preferences → Checkstyle → New → Remote Configuration+Name+Location → Ok` einbinden
 - Über `Window → Preferences → Checkstyle → Set as Default` kann ein Regelsatz ausgewählt werden, der standardmäßig angewandt wird.
 - Über `Rechtsklick auf ein Projekt → Properties → Checkstyle → Main` kann die Option `Use simple configuration` deaktiviert werden. Nun ist es möglich mehrere Checkstyle Regelsätze gleichzeitig anzuwenden. Einfach im Main-Tab über `Add → File Set Name+Check Configuration` ein `File Set` anlegen, das den entsprechenden Regelsatz akzeptiert (für jeden Regelsatz muss ein neues `File Set` angelegt werden).
 - Zu guter Letzt können die Checkstyle Regelsätze nun über `Rechtklick auf Klasse/Projekt → Checkstyle → Check Code with Checkstyle` angewandt werden oder gleich für jeden Speichervorgang über `Rechtsklick auf ein Projekt → Properties → Checkstyle → Main → Checkstyle active for this project` aktiviert werden.

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

In Eclipse: Rechtsklick auf `Projekt → Build Path → Add Libraries → JUnit → Finish (JUnit 4 auswählen)`

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
