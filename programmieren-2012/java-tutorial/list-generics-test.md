```java
package edu.kit.list;

import java.util.Iterator;

import org.junit.Test;

import static org.junit.Assert.*;


public class ListTest {

    List<Integer> list = new List<Integer>();

    @Test
    public void isEmpty() {
        assertTrue(list.isEmpty());
    }

    @Test
    public void size() {
        assertEquals(0, list.size());
    }

    @Test
    public void add() {
        for (int i = 1; i <= 10; ++i) {
            list.add(i);
        }
        assertEquals(10, list.size());
    }

    @Test(expected = UnsupportedOperationException.class)
    public void getOnEmptyList() {
        list.get(0);
    }

    @Test(expected = UnsupportedOperationException.class)
    public void getOnNegativeValue() {
        list.get(-1);
    }

    @Test(expected = UnsupportedOperationException.class)
    public void getOnValueOutOfRange() {
        for (int i = 1; i <= 10; ++i) {
            list.add(i);
        }
        list.get(10);
    }

    @Test
    public void get() {
        for (int i = 1; i <= 10; ++i) {
            list.add(i);
        }
        int i = list.get(0);
        assertEquals(1, i);
        i = list.get(9);
        assertEquals(10, i);
    }

    @Test
    public void mkString() {
        assertEquals("IntList()", list.toString());
        for (int i = 1; i <= 3; ++i) {
            list.add(i);
        }
        assertEquals("IntList(1, 2, 3)", list.toString());

        assertEquals("[1, 2, 3]", list.mkString("[", ", ", "]"));
        assertEquals("123", list.mkString("", "", ""));
    }

    @Test
    public void contains() {
        assertFalse(list.contains(1));
        for (int i = 1; i <= 3; ++i) {
            list.add(i);
        }
        assertTrue(list.contains(1));
        assertTrue(list.contains(3));
        assertFalse(list.contains(4));
        assertFalse(list.contains(-1));
    }

    @Test
    public void of() {
        List<Integer> list = List.of(1, 2, 3, 4, 5);
        assertEquals(5, list.size());
        assertEquals("IntList(1, 2, 3, 4, 5)", list.toString());
    }

    @Test(expected = UnsupportedOperationException.class)
    public void nextOnEmptyIterator() {
        Iterator<Integer> iter = List.<Integer>of().iterator();
        iter.next();
    }

    @Test
    public void iterator() {
        Iterator<Integer> iter = List.of(1, 2, 3, 4, 5).iterator();

        assertTrue(iter.hasNext());
        while (iter.hasNext()) {
            int elem = iter.next();
            list.add(elem);
        }
        assertFalse(iter.hasNext());
        
        /*
        // alternative version of iteration
        for (IntIterator it = IntList.of(1, 2, 3, 4, 5).iterator(); it.hasNext();) {
            int elem = it.next();
            list.add(elem);
        }
        */
        
        Iterable<Integer> list = List.of(1, 2, 3, 4, 5);
        for (Integer i : list) {
            System.out.println(i);
        }
        
        int[] array = { 1, 2, 3, 4, 5 };
        for (int i : array) {
            System.out.println(i);
        }

        assertEquals("IntList(1, 2, 3, 4, 5)", list.toString());
    }

    @Test
    public void equals() {
        List<Integer> list1 = List.of(1, 2, 3, 4, 5);
        List<Integer> list2 = List.of(1, 2, 3, 4, 5);
        List<Integer> list3 = List.of(1, 2, 3, 4);
        List<Integer> list4 = List.of(1, 2, 3, 4, 6);
        
        assertEquals(list1, list2);
        assertFalse(list1.equals(list3));
        assertFalse(list1.equals(list4));
    }
    
    @Test
    public void foreach() {
        List<Integer> list = List.of(1, 2, 3, 4, 5);
        list.foreach(new Command<Integer>() {
            public void execute(Integer element) {
            }
        });
    }
}
```