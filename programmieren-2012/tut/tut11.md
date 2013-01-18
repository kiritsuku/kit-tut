#Tutorium 11 (18.01.13)

Quiz am Anfang (Kompiliert der Code? Wird er korrekt ausgeführt? Was wird ausgegeben?):

```java
public class Test {

  public static void main(final String... args) {
    final List<Integer> xs = Arrays.asList(2, 4, 3, 5, 1);

    final List<List<Integer>> ys = new ArrayList<List<Integer>>();
    ys.add(xs);

    System.out.println(ys);
    Collections.sort(ys);
    System.out.println(ys);
  }
}
```

Hinzufügen eines `Comparators`:

```java
Collections.sort(ys, new Comparator<List<Integer>>() {
  public int compare(List<Integer> o1, List<Integer> o2) {
    // no implementation
    return 0;
  }
});
```

##Häufige Fehler auf dem Übungsblatt

Es hat fast niemand abgegeben, also gibt es nicht viel zu sagen :(

##Entwicklung einer Shell (Fortsetzung)
[Link zum Tutorial](../java-tutorial/entwicklung-shell-fort.md)

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://imgs.xkcd.com/comics/good_code.png)