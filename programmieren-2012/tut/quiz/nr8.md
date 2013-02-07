Was gibt das folgende Programm aus? ([Quelle](http://www.java-forum.org/plauderecke/22639-java-quiz-46.html#post471161))

```java
public class Quiz {

        public static final Quiz QUIZ = new Quiz();

	private static final Boolean EASY = true;
	private static final String NAME = "Quiz";
	private final Boolean isEasy = EASY;
	private final String name = NAME;

	public final Boolean isEasy() {
		return isEasy;
	}

	public final String getName() {
		return name;
	}

	public static void main(String[] args) {
		System.out.println(QUIZ.getName() + " is " + QUIZ.isEasy());
	}
}
```

Antwortmöglichkeiten:

```
Antwort 1:
Quiz is null

Antwort 2:
null is null

Antwort 3:
Quiz is true

Antwort 4:
Compilerfehler
```