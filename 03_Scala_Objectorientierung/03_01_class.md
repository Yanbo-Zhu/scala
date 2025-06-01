
# 1 Klassen, Singletons und Anwendungen


class field  (类字段 类成员)
class method 

## 1.1 Singletons


方法1 
```
object FeaturedProduct {

  private var id: Int = 0
  private var name: String = "Undefined"
  private var price: Double = 0.0

  def setProduct(newId: Int, newName: String, newPrice: Double): Unit = {
    id = newId
    name = newName
    price = newPrice
  }

  def applyDiscount(percent: Double): Unit = {
    price = price * (1 - percent / 100)
  }

  def info(): String = {
    f"Featured Product[$id]: $name costs $$${price}%.2f"
  }
}
```

```
FeaturedProduct.setProduct(201, "Smartphone", 999.0)
println(FeaturedProduct.info())

FeaturedProduct.applyDiscount(10)
println(FeaturedProduct.info())

```

方法2 
```
class Product(val id: Int, val name: String, var price: Double) {
  
  def applyDiscount(percent: Double): Unit = {
    price = price * (1 - percent / 100)
  }

  def info(): String = {
    f"Product[$id]: $name costs $$${price}%.2f"
  }
}
```

```
// Verwendung
val p = new Product(101, "Headphones", 89.99)
p.applyDiscount(10)
println(p.info())
```


方法2 和方法1 的不同1

就是创建一个方法2 的 instanz 的时候 id name, price  必须都给给值.  方法1 不用. 因为 方法2 的**Klassenparametern** 都给到了 klassekopf 他的 constructor 是锁死的

详细解释见 [[#]]

## 1.2 Anwendungen: object and main()


```
object HelloApp extends App {
  println("Hello from Scala App!")

  val product = new Product(101, "Headphones", 89.99)
  println(product.info())
}


object HelloMain {
  def main(args: Array[String]): Unit = {
    println("Hello from Scala main!")

    val product = new Product(102, "Keyboard", 49.99)
    println(product.info())
  }
}
```

Warum muss main methode immer static sein?
![](image/Pasted%20image%2020250601175437.png)


## 1.3 object 和 class 的区别


| 特性    | `class`         | `object`              |
| ----- | --------------- | --------------------- |
| 实例数量  | 可创建多个实例         | 只能有一个（单例）             |
| 实例化方式 | 需要 `new`        | 不需要 `new`             |
| 状态    | 每个实例独立          | 全局唯一状态                |
| 主要用途  | 定义对象模板          | 定义工具类/静态方法/伴生对象       |
| 示例    | `new MyClass()` | `MyObject.myMethod()` |
|       |                 |                       |

**`object` 是单例对象**
- Scala 中的 `object` 定义的是一个 **单例** —— 只能存在一个实例（自动创建）。
- 没有 `new`，直接通过名称访问其成员。

```
object Hello {
  def greet(): Unit = println("Hello from object")
}
Hello.greet()  // ✅ 不需要 new
```

class 是面向对象的结构，可以创建多个实例。

需要用 new 来实例化。
```
class Person(val name: String) {
  def greet(): Unit = println(s"Hello, I am $name")
}
val p = new Person("Anna")
p.greet()  // 输出: Hello, I am Anna
```



可以配合使用：companion object
当 object 和 class 同名并在同一个文件中定义时，称为伴生对象（companion object）。
class 可以包含实例方法，object 用于定义伴生的静态方法/工厂方法。

```
class Car(val model: String)

object Car {
  def apply(model: String): Car = new Car(model)  // 工厂方法
}

val c = Car("BMW")  // 使用 object Car 的 apply 方法
```





# 2 Methoden  

## 2.1 Funktionen vs. Methoden

![](image/Pasted%20image%2020250601174445.png)


```
//Methode in Klasse 
class LowerCaser{
	def toLowerCase(s:String): String = s.toLowerCase
}

//Instanz erstellen
val lowerInstance = new LowerCaser()

//Methode auf Instanz aufrufen
lowerInstance.toLowerCase("JHABFJBHAF")


//Funktion dagegen:
val toLowerCase: (String => String) = (s) => s.toLowerCase

//Aufrufe von Funktion eigenständig möglich
toLowerCase("FUNKTION") //funktion
```



----

Ermöglicht Umwandlung: Methode → Funktion
Sinn: Methoden können dann ebenfalls wie Werte behandelt und übergeben werden

```scala
class Calculator {
  def add(x: Int, y: Int): Int = x + y  // Dies ist eine Methode
}

val calc = new Calculator
val addFunc = calc.add _ // Dies ist eine Funktion, erzeugt durch Eta-Expansion

addFunc(3,2) //5
```


## 2.2 Dot-Notation and Opeator-Notation 

- Bei der **Dot-Notation** wird die Methode über einen Punkt nach dem Objekt aufgerufen
- Operatoren wie `**+**`, `**-**`, `**/**`, und `**==**` sind als Methoden auf Objekten implementiert

- Bei der **Operator-Notation** (**Infix-Notation**) werden Methoden wie ein Operator aufgerufen, d.h. Objekt, Methodenname und Argument werden durch ein Leerzeichen voneinander getrennt
- Die Operator-Notation kann nur auf Methoden angwendet werden, die genau ein Argument haben


```
class Product(val name: String, val price: Double) {
  def discounted(percent: Double): Double =
    price * (1 - percent / 100)
}

val p = new Product("Laptop", 1000.0)

// Dot-Notation:
val d1 = p.discounted(10.0)

// Operator-Notation (infix):
val d2 = p discounted 10.0
```



## 2.3 Klassenparamter und Primärkonstruktor


- **Klassenparameter** sind Eingabevariablen, die bei der Instanziierung der Klasse übergeben werden, und die automatisch zu Feldern der Klasse werden
- Ergänzung von Klassenparametern mit `**val**` oder **`var`** macht sie zu Feldern, die `**public**` sind, also von außerhalb der Klasse zugreifbar
- Klassenparameter, die ohne `**val**` oder `**var**` deklariert werden, sind `**private val**` und von außerhalb der Klasse nicht zugreifbar und nach der Instanziierung auch nicht mehr veränderbar (weil `**val**`)

![](image/Pasted%20image%2020250601174911.png)

```
//public:
class PublicUser (val name: String){

}

//private: 
class PrivateUser (name: String){

}
```

---


- Jede Klasse hat einen **primären Konstruktor** (**Primary Constructor**), der implizit durch die Klassenparameter und den Rumpf der Klasse gegeben ist
- Keine explizite Definition des Konstruktors wie in Java (vgl. vorherige Folie)
- Anweisungen, Ausdrücke und Felder außerhalb von Methoden im Rumpf der Klasse gehören zum primären Konstruktor
- Klassenparameter werden durch den primären Konstruktor zu Instanzvariablen des Objekts und mit den übergebenen Werten initialisiert

![](image/Pasted%20image%2020250601155612.png)








### 2.3.1 Argument  默认是 val 还是 var  in class definition


Sichtbarkeit von Variablen im 
- `var` → **public**, veränderlich
- `val` → **public**, unveränderlich
- `Keine` Modifikatoren → **private**, veränderlich
- `private val` → **private**, unveränderlich


在 Scala 的类定义中，默认既不是 val 也不是 var。如果你在类构造函数中写参数而不加 val 或 var，这些参数：
- 只在 构造函数中有效（即仅限于 class 的构造阶段）
- 不会成为该类的 成员字段，不能在类外访问或修改

```
class Person(name: String)
```

- 这里的 `name` 只是一个构造函数参数
- 不能通过 `p.name` 来访问（即使你创建了 `val p = new Person("Anna")`）


正确创建类属性的方式：
- class Person(val name: String)  // name 成为只读属性
- class Person(var name: String)  // name 成为可读写属性


|写法|是否为成员字段|可读|可写|
|---|---|---|---|
|`name: String`|❌ 否|❌|❌|
|`val name: String`|✅ 是|✅|❌|
|`var name: String`|✅ 是|✅|✅|


### 2.3.2 var und val 的区别

`val`：不可变变量（immutable）
- 定义后不能被修改（像 `final` 或 `const`）
- 更安全，推荐使用


`var`：可变变量（mutable）

- 可以在之后修改其值


```
class Student(val name: String, var age: Int, studyField: String)
```

- `val name` → 自动生成只读属性 `name`，可以从对象访问，但不能修改
- `var age` → 自动生成可读写属性 `age`，可以从对象访问和修改
- `studyField`（没有 `val` 或 `var`）→ 只是构造参数，**不会成为成员变量**，对象外部不能访问



```
val s = new Student("Anna", 22, "Math")
println(s.name)   // ✅ 可以访问
s.name = "Ben"    // ❌ 错误：val 是只读
s.age = 23        // ✅ var 可以修改
println(s.studyField) // ❌ 错误：不能访问（不是成员）
```



## 2.4 Zugriffsmodifikatoren


- **Zugriffsmodifikatoren** in Scala steuern, wo auf Felder und Methoden einer Klasse zugegriffen darf
- `**public**` (Standard): überall sichtbar
- `**private**`: innerhalb der Klasse sichtbar
- `**protected**`: innerhalb der Klasse und Subklassen sichtbar
- `**private[this]**`: nur innerhalb der Instanz sichtbar
- Durch `**private[this]**` kann der Compiler garantieren, dass ein Feld niemals von einer anderen Instanz berührt wird, was die Nebenläufigkeit vereinfacht
- Java implementiert die Instanzunterscheidung nicht


```scala
class Product(val basePrice: Double) {

  private var discountApplied: Boolean = false
  private[this] val internalCode: String = generateInternalCode()

  def applyDiscount(): Unit = {
    if (!discountApplied) {
      discountApplied = true
      println(s"Discount applied to product $internalCode")
    }
  }

  // Vergleicht Rabattstatus zweier Produkte, erlaubt da private
  def hasSameDiscountStatus(other: Product): Boolean = {
    this.discountApplied == other.discountApplied
  }

  // Vergleicht interne Codes (geht nicht bei private[this])
  // def hasSameInternalCode(other: Product): Boolean = {
  //   this.internalCode == other.internalCode 
  // }

  private def generateInternalCode(): String = {
    "PRD-" + System.nanoTime().toString.takeRight(6)
  }
}
```

- **Zugriffsmodifikatoren** in Scala steuern, wo auf Felder und Methoden einer Klasse zugegriffen darf
- `**public**` (Standard): überall sichtbar
- `**private**`: innerhalb der Klasse sichtbar
- `**protected**`: innerhalb der Klasse und Subklassen sichtbar
- `**private[this]**`: nur innerhalb der Instanz sichtbar
- Durch `**private[this]**` kann der Compiler garantieren, dass ein Feld niemals von einer anderen Instanz berührt wird, was die Nebenläufigkeit vereinfacht
- Java implementiert die Instanzunterscheidung nicht

## 2.5 Sekundärkonstruktoren


- Eine Klasse hat optional **Sekundärkonstruktoren**
- Sie ermöglichen alternative Instanziierungen mit einer Untermenge der Klassenparameter des Primärkonstruktors
- Sekundärkonstruktoren werden mit `**this**` benannt und beginnen mit einem Aufruf des zuvor definierten Sekundärkonstruktors oder des primären Konstruktors
- Die Sekundärkonstruktoren einer Klasse müssen unterschiedliche Signaturen haben, und sie definieren keinen Rückgabetyp (nicht mal `**Unit**`)


```scala
class Product(val id: Int, val name: String, var price: Double) {

  var category: String = "General"
  println(s"New product created: $name with price $$${price}")
  def info(): String = f"Product[$id]: $name in $category costs $$${price}%.2f"

  // 🔹 Sekundärkonstruktor 1: ohne Kategorie
  def this(id: Int, name: String) = {
    this(id, name, 0.0) // Aufruf des Primärkonstruktors
    this.category = "Uncategorized"
  }

  // 🔹 Sekundärkonstruktor 2: nur mit ID
  def this(id: Int) = {
    this(id, "Unknown", 0.0)
    this.category = "Unknown"
  }
}
```


```scala
val p1 = new Product(1001, "Monitor", 199.99)
val p2 = new Product(1002, "Keyboard")         // Sekundärkonstruktor 1
val p3 = new Product(1003)                     // Sekundärkonstruktor 2

println(p1.info())
println(p2.info())
println(p3.info())
```

## 2.6 Methoden ohne Argumente

```
class Product(val basePrice: Double) {

  var currentPrice: Double = basePrice  // aktueller, veränderbarer Preis

  val taxRate: Double = 0.19

  // Reine Methode: immer basierend auf basePrice
  def baseTaxIncludedPrice: Double = basePrice * (1 + taxRate)

  // Nicht-reine Methode: basierend auf aktuellem Preis
  def currentTaxIncludedPrice(): Double = currentPrice * (1 + taxRate)

  // Nicht-reine Methode: ändert currentPrice
  def applyDefaultDiscount(): Unit = {
    println("Applying 10% discount")
    currentPrice = currentPrice * 0.9
  }
}
```


- Eine übliche Konvention in Scala besteht darin, Methoden ohne Argumente
    - ohne Klammern zu definieren, wenn sie rein sind und
    - mit Klammern zu definieren, wenn sie nicht rein sind.
- Diese Methoden sollten dann gemäß ihrer Definition mit oder ohne Klammern aufgerufen werden

- Worin besteht der Unterschied zwischen einer reinen Methode ohne Argumente und der Deklaration und Initialisierung einer Variablen?
- Der Wert der Variablen wird bei der Initialisierung berechnet, also nur einmal
- Der Rückgabewert der Methode wird immer dann berechnet, wenn sie aufgerufen wird


## 2.7 Getter- und Setter-Methoden


```
class Product(val id: Int, val name: String, var price: Double)


val p = new Product(101, "Headphones", 89.99)

println(p.id)           // Aufruf des automatisch generierten Getters
println(p.name)         // Aufruf des automatisch generierten Getters
println(p.price)        // Aufruf des Getters für var

p.price = 79.99         // Aufruf des automatisch generierten Setters
println(p.price)        // Neuer Wert
```

- Scala generiert für jede Klasse für ihre Klassenparamter (wenn sie den Präfix `**val**` oder `**var**` haben) und für Instanzvariablen automatisch Getter-Methoden (wenn sie den Präfix `**val**` oder `**var**` haben) und Setter-Methoden (nur bei `**var**`)
- Die Zugriffsnotation ist identisch, d.h. `**p.price=7.99**` anstelle von `**p.setPrice(79.99)**` wie in Java
- Scala erzeugt für `**val name: String**` automatisch eine Methode `**def name: String**` (Getter)
- Scala erzeugt für `**val price: Double**` automatisch zwei Methoden `**def price: Double**` (Getter) und `**def price_=(newPrice: Double): Unit**` (Setter)
- Diese Methoden sind für den Compiler sichtbar, aber für den Benutzer wirken sie wie ein direkter Zugriff auf die Felder
- Beachte: Der Name der Setter-Methode ist nicht `**price**`, sondern `**price_=**` , d.h. beim Aufruf `**price=0.99**` übersetzt der Compiler die Anweisung zu `**price_=(0.99)**`


Eigene Getter- und Setter-Methoden
```
class Product(val id: Int, val name: String) {

  private var _price: Double = 0.0

  def price: Double = _price

  def price_=(newPrice: Double): Unit = {
    if (newPrice >= 0) _price = newPrice
    else println("Invalid price")
  }
}


val p = new Product(101, "Keyboard")

println(p.price)  // Ausgabe: 0.0 (Standardwert)

p.price = 49.99
println(p.price)  // Ausgabe: 49.99

p.price = -10.0   // Ausgabe: "Invalid price"
println(p.price)  // bleibt 49.99
```

- Eigene Getter- und Setter-Methoden überschreiben die automatisch durch den Compiler erzeugten Getter- und Setter-Methoden
- Der Wert der zugrundeliegenden Variable wird in einer mit `**private**` gekennzeichneten Hilfsvariable `**_name**` gespeichert


## 2.8 Methodenüberladung


```
class Product(val id: Int, val name: String, var price: Double) {

  // 1. info ohne Parameter
  def info(): String =
    f"Product[$id]: $name costs $$${price}%.2f"

  // 2. info mit zusätzlichem Rabatt
  def info(discount: Double): String = {
    val discounted = price * (1 - discount / 100)
    f"Product[$id]: $name costs $$${discounted}%.2f after $discount%% discount"
  }

  // 3. info mit Währung
  def info(currencySymbol: String): String =
    f"Product[$id]: $name costs $currencySymbol${price}%.2f"
}
```


```
val p = new Product(101, "Monitor", 199.99)

println(p.info())        
println(p.info(10.0))     
println(p.info("€"))
```


- **Methodenüberladung** (**Method Overloading**) bedeutet, dass mehrere Methoden denselben Namen haben, aber unterschiedliche Parameterlisten besitzen (Typ, Anzahl, Reihenfolge)
- Scala entscheidet beim Aufruf anhand der Signatur, welche Variante gemeint ist
- Unterschiedliche Rückgabetypen bei zwei überladenen Methoden reichen nicht, d.h. die Eingabeparameter müssen sich unterscheiden


## 2.9 Optionen für Methodenparameter

![](image/Pasted%20image%2020250601161305.png)
- Parameter können in Scala auch über geschweifte Klammern übergeben werden, was mehrzeilige Argumente mit Berechnungen ermöglich
- Der Eingabeparameter in der Form `**x:T**` nennt sich **by-value-Parameter** und bedeutet, dass der Wert im Blockausdruck zuerst berechnet und dann an die Methode übergeben wird
- Der Eingabeparameter in der Form `**x: => T**` nennt sich **by-name-Parameter** und bedeutet, dass der Code des Blockausdruckes übergeben wird und der Wert immer dann berechnet wird, wenn die aufnehmende Variable (hier `**message**`) verwendet wird  
- by-name-Parameter ermöglichen eine Lazy- und Mehrfach-Evaluation und sind Funktionen höherer Ordnung sehr ähnlich





# 3 companion Object


- Java: `static`
- Scala: **Companion `object`**

- Scala erlaubt keine statischen Methoden innerhalb von Klassen
- Alternative: Companion-Objekte
- Ein **Companion-Objekt** ist ein Objekt, welches denselben Namen wie eine Klasse trägt und sich in derselben Quelldatei befindet 
- Companion-Klasse und Companion-Objekt könne gegenseitig auf ihre privaten Felder und Methoden zugreifen (aber nicht auf die mit `**private[this]**` gekennzeichneten)
- ==Sie ermöglichen es, gemeinsame Daten und Hilfsmethoden zu definieren, ohne sie in jeder Instanz neu zu speichern oder erzeugen zu müssen==
- Companion-Objekte sind die natürliche Heimat für alles, was logisch zur Klasse gehört, aber nicht an eine bestimmte Instanz gebunden sein soll


```
class Product(val name: String, var price: Double) {
  
  // ID wird automatisch beim Erzeugen geholt
  val id: Int = Product.nextId()   

  def priceWithTax(): Double = {
    price * (1 + Product.taxRate)
  }

  def info(): String = {
    f"Product[$id]: $name costs $$${priceWithTax()}%.2f with tax"
  }
}

object Product {
  private var counter: Int = 0

  val taxRate: Double = 0.19

  // Methode zur Vergabe neuer IDs
  def nextId(): Int = {
    counter += 1
    counter
  }
}

```


## 3.1 warum Companion Objekte

1 Factory Methods (z. B. apply)
Damit kann man Objekte erzeugen, ohne new zu schreiben.
```
class Person(val name: String)

object Person {
  def apply(name: String): Person = new Person(name)
}

val p = Person("Alice") // statt: new Person("Alice")

```
Use Case: Elegantere Syntax, oft genutzt bei Case Classes oder API-Design.

2  Konstanten und Hilfsfunktionen
Companion-Objekte können val, def und import enthalten, die nicht an die Instanz gebunden sind.

```
class Circle(val radius: Double)

object Circle {
  val PI = 3.14159
  def area(c: Circle): Double = PI * c.radius * c.radius
}
```


Use Case: Trennung von Instanzdaten (in der Klasse) und stateless Logik (im Objekt).

3 Pattern Matching (mit unapply)
Companion-Objekte implementieren unapply für Pattern Matching.
```
case class Email(user: String, domain: String)

val e = Email("bob", "mail.com")
e match {
  case Email(u, d) => println(s"user: $u, domain: $d")
}
```

Use Case: Ermöglicht Destrukturierung in match-Ausdrücken.


4 Type-Class Pattern (z. B. Ordering, Show, etc.)
Companion-Objekte enthalten oft implizite Instanzen für Typklassen.

```
object MyType {
  implicit val myOrdering: Ordering[MyType] = ...
}
```

Use Case: Ermöglicht automatische Auswahl von Verhalten basierend auf Typ.


5 Singletons mit Zugriff auf private Klassenmember
Da Klasse und Companion im gleichen Scope liegen, können sie private Members teilen.

```
class Secret(val data: String) {
  private def hidden = "secret"
}

object Secret {
  def reveal(s: Secret): String = s.hidden // Zugriff erlaubt
}


```

Use Case: Ermöglicht gezielten Zugriff auf private API ohne sie öffentlich zu machen.


6 
![](image/Pasted%20image%2020250601175309.png)

## 3.2 例子 

```scala
//1. Erstellen Sie eine Student-Klasse, in der jeder Student einen Namen, ein Alter und eine Studienrichtung hat. Der Primärkonstruktor soll den Namen, das Alter und die  Studienrichtung beim Erstellen eines Objekts erwarten. Die Klassenparameter sollen dabei private val sein.  
//2. Fügen Sie zusätzlich einen Sekundärkonstruktor hinzu, der es erlaubt, einen Student ohne Studienrichtung zu erstellen. In diesem Fall soll die Studienrichtung auf “unknown” gesetzt werden.  
  
//3. Schreibe eine Methode introduce in der Student-Klasse, die eine kurze Vorstellung des Studenten ausgibt, einschließlich des Namens, des Alters und der Studienrichtung. Was ist der Unterschied zwischen einer Methode und einer Funktion in Scala?  
// Eine Methode ist eine Funktion, die an ein Objekt gebunden ist und auf dessen Daten zugreifen kann. Eine Funktion hingegen ist ein eigenständiges Stück Code, das unabhängig von einem Objekt existiert.  
  
//4. Erweitern Sie die Student-Klasse um die Eigenschaft id. Jeder Student sollte beim  Erstellen des Objekts automatisch eine eindeutige ID zugewiesen bekommen. Implementieren Sie eine Lösung, die sicherstellt, dass jede Instanz der Klasse Student eine eindeutige ID erhält.  
  
class Student(val name: String, val age: Int, val studyField: String) {  
  // Primärkonstruktor. It will be automatically generated  
  // private val name: String = name  // private val age: Int = age  // private val studyField: String = studyField  
  
  
  private val id: Int = Student.generateId()  
  
  // Sekundärkonstruktor  
  //def this(name: String, age: Int) = this(name, age, "unknown")  // also correct  def this(name: String, age: Int) = {  
    this(name, age, "unknown")  
  }  
  
  
  // Methode introduce  
  // keine Parameter benotigt in method declarition, weil wir die Parameter der Klasse verwenden. In order to use this method, we need to create an instance of the class.  def introduce(): Unit = {  
    println(s"Hallo, ich heiße $name, bin $age Jahre alt und studiere $studyField. Meine ID ist $id.")  
  }  
}  
  
// ID-Eigenschaft  
// Die ID wird automatisch generiert und ist eindeutig für jede Instanz der Klasse.  
// use companion object to generate the id  
object Student {  
  private var idCounter: Int = 0  
  
  // Methode zur Generierung einer eindeutigen ID  
  private def generateId(): Int = {  
    idCounter += 1  
    idCounter // ID wird um 1 erhöht und zurückgegeben  
  }  
}  
  
  
  
val s1 = new Student("Max", 20, "Informatik")  
val s2 = new Student("Anna", 22)  
  
s1.introduce() // Hallo, ich heiße Max, bin 20 Jahre alt und studiere Informatik.  
s2.introduce() // Hallo, ich heiße Anna, bin 22 Jahre alt und studiere unknown.

```


Einsatzgebiete

| Zweck                             | Beschreibung                                                          | Beispielidee                           |
| --------------------------------- | --------------------------------------------------------------------- | -------------------------------------- |
| Konstanten verwalten              | Gemeinsame, unveränderliche Werte speichern                           | z.B. Steuersätze, Schwellenwerte       |
| Utility-Methoden                  | Methoden, die logisch zur Klasse gehören, aber keine Instanz brauchen | z.B. Preisformate, Validierungen       |
| Zähler, IDs, globale Zustände     | Verwaltung gemeinsamer Ressourcen                                     | z.B. Instanzzähler, Logik für IDs      |
| Erweiterte Konstruktoren          | Mehrere Arten, ein Objekt zu erzeugen                                 | z.B. mit und ohne Default-Werte        |
| Pattern Matching (unapply)        | Companion kann unapply erzeugen für Pattern Matching                  | Case-Class ähnliches Verhalten         |
| Custom Serialization              | Spezielle Methoden zum Konvertieren                                   | z.B. `**toJson**`, `**fromJson**`      |
| Einkapselung von privaten Details | Verstecken von Implementierungsdetails                                | Hilfsmethoden, die die Klasse benötigt |
| Access-Control                    | Private Factory-Methoden erzwingen interne Regeln                     | nur erlaubt Instanzen erzeugen         |


## 3.3 Factory Method: Companion-Objekte und innere Klassen**    

工厂方法是一种用于封装对象创建逻辑的模式。用户不直接通过 new 创建对象，而是通过一个静态方法（Scala 中通常是 apply 方法）来创建。
在中文中，“工厂方法”（Factory Method）是一种常见的设计模式，用于通过一个静态方法来创建类的实例，而不是直接调用构造函数。在你的 Product 类中，apply 方法就相当于工厂方法。


Factory-Methoden

```
class Product(val id: Int, val name: String, var price: Double) {
	
  
	
  def info(): String = f"Product[$id]: $name costs $$${price}%.2f"
	
}
	
 
	
object Product {
	
  private var nextId: Int = 0
	
 
	
  def apply(name: String): Product = {
	
    nextId += 1
	
    new Product(nextId, name, 10.0)
	
  }
	
 
	
  def apply(name: String, price: Double): Product = {
	
    nextId += 1
	
    new Product(nextId, name, price)
	
  }
	
 
	
  def apply(name: String, category: String): Product = {
	
    nextId += 1
	
    val price = category match {
	
      case "Electronics" => 200.0
	
      case "Books"       => 30.0
	
      case "Toys"        => 20.0
	
      case _             => 15.0
	
    }
	
    new Product(nextId, name, price)
	
  }
	
}    how to use factory method  in chienese
```



```
val p1 = Product("玩具熊")  // 使用 apply(name: String)，默认价格为 10.0
println(p1.info())          // 输出: Product[1]: 玩具熊 costs $10.00
```


```
val p2 = Product("电动牙刷", 99.9)  // 使用 apply(name, price)
println(p2.info())                 // 输出: Product[2]: 电动牙刷 costs $99.90
```


## 3.4 什么时候用 new 


在 Scala 中，**你可以用 `new` 创建对象**，但如果定义了伴生对象（`object`）中的 `apply` 方法，**通常推荐用 `apply` 这种“工厂方法”代替 `new`**，因为它更灵活、可读性更高、也更易于扩展。

1️⃣ 你没有定义伴生对象的 apply 方法
```
class User(val name: String)
val u = new User("Alice")  // 必须用 new，因为没有 apply 方法
```

只要你在 object 中定义了 apply 方法（就是你那段代码中的工厂方法），你就可以直接像函数一样调用它，而不用 new：

```
val p = Product("iPad", "Electronics")  // 自动生成 ID 和价格
```

这是因为 Scala 编译器看到你写 Product(...)，它会自动转换成：
```
Product.apply("iPad", "Electronics")
```


----

innere oder eingebettete Klassen
![](image/Pasted%20image%2020250601161739.png)

- Klassen können andere Klassen enthalten (**innere Klassen**, **eingebettete Klassen**, **Nested Classes**)
- Jede Instanz der äußeren Klasse hat dann eine eigene innere Klasse
- Innere Klassen unterschiedlicher Instanzen der äußeren Klasse sind nicht zueinander kompatibel



# 4 Vererbung


![](image/Pasted%20image%2020250601181124.png)

- Vererbung von Klassen erfolgt mit dem Schlüsselwort `extends`, Vererbung von Traits (ähnlich wie Interfaces) über `extends` und `with`
- Wenn eine abstrakte Methode übernommen wurde, muss diese auch in der Subklasse implementiert werden

- Eine Klasse (**Subclass**, **Unterklasse**) kann von maximal einer anderen Klasse (**Superclass**, **Oberklasse**) mit dem Schlüsselwort `**extends**` erben
- Der Konstruktor der vererbenden Klasse muss angegeben werden und wird bei der Instanziierung aufgerufen 
- Die erbende Klasse kann Felder und Methoden der vererbenden Klasse mit `**override**` überschreiben
- Felder und Methoden der Oberklasse können mit `**super**` angesprochen werden (nützlich wenn die erbenden Klasse diese überschreibt)


![](image/Pasted%20image%2020250601161857.png)



## 4.1 Overriding und Dynamic Binding 


- **Dynamic Binding** bezeichnet die Technik, bei der die Methode, die bei einem polymorphen Aufruf ausgeführt wird, erst zur Laufzeit basierend auf dem tatsächlichen Objekttyp ausgewählt und gebunden wird – nicht anhand des statischen Typs der Variablen
- Dynamic Binding gilt für überschriebene Methoden und macht Polymorphie in objektorientierten Sprachen möglich 
- Die Auswahl der konkreten Methode erfolgt zur Laufzeit, nicht beim Kompilieren
- Merke: Dynamic Binding != Method Overloading, denn beim Method Overloading wird die aufzurufende Methode zur Kompilierzeit anhand der Signatur ausgewählt



beim override  kann datatyp und modifier nichtverandern 

```
class Car(override val Id:Int)  extends Veh 


val 和 int 必须和 Veh 相一致 
```


```
class Product {
  def info(): String = "Generic product"
}

class DiscountedProduct extends Product {
  override def info(): String = "Discounted product"
}

val x: Product = if (scala.util.Random.nextBoolean()) new Product() else new DiscountedProduct()
println(x.info())
```


# 5 Warum kann man var nicht überschreiben?


![](image/Pasted%20image%2020250601181523.png)

![](image/Pasted%20image%2020250601181538.png)





# 6 Abstrakte Klassen

- Eine **abstrakte Klasse** ist nicht vollständig implementiert, sie ist eine unvollständige Bauanleitung für Objekte
- Sie kann abstrakte Felder und Methoden enthalten, und sie kann nicht direkt instanziiert werden
- Sie dient als Basisklasse für konkrete Unterklassen, welche die offenen Teile ausfüllen
- Sie wird verwendet, um zu erzwingen, dass bestimmte Details von den Unterklassen festgelegt werden
- Abstrakte Klassen werden in Scala durch `**abstract**` explizit kenntlich gemacht
- Die erbenden Unterklassen müssen die abstrakten Elemente implementieren


- können **nicht instanziiert** werden.
- enthalten mindestens **eine körperlose Methode** (ohne Implementierung)
- eine Klasse, die von einer abstrakten Klasse erbt, **muss** alle abstrakten Methoden implementieren (überschreiben).
- bereits implementierte Methoden in der abstrakten Klasse **können** überschrieben werden, **müssen aber nicht**.


![](image/Pasted%20image%2020250521143535.png)

![](image/Pasted%20image%2020250601181223.png)




![](image/Pasted%20image%2020250601162304.png)

```
abstract class Car {
  val year: Int
  val automatic: Boolean = false
  def colour: String
}
class BMW(val year: Int) extends Car {
  def colour: String = "red"
  override val automatic: Boolean = true
}
```

- die Methode `colour` **muss** überschrieben werden, da sie abstrakt ist.
- das Attribut `automatic` **kann** überschrieben werden.
- der Konstruktor `val year: Int` übernimmt die Implementierung des abstrakten `val` in Car.


```scala
abstract class Car {
	val year: Int // abstraktes Feld - MUSS überschrieben werden
	val automatic: Boolean = true // konkretes Feld - kann überschrieben werden
	def color: String // abstrakte Methode
} 

也可以这样定义 
abstract class FamilyMember(val name: String, val birthDate: String, val placeOfBirth: String)


但是 trait 中这样 () 中 定义 felder Jiubuxingle1 , 必须用 {}



class RedMini(val year: Int) extends Car {
	def color = "Red" // Implementierung der abstrakten Methode aus Car
}


var auto2: RedMini = new RedMini(2006)
// Es geht nicht auto2 = new BlueMini(2000)

var auto: Car = new RedMini(2005)
auto1 = new BlueMini(2000)

auto1 = new Car () // 是不对的   会报错

```




# 7 Traits


![](image/Pasted%20image%2020250521144152.png)

![](image/Pasted%20image%2020250601181236.png)


![](image/Pasted%20image%2020250601181400.png)


Problem: keine Mehrfachvererbung
Lösung: Traits
-4 Java Äquivalent: Interfaces
• Traits haben abstrakte und nicht-abstrakte Methoden und Variablen
• Wird von einer Klasse und von Traits geerbt, muss die Klasse zuerst benanot werden
• Erbende Klasse muss alle abstrakten Methoden des Traits implementieren und alle abstrakten Variablen instanziierem
• Nicht-abstrakte Methoden und Variablen des Traits können in der erbenden Klasse mit override überschrieben werden
Nutzung:
• Traits werden zum Vererben mit extends an die erbende Klasse angehängt - mehrere Traits werden mit dem Schlüsselwort with voneinander getrennt
• Vererbender Trait Wird bei der Instanziierung des Objekts mit with angegeben


---

- Ein **Trait** ist einer wiederverwendbare, teilabstrakte Komponente, die Eigenschaften (z.B. `**val**`/`**var**`) und/oder Verhalten (z.B. `**def**`) kapselt und von Klassen mit `**with**` vererbt bzw. mixin-artig eingebunden werden kann  
- Traits sind vergleichbar mit Interfaces in Java, können aber Implementierungen enthalten (also Methoden im Rumpf) und sowohl Felder als auch Methoden definieren
- Traits können mit Klassen kombiniert und von anderen Traits erweitert werden
- Die abstrakten Felder und Methoden eines Traits müssen von der aufnehmenden Klasse implementiert werden 
- Eine Klasse kann mehrere Traits gleichzeitig verwenden, **Mehrfachvererbung** über Traits ist erlaubt

## 7.1 例子 

```
trait Friend {
  val name: String
  def isFriend() = println(s"Your friend is $name")
}

trait Colleague {
  val name: String
  def isColleague() = println(s"Your colleague is $name")
}

class Human(val name: String) extends Colleague with Friend


class Woman(override val name: String) extends Human(name)

val carla = new Woman("Carla")

carla.isFriend() //Your friend is Carla
carla.isColleague() //Your colleague is Carla



class Tutor(val name:String) extends Friend

val loris = new Tutor("Loris") with Colleague //trait mixing during instance creation

loris.isFriend() //Your friend is Loris
loris.isColleague() //Your colleague is Loris
```



## 7.2 `trait` vs `interface` 区别

|特性|Scala `trait`|Java `interface`（尤其是 Java 8+）|
|---|---|---|
|是否允许实现方法|✅ 是的（可以包含方法实现）|✅ Java 8+ 支持 default 方法实现|
|是否允许定义字段（属性）|✅ 可以包含 `val` 或 `var`|⛔ 不允许定义字段，只能有常量 (`static final`)|
|是否可以有构造代码块|✅ 可以有|⛔ 不可以|
|多继承支持|✅ 一个类可以继承多个 trait|✅ 一个类可以实现多个 interface|
|继承方式|`extends`（第一个），`with`（后续）|`implements`|
|是否可以被实例化|⛔ 不可以|⛔ 不可以|
|是否可以带状态|✅ 可以存储状态（有变量）|⛔ 不建议，设计上应无状态|


在 Scala 中，`trait` 可以直接写构造代码块。这些代码会在**类混入（mixin）该 trait 的时候执行**。也就是说，`trait` 不只是定义接口，还可以在混入时执行一些初始化行为。

```
trait Logger {
  println("Logger trait initialized")  // 这是构造代码块

  def log(message: String): Unit = println(s"LOG: $message")
}

class MyApp extends Logger {
  println("MyApp class initialized")
}

val app = new MyApp()
// 输出：
// Logger trait initialized
// MyApp class initialized

```


Java `interface` 不允许有构造代码块
Java 的接口中**不能**包含构造代码或初始化代码块：
```
public interface Logger {
    // static initializer 是允许的，但不能像类或 Scala trait 那样用于实例初始化
    // 以下写法是非法的：
    // {
    //     System.out.println("This is not allowed in Java interface");
    // }

    default void log(String msg) {
        System.out.println("LOG: " + msg);
    }
}

```


Java 接口中：
- 不允许有实例构造代码块。
- 只能有 `default` 或 `static` 方法（从 Java 8 起）。    
- 可以有 `static` 初始化代码，但仅限于静态上下文，不针对实例。


## 7.3 Traits als Klassenbeimischungen


```
trait Physical {
  val weight: Double

  def shippingCost: Double =
    5.0 + 0.1 * weight
}

trait Digital {
  val fileSizeMB: Int

  def downloadTime(mbps: Double): Double =
    fileSizeMB * 8 / (mbps * 60)
}
```


![](image/Pasted%20image%2020250514105809.png)



Ein Trait ist einer wiederverwendbare, teilabstrakte Komponente, die Eigenschaften (z.B. val/var) und/oder Verhalten (z.B. def) kapselt und von Klassen mit with vererbt bzw. mixin-artig eingebunden werden kann  
Traits sind vergleichbar mit Interfaces in Java, können aber Implementierungen enthalten (also Methoden im Rumpf) und sowohl Felder als auch Methoden definieren
Traits können mit Klassen kombiniert und von anderen Traits erweitert werden
Die abstrakten Felder und Methoden eines Traits müssen von der aufnehmenden Klasse implementiert werden 
Eine Klasse kann mehrere Traits gleichzeitig verwenden, Mehrfachvererbung über Traits ist erlaubt


```
class Product(val id: String, val name: String, val price: Double)

class TShirt(
  id: String,
  name: String,
  price: Double,
  val weight: Double
) extends Product(id, name, price) with Physical

class Book(
  id: String,
  name: String,
  price: Double,
  val weight: Double,
  val fileSizeMB: Int
) extends Product(id, name, price) with Physical with Digital

class Software(
  id: String,
  name: String,
  price: Double,
  val weight: Double,
  val fileSizeMB: Int
) extends Product(id, name, price) with Physical with Digital

class Movie(
  id: String,
  name: String,
  price: Double,
  val fileSizeMB: Int
) extends Product(id, name, price) with Digital

```


```
val tshirt = new TShirt("T001", "Scala Shirt", 29.99, 0.5)
println(tshirt.shippingCost) // 5.05 EUR

val book = new Book("B001", "Functional Programming", 49.99, 1.2, 500)
println(book.shippingCost)   // 5.12 EUR
println(book.downloadTime(50)) // Downloadzeit bei 50 Mbps

val software = new Software("S001", "IDE Tool", 89.99, 0.3, 800)
println(software.shippingCost)  // 5.03 EUR
println(software.downloadTime(100)) // Downloadzeit bei 100 Mbps

val movie = new Movie("M001", "Epic Movie", 14.99, 1200)
println(movie.downloadTime(75)) // Downloadzeit bei 75 Mbps
```


## 7.4 Traits als Objektbeimischungen


![](image/Pasted%20image%2020250514110426.png)





# 8 Class Design: Generic 


- **Generics** sind ein Sprachfeature in Scala und Java (und anderen Sprachen), das es ermöglicht, Klassen, Methoden oder Traits so zu definieren, dass sie mit verschiedenen Typen arbeiten können
- So wie eine Methode Werte als Parameter bekommen kann, bekommt eine generische Klasse oder Methoden Typ-Parameter
- Typ-Parameter werden durch Großbuchstaben in eckigen Klammern repräsentiert, die immer vor runden oder geschweiften Klammern stehen
- ==Mit Hilfe von Generics schreibt man Code nur einmal und verwendet ihn für viele Typen – mit voller Typsicherheit, die bereits beim Kompilieren geprüft wird==


![](image/Pasted%20image%2020250601172502.png)

![](image/Pasted%20image%2020250514110947.png)


![](image/Pasted%20image%2020250514110958.png)


----

```scala
import java.time.LocalDate

// Oberklasse
abstract class Product(val name: String)

// Lebensmittel mit Ablaufdatum
class FreshFood(name: String, val expiryDate: LocalDate) extends Product(name)
class Fruit(name: String, expiryDate: LocalDate) extends FreshFood(name, expiryDate)
class Apple(name: String, expiryDate: LocalDate) extends Fruit(name, expiryDate)

// Non-Food mit Sicherheitskennzeichnung
class NonFood(name: String, val dangerousForChildren: Boolean) extends Product(name)
class CleaningProduct(name: String, dangerousForChildren: Boolean) extends NonFood(name, dangerousForChildren)
```

![](image/Pasted%20image%2020250514111016.png)



## 8.1 Typ-schranken


- werden eingesetzt, um Typparameter auf bestimmte Typbereiche einzuschränken
- besonders nützlich bei Vererbungshierarchien:

- **Typ-Schranken** schränken die möglichen Datentypen eines Typ-Parameters ein
- `**A<:B**` bedeutet, dass `**B**` eine **obere Schranke** (**Upper Bound**) ist und dass `**A**` vom Typ `**B**` oder ein Subtyp von `**B**` sein muss
- `**A>:B**` bedeutet, dass `**B**` eine **untere Schranke** (**Lower Bound**) ist und dass `**A**` vom Typ `**B**` oder ein Supertyp von `**B**` sein muss
- Typ-Schranken werden direkt beim Typ-Parameter einer Klasse oder eines Traits oder in Methodensignaturen gesetzt

![](image/Pasted%20image%2020250601172550.png)

![](image/Pasted%20image%2020250601172559.png)


---


```
class Shelf[T] {
	
  private var items: List[T] = List()
	
  def add(item: T): Unit = {
    items = item :: items
    println(s"Added: ${item.name}")
	
  }
	
 
	
  def listItems(): Unit = {
    println("Shelf contains:")
    items.foreach(p => println(p.name))
  }

}
```


```
class Shelf[T <: Product] {
  private var items: List[T] = List()

  def add(item: T): Unit = {
    items = item :: items
    println(s"Added: ${item.name}")
  }

  def listItems(): Unit = {
    println("Shelf contains:")
    items.foreach(p => println(p.name))
  }

  def moveTo[U >: T <: Product](target: Shelf[U]): Unit = {
    for (item <- items) {
      target.add(item)
    }
    items = List()
    println("All items moved; shelf is now empty.")
  }
}
```



---

Wozu benötige ich Upper Bounds?
- Um sicherzustellen, dass in einer generischen Klasse oder Methode auf Felder zugegriffen werden kann, die für eine bestimmte Superklasse garantiert sind
- Der Typ-Parameter garantiert, dass jedes konkrete Objekt, das an die generische Klasse oder Methode übergeben wird, mindestens die Methoden und Felder dieser Superklasse hat 
- Beispiel: `**class Shelf[T <: Product]**`
- Das Regal ist generisch, aber nur für Typen, die `**Product**` oder spezifischer sind – dadurch weiß man beim Zugriff innerhalb der Klasse: "Alle Produkte haben mindestens ein `**name**`-Feld", weil `**Product**` das vorgibt


Wozu benötige ich Lower Bounds?
- Um sicherzustellen, dass in einer Methode etwas in einen Container oder eine externe Struktur hineingeschrieben werden kann, die allgemein genug ist, um den konkrete Wert aufzunehmen
- Der Lower Bound stellt sicher, dass das Zielobjekt allgemein genug ist, damit dort der Wert eines konkreten Typs sicher abgelegt werden kann
- Beispiel: `**def moveTo[U >: T <: Product](target: Shelf[U]): Unit**`
- Das Produkt wird aus einem Regal in ein anderes Regal verschoben – damit das klappt, muss das andere Regal mindestens so allgemein sein wie das ursprüngliche


![](image/Pasted%20image%2020250514113056.png)



## 8.2 Typ-Varianz

In Scala gibt es drei Arten von Typ-Varianzen, die bestimmen, **wie sich Subtyp-Beziehungen von Typen auf generische Klassen übertragen**.

|变型类型|Scala 写法|子类型关系举例|用于|
|---|---|---|---|
|协变 Covariant|`+A`|`Box[Cat] <: Box[Animal]`|输出（返回值）|
|逆变 Contravariant|`-A`|`Handler[Animal] <: Handler[Cat]`|输入（函数参数）|
|不变 Invariant|`A`|无子类关系|可变/完全匹配场景|

![](image/Pasted%20image%2020250601172646.png)

![](image/Pasted%20image%2020250601181852.png)


在 Scala 中，“**协变（Covariance）**”、“**逆变（Contravariance）**” 和 “**不变（Invariance）**” 是泛型类型参数的三种变型方式，它们用来控制泛型在 **子类型关系中的行为**。以下是中文解释：



Es findet **keine** Übertragung der Subtyp-Beziehung statt:  
`Container[A]` und `Container[B]` sind **unabhängig**, auch wenn `A <: B` gilt.  
Ist der **Default**, wenn keine Varianz-Angabe gemacht wird

```
// Keine Subtyp-Beziehung: Box[Dog] und Box[Animal] sind inkompatibel, auch wenn Dog <: Animal
class Box[T](var inhalt: T)
val dogBox: Box[Dog] = new Box(new Dog)
// val animalBox: Box[Animal] = dogBox  // nicht erlaubt
```

![](image/Pasted%20image%2020250514113637.png)


```
class Shelf[T](items: List[T]) {
  def getItems: List[T] = items
}
val productShelf: Shelf[Product] = new Shelf[Book](List(new Book("Scala", "Alice"))) //Fehler
```

- Standardmäßig ist Scala in Bezug auf Generics invariant
- **Invarianz** bedeutet, dass ein generischer Typ `**Shelf[T]**` nur genau für `**T**` gültig ist, nicht für Sub- oder Supertypen
- Es gibt keine Beziehung zwischen `**Shelf[Book]**` und `**Shelf[Product]**`, auch dann nicht, wenn `**Book <: Product**` ist
- Invarianz stellt sicher, dass ein `**Shelf[Book]**` nur dort verwendet wird, wo exakt ein `**Shelf[Book]**` verlangt wird
- Das gibt maximale Typsicherheit, erlaubt aber weniger Flexibilität



---



![](image/Pasted%20image%2020250514113648.png)

` Box[Cat] 中提取一个 cat 可以放到 Box[Animal] 里面去. Box[Animal] 中提取一个值 不能放到 Box[Cat]  中`

→ Nur lesende Operationen erlaubt! (Listen sind immutable!)

```
class Shelf[+T](items: List[T]) {
  def produce: T = items.head
}
val productShelf: Shelf[Product] = new Shelf[Book](List(new Book("Scala", "Alice")))
```


- **Kovarianz** bedeutet, dass ein generischer Typ `**Shelf[+T]**` die Subtyp-Beziehung von `**T**` übernimmt
- Wenn `**Book <: Product**`, dann gilt auch `**Shelf[Book] <: Shelf[Product]**`
- Dies erlaubt, dass ein Regal mit Büchern auch als Regal mit Produkten verwendet kann
- Aber: Kovarianz (`**+T**`) erlaubt nur lesende (produzierende) Methoden, z.B. kann eine Waschmaschine nicht in ein Bücherregal gelegt werden
- Man darf aus einem `**Shelf[Book]**` über eine Variable vom Typ `**Shelf[Product]**` Produkte lesen, aber keine Produkte hineinlegen (nicht typsicher, da `**Shelf[Book]**` nur Bücher erlaubt, aber `**Shelf[Product]**` alle Produkte annimmt)
- Kovarianz erlaubt die Verwendung von spezialisierten Typen (`**Shelf[Book]**`) überall dort wo allgemeinere Typen (`**Shelf[Product]**`) erwartet werden, was typisch für immutable Collections oder lesende Strukturen ist


![](image/Pasted%20image%2020250601163511.png)


---

![](image/Pasted%20image%2020250514113732.png)

` Box[Animal] 中提取一个值 animal 可以放到 Box[Cat] 里面去. Box[Cat] 中提取一个值 不能放到 Box[Animal] 中`


![](image/Pasted%20image%2020250601163955.png)


```
class Shelf[-T] {
  def consume(item: T): Unit = println(s"Consumed: $item")
}
val bookShelf: Shelf[Book] = new Shelf[Product]()
```



- **Kontravarianz** bedeutet, dass ein generischer Typ `**Shelf[-T]**` die Subtyp-Beziehung von `**T**` umdreht
- Wenn `**Book <: Product**`, dann gilt `**Shelf[Product] <: Shelf[Book]**`
- Dies erlaubt, dass ein allgemeines Regal für Produkte auch als Regal für Bücher verwendet kann
- Aber: Kontravarianz (`**-T**`) erlaubt nur schreibende (konsumierende) Methoden, z.B. kann eine Waschmaschine nicht aus einem Bücherregal geholt werden
- Man darf über eine Variable vom Typ `**Shelf[Book]**` Bücher in ein `**Shelf[Product]**` legen, aber keine Bücher aus diesem Regal lesen (nicht typsicher, da `**Shelf[Product]**` auch Dinge enthält, die keine Bücher sind)
- Kontravarianz erlaubt die Verwendung von allgemeinen Typen (`**Shelf[Product]**`) auch dort, wo ein spezifischer Typ (`**Shelf[Book]**`) erwartet wird, was typisch für Handler, Listener oder Consumer ist, die nur verarbeiten

→ Nur schreibende Operationen erlaubt!
→ Kontravarianz erlaubt die Verwendung von allgemeinen Typen` (Container[Animal]) `auch dort, wo ein spezifischer Typ `(Container[Dog]) `erwartet wird, was typisch für Handler, Listener oder Consumer ist, die nur verarbeiten


### 8.2.1 Kontravarianz (-T) ist nur sinnvoll bei Konsumenten

Ein Typ ist kontravariant in T, wenn er Werte vom Typ T konsumiert (z. B. als Methodenparameter) – aber keine Werte vom Typ T zurückgibt oder speichert, die später typisiert weiterverwendet werden könnten.

Kontravarianz (-T) ist nützlich für „Konsumenten von T“ – Dinge, die T verarbeiten, aber nicht zurückgeben oder speichern.


Intuition: „Ich akzeptiere allgemeinere Typen“
![](image/Pasted%20image%2020250601182216.png)

![](image/Pasted%20image%2020250601182229.png)



### 8.2.2 例子 

![](image/Pasted%20image%2020250601164242.png)

![](image/Pasted%20image%2020250601164256.png)


![](image/Pasted%20image%2020250601164301.png)


## 8.3 Verwendungsseitige Varianz in Java

- Scala basiert auf **deklarationsseitiger Varianz** (**Declaration-Site Variance**), d.h. die Varianz wird direkt an der Klasse oder am Typ-Parameter festgelegt
- Die Varianz-Regeln gelten überall, sind aber weniger flexibel, wenn man verschiedene Varianz-Anforderungen hat
- Java basiert auf **verwendungsseitiger Varianz** (**Use-Site Variance**), d.h. die Varianz wird mit Wildcards erst dort festgelegt, wo der Typ verwendet wird
- Sehr flexibel, aber mehr Komplexität an der Verwendungsstelle


![](image/Pasted%20image%2020250601164359.png)

