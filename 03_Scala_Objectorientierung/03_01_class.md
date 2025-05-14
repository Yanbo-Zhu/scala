

# 1 object 和 class 的区别


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




# 2 Argument 默认是 val 还是 var  in class definition

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


## 2.1 var und val 

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


# 3 companion Object


## 3.1 例子 

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



## 3.2 **Companion-Objekte und innere Klassen**    

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



## 3.3 什么时候用 new 


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




# 4 Vererbung und Traits



## 4.1 Traits

✅ 总览：`trait` vs `interface` 区别

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


## 4.2 Traits als Klassenbeimischungen


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


## 4.3 Traits als Objektbeimischungen


![](image/Pasted%20image%2020250514110426.png)


# 5 Class Design: Generic 


- **Generics** sind ein Sprachfeature in Scala und Java (und anderen Sprachen), das es ermöglicht, Klassen, Methoden oder Traits so zu definieren, dass sie mit verschiedenen Typen arbeiten können
- So wie eine Methode Werte als Parameter bekommen kann, bekommt eine generische Klasse oder Methoden Typ-Parameter
- Typ-Parameter werden durch Großbuchstaben in eckigen Klammern repräsentiert, die immer vor runden oder geschweiften Klammern stehen
- Mit Hilfe von Generics schreibt man Code nur einmal und verwendet ihn für viele Typen – mit voller Typsicherheit, die bereits beim Kompilieren geprüft wird

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



## 5.1 Typ-schranken

- **Typ-Schranken** schränken die möglichen Datentypen eines Typ-Parameters ein
- `**A<:B**` bedeutet, dass `**B**` eine **obere Schranke** (**Upper Bound**) ist und dass `**A**` vom Typ `**B**` oder ein Subtyp von `**B**` sein muss
- `**A>:B**` bedeutet, dass `**B**` eine **untere Schranke** (**Lower Bound**) ist und dass `**A**` vom Typ `**B**` oder ein Supertyp von `**B**` sein muss
- Typ-Schranken werden direkt beim Typ-Parameter einer Klasse oder eines Traits oder in Methodensignaturen gesetzt

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



## 5.2 Typ-Varianz


课上的例子 


book 和 waschinemachine 都是 product 的 subclass, 
book 和 waschinemachine 直接没有关系 

---


|变型类型|Scala 写法|子类型关系举例|用于|
|---|---|---|---|
|协变 Covariant|`+A`|`Box[Cat] <: Box[Animal]`|输出（返回值）|
|逆变 Contravariant|`-A`|`Handler[Animal] <: Handler[Cat]`|输入（函数参数）|
|不变 Invariant|`A`|无子类关系|可变/完全匹配场景|




在 Scala 中，“**协变（Covariance）**”、“**逆变（Contravariance）**” 和 “**不变（Invariance）**” 是泛型类型参数的三种变型方式，它们用来控制泛型在 **子类型关系中的行为**。以下是中文解释：


![](image/Pasted%20image%2020250514113637.png)


![](image/Pasted%20image%2020250514113648.png)



![](image/Pasted%20image%2020250514113732.png)


