#Kontrollstrukturen

##if-Abfragen

```java
if (<if_condition>) {
  <then_condition>
} else {
  <else_condition>
}
```

Mehrere if-Abfragen:
```java
if (<if_condition1>) {
  <then_condition1>
} else if (<if_condition2>) {
  <then_condition2>
} else if (<if_conditionN>) {
  <then_conditionN>
} else {
  <else_condition>
}
```

Yoda Conditions:

![Yoda Conditions Image](http://www.codinghorror.com/.a/6a0120a85dcdae970b0176169611b8970c-800wi)

Using `if(constant == variable)` instead of `if(variable == constant)`, like `if(4 == foo)`. Because it's like saying "if blue is the sky" or "if tall is the man". ([Source](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html))

conditional-operator:
```java
<if_condition> ? <then_condition> : <else_condition>;
```
Dies ist eine Kurzschreibwei der if-Abfrage mit else-Block und ist außerdem eine Expression, was bedeutet, dass es zu einem Wert evaluiert.

##switch-case

```java
switch (<value>) {
  case <case1>:
    <case_body1>
    break;
  case <case2>:
    <case_body2>          // missing break, fall-through
  case <caseN>:
    <case_bodyN>
    break;
  default:                // optional
    <default_case_body>
}
```

##while-Schleife

```java
while (<condition>) {
  <body>
}
```

Schleifen können mit `break` unterbrochen werden:

```java
while (<condition>) {
  if (<if_condition>) {
    break;
  }
  <body>
}
// continue execution+
```

Mit `continue` kann der momentane Schleifendurchlauf abgebrochen werden, damit beim Nächsten weitergemacht wird:
```java
while (<condition>) {
  if (<if_condition>) {
    continue;
    // don't execute <body>
  }
  <body>
}
```

##do-while-Schleife

```java
do {
  <body>
} while (<condition>)
```

Führt eine while-Schleifen mindestens einmal aus.

##for-Schleife

```java
// general form
for (<start_value>; <condition>; <next_step>) {
  <body>
}

// example
for (int i = 0; i < 10; ++i) {
  System.out.println(i);
}
```

#Update für `Rational`

Bessere Implementierung von `toString`:

```java
// in class Rational
public String toString() {
  return denominator == 1
      ? String.valueOf(numerator)
      : numerator + "/" + denominator;
}
```

Implementierung von `equals` und `hashCode`:

```java
// in class Rational
public boolean equals(Object obj) {
  if (obj instanceof Rational) {
    Rational r = (Rational) obj;
    return numerator * r.denominator - denominator * r.numerator == 0;
  }
  return false;
}

public int hashCode() {
  return toString().hashCode();
}
```

Bearbeitung von negativen Nennern:

```java
// in class Rational
public static Rational valueOf(int numerator, int denominator) {
  return denominator > 0
      ? new Rational(numerator, denominator)
      : new Rational(-numerator, -denominator);
}

// set ctor of Rational private to forbid creation by new
private Rational(int numerator, int denominator) {
  this.numerator = numerator;
  this.denominator = denominator;
}
```

Berechnung des GGT:
```java
// Euclidean algorithm: http://en.wikipedia.org/wiki/Euclidean_algorithm
int gcd(int a, int b) {
  while (b != 0) {
    int t = b;
    b = a % b;
    a = t;
  }
  return a;
}
```

Benutzung des GGT:

```java
// in class Rational
Rational(int numerator, int denominator) {
  int g = gcd(Math.abs(numerator), Math.abs(denominator));
  this.numerator = numerator / g;
  this.denominator = denominator / g;
}
```