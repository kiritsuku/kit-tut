Was wird bei dem Syso ausgegeben? ([Quelle](http://www.java-forum.org/plauderecke/22639-java-quiz-81.html#post968007))

```java
public abstract class Basic {
    protected final Integer  value = 4;
    
    public Basic(){
        this.init();
    }
    
    public abstract void init();
}

public class Sub extends Basic {
    private final Integer zahl = 5;
    
    @Override
    public void init() {
        System.out.println("Basic: " +value+ ", Sub: " +zahl);
 
    }
    
    public static void main(String[] args){
        new Sub();
    }
 
}
```

Antwortmöglichkeiten:

```
Antwort 1:
Basic: null, Sub: null

Antwort 2:
Basic: 4, Sub: 5

Antwort 3:
Basic: 4, Sub: null

Antwort 4:
Compilerfehler
```