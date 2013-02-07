Kompiliert der Code? Wenn ja, was wird ausgegeben? ([Quelle](http://www.java-forum.org/plauderecke/22639-java-quiz-81.html#post968833))

```java
public class Glommer<T>
{
    String glom(Collection<?> objs)
    {
        String result = "";
        for (Object o : objs)
            result += o;
        return result;
    }
 
    int glom(List<Integer> ints)
    {
        int result = 0;
        for (int i : ints)
            result += i;
        return result;
    }
 
    public static void main(String args[])
    {
        List<String> strings = Arrays.asList("1", "2", "3");
        System.out.println(new Glommer().glom(strings));
    }
 
}
```

Antwortmöglichkeiten:

```
Antwort 1:
Fehler zur Laufzeit

Antwort 2:
Compilerfehler

Antwort 3:
1 2 3

Antwort 4:
Endlosschleife
```