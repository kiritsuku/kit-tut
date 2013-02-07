Was wird ausgegeben?

```java
public class Test {
  public static void main(final String[] args) {
    final Integer i1 = 15;
    final Integer i2 = 15;
    final Integer j1 = 153;
    final Integer j2 = 153;

    System.out.println(i1 == i2);
    System.out.println(i1.equals(i2));
    System.out.println(j1 == j2);
    System.out.println(j1.equals(j2));
  }
}
```

Antwortmöglichkeiten:

```
Antwort 1:
true true false true

Antwort 2:
false true fals true

Antwort 3:
true true true true

Antwort 4:
false false true false
```