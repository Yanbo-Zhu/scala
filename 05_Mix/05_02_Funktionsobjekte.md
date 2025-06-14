
```
val discount: Double => Double = price => price * 0.9

object Discount {
  def apply(price: Double): Double = price * 0.9
}

val originalPrice = 100.0

val discountedA = discount(originalPrice) // über die benannte Funktion
val discountedB = Discount(originalPrice) // über das Funktionsobjekt

```



- In Scala lassen sich Funktionen nicht nur deklarieren, sondern auch als **Funktionsobjekte** ausdrücken
- Sowohl benannte als auch anonyme Funktionen werden beim Kompilieren zu Objekten (in der JVM), welche die Funktionslogik in einer `**apply**`-Methode kapseln
- Zusätzlich kann man solche Funktionsobjekte auch explizit selber schreiben, indem man eine `**apply**`-Methode in einem Objekt definiert
- Somit gibt es nun drei Möglichkeiten, Funktionen zu schreiben:
    - Als benannte Funktionen,
    - als anonyme Funktionen und
    - als Funktionsobjekte.


# 1 Probleme bei Funktionsobjekten

- Ein einfaches so definiertes Funktionsobjekt mit einer `**apply**`-Methode kann zwar wie eine Funktion aufgerufen werden, es lässt sich aber nicht einer Variablen zuweisen und auch nicht als ein Argument beim Aufruf einer Funktion höherer Ordnung übergeben
- Problem: Die so definierte Funktion wird nicht als Funktionstyp erkannt, d.h. sie ist nicht typisiert
- Um Funktionsobjekte zu typisieren, definiert Scala die Traits `**Function0**`, `**Function1**`,`**...**`,`**Function22**`, die Funktionsobjekten beigemicht werden können

![](image/Pasted%20image%2020250614174154.png)


# 2 Verwendung von Function-Traits



```
val Discount: Function1[Double, Double] = x => x * 0.9

object Discount extends Function1[Double, Double] {
  def apply(price: Double): Double = price * 0.9
}

val Discount: Function1[Double, Double] = new Function1[Double, Double] {
  def apply(price: Double): Double = price * 0.9
}
```


- Function-Traits in Scala typisieren Funktionsobjekte und machen sie damit zu vollwertigen Funktionen (d.h. First Class Citizens) 
- Bei `**FunctionX**` spezifiziert `**X**` die Anzahl der Argumente
- `**FunctionX**` kann daher als Alternative zu den bisherigen Funktionstypen (die nur "Syntaxzucker" für `**FunctionX**` sind) analog verwendet werden, wobei die Datentypen der Argumente durch Typparameter der Generics festzulegen sind


# 3 Zusätzliche Methoden auf den Function-Traits


- Die Functions-Traits in Scala bieten neben einer abstrakten `**apply**`-Methode noch ein paar nützliche implementierte Methoden, die auf einem Funktionsobjekt aufgerufen werden können
- `**andThen**` ermöglicht die nachgeschachtelte Verarbeitung, d.h. es wird eine Funktion erzeugt, die zunächst `**f(x)**` und dann `**g(f(x))**` ausführt
- `**compose**` ermöglicht die Vorverarbeitung, d.h. es wird eine Funktion erzeugt, die zunächst `**g(x)**` und dann `**f(g(x))**` aufruft

andthen
```scala
val f: Int => Int = x => x + 1
val g: Int => String = x => s"Result: $x"

val combined1: Int => String = f.andThen(g)

combined1(5)
```

![](image/Pasted%20image%2020250614175325.png)



Compose
```scala
val f: Int => Int = x => x + 1
val g: String => Int = _.toInt

val combined2: String => Int = f.compose(g)

combined2("41")
```


# 4 Eta-Expansion


 Können Methoden auch wie Funktionen behandelt werden?
 
```scala
case class Product(name: String, category: String)

object ProductUtils {

  def isElectronics(product: Product): Boolean = 
    product.category == "electronics"

  def filterProducts(products: List[Product], predicate: Product => Boolean): List[Product] =
    products.filter(predicate)

  def demo(): Unit = {
    val products = List(
      Product("TV", "electronics"),
      Product("Shirt", "fashion"),
      Product("Laptop", "electronics")
    )

    // Hier übergeben wir eine Methode? Echt jetzt?
    val electronics = filterProducts(products, isElectronics)

    println("Electronics products: " + electronics)
  }
}
```

```
ProductUtils.demo()
```


---

- Die **Eta-Expansion** ist ein Konzept aus der funktionalen Programmierung zur expliziten Darstellung der Anwendung einer Funktion auf ein Argument
- Sie bedeutet: Eine Funktion `**f**` wird in eine gleichwertige Funktion der Form `**x=>f(x)**` überführt, d.h. die neue Funktion hat dasselbe Verhalten wie `**f**`, aber ihr Funktionsparameter ist explizit sichtbar
- Scala verwendet die Eta-Expansion um Methoden zu Funktionen zu konvertieren, die dann an Variablen gebunden und an Funktionen höherer Ordnung übergeben werden können
- Die Eta-Expansion generiert ein neues Objekt mit einer `**apply**`-Methode mit der Signatur der Methode, aus der die Methode aufgerufen wird
- Achtung: Auf Methoden ohne Parameter, die ohne Klammern definiert wurden, kann die Eta-Expansion nicht angewendet werden

**Eta-Expansion（η-展开）** 是函数式编程中的一个概念，用于**显式地表示将函数应用于参数的过程**。

- 它的含义是：一个函数 `**f**` 被转换成一个等价的函数形式 `**x => f(x)**`，也就是说，新函数与 `f` 的行为完全相同，但它的函数参数是**显式可见**的。
- Scala 使用 Eta-Expansion 将**方法转换为函数**，以便这些方法可以绑定到变量上，并作为**高阶函数的参数**进行传递。
- Eta-Expansion 会生成一个具有 `**apply**` 方法的新对象，该 `apply` 方法的签名与原方法一致。
- 注意：**对没有参数并且定义时没有括号的方法，Eta-Expansion 无法应用。**


![](image/Pasted%20image%2020250614175813.png)

Eta-Expansion 背后的原理：生成一个函数对象
```
def add(x: Int, y: Int): Int = x + y

val f: (Int, Int) => Int = add _ // eta-expansion 生成一个对象：f(x, y) = add(x, y)

`val f = (x: Int, y: Int) => add(x, y)`

```

|情况|是否支持 Eta-Expansion|示例|
|---|---|---|
|有参数且带括号的方法|✅ 支持|`def f(x: Int): Int = ...`|
|无参数但带括号的方法|✅ 支持|`def f(): String = ...`|
|无参数且不带括号的方法|❌ 不支持|`def f: String = ...`|
|`case class` 自动生成的方法|✅ 自动支持|`apply`, `unapply` 等|


---

简单地说，就是把一个方法（不是函数）转换成一个函数值 `x => f(x)` 的过程，使它可以作为值传递，比如给 `map` 这类高阶函数。

1 从方法到函数的 Eta-Expansion
```scala
object Demo {
  def inc(x: Int): Int = x + 1   // 方法，不是函数

  val f1: Int => Int = inc _     // Eta-Expansion：将方法转为函数
  val f2: Int => Int = x => inc(x) // 手动展开（等价于上面）

  val result = f1(5)  // result = 6
  println(result)
}

```


- `inc` 是方法，不能直接赋值给函数变量。
- `inc _` 会触发 eta-expansion，把它变成函数 `x => inc(x)`。
- `_` 是 Scala 2 中的方式，在 Scala 3 中大多数情况下可省略，自动 eta-expansion。



2 将方法作为高阶函数参数

```scala
object Demo {
  def square(x: Int): Int = x * x

  val numbers = List(1, 2, 3)

  // Eta-expansion happens here automatically
  val squares = numbers.map(square) // OK ✅
  println(squares)  // List(1, 4, 9)
}
```

- `map` 期望一个函数 `Int => Int`
- 方法 `square` 被自动转换为函数 `x => square(x)`，即自动进行 eta-expansion



3 方法无括号时不可 eta-expand

```scala
object Demo {
  def greet: String = "Hello"

  // val f = greet _  // ❌ 编译错误！greet 没有括号

  def greetWithParens(): String = "Hi"
  val f = greetWithParens _     // ✅ OK

  println(f()) // 输出 Hi
}
```

- `greet` 是无参无括号方法，**不能进行 eta-expansion**
- `greetWithParens()` 有括号，可以 eta-expand 成 `() => String`



## 4.1 例子


1. Was ist der Unterschied zwischen einer Methode und einer Funktion in Scala?
2. Gegeben sei die folgende Methode: def add(a: Int, b: Int): Int = a + b Verwenden Sie Eta-Expansion, um diese Methode in eine Funktion zu konvertieren und weisen Sie sie einer Variable addFunc zu.
3. Erklären Sie, warum Eta-Expansion notwendig ist und in welchen Situationen sie verwen- det werden kann.


```scala
//1. Was ist der Unterschied zwischen einer Methode und einer Funktion in Scala?  
  
//  2. Gegeben sei die folgende Methode:  
//def add(a: Int, b: Int): Int = a + b  
//Verwenden Sie Eta-Expansion, um diese Methode in eine Funktion zu konvertieren und  
//weisen Sie sie einer Variable addFunc zu.  
  
def add(a: Int, b: Int): Int = a + b  
  
val addFunc = (add _)  
// scala2 : val addFunc: (Int, Int) => Int = add _  
// scala3 : val addFunc: (Int, Int) => Int = add  
  
  
//3. Erkl鋜en Sie, warum Eta-Expansion notwendig ist und in welchen Situationen sie verwendet werden kann.  
  
// methode ist always ein Objekt. Object can not be gien to a variable directly.  
// ???????? ?????? ?????????????????????????????  
// Eta-Expansion ist notwendig, um eine Methode in eine Funktion umzuwandeln, damit sie als Wert behandelt werden kann.
```

