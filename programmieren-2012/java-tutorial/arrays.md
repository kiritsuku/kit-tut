#Arrays

- Ermöglichen Speicherung von Datenblöcken

- Besitzen eine feste Größe, die nach der Erstellung nicht mehr geändert werden kann

- Elemente sind von `0` bis `n-1` adressiert, wobei `n` der Länge des Arrays entspricht

###Deklaration eines Arrays
```java
<type>[] <name> = new <type>[<size>];
// or
<type> <name>[] = new <type>[<size>];
// or
<type>[] <name> = { <elem1>, <elem2>, <elemN> };
```

Beispiel:
```java
int[] array = new int[5];
array[0] = 1;
array[1] = 2;
array[2] = 3;
array[3] = 4;
array[4] = 5;

// shorter
int[] array = { 1, 2, 3, 4, 5 };
array.length == 5;
```
###Benutzung eines Arrays
```java
int sumOfElements(int[] array) {
  int sum = 0;
  for (int i = 0; i < array.length; ++i) {
    sum += array[i];
  }
  return sum;
}

int sumOfString(String str) {
  // either `str.toCharArray()`
  // or
  int sum = 0;
  for (int i = 0; i < str.length(); ++i) {
    sum += str.charAt(i)-'0';
  }
  return sum;
}
```

Aufruf über:
```java
int[] array = {1,2,3,4,5};
sumOfElements(array);
sumOfElements(new int[] {1,2,3,4,5})
sumOfString("12345")
```

###Varargs

Varargs sind Syntax-Zucker für Arrays:
```java
int sumOfVarArgs(int... array) {
  int sum = 0;
  for (int i = 0; i < array.length; ++i) {
    sum += array[i];
  }
  return sum;
}
```
Benutzung:
```java
sumOfVarArgs(1,2,3,4,5)
```

###Mehrdimensionale Arrays

```java
<type>[][][] <name> = new <type>[<size1>][<size2>][<size3>];
```
Benötigt `<size1>*<size2>*<size3>` Speicherblöcke.

Beispiel:
```java
int[][] array = { { 1, 2, 3 }, { 4, 5 } };
array.length == 2;
array[0].length == 3;
array[1].length == 2;
```

##Beispiel: Dynamisches Array
```java
class DynamicIntArray {
  int[] array;
  int elements;
  
  DynamicIntArray() {
    array = new int[10];
    elements = 0;
  }
  
  void add(int i) {
    if (elements >= array.length) {
      System.out.println("resizing");
      int[] tmp = new int[elements+10];
      System.arraycopy(array, 0, tmp, 0, elements);
      array = tmp;
    }
    array[elements] = i;
    ++elements;
  }
}
```
An dem internen Array kann man von außerhalb der Klasse herumpfuschen, z.B. kann man dessen Elemente verändern:
```java
DynamicIntArray dynamicIntArray = new DynamicIntArray();
for (int i = 0; i < 35; ++i) {
  dynamicIntArray.add(i);
}
dynamicIntArray.array[17] = 12345;
```

Um dies zu unterbinden kann man das interne Array als `private` deklarieren und den Zugriff nur noch über einen Getter erlauben, der das Array clont:
```java
class DynamicIntArray {
  private int[] array;
  private int elements;
  
  // rest as before
  
  int[] getArray() {
    return array.clone();
  }
}
```

Nun bekommt man bei jedem Zugriff ein neues Array zurück.