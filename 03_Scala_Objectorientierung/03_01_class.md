

# 1 object 和 class 的区别


|特性|`class`|`object`|
|---|---|---|
|实例数量|可创建多个实例|只能有一个（单例）|
|实例化方式|需要 `new`|不需要 `new`|
|状态|每个实例独立|全局唯一状态|
|主要用途|定义对象模板|定义工具类/静态方法/伴生对象|
|示例|`new MyClass()`|`MyObject.myMethod()`|

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


# 3 例子 


## 3.1 companion Object

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