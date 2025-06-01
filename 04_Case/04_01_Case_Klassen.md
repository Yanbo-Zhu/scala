
# 1 Case Klassen

Case Classes sind der Shortcut, wenn man schnell und bequem Klassen bauen will.
spezielle Klassen, die automatisch um bestimmte Methoden und Felder erweitert werden, z.b. toString, apply und unapply Methoden. dadurch wird Pattern Matching sehr erleichtert

Wenn man `case` vor eine Klasse schreibt, bekommt man automatisch:
- `toString`, `equals`, `hashCode`, `copy` alles generiert
- ein **Companion Object** mit `apply` und `unapply` Methoden


```
case class Employee(name: String, age: Int, income: Int)

val person: Employee = Employee("Loris", 23, 420)

person match {
  case Employee(name, _, income) if (income > 419) =>{
    name + " do you want to donate?"
  }

  case _ => "This person does not meet the donation criteria."
}

println(person) //Employee(Loris,38,420)
```



- Eine case Klasse mischt der Klasse den Trait `ProductN` bei, wobei N für die Anzahl der Klassenparameter der case Klasse steht
- ProductN verfügt über Methoden wie productArity,  productElement und productElementName, mit denen der Compiler automatisch die einzelnen Elemente eines Objektes extrahieren kann


Dadurch kann der Compiler automatisch alle Felder extrahieren (wie hier income):
```
person match {
  case Employee(name, _, income) if income > 419 => ...
}
//age wird hier bewusst ignoriert! 
```

# 2 Warum nutzt man nicht immer case Klasse

![](image/Pasted%20image%2020250601182124.png)

# 3 Case Klassen 的原理 


```
case class Employee(name: String, age: Int, income: Int)

val person: Employee = Employee("Loris", 23, 420)

person match {
  case Employee(name, _, income) if (income > 419) =>{
    name + " do you want to donate?"
  }

  case _ => "This person does not meet the donation criteria."
}

println(person) //Employee(Loris,23,420)
```

Eine case Klasse mischt der Klasse den Trait ProductN bei, wobei N für die Anzahl der Klassenparameter der case Klasse steht
ProductN verfügt über Methoden wie productArity,  productElement und productElementName, mit denen der Compiler automatisch die einzelnen Elemente eines Objektes extrahieren kann
Dadurch kann der Compiler automatisch alle Felder extrahieren (wie hier income):

```
person match {
  case Employee(name, _, income) if income > 419 => ...
}
//age wird hier bewusst ignoriert! 
```


# 4 apply and upapply method 


apply
- Wird im **Companion Object** automatisch generiert
- Ermöglicht die Objekterstellung **ohne `new`**
- Wird aufgerufen, wenn man schreibt: `Person("bercan")`
```
case class Person(name: String)
val bercan = Person("bercan") // ruft Person.apply("bercan") auf
```
Man sieht `apply` nicht direkt, aber sie steckt im Companion Object der Klasse.


 
 `unapply`
- Macht die Klasse **pattern-matchable**
- Gibt bei erfolgreichem Match die Felder als `Some(...)` zurück
```
p match {
  case Person(n) => println(s"Name: $n") // ruft Person.unapply(p) auf
}
```

Auch `unapply` ist automatisch da – man nutzt sie indirekt beim Pattern Matching.


# 5 Unterschiede zwischen Scala 2 und 3

In Scala 2

    unapply wird aktiv genutzt für Pattern Matching

In Scala 3

    Der Compiler übernimmt mehr Arbeit automatisch
    Auch nicht-case Klassen bekommen apply im Companion Object
    Beispiel:

```
class Zoo[T](val animals: Array[T])
object Zoo // apply wird automatisch generiert
val aquarium: Zoo[Fish] = Zoo(Array[Fish]()) // kein "new", trotzdem ok!

```


# 6 例子


1. Was ist der Unterschied zwischen case classes und regulären classes in Scala?
2. Erstellen Sie jeweils case Klassen zu den Geometrischen Formen Kreis, Rechteck
und dreieck welche alle notwendigen Informationen beinhalten um die Fläche der
jeweiligen Geometrischen Form zu berechnen.
3. Implementieren Sie die Methode calculateArea die eine Instanz dieser geometrischen
Formen übergeben bekommt und die Fläche dieser berechnet und zurückgibt.
4. erweitern Sie die Methode calculateArea so, dass keine geometrischen Formen mit
Parametern kleiner gleich 0 akzeptiert werden.

![](image/Pasted%20image%2020250601190834.png)

```
case class Circle(radius: Double)
case class Rectangle(width: Double, height: Double)
case class Triangle(base: Double, height: Double)

def calculateArea(shape: Any): Double = shape match {
  case Circle(radius) => Math.PI * Math.pow(radius, 2)
  case Rectangle(width, height) => width * height
  case Triangle(base, height) => 0.5 * base * height
  case _ => throw new IllegalArgumentException("Invalid shape.")
}

def calculateArea(shape: Any): Double = shape match {
  case Circle(radius) if radius > 0 => Math.PI * Math.pow(radius, 2)
  case Rectangle(width, height) if width > 0 && height > 0 => width * height
  case Triangle(base, height) if base > 0 && height > 0 => 0.5 * base * height
  case _ => throw new IllegalArgumentException("Invalid shape.")
}


```
