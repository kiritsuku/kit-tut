```java
package edu.kit.list;

import org.junit.Test;

import static org.junit.Assert.*;
import static edu.kit.list.IntListProblems.*;


public class IntListProblemsTest {

    @Test
    public void testLast() {
        IntList list = IntList.of(1, 1, 2, 3, 5, 8);
        assertEquals(8, last(list));
    }

    @Test
    public void testReverse() {
        IntList list = IntList.of(1, 1, 2, 3, 5, 8);
        IntList reverse = IntList.of(8, 5, 3, 2, 1, 1);
        assertEquals(reverse, reverse(list));
    }

    @Test
    public void testIsPalindrome() {
        assertTrue(isPalindrome(IntList.of(1, 2, 3, 2, 1)));
        assertFalse(isPalindrome(IntList.of(1, 2, 3, 4, 1)));
    }

    @Test
    public void testDuplicate() {
        IntList list = IntList.of(1, 1, 2, 3, 5, 8);
        IntList dup = IntList.of(1, 1, 1, 1, 2, 2, 3, 3, 5, 5, 8, 8);
        assertEquals(dup, duplicate(list));
    }

    @Test
    public void testDuplicateN() {
        IntList list = IntList.of(1, 1, 2, 3, 5, 8);
        IntList dup = IntList.of(1, 1, 1, 1, 1, 1, 2, 2, 2, 3, 3, 3, 5, 5, 5, 8, 8, 8);
        assertEquals(dup, duplicateN(3, list));
    }

    @Test
    public void testCompress() {
        IntList list = IntList.of(1, 1, 1, 2, 3, 3, 3, 1, 1, 5, 5, 5, 5, 8, 8);
        IntList compressed = IntList.of(1, 2, 3, 1, 5, 8);
        assertEquals(compressed, compress(list));
    }

    @Test
    public void testPack() {
        IntList list = IntList.of(1, 1, 1, 2, 3, 3, 3, 1, 1, 5, 5, 5, 5, 8, 8);
        IntList[] packed = {
                IntList.of(1, 1, 1),
                IntList.of(2),
                IntList.of(3, 3, 3),
                IntList.of(1, 1),
                IntList.of(5, 5, 5, 5),
                IntList.of(8, 8)
        };
        assertArrayEquals(packed, pack(list));

        assertArrayEquals(new IntList[] {}, pack(IntList.of()));
    }

    @Test
    public void testDrop() {
        IntList list = IntList.of(1, 1, 1, 2, 3, 3, 3, 1, 1, 5, 5, 5, 5, 8, 8, 4);
        IntList dropped = IntList.of(1, 1, 2, 3, 3, 1, 5, 5, 5, 8, 4);
        assertEquals(dropped, drop(3, list));
        assertEquals(IntList.of(), drop(1, list));
        assertEquals(list, drop(-1, list));
    }

    @Test
    public void testSlice() {
        IntList list = IntList.of(1, 1, 1, 2, 3, 3, 3, 1, 1, 5, 5, 5, 5, 8, 8, 4);
        IntList slice = IntList.of(3, 3, 1, 1, 5, 5, 5);
        assertEquals(slice, slice(5, 12, list));
        assertEquals(IntList.of(), slice(-7, 12, list));
        assertEquals(IntList.of(), slice(5, -7, list));
    }

    @Test
    public void testRotate() {
        IntList list = IntList.of(1, 2, 3, 1, 5, 5, 5, 8, 8, 4);
        assertEquals(IntList.of(1, 5, 5, 5, 8, 8, 4, 1, 2, 3), rotate(3, list));
        assertEquals(IntList.of(8, 4, 1, 2, 3, 1, 5, 5, 5, 8), rotate(-2, list));
    }

}
```