

Objektorientierte Sprachen betrachten Objekte als First Class Citizens, d.h. Objekte sind Werte, die Variablen zugewiesen oder als Argumente an Methoden übergeben oder von Methoden zurückgegeben werden können
In Scala sind Funktionen ebenfalls First Class Citizens, d.h. sie können in Bezug auf Variablen und Argumente wie Werte behandelt werden
Funktionen können eigenständig existieren und sind nicht in Form von Methoden an Objekte einer Klasse gebunden
Eine Funktion höherer Ordnung (Higher-order Function) ist eine Funktion, welche andere Funktionen als Argument übergeben bekommt oder als Ergebnis liefert
Funktionen können somit neben normalen Wertargumenten auch Funktionsargumente enthalten


- 面向对象的语言将对象视为第一类公民，即对象是可以赋值给变量、作为参数传递给方法或由方法返回的值。
    
- 在 Scala 中，函数也是第一类公民，这意味着它们可以像值一样处理，赋值给变量或作为参数传递。
    
- 函数可以独立存在，而不必以方法的形式绑定到类的对象上。
    
- 高阶函数（Higher-order Function）是指能够接受其他函数作为参数，或者返回其他函数的函数。
    
- 因此，函数除了可以包含常规的值参数外，还可以包含函数参数。



# 1 Funktionstypen und Funktionswerte#


![](image/Pasted%20image%2020250502220206.png)


- Ein **Funktionstyp** (**Function Type**) definiert die Signatur einer Funktion, d.h. die Funktions- oder Datentypen ihrer Argumente und ihres Rückgabewertes
- Genauso wie Variablen und Argumente einen Datentyp haben, wenn ihnen Datenwerte zugewiesen werden, benötigen sie einen Funktionstyp, wenn ihnen eine Funktion zugewiesen wird
- Funktionstypen sind das Gegenstück von Functional Descriptors als Typ eines Lambda-Ausdrucks in Java

- Ein **Funktionswert** oder **Funktionsliteral** ist ein Ausdruck oder ein Blockausdruck, der die Semantik der Funktion repräsentiert
- Eingabeargumente werden geklammert, es denn, es existiert nur ein Argument
- Funktionen enden typischerweise mit einem Ausdruck, der den Rückgabewert der Funktion repräsentiert


- Mehrere Anweisungen einer Funktion werden in einem Block zusammengefasst
- Die letzte Anweisung einer Funktion ist ein Ausdruck, der den Rückgabewert repräsentiert
- Dabei kann es sich um einen Daten- oder Funktionswert handeln
- Ausnahme: Prozeduren haben Nicht-Ausdrücke als letzte Anweisung und der Rückgabewert ist daher vom Datentyp `**Unit**`

- Der Sichtbarkeitsbereich (Scope) von Variablen entspricht dem von Blockausdrücken, d.h. sie sind nur innerhalb eines Blockes sichtbar



# 2 Funktionen in Scala vs Lambda-Ausdrücke in Java


![](image/Pasted%20image%2020250502220317.png)


```scala
val classifyProduct: (String, Double) => String = (category, price) => {
  if (price > 100) s"Premium $category"
  else if (price > 50) s"Standard $category"
  else s"Budget $category"
}
val result=classifyProduct("Auto", 25500)
println(result)
```


```java
import java.util.function.BiFunction;

BiFunction<String, Double, String> classifyProduct = (category, price) -> {
  if (price > 100) return "Premium " + category;
  else if (price > 50) return "Standard " + category;
  else return "Budget " + category;
};
String result = classifyProduct.apply("Auto", 25500.0);
System.out.println(result);

```


# 3 Variablen mit Wertdatentypen vs Funktionstypen


![](image/Pasted%20image%2020250502220509.png)



- Variablen, die auf Datentypen basieren, geben den in ihnen gespeicherten Wert zurück
- Sie können nicht als Funktion mit Klammerschreibweise aufgerufen werden, da sie lediglich einen Datenwert, aber keinen Funktionswert referenzieren
- Variablen, die auf Funktionstypen basieren, geben die Referenz auf die Funktion zurück, wenn sie ohne Klammern aufgerufen werden
- Variablen, die auf Funktionstypen basieren und die mit Klammern aufgerufen werden, wenden die Funktion mit dem oder den in Klammern aufgeführten Argument(en) an und geben das Ergebnis der Berechnungen zurück
- Hat die Funktion keine Argumente, so müssen leere Klammern beim Aufruf angegeben werden


# 4 Typableitung bei Funktionstypen


![](image/Pasted%20image%2020250502220526.png)


- Typableitung, wie sie bei Scala für Datentypen existiert, funktioniert bei Funktionstypen nicht, d.h. der Funktionstyp muss meistens explizit bei der Deklaration von Variablen oder Argumenten angegeben werden
- Ausnahme: Die Zuweisung einer Funktion geschieht bereits über eine Variable (auf der rechten Seite von `**=**`), die bereits einen Funktionstyp hat – in diesem Fall braucht die aufnehmende Variable nicht explizit mit dem Funktionstyp deklariert werden


# 5 Referenzielle Transparenz


![](image/Pasted%20image%2020250502220546.png)

- **Reine Funktionen** (**Proper Functions**) sind deterministisch, d.h. sie liefern immer den gleichen Wert für die gleichen Argumente
- Sie haben keine **Seiteneffekte** (**Side Effects**), d.h. sie verändern nicht den Objektzustand und führen keine Ausgabe (`**println**`) oder externen Interaktionen (Dateisystem, Netzwerk) durch
- Aber: Zugriff auf externe `**val**` ist erlaubt
- **Nicht-reine Funktion** (**Non-Proper Functions**) verändern den Zustand von externen Objekten, erzeugen Seiteneffekte (`**println**`) oder hängen von äußeren veränderlichen Faktoren ab (z.B. Systemzeit, Zufall, var) 
- Sie haben **Seiteneffekte** (**Side Effects**), d.h. sie nutzen oder beeinflussen veränderliche Werte außerhalb der Funktion und sind deshalb nicht-deterministisch

- 纯函数（Proper Functions）是确定性的，即对于相同的参数，它们总是返回相同的值。    
- 纯函数没有副作用（Side Effects），即它们不会改变对象的状态，也不会进行输出（如 println）或外部交互（如文件系统、网络等）。
- 但：访问外部的 `val` 是允许的。
- 非纯函数（Non-Proper Functions）会改变外部对象的状态，产生副作用（如 println），或者依赖于外部可变因素（如系统时间、随机数、`var`）等。
- 它们有副作用（Side Effects），即它们使用或影响函数外部的可变值，因此是非确定性的


- Ein Ausdruck ist **referenziell transparent**, wenn er durch seinen Wert ersetzt werden kann, ohne das Verhalten des Programms zu verändern
- Wenn `**f(x)**` immer denselben Wert liefert wie `**y**`, dann darf überall `**f(x)**` durch `**y**` ersetzt werden – und umgekehrt
- Referenzielle Transparenz ist nur durch reine Funktionen erreichbar
- Referenzielle Transparenz ist die Grundlage für viele Vorteile der funktionalen Programmierung:
    - Programme können leichter getestet werden,
    - sind leicht analysierbar, 
    - sind verständlicher und
    - besser optimierbar (z.B. durch Caching, Parallelisierung, Umordnung).


# 6 Prozeduren


![](image/Pasted%20image%2020250502220837.png)


- Eine **Prozedur** in Scala ist eine Funktion (oder Methode) vom Typ `**Unit**`, die nicht rechnet, sondern wirkt – z.B. druckt, speichert, verändert
- Jede Funktion, die mit einer Anweisung endet, ist eine Prozedur
- Anweisung werden durch Scala als Typ `**Unit**` abgeleitet, ein künstlicher Datentyp, der das Fehlen eines Wertes repräsentiert
- Prozeduren haben in der Regel Seiteneffekte und sind somit nicht-rein, da ihre Zweckmäßigkeit einzig und allein darin bestehen kann, etwas außerhalb ihres Scopes zu machen, weil sie keinen Wert zurückgeben

# 7 Anonyme Funktionen


![](image/Pasted%20image%2020250502220852.png)


- Funktionen können auf verschiedene Arten angelegt werden
    - Als **benannte Funktion**, d.h. Funktionsname, Funktionstyp, Funktionswert mit Argumenten und Funktionsrumpf (bisheriger Ansatz)
    - Als **anonyme Funktion**, d.h. Funktionskopf (Liste mit Argumenten und deren Typen) und Funktionsrumpf

- Bei anonymen Funktionen wird der Typ des Rückgabeparamters durch Typableitung bestimmt
- Wird eine anonyme Funktion als Argument an eine Funktion höherer Ordnung übergeben, werden ihre Argumenttypen durch den Funktionstyp des aufnehmenden Arguments bestimmt und brauchen nicht angegeben zu werden


# 8 Platzhaltersyntax


![](image/Pasted%20image%2020250502220936.png)

- Die **Platzhaltersyntax** (**Underscore Notation**) ist eine verkürzte Schreibweise für Funktionsliterale in Scala, wenn die Argumente nur einmal verwendet werden und in ihrer Reihenfolge eindeutig sind
- Der Funktionstyp muss explizit außerhalb des Literals spezifiziert worden sein und die Anzahl der Platzhalter im Literal muss mit der Anzahl der Argumente im Funktionsaufruf übereinstimmen



# 9 Funktionsaufruf mit Blockausdrücken


![](image/Pasted%20image%2020250502221005.png)


- Funktionen können mit Blöcken aufgerufen werden, wenn der Block einen Wert ergibt, der dem erwarteten Typ des Arguments entspricht
- Funktionen mit nur einem Argument kann man direkt mit einem Block aufrufen, bei mehreren Argumenten benötigt man runde Klammern und pro Argument eines Block


# 10 Funktionen höherer Ordnung

- Eine Funktion ist eine **Funktion höherer Ordnung**, wenn sie mindestens eines der folgenden Merkmale besitzt:
    - Sie nimmt eine Funktion als Argument entgegen oder
    - sie gibt eine Funktion als Ergebnis zurück.
- Funktionen höherer Ordnung ermöglichen eine Abstraktion auf höherem Niveau (siehe deklarativ versus imperativ), die Parametrisierung von Verhalten und sie sind die Grundlage für funktionale Programmiermuster
- In Scala können Funktionen als Werte an Argumente mit dem entsprechenden Funktionstyp übergeben werden
- Die übergebene Funktion wird dann mit dem Namen des Arguments aufgerufen


```
val filterApples: (List[Apple], Apple => Boolean) => List[Apple] = (inventory, predicate) => {
  var result = List.empty[Apple]
  for (apple <- inventory) {
    if (predicate(apple)) {
      result = result :+ apple
    }
  }
  result
}



```

```
List<Apple> heavyApplesLambda = filterApples(inventory, apple -> apple.getWeight() > 150);
```


- In Java werden Funktionen in Form von Lambda-Ausdrücken an Methoden übergeben
- Da es keine Funktionstypen gibt, muss die Signatur der übergeben Funktion durch ein Functional Descriptor eines Functional Interfaces definiert werden, welches den Datentyp des aufnehmenden Arguments bildet
- Der übergebene Lambda-Ausdruck wird dann mit dem Namen der Methode aufgerufen, der im Functional Interface definiert wurde


# 11 Funktionen als Rückgabe

![](image/Pasted%20image%2020250502221150.png)



# 12 Closures


- Ein **Closure** ist eine Funktion, welche die freien Variablen aus ihrer äußeren Umgebung "einfängt" und diese eingefangenen Variablen auch dann noch verwenden kann, wenn der ursprüngliche Kontext nicht mehr existiert
- Eine Funktion + ihr Kontext=Closure
- `**weightLimit**` ist eine Variable außerhalb der anonymen Funktion
- Die anonyme Funktion greift auf `**weightLimit**` zu, speichert deren Wert und verwendet ihn später in der Ausführung einer höheren Funktion, selbst wenn `**weightLimit**` im ursprünglichen Gültigkeitsbereich nicht mehr existieren würde

- **闭包（Closure）**是一种函数，它“捕获”了来自其外部环境的自由变量，即使这些变量原本所在的上下文已经不存在，闭包仍然可以使用它们。
    
- 一个闭包 = 函数 + 它的上下文环境。
    
- `**weightLimit**` 是一个在匿名函数外部定义的变量。
    
- 匿名函数访问了 `**weightLimit**`，保存了它的值，并在之后执行高阶函数时仍然能够使用，即使 `**weightLimit**` 已经超出了原来的作用域。




Java 
- Java unterstützt Closures ebenfalls seit Java 8
- Allerdings müssen in Java die eingefangenen Variablen final oder effektiv final sein
- Eine Variable ist **final**, wenn sie explizit mit final deklariert wurde
- Eine Variable ist **effektiv final**, wenn sich der Wert nach der Initialisierung nicht mehr verändert

![](image/Pasted%20image%2020250502221224.png)



```
val weightLimit: Double = 150.0

val isHeavyApple: Apple => Boolean = apple => apple.weight > weightLimit

val heavyApples: List[Apple] = filterApples(inventory, isHeavyApple)

println(heavyApples)
```





```
def makeWeightChecker(weightLimit: Int): Int => Boolean = {
  // 返回一个匿名函数，这个匿名函数使用了外部变量 weightLimit
  weight => weight <= weightLimit
}

val checker = makeWeightChecker(100)  // 创建一个闭包，捕获了 weightLimit = 100

println(checker(90))  // 输出 true，因为 90 <= 100
println(checker(110)) // 输出 false，因为 110 > 100
```


- `makeWeightChecker` 是一个返回函数的函数（高阶函数）。
    
- `weightLimit` 是该函数的参数，但在返回的匿名函数中被引用。
    
- `checker` 是一个闭包，它记住了 `weightLimit = 100`，即使 `makeWeightChecker` 已经执行完退出。
    
- 每次调用 `checker` 时，它都能访问并使用 `weightLimit`。



`def makeWeightChecker(weightLimit: Int): Int => Boolean =`
你定义了一个高阶函数，它的名字是 makeWeightChecker。
它接收一个参数：weightLimit，类型是 Int。
它返回的是另一个函数：这个返回的函数接收一个 Int 参数，并返回一个 Boolean 类型的结果。


`weight => weight <= weightLimit`
这是一个 匿名函数（也称 Lambda 表达式），参数是 weight。
函数体是：判断 weight 是否小于等于外部的 weightLimit。
注意这里访问了 weightLimit，它不是这个匿名函数的参数，而是外部函数的参数 —— 这就是“捕获外部变量”，也叫做 闭包（closure）。

## 12.1 Closures – Veränderung der eingefangenen Variablen



Scala
```
var weightLimit: Double = 150.0

val isHeavyApple: Apple => Boolean = apple => apple.weight > weightLimit

// weightLimit wird verändert!
weightLimit = 120.0

// Jetzt benutzen wir das Closure:
val heavyApples: List[Apple] = filterApples(inventory, isHeavyApple)

println(heavyApples)
```


- In Scala schließt ein Closure eine Referenz auf die Variable ein, nicht deren Wert zum Zeitpunkt der Erstellung
- Wenn nach der Erstellung des Closures der Wert von eightLimit verändert wird, dann sieht das Closure zum Zeitpunkt der Ausführung immer den Aktuellen Wert der eingefangenen Variable


---

![](image/Pasted%20image%2020250502222208.png)

- In Java müssen Variablen, die in Closures verwendet werden, final oder effektiv final sein
- Daher kann der Inhalt der Variablen nach ihrer Initialisierung nicht mehr verändert werden
- Das gilt für primitive Typen und für Referenztypen
- Aber: Der Inhalt des Objekts, auf das die Referenz zeigt, darf verändert werden



## 12.2 Beispiel 


![](image/Pasted%20image%2020250502222234.png)