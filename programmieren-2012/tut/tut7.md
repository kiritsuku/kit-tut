#Tutorium 7 (07.12.12)

##Einfach verkettete Listen (Fortsetzung)

[Beispiel-Implementierung](../java-tutorial/list-simple-sequel.md)

[Test-Code](../java-tutorial/list-simple-test-sequel.md)

##Iterationen durch Collections

Wozu überhaupt?

- Zugriff auf Elemente über den Index kann langsam sein

- Nicht alle Collections sind indexbasiert

###Externe Iteration

Externe Iteration wird in der Regel durch Iteratoren ermöglicht.

Iteratoren ermöglichen es durch die Elemente einer Collection zu laufen, ohne dass man wissen muss wie die Elemente in dieser Collection angeordnet sind.

```java
/**
 * An iterator to iterate over sequences of integers.
 */
class IntIterator {

    /**
     * Checks if the iterator is not yet empty.
     * 
     * @return {@code true} if the iterator has another element, {@code false}
     *         otherwise.
     */
    public boolean hasNext() {
        throw new UnsupportedOperationException(
                "IntIterator#hasNext doesn't have an implementation yet");
    }

    /**
     * Gets the next element of the iterator.
     * 
     * @return the next element
     */
    public int next() {
        throw new UnsupportedOperationException(
                "IntIterator#next doesn't have an implementation yet");
    }
}
```

Implementierung in `IntList`:

```java
public IntIterator iterator() {
    return new IntIterator() {

        Entry cur = head;

        public boolean hasNext() {
            return cur != null;
        }

        public int next() {
            if (!hasNext()) {
                Error.log("next on empty iterator called");
                return Integer.MIN_VALUE;
            }
            Entry next = cur;
            cur = cur.next;
            return next.element;
        }
    };
}
```

Benutzung des Iterators:

```java
IntIterator iter = IntList.of(1, 2, 3, 4, 5).iterator();

while (iter.hasNext()) {
    int elem = iter.next();
    list.add(elem);
}

// another way to loop over the elements of an iterator
for (IntIterator it = IntList.of(1, 2, 3, 4, 5).iterator(); it.hasNext();) {
    int elem = it.next();
    list.add(elem);
}
```

###Interne Iteration

Wird in Java durch das sogenannte [Command-Pattern](http://en.wikipedia.org/wiki/Command_pattern) umgesetzt:

```java
class IntCommand {
    void execute(int element) {}
}
```

Implementierung einer Methode, die `IntCommand` benutzt:

```java
/**
 * Applies a command to all elements of this list.
 * 
 * @param command
 *        the command to apply
 */
public void foreach(IntCommand command) {
    Entry cur = head;
    while (head != null) {
        command.execute(cur.element);
        cur = cur.next;
    }
}
```

Anwendung:

```java
IntList list = IntList.of(1, 2, 3, 4, 5);
list.foreach(new IntCommand() {
    void execute(int element) {
        System.out.println(element);
    }
});
```

##Übungsaufgabe

[Listenimplementierung](../java-tutorial/list-simple-full.md)

[Übungsaufgabe](../java-tutorial/list-simple-uebung.md)

[Unit-Tests](../java-tutorial/list-simple-uebung-test.md)

Alle Einzelaufgaben als [Eclipse Projekt](https://github.com/downloads/sschaef/kit-tut/tutub.zip) herunterladen

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://imgs.xkcd.com/comics/standards.png)