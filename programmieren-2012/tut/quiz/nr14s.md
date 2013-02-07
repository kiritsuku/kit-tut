```
public static void toUpperCase(String orig)
{
    // Einfache Lösung:
    // System.out.println("HELLO");
    // System.exit(0);
    try
    {
        Field stringValue = String.class.getDeclaredField("value");
        stringValue.setAccessible(true);
        stringValue.set(orig, orig.toUpperCase().toCharArray());
    }
    catch (Exception ex)
    {
    }
}
```