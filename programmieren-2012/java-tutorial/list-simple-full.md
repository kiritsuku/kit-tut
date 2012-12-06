```java
package edu.kit.list;


/**
 * A single linked list containing integers.
 */
public class IntList {

    private Entry head;

    /**
     * Creates a {@link IntList} containing the elements passed to this method.
     * 
     * @param elements
     *        the elements to add to the list
     * @return the list containing all elements
     */
    public static IntList of(int... elements) {
        IntList list = new IntList();
        for (int i = 0; i < elements.length; ++i) {
            list.add(elements[i]);
        }
        return list;
    }
    
    /**
     * Applies a command to all elements of this list.
     * 
     * @param command
     *        the command to apply
     */
    public void foreach(IntCommand command) {
        Entry cur = head;
        while (cur != null) {
            command.execute(cur.element);
            cur = cur.next;
        }
    }

    /**
     * Checks if the list is empty.
     * 
     * @return {@code true} if the list is empty, {@code false} otherwise
     */
    public boolean isEmpty() {
        return head == null;
    }

    /**
     * Returns the size of the list.
     * 
     * @return the size of the list
     */
    public int size() {
        Entry cur = head;
        int size = 0;

        while (cur != null) {
            size++;
            cur = cur.next;
        }

        return size;
    }

    /**
     * Adds an element to the list.
     * 
     * @param element
     *        the element to add
     */
    public void add(int element) {
        if (isEmpty()) {
            head = new Entry(element, null);
        } else {
            Entry cur = head;

            while (cur.next != null) {
                cur = cur.next;
            }

            cur.next = new Entry(element, null);
        }
    }

    /**
     * Returns the element at a given index.
     * 
     * @param index
     *        the index of the element
     * @return the element if the index is valid, {@code Integer#MIN_VALUE}
     *         otherwise.
     */
    public int get(int index) {
        if (isEmpty()) {
            Error.log("get on empty list");
            return Integer.MIN_VALUE;
        }
        if (index < 0 || index >= size()) {
            Error.log("invalid index");
            return Integer.MIN_VALUE;
        }
        Entry cur = head;

        while (cur.next != null && index > 0) {
            cur = cur.next;
            index -= 1;
        }

        return cur.element;
    }

    /**
     * Checks if a element exists in the list.
     * 
     * @param element
     *        the element to check
     * @return {@code true} if the element exists, {@code false} otherwise.
     */
    public boolean contains(int element) {
        Entry cur = head;

        while (cur != null) {
            if (cur.element == element) {
                return true;
            }
            cur = cur.next;
        }

        return false;
    }

    /**
     * Appends all elements of this list to a string using start, end and
     * separator strings. The written text begins with the string `start` and
     * ends with the string `end`.
     * <p>
     * Example:
     * 
     * <pre>
     * String s = List.of(1, 2, 3).mkString(&quot;[&quot;, &quot;, &quot;, &quot;]&quot;);
     * s.equals(&quot;[1, 2, 3]&quot;);
     * </pre>
     * 
     * @param start
     *        the starting string
     * @param sep
     *        the separator string
     * @param end
     *        the ending string
     * @return the string to which all elements were appended.
     */
    public String mkString(String start, String sep, String end) {
        if (isEmpty()) {
            return start + end;
        }
        StringBuilder sb = new StringBuilder(start);
        Entry cur = head;

        while (cur.next != null) {
            sb.append(cur.element);
            sb.append(sep);
            cur = cur.next;
        }

        sb.append(cur.element);
        sb.append(end);
        return sb.toString();
    }

    @Override
    public String toString() {
        return mkString("IntList(", ", ", ")");
    }

    /**
     * Creates an iterator to iterate over the elements of this list.
     * 
     * @return the iterator
     */
    public IntIterator iterator() {
        return new IntIterator() {

            Entry cur = head;

            @Override
            public boolean hasNext() {
                return cur != null;
            }

            @Override
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
    
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj instanceof IntList) {
            Entry thatCur = ((IntList) obj).head;
            Entry thisCur = head;
            
            while (thatCur != null && thisCur != null) {
                if (thatCur.element != thisCur.element) {
                    return false;
                }
                thatCur = thatCur.next;
                thisCur = thisCur.next;
            }
            if (thatCur != null || thisCur != null) {
                return false;
            }
            return true;
        }
        return false;
    }

    /**
     * Internal representation of the list entries.
     */
    private class Entry {

        int element;
        Entry next;

        Entry(int i, Entry next) {
            this.element = i;
            this.next = next;
        }
    }
}

class IntCommand {
    void execute(int element) {}
}

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
                "Iterator#hasNext doesn't have an implementation yet");
    }

    /**
     * Gets the next element of the iterator.
     * 
     * @return the next element
     */
    public int next() {
        throw new UnsupportedOperationException(
                "Iterator#next doesn't have an implementation yet");
    }
}

class Error {

    static void log(String message) {
        throw new UnsupportedOperationException(message);
    }
}
```