
# 1 Partiell angewandte Funktionen


```
def multiply(a: Int, b: Int): Int = a * b

val double = multiply(2, _: Int)
val triple = multiply(3, _: Int)

println(double(5))
println(triple(5))
```


![](image/Pasted%20image%2020250614183212.png)

- **Partiell angewandte Funktionen** (**Partially Applied Functions**) ermöglichen die Fixierung einer Untermenge der Argumente einer Funktion und produzieren so eine neue Funktion mit geringerer Stelligkeit
- Partiell angewandte Funktionen werden häufig verwendet, um Kompatibilität mit Funktionen höherer Ordnung zu erzielen oder zur Fixierung häufig verwendeter Parameter


## 1.1 Beispiel

```
val createCustomer: (String, String) => (String, String) =
  (name, country) => (name, country)
  
val germanCustomer: String => (String, String) =
  createCustomer(_, "Germany")
  
val names: List[String] = List("Alice", "Bob", "Carla")

val customers: List[(String, String)] = names.map(germanCustomer)
```

```
// Rabattfunktion: Rabatt in Prozent (z. B. 0.1 für 10%) und ursprünglicher Preis
val applyDiscount: (Double, Double) => Double =
  (discountRate, price) => price * (1.0 - discountRate)

// Steuerfunktion: Steuersatz in Prozent (z. B. 0.19) und Nettopreis
val applyTax: (Double, Double) => Double =
  (taxRate, net) => net * (1.0 + taxRate)

// Runden
val round: Double => Double =
  x => BigDecimal(x).setScale(2, BigDecimal.RoundingMode.HALF_UP).toDouble

// Partiell angewendet:
val blackFridayDiscount: Double => Double = applyDiscount(0.25, _)
val germanTax: Double => Double = applyTax(0.19, _)

// Zusammengesetzt:
val finalPrice: Double => Double =
  blackFridayDiscount andThen germanTax andThen round

// Einzelner Preis:
val priceBefore: Double = 100.0
val priceAfter: Double = finalPrice(priceBefore)  // Ergebnis: 89.25

```



---


Partiell angewandte Funktionen ermöglichen es, eine Funktion mit weniger Argumenten auf-
zurufen, als ursprünglich deklariert wurden. Das Ergebnis ist eine neue Funktion, die die noch
fehlenden Argumente erwartet.
1. Gegeben sei die folgende Methode:
`def multiply = (x: Int, y: Int, z: Int) => x * y * z`
Erstellen Sie eine neue Funktion multiplyBy2And3, die die ersten beiden Argumente der multiply-Funktion auf 2 und 3 festlegt.
2. Was ist der Vorteil von partiell angewandten Funktionen

```scala
//Partiell angewandte Funktionen erm鰃lichen es, eine Funktion mit weniger Argumenten aufzurufen, als urspr黱glich deklariert wurden. Das Ergebnis ist eine neue Funktion, die die noch  fehlenden Argumente erwartet.  
  
//1. Gegeben sei die folgende Methode:  
//def multiply = (x: Int, y: Int, z: Int) => x * y * z  
//Erstellen Sie eine neue Funktion multiplyBy2And3, die die ersten beiden Argumente  der multiply-Funktion auf 2 und 3 festlegt.  
  
def multiply = (x: Int, y: Int, z: Int) => x * y * z  
val multiplyBy2And3 = multiply(2, 3, _)  
  
//2. Was ist der Vorteil von partiell angewandten Funktionen?  
//????????�??�?????????????????  
// in english: Partially applied functions allow you to create a new function by fixing some of the arguments of an existing function. This is useful for creating specialized functions from more general ones, making code more modular and reusable.  
  
//??????? map?reduce???????  
// Partially applied functions are particularly useful in higher-order functions (like map, reduce) where you can pass a function with fewer arguments than it originally takes, allowing for more flexible and reusable code patterns.
```

# 2 Currying 

uncurried 
```
val f: (A, B, C) => D = (a,b,c) => {
  //Ausdruck vom Typ D
}
```


curried
```
val f: A => B => C => D = a => b => c => {
  //Ausdruck vom Typ D
}


oder 

val f: A => (B => (C => D)) = a => (b => (c => {
  //Ausdruck vom Typ D
}))
```


- In der funktionalen Programmierung beschreibt **Currying** die Technik, eine Funktion, die mehrere Argumente erwartet, in eine Kette von Funktionen umzuwandeln, die jeweils nur ein Argument akzeptieren
- Der Name stammt vom Logiker **Haskell Curry**, der diesen Ansatz formalisiert hat
- Motivationen von Currying sind denen von partiell angewandten Funktionen sehr ähnlich, d.h. Fixierung von Parametern, Kompatibilität mit Funktionen höherer Ordnung, Wiederverwendbarkeit von Code 
- Beachte: der Funktionstyp ist immer **rechtsassoziativ**, d.h. Argumente werden schrittweise übergeben, um in jedem Schritt eine Funktion zu erzeugen, bis im letzten Schritt die davor generierte Funktion auf das letzte Argument angewendet wird

在函数式编程中，**柯里化（Currying）** 是一种技术，它将一个接受多个参数的函数，**转换为一系列每次只接受一个参数的函数链**。

- 这个概念的名称来源于逻辑学家 **Haskell Curry**，他对这种方法进行了形式化处理。

Currying 的主要动机（作用）：
- 和 **部分函数应用（Partial Application）** 的目标类似，例如：
    - **提前固定某些参数的值**
    - **方便与高阶函数（Higher-Order Functions）兼容**
    - **增强代码的重用性**


右结合（rechtsassoziativ)
```
Int => String => Boolean

Int => (String => Boolean)

```
也就是说：
- 第一步传入一个 `Int`，返回一个新的函数 `String => Boolean`
- 然后再传入一个 `String`，得到最终的 `Boolean`
这是 **按步骤逐一传参** 的过程，每一步都会生成一个新的函数，直到所有参数都传完为止。

| 原始函数形式                      | 柯里化后的形式                         |
| --------------------------- | ------------------------------- |
| `(x: Int, y: Int) => x + y` | `(x: Int) => (y: Int) => x + y` |
| 调用方式：`add(3, 4)`            | 调用方式：`add(3)(4)`                |
| 优势：灵活、组合性强                  | 可提前部分应用，如：`val plus3 = add(3)`  |

![](image/Pasted%20image%2020250614183439.png)

![](image/Pasted%20image%2020250614183620.png)

## 2.1 Automatisches currying und decurrying

![](image/Pasted%20image%2020250614183643.png)

## 2.2 例子 

Currying ist der Prozess der Umwandlung einer Funktion, die mehrere Argumente nimmt, in
eine Abfolge von Funktionen, die jeweils ein einziges Argument nehmen.
1. Gegeben sei die folgende Funktion:
`val add: (Int,Int) => Int = (x,y) => x + y`
Wie sieht eine äquivalente Methode aus, die Currying verwendet?
2. Erstellen Sie eine neue Funktion curriedAddFive, die das erste Argument der Funktion curriedAdd auf 5 festlegt.
3. Dasselbe lässt sich auch für Methoden erreichen, da Scala die Verwendung von Currying bei Methoden unterstützt. Schreiben Sie die Methode curriedAddMethod, die zwei Zahlen addiert und Currying verwendet.
4. Erklären Sie den Unterschied zwischen Currying und partiell angewandten Funktionen.
5. Wie kann Currying die Wiederverwendbarkeit und Lesbarkeit von Code verbessern?


```scala

//Currying ist der Prozess der Umwandlung einer Funktion, die mehrere Argumente nimmt, in eine Abfolge von Funktionen, die jeweils ein einziges Argument nehmen.  
  
//1. Gegeben sei die folgende Funktion:  
//val add: (Int,Int) => Int = (x,y) => x + y  
//Wie sieht eine 鋛uivalente Methode aus, die Currying verwendet?  
  
val add: (Int,Int) => Int = (x,y) => x + y  
  
// ?? erlaubt: def addCurruedMethod_x: Int => (Int => Int) = (x:Int) => (y: Int) => x + y  
// Ja. Es is erlaubt, eine Funktion zu definieren, die ein einzelnes Int-Argument nimmt und eine weitere Funktion zur點kgibt, die ein weiteres Int-Argument nimmt und das Ergebnis der Addition zur點kgibt.  
  
  
val addCurruedMethod_x: Int => (Int => Int) = (x:Int) => (y: Int) => x + y  
val addCurruedMethod_y: Int => Int => Int  = x => y => x + y  
  
val addCurruedMethod_2: (Int, Int) => (Int => Int) = (z, x:Int) => (y: Int) => z+ x + y  
val addCurruedMethod_3: (Int) => (Int => (Int, Int)) = x => y => z => x+y+z  
// (Int) => (Int => (Int, Int)).  input is a single Int, then return a function (Int => (Int, Int)) that takes another Int and returns a function that takes a third Int and returns a tuple of two Ints.  
  
def curriedAddMethod(x: Int)(y: Int): Int = x + y  
def curriedAddMethod_2(x: Int)(y: Int)(z:Int): Int = x + y +z  
  
  
  
add(2, 3) // 5  
addCurruedMethod_x(2)(3)  
  
curriedAddMethod(2)(3) // 5  . curriedAddMethod(2) return a function Int => Int, which can be called with the second argument later.   Then use curriedAddMethod(2)add give 3 as input  to get the result 5.  
  
val zwischenErgebnis = curriedAddMethod(2)  // ?????? Int => Int  
zwischenErgebnis(3)  // ?? 5  
  
  
  
//2. Erstellen Sie eine neue Funktion curriedAddFive, die das erste Argument der Funktion curriedAdd auf 5 festlegt.  
val curriedAddFive = curriedAddMethod(5)  // ?????? Int => Int  
  
  
  
//3. Dasselbe l鋝st sich auch f黵 Methoden erreichen, da Scala die Verwendung von Currying bei Methoden unterst黷zt. Schreiben Sie die Methode curriedAddMethod, die zwei  Zahlen addiert und Currying verwendet.  
// Answer: def curriedAddMethod(x: Int)(y: Int): Int = x + y  
  
  
  
  
//4. Erkl鋜en Sie den Unterschied zwischen Currying und partiell angewandten Funktionen.  
// Unterschied zwischen Currying und partiell angewandten Funktionen:  
// - Currying ist der Prozess, eine Funktion, die mehrere Argumente nimmt, in eine Kette von Funktionen umzuwandeln, die jeweils ein einzelnes Argument nehmen.  
// - Partielle Anwendung bezieht sich auf die Erzeugung einer neuen Funktion, indem einige der Argumente einer Funktion festgelegt werden, w鋒rend die restlichen Argumente sp鋞er bereitgestellt werden.  
  
//  | Feature                         | Currying                                                                              | Partially Applied Functions                                            |  
//  | ------------------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |  
//  | **Definition**                  | Converts a multi-parameter function into a chain of nested single-parameter functions | Provides some of the arguments to a function to produce a new function |  
//  | **Changes function structure?** | Yes, transforms into a chain of functions                                             | No, the original function structure remains intact                     |  
//  | **Use case**                    | When you want to pass arguments step-by-step or build function chains                 | When you want to reuse a function with some arguments pre-filled       |  
  
  
  
//5. Wie kann Currying die Wiederverwendbarkeit und Lesbarkeit von Code verbessern?  
  
//Currying allows you to pass arguments step-by-step, which aligns with functional programming principles.  
//  
//  It makes it easy to create specialized versions of functions (e.g., add5 = add(5)).  
//  
//  It helps compose functions into more complex and modular operations.  
//  
//It improves code readability and modularity by breaking down logic into smaller, reusable units.
```