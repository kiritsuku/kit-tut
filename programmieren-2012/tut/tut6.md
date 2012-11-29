#Tutorium 6 (30.11.12)

##Häufige Fehler auf Übungsblatt 2

Aufgabenstellung lesen und das machen was verlangt ist. Meistens wurden zwar nur Kleinigkeiten nicht eingehalten aber je nach Anwendungsfall können diese Kleinigkeiten verdammt wichtig sein.

####Formatierungsunschönheiten

Der Praktomat testet nicht ob Leerzeichen mit Tabs vermischt werden → Gibt in Zukunft Punkteabzug

Geschachtelte ifs:
```java
if (cond1) {
  if (cond2) {
    if (cond3) {
      ...
      return ...
    }
    ...
    return ...
  }
  ...
  return ...
}
...

// =>

if (cond1) {
} else if (cond2) {
} else if (cond3) {
} else {
  ...
}
```

Bei Kommentierung auf Deutsch: Umlaute ab sofort verboten → am besten gleich noch UTF-8 als Zeichensatz verwenden

Vereinfachungen:
```javaa
boolean x;
if (cond) x = true;
else x = false;

// <=>

boolean x = cond;
```

```java
while (var) {
  if (cond) {
    var = false;
  }
  cond = ...;
}

// <=>

while (cond) {
  cond = ...;
}
```
Die einzelnen Felder eines Arrays müssen nicht mit Standardwerten initialisiert werden → übernimmt die JVM.

Verwendet die Java API wenn es euch das Übungsblatt gestattet. Momentan gilt: Alles was nicht importiert werden muss darf verwendet werden.
```java
// instead of copying array by hand
System.arraycopy(src, srcPos, dest, destPos, length);
```

####Kommentierung

Typische Fragen, die in Kommentaren zu klären sind:

- Wozu ist der Code da bzw. was macht er?

- Was sind gültige Werte für die Parameter der Funktion?

- Kann die Funktion fehlschlagen?

- Wie benutzt man den Code?

Es soll __nicht__ kommentiert werden wie der Code funktioniert.

Faustregeln:
- Code, der Kommentare benötigt, muss refaktorisiert werden (meistens).

- Erlaubt sind nur Kommentare, die Ausnahmesituationen beschreiben (z.B. Bug in benutzter Library) und die somit auch für andere Entwickler relevant sind.

####Testen

- Es sah nicht so aus wie wenn das viele gemacht hätten...

- Es reicht nicht nur zu schauen ob euer Programm vom Praktomaten auch angenommen wird

- Der Sinn von Tests ist nicht zu prüfen ob der Code korrekt funktioniert, sondern zu klären ob man es schafft seinen Code in die Knie zu zwingen

- Offensichtliche Fehler wie Endlosschleifen, AIOOBE oder NPE, die dazu führen, dass ein Programm gar nicht normal beendet werden kann kosten in Zukunft min. die Hälfte der Punkte einer Teilaufgabe

- Randfälle eines Algo überprüfen

- Große Eingabemengen testen (wenn möglich)

- Parameter einer (öffentlichen) Funktion auf Gültigkeit überprüfen

- Debugausgaben möglichst nur für wichtige Einzelschritte
  - Nicht innerhalb einer geschachtelten Schleife

##Pakete

- Pakete strukturieren ein Program

- Gängige Namenskonvention entspricht umgedrehten Domainnamen, z.B. `edu.kit.info`

- Werden durch Verzeichnisse auf Festplatte repräsentiert

##Sichtbarkeiten

- Es gibt 4 verschiedene Sichtbarkeitsmodifizierer: `public`, `private`, `package-protected` und `protected`

- [Sichtbarkeitstabelle](http://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)

##Einfach verkettete Liste

[Beispiel-Implementierung](../java-tutorial/list-simple.md)

[Test-Code](../java-tutorial/list-simple-test.md)

##Das Wort zum Schluss

- Eclipse kann über `File → Export → General → Archive File → Select 'Create only selected directories' and all files to export → Finish` ein Archiv anlegen, das im Praktomat hochgeladen werden kann ⇒ __Achtung! Nur Source-Code auswählen, keine Binaries__

- Testet euren Code!

- Einhaltung von Javadoc!

- Sichtbarkeiten bei den Übungsaufgaben ergänzen

- Falls ihr Nachhilfe benötigt, dann meldet euch bei mir oder dem Übungsleiter (Florian Merz) → Euch wird dann ein Nachhilfelehrer vermittelt.

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://geekandpoke.typepad.com/.a/6a00d8341d3df553ef017d3e42b513970c-pi)