#Tutorium 1 (26.10.12)

##Organisatorisches

###Was sind die Aufgaben eines Tutors?
* Fragen zur Vorlesung / zum Stoff beantworten
* Übungsblätter korrigieren
* Übungsaufgaben machen

###Was gehört nicht zu den Aufgaben eines Tutors?
* Wiederholung der Vorlesung
* Bespaßung im Tutorium
* Jeden durch die Klausur bringen

###Für was ist der Student verantworlich?
* Rechtzeitige Abgabe der Übungsblätter
* Rechtzeitige Anmeldung zu Klausuren etc.
* Vor- und Nachbereitung der Vorlesung
* Finden relevanter Informationen

###Bei Fragen und Problemen?
* Kommilitonen -> Tutor -> Übungsleiter -> Dozent
* Forum im Praktomat
* [StackOverflow](http://stackoverflow.com/questions/tagged/java)
* [java-forum.org](http://www.java-forum.org/)
* Programmierberatung
  * Jeden Dienstag und Mittwoch (15:45-17:15) im hinteren Teil der ATIS
  * Andrang nicht vorhersehbar -> nicht bis kurz vor knapp mit dem Anliegen warten

###Schon im [Praktomat](https://praktomat.info.uni-karlsruhe.de/praktomat_2012_WS/) angemeldet?
* Abgabe der Übungsblätter
* Einsicht der Korrektur
* Abgabe beliebig häufig möglich, aber nur bis zur Abgabefrist
* Verwendung nur im KITnet (oder mit VPN) möglich
* Rechnet nicht mit dessen Erreichbarkeit kurz vor der Deadline
* Tipp: Teillösungen hochladen

###Übungsblätter
* sechs Übungsblätter insgesamt
* 20 Punkte pro Übungsblatt -> max. 120 Punkte erreichbar
* mind. 50% (60 Punkte) müssen für den Übungsschein erreicht werden
* werden schnell deutlich anspruchsvoller -> viel Punkte zu Beginn sammeln

###Die Sache mit dem Abschreiben...
* Nicht erlaubt -> Schon bei **einmaligem** Nachweis erfolgt Ausschluss vom Übungsschein + Klausur
* Automatische Plagiatsprüfung -> Also sehr gefährlich
* Programmieren ist Teil der Orientierungsprüfung -> muss bis zum 3. Semester bestanden werden

###Sonstiges
* [Graphische Prüfungsordnung](kit-tut/programmieren-2012/tut/pruefungsordnung.pdf)
* Abgabe der [Einverständniserklärung](https://studium.kit.edu/sites/vab/0xBCAF741C5EC2E7498A5A3CA436270F21/Vorlesungsunterlagen/2012-10-15_Einverst%C3%A4ndniserkl%C3%A4rung.pdf) (bis 02.11.12) für den Praktomaten
* Anmeldung am [Praktomat](https://praktomat.info.uni-karlsruhe.de/praktomat_2012_WS/) (bis 02.11.12)
* Anmeldung des [Übungsscheins](https://studium.kit.edu/meinsemester/Seiten/pruefungsanmeldung.aspx) (bis 31.03.12)
* Erledigung des 1\. Übungsblatt (bis 04.11.12 00:00 anywhere on earth)
  * Also vermutlich bis 05.11.12 13:00 ;)

##Programmierumgebung

###Wie arbeitet man mit Java?

* Installation des [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) ([Anleitung](http://docs.oracle.com/javase/7/docs/webnotes/install/index.html))
* Die Datei `Test.java` anlegen und die Java-Klasse `Test` mit einem Editor erstellen (**muss gleich heißen wie die Datei!**)
* Compilierung mit `javac Test.java`
* Compiler erzeugt `Test.class`
* Ausführung der class-Datei mit `java Test` (**kein `.class` am Ende!**)

Ein Hello-World Programm in Java:
```java
public class Test {
  public static void main(String[] args) {
    System.out.println("Hello World");
  }
}
```
Der komplette Erstellvorgang:

```sh
$ ls
Test.java
$ cat Test.java
public class Test {
  public static void main(String[] args) {
    System.out.println("Hello World");
  }
}
$ javac Test.java
$ ls
Test.class  Test.java
$ java Test
Hello World
```

###Editoren (Auswahl)
* [Sublime Text](http://www.sublimetext.com/)
* [Textadept](http://foicica.com/textadept/)
* [Notepad++](http://notepad-plus-plus.org/) (nur Windows)
* [gedit](http://projects.gnome.org/gedit/) (nur Linux)
* TextEdit (nur Mac OS X)

_Word oder Writer sind auch Editoren aber für uns gänzlich ungeeignet!_

###Integrated Development Environments (IDEs) (Auswahl)
* [eclipse](http://www.eclipse.org/)
* [NetBeans](http://netbeans.org/)

##Einführung in die Objektorientierung
[Link zum Tutorial](kit-tut/programmieren-2012/java-tutorial/einfuehrung-oo.md)

##Codestyle
Programmiert _sauber_ und _schön_, aber was ist das überhaupt?

Das hier auf jeden Fall nicht:
```
class Person{int age;String name;Color
faveColor;BankAccount account;Address address
;}class BankAccount{double money;}class Address
{String town;String street;int streetNumber;int
zip;}enum Color{RED, BLUE, GREEN;}
```

Vielleicht aber das:
```java
class Person {          // class names in UpperCamelCase
  int age;              // indentation
  String name;          // pleasant naming
  Color faveColor;      // Variables in lowerCamelCase
  BankAccount account;  // same language everywhere
  Address address;
}

class BankAccount {     // same brace style everywhere
  double money;         // same indentation style everywhere
}

class Address {
  String town;
  String street;
  int streetNumber;
  int zip;
}

enum Color {
  RED, BLUE, GREEN;     // Constants in UPPER_CASE
}
```
[Java Code Conventions](http://www.oracle.com/technetwork/java/codeconv-138413.html)

##Das Wort zum Schluss

* Tipp für das Übungsblatt: Bei [Oracle](http://docs.oracle.com/javase/tutorial/java/concepts/index.html) findet man ein Java Tutorial, das Teile des Übungsblattes anspricht.
* [Einverständniserklärung](https://studium.kit.edu/sites/vab/0xBCAF741C5EC2E7498A5A3CA436270F21/Vorlesungsunterlagen/2012-10-15_Einverst%C3%A4ndniserkl%C3%A4rung.pdf) abgeben!

##Vielen Dank für eure Aufmerksamkeit!
![Comic am Ende](http://abstrusegoose.com/strips/ars_longa_vita_brevis.PNG)