

# 1 Totale vs partiell Definierte Funktionen


Totale Funktion
```
val describeDay: String => String = day => day match {
  case "Mon" => "Start of the week"
  case "Tue" => "Second day"
  case "Wed" => "Midweek"
  case "Thu" => "Almost there"
  case "Fri" => "Last workday"
  case "Sat" => "Weekend"
  case "Sun" => "Weekend"
  case _     => "Unknown day"
}
```

- Eine **totale Funktion** ist für alle Eingabewerte ihres Typs definiert
- Die obige Funktion ist total, weil sie alle denkbaren Eingaben, die nicht über spezielle "Cases" abgedeckt sind, mit einem `**case _**` abdeckt


Partiell DEfinierte Funktion
```
val shortWeekdayNameToGerman: String => String = day => day match {
  case "Mon" => "Montag"
  case "Tue" => "Dienstag"
  case "Wed" => "Mittwoch"
}
```

- Eine **partiell definierte Funktion** ist eine Funktion `**f: A => B**`, die nur für bestimmte Werte aus dem Typ `**A**` einen Rückgabewert vom Typ `**B**` liefert
- Für andere Werte ist sie nicht definiert und wirft typischerweise eine Ausnahme, oder wird in einem sicheren Kontext nur auf gültige Werte angewendet
- Wenn man unvollständige `**match**`-Ausdrücke als normale Funktion schreibt, hat man keine Kontrolle, ob sie mit einem ungültigen Wert aufgerufen werden  
- Besser: In Scala kann man partiell definierte Funktionen mit `**PartialFunction[A, B]**` ausdrücken

一 个 部分定义的函数（Partial Function）是一个函数 **f: A => B**，它只对某些特定的 A 类型的输入值有定义，即只对一部分输入给出一个 B 类型的返回值。
✅ 主要特点：

对于其他不在定义域内的输入值，这个函数就没有定义：
    通常会抛出异常（如 MatchError），
    或者在受控环境下，只对有效值调用（例如通过 isDefinedAt 检查）


|项目|普通函数 `A => B`|部分函数 `PartialFunction[A, B]`|
|---|---|---|
|定义域|所有 A 类型值|A 类型的**部分值**|
|不在定义域的处理方式|抛出异常|可以提前检查 `isDefinedAt`|
|模式匹配支持|不完全匹配会出问题|天生支持 `case` + 匹配语句|
|更安全的函数定义|❌|✅|


---

 Die Inhalte einer partiell definierten Funktion

```scala
val weekdayToGerman: PartialFunction[String, String] =
  new PartialFunction[String, String] {

    def isDefinedAt(day: String): Boolean = day == "Mon" || day == "Tue" || day == "Wed"

    def apply(day: String): String = day match {
      case "Mon" => "Montag"
      case "Tue" => "Dienstag"
      case "Wed" => "Mittwoch"
    }
  }


weekdayToGerman.isDefinedAt("Tue")
weekdayToGerman("Tue")


weekdayToGerman.isDefinedAt("Fri")
weekdayToGerman("Fri")    

```

- Eine `**PartialFunction[A, B]**` ist nur für einen Teil der Eingabewerte vom Typ `**A**` definiert
- Sie besteht aus zwei zentralen Methoden
- `**isDefined(x: A): boolean**` ergibt `**true**`, wenn die Funktion für `**x**` definiert ist
- `**apply(x: A): B**` führt die Funktion aus, sollte aber nur dann aufgerufen werden, wenn `**isDefined**` zuvor `**true**` ergibt

# 2 Partialfunction[-A, +B]

```
val weekdayToGerman: PartialFunction[String, String] =
  new PartialFunction[String, String] {

    def isDefinedAt(day: String): Boolean = day == "Mon" || day == "Tue" || day == "Wed"

    def apply(day: String): String = day match {
      case "Mon" => "Montag"
      case "Tue" => "Dienstag"
      case "Wed" => "Mittwoch"
    }
  }

```


Verkürzte Schreibweise (Syntaktischer Zucker)
```
val weekdayToGerman: PartialFunction[String, String] = {
  case "Mon" => "Montag"
  case "Tue" => "Dienstag"
  case "Wed" => "Mittwoch"
}
```

- Scala erlaubt eine kompakte Definition partiell definierter Funktionen mit `**case**`-Blöcken in geschweiften Klammern
- Die Schreibweise erzeugt implizit ein Objekt, das den Trait `**PartialFunction**` implementiert 
- Der Compiler leitet automatisch die beiden Methoden `**isDefinedAt**` und `**apply**` ab
- Achtung: Die Kurzform funktioniert nur für `**PartialFunction**`, nicht für normale Funktionen `**A=>B**`


Scala 允许使用 `case` 代码块和花括号 `{}` **简洁地定义部分函数（partiell definierte Funktionen）**。 
- 这种写法会**自动生成一个对象**，它实现了 `PartialFunction` 特质（Trait）。
- 编译器会自动推导出两个方法：`isDefinedAt`（判断是否匹配） 和 `apply`（应用函数）。
- ⚠️ 注意：这种简写形式**只适用于 `PartialFunction`**，**不能用于普通函数类型 `A => B`**。


# 3 Anwendung von PartialFunction


mit PartialFunction
```
val weekdayToGerman: PartialFunction[String, String] = {
  case "Mon" => "Montag"
  case "Tue" => "Dienstag"
  case "Wed" => "Mittwoch"
}
val inputs = List("Mon", "Tue", "Fri", "Sun")
val results = inputs.collect(weekdayToGerman)
```

Ohne
```
val results = inputs.flatMap(day =>
  if (day == "Mon") Some("Montag")
  else if (day == "Tue") Some("Dienstag")
  else if (day == "Wed") Some("Mittwoch")
  else None
)
```


- Die Überprüfung mit `**isDefinedAt**` vor dem Aufruf von `**apply**` scheint umständlich
- Allerdings wird `**PartialFunction**` sehr häufig im Kontext von Funktionen höherer Ordnung genutzt, die mit `**isDefinedAt**` zuvor intern die Typsicherheit überprüfen
- Beispiel: `**collect**` wendet eine `**PartialFunction**` nur auf die Elemente an, für die sie definiert ist – also bei denen `**isDefinedAt**` `**true**` ergibt – die übrigen Elemente werden einfach ausgelassen

