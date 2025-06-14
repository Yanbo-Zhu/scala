

Product Types (UND-Typen)
```scala
case class CreditCard(number: String, holder: String, cvv: String)
```


SUM Types (oder-Typen)
```scala
sealed trait PaymentMethod
case class CreditCard(number: String, holder: String, cvv: String) extends PaymentMethod
case class PayPal(email: String) extends PaymentMethod
case object CashOnDelivery extends PaymentMethod
```


```scala
PaymentMethod = 
  (String × String × String)  // CreditCard
+ (String)                    // PayPal
+ 1                           // CashOnDelivery (kein Inhalt)
```


- **Algebraische Datentypen** (**ADTs**) sind ein grundlegendes Konzept der funktionalen Programmierung
- Sie ermöglichen die Definition eigener Datenstrukturen, indem einfache Typen miteinander kombiniert werden
- Dadurch lassen sich komplexe Daten auf strukturierte und typsichere Weise darstellen, was zu besser lesbarem und wartbarem Code führt
- ADTs sind Typen, die aus Summe und Produkt von Typen bestehen
- Große Vorteile hinsichtlich Kompaktheit und Ausdrucksstärke ergeben sich aus der Kombination von ADTs und Pattern Matching

- **代数数据类型（ADTs）** 是函数式编程中的一个基本概念。
- 它们通过**组合简单类型**，使我们能够定义**自定义的数据结构**。
- 这使得我们可以**以结构化且类型安全的方式表示复杂数据**，从而编写出更易读、更易维护的代码。
- ADTs 是由**类型的“和”（Sum）与“积”（Product）** 组合而成的类型。
- 将 ADTs 与 **模式匹配（Pattern Matching）** 结合使用，能带来高度的**简洁性与表达力**


- ADTs sind komponierte Typen, mit denen sich Datenstrukturen präzise und typsicher beschreiben lassen
- Sie bestehen aus UND- und ODER-Typen
- **UND-Typen** kombinieren mehrere Typen in einem Typ, z.B. einer `**case class**` mit mehreren Feldern
- Ein **ODER- Typ** hat eine von mehreren Varianten - z.B. Zustände
- Bemerkung: `**sealed**` bedeutet, dass alle direkten Untertypen einer `**sealed**`-Klasse oder eines `**sealed**`-Trait in derselben Datei definiert sein müssen
- Mit `**sealed**` ist sichergestellt, dass der Compiler alle Untertypen kennt und nicht noch anderswo im Code neue Untertypen auftauchen
- - **ADTs 是组合类型**，可以用来**精确且类型安全地描述数据结构**。
- 它们由 **“与”（AND）类型** 和 **“或”（OR）类型** 组成。

![](image/Pasted%20image%2020250614182916.png)

# 1 关于 `sealed`

- `sealed`（封闭的）表示：
    
    - 所有直接子类型**必须在同一个文件中定义**。
        
- 好处：
    
    - **编译器可以知道所有的可能类型**，这对 `match` 表达式很重要，因为它能检查是否已经匹配了所有分支。
        
    - 防止其他文件中随意添加新的子类型，增强了类型系统的安全性


# 2 Beispiel: Zalhungsmethoden

ADT
```scala
sealed trait PaymentMethod
case class CreditCard(number: String, holder: String, cvv: String) extends PaymentMethod
case class PayPal(email: String) extends PaymentMethod
case object CashOnDelivery extends PaymentMethod

def processPayment(method: PaymentMethod): String = method match {
  case CreditCard(number, holder, _) =>
    s"Charging credit card $number (Holder: $holder)"
  case PayPal(email) =>
    s"Redirecting to PayPal for $email"
  case CashOnDelivery =>
    "Marking as cash on delivery"
}
```


```scala
val p1: PaymentMethod = CreditCard("1234-5678-9876-5432", "Alice Example", "123")
val p2: PaymentMethod = PayPal("alice@example.com")
val p3: PaymentMethod = CashOnDelivery

println(processPayment(p1))
println(processPayment(p2))
println(processPayment(p3))
```



Klassischer Stil
```scala
class PaymentMethod(
  val methodType: String,       // "credit", "paypal", "cod"
  val number: String = "",
  val holder: String = "",
  val cvv: String = "",
  val email: String = ""
)

def processPayment(method: PaymentMethod): String = {
  method.methodType match {
    case "credit" =>
      s"Charging credit card ${method.number} (Holder: ${method.holder})"
    case "paypal" =>
      s"Redirecting to PayPal for ${method.email}"
    case "cod" =>
      "Marking as cash on delivery"
    case _ =>
      "Unknown payment method"
  }
}
```
