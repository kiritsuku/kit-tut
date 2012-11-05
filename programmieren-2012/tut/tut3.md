#Tutorium 3 (09.11.12)

##Javadoc

Normale Kommentare:

```java

// 
// Dear maintainer:
// 
// Once you are done trying to 'optimize' this routine,
// and have realized what a terrible mistake that was,
// please increment the following counter as a warning
// to the next guy:
// 
// total_hours_wasted_here = 42
// 
void method() {
  //When I wrote this, only God and I understood what I was doing
  //Now, God only knows
  System.out.println("This method does something cool");
}

/**
 * Always returns true.
 */
public boolean isAvailable() {
    return false;
}
```
[Source of above comments](http://stackoverflow.com/questions/184618/what-is-the-best-comment-in-source-code-you-have-ever-encountered)

Javadoc:
```java



/**
 * Returns the days of a given month and a given year. The month can be given
 * as its name regardless if it contains upper or lower case. Thus, the names
 * "january", "January" or "jaNuARy" are equivalent.
 * 
 * @param month
 *        the name of a month
 * @param year
 *        the year
 * @return the days of a month or {@code 0} if the name does not exist or the
 *         year is negative.
 */
int getDaysOfMonth(String month, int year) {
  // needs to be implemented
  return 0;
}

/**
 * Drunk, fix later.
 * 
 * @author My humble self
 * @version 0.1
 * @since JDK1.6, Nov 5, 2012
 */
class Class {}
```

##Kontrollstrukturen
[Link zum Tutorial](../java-tutorial/kontrollstrukturen.md)

##Übungsaufgabe zu Kontrollstrukturen
[Link zur Übung](../java-tutorial/kontrollstrukturen-uebung.md)

##Schleifen
Link zum Tutorial

##Das Wort zum Schluss
- Checkstyle wird euch in Zukunft foltern!
- Abgabetermin des 2. Übungsblattes: 19.11.12 13:00

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://geekandpoke.typepad.com/.a/6a00d8341d3df553ef017617629460970c-pi)