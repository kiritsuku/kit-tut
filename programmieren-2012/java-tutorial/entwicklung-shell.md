#Entwicklung einer Shell

In einer Shell können Eingaben getätigt werden, die dann vom Programm verarbeiten werden können. Ein erstes Beispiel:

```java
public class Shell {

  public static void main(final String... args) {
    new Shell().run();
  }

  void run() {
    System.out.println("Welcome to Number Doubler!");

    boolean isRunning = true;
    while (isRunning) {
      final String input = MyTerminal.askString("> ");
      final int num = Integer.parseInt(input);
      System.out.println(num * 2);
    }
  }
}
```
Der Code benötigt [TyTerminal.java](MyTerminal.java) zum Compilieren. Eine Variable wie `isRunning`, die prüft ob die Shell noch weiter ausgeführt wird, ist einem `while(true)` + `break` zu bevorzugen, da man sie auch in einer aufgerufenen Methode auf `false` setzen kann (sofern sie als Instanzvariable sichtbar ist).

Der Code hat noch eine wichtige Schwachstelle: Wird keine valide Zahl eingegeben, fliegt eine Exception und das Program wird beendet. Dies kann behoben werden indem Exceptions gefangen werden:

```java
// in method run
while (isRunning) {
  final String input = MyTerminal.askString("> ");
  try {
    final int num = Integer.parseInt(input);
    System.out.println(num * 2);
  } catch (final NumberFormatException e) {
    System.out.println("Invalid input! Please insert a number");
  }
}
```

Es sollten jedoch nicht alle möglichen Exceptions gefangen werden, sondern nur die, auf die man tatsächlich auch reagieren kann.

Fangen von `Exception` oder gar `Throwable` tunlichst vermeiden. Dies ist auch als "Pokémon Exception Handling" bekannt:

![Pokémon Exception Handling](http://www.codinghorror.com/.a/6a0120a85dcdae970b0176169669bd970c-800wi)

For when you just Gotta Catch 'Em All.

```java
try {
}
catch (Exception ex) {
   // Gotcha!
}
```
[Quelle](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html)

Versucht wenn möglich nicht auf `System.err`, sondern immer auf `System.out` auszugeben ⇒ Die Ausgabe kann sonst zerstückelt werden.

Ein weiteres Problem ist, dass die Shell nicht beendet werden kann. Beheben wir dies:

```java
// in method run
while (isRunning) {
  final String input = MyTerminal.askString("> ");
  if (":q".equals(input)) {
    isRunning = false;
  } else {
    try {
      final int num = Integer.parseInt(input);
      System.out.println(num * 2);
    } catch (final NumberFormatException e) {
      System.out.println("Invalid input! Please insert a number");
    }
  }
}
```

Durch die Eingabe von `:q` kann man die Shell nun beenden.

Wenn wir uns dafür interessieren was für Zahlen wir bereits eingegeben haben, dann können wir uns diese merken:

```java
final ArrayList<Integer> list = new ArrayList<Integer>();
while (isRunning) {
  final String input = MyTerminal.askString("> ");
  if (":q".equals(input)) {
    isRunning = false;
  } else if ("in?".equals(input)) {
    for (final Integer i : list) {
      System.out.println(i);
    }
  } else {
    try {
      final int num = Integer.parseInt(input);
      System.out.println(num * 2);
      list.add(num);
    } catch (final NumberFormatException e) {
      System.out.println("Invalid input! Please insert a number");
      e.printStackTrace();
    }
  }
}
```

Nun werden alle eingegebenen Zahlen in eine `ArrayList` gespeichert, deren Inhalt über die Eingabe von `in?` ausgegeben werden kann.

Nun können wir einzelne Codeteile noch in separate Methoden auslagern:

```java
public class Shell {

  private boolean isRunning;
  private ArrayList<Integer> list;

  public static void main(final String... args) {
    new Shell().run();
  }

  void run() {
    System.out.println("Welcome to Number Doubler!");

    isRunning = true;
    list = new ArrayList<Integer>();

    while (isRunning) {
      final String input = MyTerminal.askString("> ");
      if (":q".equals(input)) {
        shutdown();
      } else if ("in?".equals(input)) {
        printInput();
      } else {
        doubleNumber(input);
      }
    }
  }

  private void doubleNumber(final String input) {
    try {
      final int num = Integer.parseInt(input);
      System.out.println(num * 2);
      list.add(num);
    } catch (final NumberFormatException e) {
      System.out.println("Invalid input! Please insert a number");
      e.printStackTrace();
    }
  }

  private void printInput() {
    for (final Integer i : list) {
      System.out.println(i);
    }
  }

  private void shutdown() {
    isRunning = false;
  }
}
```