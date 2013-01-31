#Tutorium 13 (01.02.13)

##Häufige Fehler auf dem Übungsblatt

###Konstanten

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

###enum as state

- boolean-Variablen können nur zwei Zustände besitzen

- Ein enum kann beliebig viele Zustände besitzen

```java
enum GameMode {
  NEW_MODE,
  ACTIVE_MODE,
  GAME_OVER_MODE
}
```

###Hiflsmethoden schreiben wenn nötig

```java
public class InputValidation {

	/**
	 * Tests an expression, throwing an IllegalArgumentException if false.
	 * 
	 * @param requirement
	 *        the expression to test
	 * @param message
	 *        a string to include in the failure message
	 */
	public static void require(final boolean requirement, final String message) {
		if (!requirement) {
			throw new IllegalArgumentException(message);
		}
	}
}

// usage

InputValidation.require(game.getMode().equals(GameMode.NEW_MODE)
    || game.getMode().equals(GameMode.ACTIVE_MODE),
    "game not started");
```

###API-Tipp

```java
String string = "198357"
int number = java.lang.Character.getNumericValue(string.charAt(0))
```

##VCS am Beispiel Git

Warum Git?

- Standard in der OpenSource-Welt

- Beste Free-Hosting Angebote verfügbar: [GitHub](https://github.com/), [Bitbucket](https://bitbucket.org/)

- [Git Einführung](http://git-scm.com/)

##Buchempfehlungen

- [Effective Java](http://www.amazon.de/Effective-Java-Programming-Language-Series/dp/0321356683)

- [Clean Code: A Handbook of Agile Software Craftsmanship](http://www.amazon.de/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)

- [StackOverflow - What is the single most influential book every programmer should read?](http://stackoverflow.com/questions/1711/what-is-the-single-most-influential-book-every-programmer-should-read)

##Sonstiges

Abschlussaufgaben:

```
Anmeldebeginn: 28.1.
Anmeldeschluss 28.2.
Abmeldeschluss: 28.2.
Ausgabetermin für Teil 1: 28.1.
Ausgabetermin für Teil 2: 11.2.
Abgabefrist für Teil 1: 11.3.
Abgabefrist für Teil 2: 25.3.
```

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://geekandpoke.typepad.com/.a/6a00d8341d3df553ef0120a5bf8453970b-pi)