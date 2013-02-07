```java
public class IgelHase {
 
    public static void main(String[] args) {
        System.out.println("Hase " + new IgelHase() {
            @Override
            public String toString() {
                System.out.print("Igel ");
                return "";
            }
        }.toString());
    }
    
}
```