```java
/*
 * Exercise for array handling.
 */

class Exercise {

  public static void main(String[] args) {
    require(getMonthName(1).equals("January"), "1 is not January");
    require(getMonthName(10).equals("October"), "10 is not October");
    require(getMonthName(-1).equals(""), "-1 is invalid");
    require(getMonthName(13).equals(""), "13 is invalid");
    
    require(average(new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}) == 5.5, "average of elements 1 to 10 is 5.5");
    
    int[] vectorA = {7, 6, 5, 4};
    int[] vectorB = {3, 4, 5, 6};
    int[] vectorACopy = {7, 6, 5, 4};
    int[] vectorBCopy = {3, 4, 5, 6};
    require(java.util.Arrays.equals(addVectors(vectorA, vectorB), new int[] {10, 10, 10, 10}), "vectorA+vectorB is 10 forall elements");
    require(java.util.Arrays.equals(vectorA, vectorACopy), "addVectors has changed elements of vectorA");
    require(java.util.Arrays.equals(vectorB, vectorBCopy), "addVectors has changed elements of vectorB");
    require(addVectors(new int[] {1, 2}, new int[] {1}).length == 0, "invalid dimensions must result in empty vector");
    
    int[] indices = {0,7,3,9,1};
    int[] elements = {11,5,8,4,7,4,15,6,18,3,5};
    require(sum(indices, elements) == 29, "sum of elements must be 29");
    require(sum(indices, new int[] {}) == 0, "sum with invalid indices must be 0");
    
    int[][] matrixA = {{9,8,7},{6,5,4},{3,2,1}};
    int[][] matrixB = {{1,2,3},{4,5,6},{7,8,9}};
    int[][] matrixACopy = {{9,8,7},{6,5,4},{3,2,1}};
    int[][] matrixBCopy = {{1,2,3},{4,5,6},{7,8,9}};
    require(java.util.Arrays.deepEquals(addMatrices(matrixA, matrixB), new int[][] {{10,10,10},{10,10,10},{10,10,10}}), "matrixA+matrixB is 10 for all elements");
    require(java.util.Arrays.deepEquals(matrixA, matrixACopy), "addMatrices has changed elements of matrixA");
    require(java.util.Arrays.deepEquals(matrixB, matrixBCopy), "addMatrices has changed elements of matrixB");
    require(addMatrices(new int[][] {{1}, {2}}, new int[][] {{1},{2,3}}).length == 0, "invalid dimensions must result in empty matrix");

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
    // Tip: use an array to get the name of the month
    return "";
  }

  /**
   * Calculates the average of all elements in the array.
   * 
   * @param array
   *        the array with the elements
   * @return the average of the elements
   */
  static double average(int[] array) {
    // needs to be implemented
    return 0.0;
  }

  /**
   * Adds two vectors if they have the same dimension and returns the result as
   * new vector.
   * 
   * @param vectorA
   *        the first vector
   * @param vectorB
   *        the second vector
   * @return the sum of both vectors or an empty vector if the dimensions of the
   *         passed vectors are different.
   */
  static int[] addVectors(int[] vectorA, int[] vectorB) {
    // needs to be implemented
    return new int[] {};
  }
  
  /**
   * Sums up all elements referenced by an index.
   * 
   * @param indices
   *        the indices of the elements to be summed up
   * @param elements
   *        the available elements
   * @return the sum of the elements or {@code 0} if an invalid index value
   *         occurs.
   */
  static int sum(int[] indices, int[] elements) {
    // needs to be implemented
    return 0;
  }

  /**
   * Adds two matrices if they have the same same dimension and returns the
   * result as new matrix.
   * 
   * @param matrixA
   *        the first matrix
   * @param matrixB
   *        the second matrix
   * @return the sum of both matrices or an empty matrix if the dimensions of
   *         the passed matrices are different.
   */
  static int[][] addMatrices(int[][] matrixA, int[][] matrixB) {
    // needs to be implemented
    return new int[][] {};
  }
}
```