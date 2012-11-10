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
sumOfElements(arry);
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
