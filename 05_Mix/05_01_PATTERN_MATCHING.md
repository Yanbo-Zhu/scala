

# 1 Match-ausdrücke

```
val day = "Monday"

val message = day match {
  case "Monday"    => "Start of the week"
  case "Friday"    => "Almost weekend"
  case "Saturday" | "Sunday" => "Weekend!"
  case _           => "Midweek day"
}


val day = "Monday"

val message = day match {
  case "Monday"    => "Start of the week"
  case "Friday"    => "Almost weekend"
  case "Saturday" | "Sunday" => "Weekend!"
  case _           => "Midweek day"
}


```


- Ein `**match**`-Ausdruck ist ein Ausdruck zur Fallunterscheidung, der einen Wert gegen eine Reihe von Mustern prüft
- Der `**match**`-Ausdruck besteht aus
    - der Ausdrucksquelle, welche den Wert oder Ausdruck, der gematcht werden soll, enthält,
    - dem Schlüsselwort `**match**`,
    - einem oder mehreren `**case**`-Zweigen, die aus dem Muster und einem Rumpf bestehen, der einen Ausdruck darstellt und
    - einem Rückgabewert, basierend auf dem Rumpf des zutreffenden **`case`**
- Der `**_**`-case beschreibt den Standardfall, der ausgeführt wird, wenn kein anderes Muster zutrifft
- Es kann nur ein Muster matchen, d.h. die Berechnung des match-Ausdrucks ist bei einem Treffer beendet und nachfolgende Muster werden nicht mehr geprüft


Wildcard Pattern
- Das **Wildcard Pattern** ist ein Platzhalter `**_**` für jedes beliebige Objekt 
- Verwendung als Default Case, d.h. als letzte Alternative in einem Match-Ausdruck

 Constant Pattern
- Das **Constant Pattern** ist ein Vergleichsmuster in Form von Literalen, z.B. vom Typ `**String**` oder `**Int**`


# 2 Type Pattern


- Das **Type Pattern** überprüft den Typ eines Wertes und gebt den Wert direkt gebunden weiter an den Ausdruck rechts von `**=>**`
- Erkennbar anhand des `**:**`, der die Variable vom Typ links von `**=>**` trennt
- Mischung von Type und Constant Pattern möglich


```
val x: Any = "Hallo"

val dataTypeAndValue = x match {
  case a: Int => s"Data type: Integer; Value $a"
  case b: String => s"Data type: String; Value $b"
  case _ => "Any other data type"
}
```


```
val x: Any = "Hallo"

val dataTypeAndValue = x match {
  case 0 => "Zero"
  case 1 => "One"
  case "Hallo" => "Zeichenkette Hallo"
  case a: Int => s"Data type: Integer; Value $a"
  case b: String => s"Data type: String; Value $b"
  case _ => "Any other data type"
}
```




# 3 Guard Pattern

- Das **Guard Pattern** ergänzt andere Muster mit `**if**`-Ausdrücken
- `**if**`-Ausdrücke können Variablen des Musters verwenden
- Ein Matching erfordert eine Übereinstimmung mit dem Muster und ein auf `**true**` evaluierender `**if**`-Ausdruck


```
val x=1

val test = x match {
  case n if n > 0 => "positive"
  case 0          => "zero"
  case _          => "negative"
}
```


# 4 Factory Pattern


```
case class Product(name: String, price: Double)

val p=Product("Smartphone", 199.00)

val label = p match {
  case Product(name, price) if price < 10 => s"$name is cheap"
  case Product(name, _)                   => s"$name is regular"
}
```

- Das **Factory Pattern** wird durch die Factory-Methode des Companion-Objekts einer Klasse gebildet
- Die Klassenparameter können an Variablen gebunden oder ausgelassen werden
- Voraussetzung für die Anwendung des Factory Pattern ist das Vorhandensein von Companion-Objekten zu den jeweiligen Klassen, welche Extraktoren mit dem Trait `**ProductN**` implementieren
- Das Factory Pattern sollte deshalb bevorzugt auf Objekte von Case-Klassen angewendet werden, eigene Companion-Objekte mit `**unapply**` werden aber auch unterstützt



# 5 Komplexes Beispiel


```scala
case class Product(name: String, price: Double, category: String, discount: Option[Double])
val label: Product => String = (p: Product) => {
  p match {
    case Product(name, price, "books", Some(discount)) if discount > 0.2 =>
      s"$name is a heavily discounted book (${discount * 100}%)"

    case Product(name, price, _, Some(discount)) =>
      f"$name has a ${discount * 100}%.1f%% discount. Final price: ${price * (1 - discount)}%.2f"

    case Product(name, price, _, None) if price < 10 =>
      s"$name is cheap"

    case Product(name, _, _, None) =>
      s"$name is a regular product"

    case _ =>
      "Unknown product"
  }
}
```


```scala
val products = List(
  Product("Scala Book", 39.99, "books", Some(0.25)),
  Product("USB Cable", 5.99, "electronics", None),
  Product("Coffee Mug", 12.99, "kitchen", Some(0.1)),
  Product("Notebook", 2.99, "stationery", None)
)
products.map(label).foreach(println)
```




