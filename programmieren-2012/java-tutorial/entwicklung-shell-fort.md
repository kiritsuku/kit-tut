#Entwicklung einer Shell (Fortsetzung)

Die bisherige Version der Shell kann noch nicht sonderlich viel. Versuchen wir dies zu ändern. Zuerst einmal der Ausgangscode:

```java
public class Shell {

  private boolean isRunning;

  public static void main(final String... args) {
    new Shell().run();
  }

  void run() {
    System.out.println("Welcome to Number Collector!");

    isRunning = true;

    while (isRunning) {
      final String input = MyTerminal.askString("> ");
      if (":q".equals(input)) {
        shutdown();
      }
    }
  }

  private void shutdown() {
    isRunning = false;
  }
}
```

In der [TyTerminal.java](MyTerminal.java) Klasse hat sich seit dem ersten Teil dieses Tutorials nichts geändert.

Wir wollen den Code nun so erweitern, dass man Kommandos eingeben kann, die Parameter besitzen dürfen:

```java
// in class Shell
void run() {
  System.out.println("Welcome to Number Collector!");

  isRunning = true;

  while (isRunning) {
    final String input = MyTerminal.askString("> ");
    final List<String> args = Arrays.asList(input.split("\\s+"));
    final String command = args.get(0);

    try {
      final long start = System.nanoTime();
      handleCommand(command, args.subList(1, args.size()));
      System.out.println("time: " + (System.nanoTime()-start)/1e6 + "ms");
    } catch (final IllegalArgumentException e) {
      System.out.println("Error!" + e.getMessage());
    }
  }
}

private void handleCommand(final String command, final List<String> args) {
  System.out.println(command + args);
  if (":q".equals(command)) {
    shutdown();
  }
}
```

Wir haben hier mehrere Dinge geändert:

- Die Argumente werden mit einem Regex `\s+`, das einzelne Wörter erkennt, auseinandergenommen

- Es wird eine Methode aufgerufen (`handleCommand`), die die Kommandos verarbeitet. Dabei wird das erste Element von der Liste abgetrennt, da es das Argument selbst und nicht desssen Parameter beinhaltet

- Um die Shell nicht unfreiwillig zu beenden, ist die Verarbeitung des Kommandos mit einem try-catch-Block abgesichert.

- Dank dem Regex + `Arrays.asList` besitzt die Argument-Liste immer min. ein Element, dieser Code muss also nicht weiter abgesichert werden

Fügen wir das erste Kommando hinzu:

```java
public class Shell {

  private boolean isRunning;

  private final Database database = new Database();

  public static void main(final String... args) {
    new Shell().run();
  }

  void run() {
    System.out.println("Welcome to Number Collector!");

    isRunning = true;

    while (isRunning) {
      final String input = MyTerminal.askString("> ");
      final List<String> args = Arrays.asList(input.split("\\s+"));
      final String command = args.get(0);

      try {
        final long start = System.nanoTime();
        handleCommand(command, args.subList(1, args.size()));
        System.out.println("time: " + (System.nanoTime()-start)/1e6 + "ms");
      } catch (final IllegalArgumentException e) {
        System.out.println("Error! " + e.getMessage());
      }
    }
  }

  private void handleCommand(final String command, final List<String> args) {
    System.out.println(command + args);
    if (":q".equals(command)) {
      shutdown();
    } else if ("create".equals(command)) {
      createUser(args);
    } else {
      System.out.println("command not found: " + command);
    }
  }

  private void createUser(final List<String> args) {
    if (args.size() != 1) {
      throw new IllegalArgumentException("Invalid number of arguments! Expected format: create <name>");
    }
    database.createUser(args.get(0));
  }

  private void shutdown() {
    isRunning = false;
  }
}

public class Database {

  private final List<String> userNames = new ArrayList<String>();

  public void createUser(final String name) {
    if (userNames.contains(name)) {
      throw new IllegalArgumentException(String.format("username '%s' already exists!", name));
    }
    userNames.add(name);
  }
}
```

Es ist nun möglich `create <user>` einzugeben, aber nur einmal! Bei der erneuten Eingabe des gleichen Benutzernamens fliegt eine Exception.

Mit `String.format` kann man einen String über spezielle Marker formatieren. Für mehr Informationen lohnt ein Blick in die Dokumentation der von [java.util.Formatter](http://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html).

Die Logik zur Speicherung der Daten wurde in eine eigene Klasse ausgelagert um die Übersichtlichkeit zu erhöhen. Dies erlaubt uns weiterhin auch die Datenbank-Klasse später mit einer richtigen Datenbank auszutauches, also einer, die ihre Daten auf die Festplatte schreibt.

Wir wollen nun einem Benutzer Zahlen zuordnen:

```java
// in class Shell
private void handleCommand(final String command, final List<String> args) {
  System.out.println(command + args);
  if (":q".equals(command)) {
    shutdown();
  } else if ("create".equals(command)) {
    createUser(args);
  } else if ("add".equals(command)) {
    addNumbers(args);
  } else {
    System.out.println("command not found: " + command);
  }
}

private void addNumbers(final List<String> args) {
  if (args.size() < 2) {
    throw new IllegalArgumentException("Invalid number of arguments! Expected format: add <name> <number1> <number2> <numberN>");
  }
  database.addNumbers(args.get(0), args.subList(1, args.size()));
}
```

Weiterhin benötigen wir ein paar Änderungen in `Database`:

```java
public class Database {

  private final List<String> userNames = new ArrayList<String>();

  private final Map<String, List<Integer>> datasets = new HashMap<String, List<Integer>>();

  public void createUser(final String name) {
    if (userNames.contains(name)) {
      throw new IllegalArgumentException(String.format("username '%s' already exists!", name));
    }
    userNames.add(name);
    datasets.put(name, new ArrayList<Integer>());
  }

  public void addNumbers(final String name, final List<String> numbers) {
    if (!userNames.contains(name)) {
      throw new IllegalArgumentException(String.format("username '%s' doesn't exist!", name));
    }

    for (final String rawNumber : numbers) {
      final int number = Integer.parseInt(rawNumber);
      datasets.get(name).add(number);
    }
  }
}
```

Wir benutzen eine Map um die Daten einem Benutzer zuzuordnen.

```java
// in class Shell
private void handleCommand(final String command, final List<String> args) {
  System.out.println(command + args);
  if (":q".equals(command)) {
    shutdown();
  } else if ("create".equals(command)) {
    createUser(args);
  } else if ("add".equals(command)) {
    addNumbers(args);
  } else if ("contains".equals(command)) {
    containsNumber(args);
  } else {
    System.out.println("command not found: " + command);
  }
}

private void containsNumber(final List<String> args) {
  if (args.size() < 2) {
    throw new IllegalArgumentException("Invalid number of arguments! Expected format: contains <name> <number>");
  }
  final boolean contains = database.containsNumber(args.get(0), args.get(1));
  System.out.println(contains);
}

// in class Database
public boolean containsNumber(final String name, final String number) {
  final List<Integer> numbers = datasets.get(name);
  if (numbers == null) {
    throw new IllegalArgumentException(String.format("username '%s' doesn't exist!", name));
  }
  return numbers.contains(Integer.valueOf(number));
}
```

Wir wollen den Code nun noch so abändern, dass en möglich ist Zahlenbereiche einzugeben:

```java
public void addNumbers(final String name, final List<String> numbers) {
  if (!userNames.contains(name)) {
    throw new IllegalArgumentException(String.format("username '%s' doesn't exist!", name));
  }

  for (final String rawNumber : numbers) {
    final List<Integer> data = datasets.get(name);
    if (rawNumber.contains("-")) {
      final String[] range = rawNumber.split("-");
      if (range.length != 2) {
        throw new IllegalArgumentException("illegal format for range. Expected: <start>-<end>");
      }
      final int start = Integer.valueOf(range[0]);
      final int end = Integer.valueOf(range[1]);
      for (int i = start; i <= end; ++i) {
        data.add(i);
      }
    } else {
      final int number = Integer.parseInt(rawNumber);
      data.add(number);
    }
  }
}
```

Bei einer Eingabe von `add simon 1-10` wirdem dem Benutzer die Zahlen 1-10 zugeordnet.

Beispielablauf:

```
Welcome to Number Collector!
> create simon
create[simon]
time: 0.755593ms
> add simon 1-10000000
add[simon, 1-10000000]
time: 1682.96276ms
> contains simon 1000000
contains[simon, 1000000]
true
time: 21.868627ms
> contains simon 1
contains[simon, 1]
true
time: 0.139274ms
> :q
:q[]
time: 0.09553ms
```

Was man hier sieht, ist, dass es sehr lange dauert alle Zahlen anzulegen (1,6s) und auch vergleichsweise lange um eine hintere Zahl zu suchen (21ms im Gegensatz zu 0.1ms zu einer vorderen Zahl). Wir können die Suchgeschwindigkeit ändern indem wir die Zahlen nicht in einer `List` ablegen, sondern in einem `Set`:

```java
public class Database {

  private final List<String> userNames = new ArrayList<String>();

  private final Map<String, Set<Integer>> datasets = new HashMap<String, Set<Integer>>();

  public void createUser(final String name) {
    if (userNames.contains(name)) {
      throw new IllegalArgumentException(String.format("username '%s' already exists!", name));
    }
    userNames.add(name);
    datasets.put(name, new HashSet<Integer>());
  }

  public void addNumbers(final String name, final List<String> numbers) {
    if (!userNames.contains(name)) {
      throw new IllegalArgumentException(String.format("username '%s' doesn't exist!", name));
    }

    for (final String rawNumber : numbers) {
      final Set<Integer> data = datasets.get(name);
      if (rawNumber.contains("-")) {
        final String[] range = rawNumber.split("-");
        if (range.length != 2) {
          throw new IllegalArgumentException("illegal format for range. Expected: <start>-<end>");
        }
        final int start = Integer.valueOf(range[0]);
        final int end = Integer.valueOf(range[1]);
        for (int i = start; i <= end; ++i) {
          data.add(i);
        }
      } else {
        final int number = Integer.parseInt(rawNumber);
        data.add(number);
      }
    }
  }

  public boolean containsNumber(final String name, final String number) {
    final Set<Integer> numbers = datasets.get(name);
    if (numbers == null) {
      throw new IllegalArgumentException(String.format("username '%s' doesn't exist!", name));
    }
    return numbers.contains(Integer.valueOf(number));
  }
}
```

Wie zu sehen müssen wir nur die Klasse `Database` ändern, die Klasse `Shell` bleibt komplett unberührt.

Resultat:

```
// as before
> contains simon 1
contains[simon, 1]
true
time: 0.167838ms
> contains simon 10000000
contains[simon, 10000000]
true
time: 0.123017ms
```

Positiver Nebeneffekt: Das `Set` speichert nun keine Duplikate mehr, was Speicherplatz schonen kann.

###Aufgaben

[Das Projekt mit den Sourcen](tutconsole.zip)

1. Füge einen Befehl `count <name>` hinzu, der die Anzahl der Zahlen eines Benutzers auf der Konsole ausgibt
2. Füge einen Befehl `list` hinzu, der die Namen aller Benutzer anzeigt
3. Füge einen Befehl `remove <name> <number_or_range1> <number_or_rangeN>` hinzu, mit dem man Zahlen eines Benutzers löschen kann
4. Füge enien Befehl `removeUser <name>` hinzu, der den Benutzer mit all seinen Zahlen löscht. Implementiere hierzu eine Sicherheitsabfrage (z.B. `are you sure y/n?`), die aber erst aktiviert wird wenn `removeUser <name>` eingegeben wurde
5. Selber eine Idee? Implementiere sie!