Was gibt dieses Programm aus? ([Quelle](http://www.java-forum.org/plauderecke/22639-java-quiz-81.html#post977351))

```java
public class Quiz{
  public static boolean toBoolean(String s){
    s = s.toLowerCase();
    if(s.equals("y") || s.equals("yes"))
      s = "true";
    return Boolean.getBoolean(s);
  }
 
  public static void main(String[] args){
    String[] input = {"true", "Y", "yes", "false"};
    for(String s: input){
      System.out.printf("%s\t%b%n", s, toBoolean(s));
      // #printf()entspricht auch
      //System.out.println(s + "\t" + toBoolean(s));
    }
  }
}
```

Antwortmöglichkeiten:

```
Antwort 1:
true true true false

Antwort 2:
false false false false

Antwort 3:
false false false true

Antwort 4:
true true true true
```