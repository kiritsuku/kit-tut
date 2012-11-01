#Tutorium 2 (02.11.12)

## Arithmetik in Java

Was ist das Ergebnis der folgenden Operationen?
```java
byte by = 1;
int i = 2;
long l = 5;
double d = 7.0;
boolean b = true;
char c = 'c';
String st = "|";

int res1 = by + i;
long res2 = i * l;
double res3 = i + l;
int res4 = c + 1;
long res5 = l % i;
long res6 = l / i;
double res7 = l / i;
double res8 = l / 7D;
double res9 = l / (double) i;
double res10 = l / d;
long res11 = 9223372036854775807L + 1;
String res12 = c + st;
String res13 = st + l;
String res14 = b + st;
```

## Methoden und Konstruktoren
[Link zum Tutorial](../java-tutorial/methoden-konstruktoren.md)

## Übung: Autofabrik

Schreibe eine Autofabrik, die Autos verschiedenen Typs herstellt:
- VW Golf VI
  - Baujahr 2009, Preis 22.000 €
  - Motor: 500 kg, 1.6 l Hubraum, 75 kW, 4 Zylinder
  - Karosserie: blau, 900kg, 5 Sitzplätze
- Ferrari 599
  - Baujahr 2007, Preis 125.000 €
  - Motor: 600 kg, 6 l Hubraum, 456 kW, 12 Zylinder
  - Karosserie: rot, 1000 kg, 2 Sitzplätze
- Mercedes E-Klasse
  - Baujahr 2001, Preis 47.000 €
  - Motor: 500 kg, 2.6 l Hubraum, 125 kW, 6 Zylinder
  - Karosserie: schwarz, 900 kg, 5 Sitzplätze

Implementiere folgende Methoden:

- `double consumption()` - Berechnet den Kraftstoffverbrauch des Autos in Litern. Für diesen Kraftstoffverbrauch gelte
die Formel:
```Verbrauch(l) = 0,035 * Leistung(kW) - 0,0063 * Gewicht(kg) + 13,33```

- `int maximumSpeed()` - Berechnet die Höchstgeschwindigkeit des Autos in km/h. Hierfür gelte die Formel:
```Höchstgeschwindigkeit(km/h) = 5 * Wurzel(Leistung(kW)) + 0.04 * Gewicht(kg) + 66.4 * Wurzel(Hubraum(l))```

- `boolean isEgolocial()` - Liefert `true` zurück falls das Auto ökologisch ist.

## Logische Verknüpfungen

Was für Werte nimmt `d` an?
```java
boolean a = true;
boolean b = false;
boolean c = true;
boolean d;
d = !a;
d = a && b;
d = !a || !c;
d = (a && b) || !c;
d = !(!a && b) || !c;
```

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://imgs.xkcd.com/comics/responsible_behavior.png)