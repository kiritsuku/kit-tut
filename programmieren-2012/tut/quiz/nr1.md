Wie bei den üblichen Code-Fragen: Compilierbar? Welche Exception fliegt oder was wird ausgegeben? ([Quelle](http://www.java-forum.org/plauderecke/22639-java-quiz-79.html#post906360))

```java
public class Quiz {
  public static void main(String[] args) {
    int[] a = {2, 13, 14, 15};
    System.out.println(a[(a = new int[]{2, 3, 4, 5})[a[0]]]);
  }
}
```

Antwortmöglichkeiten:

```
Antwort 1:
Compilerfehler

Antwort 2:
4

Antwort 3:
Laufzeitfehler

Antwort 4:
14
```