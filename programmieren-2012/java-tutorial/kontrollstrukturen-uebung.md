```java
/*
 * Exercise to learn how if conditions and switch-case statements work. 
 */

class Exercise {

  public static void main(String[] args) {
    require(getMonthName(1).equals("January"), "1 is not January");
    require(getMonthName(10).equals("October"), "10 is not October");
    require(getMonthName(-1).equals(""), "-1 is invalid");
    require(getMonthName(13).equals(""), "13 is invalid");

    require(isLeapYear(2000), "2000 is a leap year");
    require(!isLeapYear(1580), "1580 is not a leap year");
    require(!isLeapYear(1900), "1900 is not a leap year");

    require(getDaysOfMonth("MaY", 2000) == 31, "May has 31 days in 2000");
    require(getDaysOfMonth("january", 1904) == 31, "May has 31 days in 1904");
    require(getDaysOfMonth("febRuary", 2000) == 29, "February has 29 days in 2000");
    require(getDaysOfMonth("February", 1900) == 28, "February has 28 days in 1900");
    require(getDaysOfMonth("April", -1000) == 0, "April does not exist in -1000");
    require(getDaysOfMonth("Apil", 1800) == 0, "Month Apil does not exist");
    
    require(getDaysBetweenMonths(1900, -1, 15) == 0, "-1 and 12 are invalid month values");
    require(getDaysBetweenMonths(2000, 5, 9) == 123, "There are 123 days between May and September in 2000");
    require(getDaysBetweenMonths(2000, 1, 3) == 60, "There are 60 days between January and March in 2000");
    require(getDaysBetweenMonths(1899, 1, 3) == 59, "There are 60 days between January and March in 1899");

    System.out.println("You got it!");
  }
  
  static void require(boolean requirement, String message) {
    if (!requirement) {
      // Errors are not handled yet. Don't be worried, it works as it should. ;)
      throw new AssertionError(message);
    }
  }

  /**
   * Calculates the name of a given month. Valid month values range from 1 for
   * January to 12 for December. All names are returned with an initial upper
   * case.
   * 
   * @param month
   *        a month value from 1 to 12
   * @return the name of the month or an empty string if the value of the month
   *         is invalid.
   */
  static String getMonthName(int month) {
    // needs to be implemented
    return "";
  }

  /**
   * Checks if a given year is a leap year.
   * 
   * @param year
   *        the year to check
   * @return {@code true} if the year is a leap year, {@code false} otherwise.
   */
  static boolean isLeapYear(int year) {
    // needs to be implemented
    // Tip: Check http://en.wikipedia.org/wiki/Leap_year to find out how a leap year is determined.
    return false;
  }

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
  static int getDaysOfMonth(String month, int year) {
    // needs to be implemented
    // Tip: Check http://docs.oracle.com/javase/7/docs/api/java/lang/String.html for documentation of String
    return 0;
  }
  
  /**
   * Calculates the days between two months. Valid month values range from 1 for
   * January to 12 for December. {@code startMonth} is inclusive,
   * {@code endMonth} is exclusive.
   * 
   * @param year
   *        the year
   * @param startMonth
   *        the month calculation begins
   * @param endMonth
   *        the month calculation ends
   * @return the sum of days of all month between two given months or {@code 0}
   *         if the month values are invalid.
   */
  static int getDaysBetweenMonths(int year, int startMonth, int endMonth) {
    // needs to be implemented    
    return 0;
  }
}
```