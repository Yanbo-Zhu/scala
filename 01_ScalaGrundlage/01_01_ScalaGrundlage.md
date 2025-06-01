

# 1 DatenType

![](image/Pasted%20image%2020250502142733.png)


AnyRef
- Obertyp aller Referenztypen
- Entspricht `java.lang.Object` in der JVM
- Beispiele: `String`, `List`, eigene Klassen

Nothing
- `**Nothing**` ist ein besonderer Typ, der keinen Wert haben kann
- Er ist ein Subtyp aller Typen
- Wird verwendet, wenn niemals ein sinnvoller Wert zurückgegeben wird, z.B. nach einem `**throw**`
- Praktisch für Fehlermeldungen oder leere Strukturen

Null
- `**Null**` ist ein eigener Typ in Scala, der **`null`** als einzigen Wert haben kann
- `**Null**` ist ein Subtyp aller Typen unterhalb von `**AnyRef**`
- Primärer Zweck von `**Null**` ist die Kompatibilität mit Java, in der reinen Java-Programmierung vermeidet man null


UNIT
- `**Unit**` wird in der Signature von Methoden, Funktionen und Blockausdrücken verwendet, wenn diese keinen Rückgabewert liefern
- Künstlicher Datentyp, der nur den Wert `**()**` haben und somit keine Information speichern kann
- Entspricht dem Schlüsselwort `**void**` in Java



## 1.1 Null
![](image/Pasted%20image%2020250502142816.png)


![](image/Pasted%20image%2020250502142833.png)


![](image/Pasted%20image%2020250502142921.png)


![](image/Pasted%20image%2020250502143031.png)


## 1.2 Val vs Var

![](image/Pasted%20image%2020250502193329.png)



- Ein **Literal** ist eine Zeichenfolge zur Darstellung eines Wertes im Quellcode
- Ein **Datentyp** ist eine formale Beschreibung, welche Art von Daten in einer Variable gespeichert werden kann und welche Operationen darauf erlaubt sind
- Eine **Referenz** ist ein Zeiger auf ein Objekt im Speicher – anstatt den Wert direkt zu speichern, enthält die Variable nur die Adresse (also, die Position) des Objekts im (Heap-) Speicher 
- Eine **Variable** ist eine **veränderliche** (**mutable**) Referenz auf einen Wert eines bestimmten Datentyps, d.h. ein wert kann jederzeit erneut zugewiesen werden
- Eine **Konstante** ist eine **unveränderliche** (**immutable**) Referenz auf einen Wert eines bestimmten Datentyps, d.h. der Wert kann initial zugewiesen, die Zuweisung aber nicht mehr geändert werden

- Mit `**var**` initialisierte Variablen sind **veränderliche** (**mutable**) Referenzen auf Objekte
- Mit `**val**` initialisierte Variablen sind **unveränderliche** (**immutable**) Referenzen auf Objekte




final:   Wert kann nicht mehr andert werden 

val in scala;  val ist nicht final 

![](image/Pasted%20image%2020250502152458.png)

![](image/Pasted%20image%2020250502152536.png)

![](image/Pasted%20image%2020250502152613.png)


![](image/Pasted%20image%2020250502152659.png)






## 1.3 Platzhalter vs Referenz


![](image/Pasted%20image%2020250502193521.png)


- In Java speichern Variablen primitiver Datentypen die Werte direkt – Variablen sind "Aufbewahrungskisten" für Werte, die im Stack-Speicher oder im Feld eines Objektes direkt gespeichert werden
- In Scala sind Werte primitiver Datentypen (Wertdatentypen) eigenständige, vollwertige Objekte, die im Heap-Speicher leben – Variablen von Wertdatentypen enthalten Referenzen auf den Heap-Speicherbereich in dem der Wert bzw. das Objekt liegt





# 2 Wertdatentypen

![](image/Pasted%20image%2020250502194406.png)


- Bei der **Dot-Notation** wird die Methode über einen Punkt nach dem Objekt aufgerufen
- Operatoren wie `**+**`, `**-**`, `**/**`, und `**==**` sind als Methoden auf Objekten implementiert

- Bei der **Operator-Notation** (**Infix-Notation**) werden Methoden wie ein Operator aufgerufen, d.h. Objekt, Methodenname und Argument werden durch ein Leerzeichen voneinander getrennt
- Die Operator-Notation kann nur auf Methoden angwendet werden, die genau ein Argument haben


## 2.1 INITIALISIERUNG VON VARIABLEN UND KONSTANTEN


![](image/Pasted%20image%2020250502193811.png)

- In Scala müssen Variablen bei der Deklaration initialisiert werden, um Nulls und uninitialisierte Zustände zu vermeiden (wie sie bei Java vorkommen können)
- Ausnahmen: Felder in abstrakten Klassen und Traits, Felder mit Default-Werten (`**_**`) sowie Variablen mit verzögerter Initialisierung (`**lazy**`)







## 2.2 Konvertierung




Datentype Wehseln 
int -> long -> float -> double 
![](image/Pasted%20image%2020250502143352.png)

Unify two branch 
- In Scala, `if` is an **expression**, not a statement. That means it **returns a value**.
- In this case:
    - `if (true)` returns `1` (which is an `Int`)
    - `else` returns `"hello"` (which is a `String`)
- Scala has to **unify** both branches to a common supertype.
- Since `Int` and `String` do **not share a more specific common type**, Scala **upcasts** both to the common supertype: `**Any**`.
    -  String ist keine UnterType von AnyVal



---



Implizite Konvertierung
![](image/Pasted%20image%2020250502194434.png)


Explizite Konvertierung

![](image/Pasted%20image%2020250502194709.png)


Explizite Konvertierung

- Alle Konvertierungen, bei denen Informationen verloren gehen können, müssen explizit durchgeführt werden
- In Scala erfolgt die **explizite Konvertierung** mittels Konvertierungsmethoden wie **`toInt`**, `**toLong**` und `**toFloat**`
- Explizite Konvertierungen können Datenverlust oder Überlauf verursachen
- Bei **Datenverlust** gehen Daten verloren, obwohl der Wert im Wertebereich des Zieltyps bleibt, z.B. beim Abschneiden von Nachkommastellen
- Beim **Überlauf** (**Overflow**) liegt der Wert außerhalb des Wertebereichs des Zieltyps – das Ergebnis ist falsch oder unbrauchbar


## 2.3 Multi-Line String-Literale und Interpolation

![](image/Pasted%20image%2020250502194805.png)


- Ein **Multiline-String-Literal** wird in Scala mit drei Anführungszeichen definiert
- Zeilenumbrüche und Whitespaces werden beibehalten
- Um störende Einrückungen zu entfernen, kann man `**stripMargin**` verwenden, um Leerzeichen bis zum Trennzeichen zu vermeiden
- Der **s-Interpolator** erlaubt es, Variablen direkt in einen String einzubauen
- Mit `**${...}**` können Ausdrücke inkludiert werden
- Der **f-Interpolator** kann Zahlen und andere Werte mit Formatangaben in Strings einfügen


## 2.4 Zeichenketten und Tuples


![](image/Pasted%20image%2020250502194843.png)

- In Scala sind **Tuples** eine Möglichkeit, mehrere Werte unterschiedlicher Typen in einer einzigen zusammengesetzten Einheit zusammenzufassen – ohne eine eigene Klasse definieren zu müssen
- Auf die Elemente eines Tuples kann mit `**_1**`, `**_2**`, `**_3**` usw. zugegriffen werden (d.h. der Index beginnt bei 1) 
- Scala unterstützt Tupel mit bis zu 22 Elementen


# 3 Ausdrücke vs Anweisungen


![](image/Pasted%20image%2020250502194930.png)

- Ein **Ausdruck** (**Expression**) ist ein Stück Programmcode, welches ein Wert zurückliefert
- Grundlage für Funktionen und funktionale Programmierung
- Ausdrücke liefern Werte als Ergebnis ihrer Berechnung zurück, sie modifizieren keine Daten
- Werte, die von Ausdrücken geliefert werden, haben einen wohldefinierten Datentyp
- Ergebnisse von Ausdrücken bilden die Eingabe für andere Ausdrücke, werde in Variablen gespeichert oder bei Methodenaufrufen übergeben
- Hinweis: Literale sind Ausdrücke
- Merke: In Scala ist (fast) alles ein Ausdruck


![](image/Pasted%20image%2020250502194942.png)


- Eine **Anweisung** (**Statement**) ist ein Stück Programmcode, welches keine Wert zurückliefert
- Anweisungen modifizieren Daten oder Speicherbereiche oder machen etwas außerhalb des Bereichs der Anwendung (auf die Konsole schreiben, Datenbank- und Netzzugriffe, GUI,...)
- Anweisungen produzieren Seiteneffekte
- Da Anweisungen keine Daten zurückliefern, haben sie häufig den Datentyp `**Unit**`
- `**Unit**` umfasst nur den einen Wert `**()**` und kann somit keine Informationen speichern


# 4 String Interpolation

```
val myName = "Sama"
println("Hello, " + myName + "!")       // ohne Interpolation
println(s"Hallo, $myName!")             // mit Interpolation
```

Mit eingebettetem Ausdruck:

```
val a = 5
val b = 6
println(s"Summe von $a und $b ist ${a + b}")

```

# 5 Blocke 


```
val total={
  val price=100.00
  val discount={
    val customerType="regular"
    if (customerType=="premium") 0.2 else 0.5
  }
  price-(price*discount)
}


val total={
  println("Berechne den zu zahlenden Preis...")
  val price=100.00
  val discount={
    val customerType="regular"
    if (customerType=="premium") 0.2 else 0.5
  }
  price-(price*discount)
}

println("Jetzt greife ich auf 'total' zu:")
println(total)
```

- Blockausdrücke können Variablen zugewiesen werden
- Wert und Datentyp der Variablen ergeben sich aus dem Wert und Typ des letzten Ausdrucks im Block
- Frage: Wann wird der Blockausdruck ausgeführt? Bei der Initialisierung der Variablen oder wenn die Variable ausgerufen bzw. gelesen wird?




---


```
lazy val total={
  println("Berechne den zu zahlenden Preis...")
  val price=100.00
  val discount={
    val customerType="regular"
    if (customerType=="premium") 0.2 else 0.5
  }
  price-(price*discount)
}

println("Jetzt greife ich auf 'total' zu:")
println(total)
```


- In Scala wird das Schlüsselwert `**lazy**` verwendet, um die Ausführung des zugewiesenen Blockausdrucks zu verzögern, bis der Wert tatsächlich das erste Mal gebraucht wird
- Der Block wird einmalig beim ersten Zugriff ausgeführt, nicht bei Initialisierung, und das Ergebnis wird gecached


# 6 if and switch




```scala

val score = 85
val grade = if (score >= 90) "A"
            else if (score >= 80) "B"
            else "C"
        
	
val amount = 120
val shippingCost = if (amount > 100) {
  println("Versand kostenlos")
  0.0
} else {
  println("Versandkosten berechnet")
  4.99
}

```


---



|Merkmal|Scala match|Java switch klassisch|Java switch ab Java 14|
|---|---|---|---|
|Art|Ausdruck|Anweisung|Ausdruck|
|Rückgabewert|ja|nein|ja|
|Mehrere Werte pro Fall|`**case "A" \| "B"**`|`**case "A": case "B"**`|`**case "A", case "B"**`|
|Fallthrough|Nie|Ja (ohne break)|Nein (bei `**->**`)|
|Pattern Matching Werte|Ja|Ja|Ja|
|Pattern Matching Typen|Ja|Nein|Nein|
|Strukturelles Matching|Ja|Nein|Nein|
|Guards|`**case "A" if ... =>**`|Nein|Nein|
|Vollständigkeitsprüfung|Bei sealed-Klassen|Nein|Ab Java 17|
|Default nötig|Nur wenn Pattern nicht exhaustive|Immer empfohlen|Immer empfohlen|
|Typprüfung durch Compiler|Ja|Nein|Teilweise mit sealed-Klassen|
|Zielgruppe|Funktionale, ausdrucksstarke Logik|Klassische imperative Programmierung|Modernes Java, funktionaler Stil möglich|


```scala
val day = "Monday"

val message = day match {
  case "Monday"    => "Start of the week"
  case "Friday"    => "Almost weekend"
  case "Saturday" | "Sunday" => "Weekend!"
  case _           => "Midweek day"
}

// Pattern Matching 
val pair = (2, 5)
val res = pair match {
  case (1,1) => "erstes 1, zweites 1"
  case (x,5) => s"erstes $x, zweites 5"
  case _ => "unbekanntes Paar"
}


//Pattern Matching mit Pattern Guards (
//if (base>0 && height >0) = Pattern Guard.
  def calculateArea(shape: Any): Double= shape match{
    case Circle(radius) if (radius>0) => Math.PI * Math.pow(radius, 2)
    case Rectangle(width, height) if (width>0 && height >0) => width* height
    case Triangle(base,height) if (base>0 && height >0) => 0.5 * base * height
    case _=> throw new IllegalArgumentException("Not a shape.")
  }


```



```java 
String day="Tuesday";
String message;

switch (day) {
  case "Monday":
    message = "Start of the week";
    break;
  case "Friday":
    message = "Almost weekend";
    break;
  case "Saturday":
  case "Sunday":
    message = "Weekend!";
    break;
  default:
    message = "Midweek day";
    break;
}

// ab java14 

String day="Tuesday";

String message = switch (day) {
  case "Monday" -> "Start of the week";
  case "Friday" -> "Almost weekend";
  case "Saturday", "Sunday" -> "Weekend!";
  default -> "Midweek day";
};

```


# 7 Tupel

![](image/Pasted%20image%2020250601173811.png)

```
val me: (String, Int, Boolean) = ("Sama", 21, true)
val me2 = ("Sama", 21, true)

//Zugriff erfolgt so:
val myName = me._1
val myAge = me._2
println(myName)
println(myAge)
```


# 8 Union Type

Ein Union Type in Scala erlaubt, dass ein Wert mehrere mögliche Typen haben kann.
Man schreibt das mit einem senkrechten Strich (|).

```
def example(x: Int | String) = println(x)
```


# 9 ranges 


![](image/Pasted%20image%2020250502195432.png)

![](image/Pasted%20image%2020250601173329.png)

- Ein **Range** ist eine sequentielle Folge von Zahlen, die einen **Startwert**, einen **Endwert** und optional einen **Schritt** enthält
- Ranges können inklusiv (`**to**`) oder exklusiv (`**until**`) definiert werden
- Ranges sind **lazy**, d.h. sie erzeugen ihre Werte erst bei Bedarf
- Ranges werden verwendet in for-Schleifen, für die Generierung von Daten und Iterationen

# 10 for-loop und for-Comprehension


![](image/Pasted%20image%2020250502145201.png)


for besteht aus folgenden Bestandteilen:

- **Generator**: Erstellt eine Variable durch Iteration über einen Range oder eine Kollektion, z.B. `**i <- 1 to 5**`
- **Guard**: Eine Bedingung (if), die bestimmte Werte ausschließt, z.B. `**if i % 2 == 0**`
- **Definition**: Zusätzliche lokale Variable in der Schleife, z.B. `**val squared=i*i**`
- **Body**: Der auszuführende Codeblock (Seiteneffekt oder Ausdruck), z.B. `**{println(i)}**`
- **`yield`-Ausdruck**: Optionaler Ausdruck zur Erzeugung einer neuen Kollektion, z.B. `**yield i*2**`


![](image/Pasted%20image%2020250502195606.png)

```scala
/*******************/
/** Schleifen  */
/*******************/

/* A)
  for (int i=0; i < 10; i++) {
    System.out.println(i);
  }
 */
 for(i <- 0 to 9) { // to ist inklusiv, und bis ist exklusiv
   println(i)
 }


//TODO: Scala-Äquivalent implementieren
/* B)
  for (int i=0; i < 10; i=i+2) {
    System.out.println(i);
  }
 */

// List mit allen zahlen
// 2 oder -2 sind beide möglich
for(i <- 0 until 10 by 2) { // to ist inklusiv, und bis ist exklusiv
  println(i)
}


//TODO: Scala-Äquivalent implementieren

/* C)
  for (int i=0; i < 10; i++) {
    for (int j=0; j < 10; j++) {
      System.out.println(i + ", " + j);
    }
  }
 */


for(i <- 0 until 10) {
  for (j <- 0 until 10) {
    println(i + ", " + j)
  }
}

for (i <- 0 until 10; j <- 0 until 10) {
  println(i + ", " + j)
}

val range = 0 until 10 by 2
for (i <- range; j <- 0 until 10) {
  println(i + ", " + j)
}
for (x <- range if x%2 == 0) {
  println(x)
}

//TODO: Scala-Äquivalent implementieren

/*******************/
/** For Loop Ausdruck  */
/*******************/

 val combination = for {
   x <- 0 until 10
    y <- 0 until 10
 } yield (x, y) // tuple, liste combination = List((0,0), (0,1), (0,2), ...)



/*******************/
/** Match Ausdruck  */
/*******************/

/* D)
String color = "rot"; 
switch (color) {
            case "rot":
                System.out.println("Stopp!");
                break;
            case "gelb":
                System.out.println("Langsam.");
                break;
            case "grün":
                System.out.println("Los!");
                break;
            case "blau":
                System.out.println("Keine Verkehrsfarbe.");
                break;
            default:
                System.out.println("Unbekannte Farbe.");
        } 
        
*/

//TODO: Scala-Äquivalent implementieren

val color = "rot"
color match {
  case "rot" => println("Stopp!")
  case "gelb" => println("Langsam.")
  case "grün" => println("Los!")
  case "blau" => println("Keine Verkehrsfarbe.")
  case _ => println("Unbekannte Farbe.")  // default
}

val x = 10
val y = 20

val max = (x>y match {  // false
  case true => x
  case false => y
  case _ => "Beide sind gleich"
}
```



# 11 While and Do-While 


![](image/Pasted%20image%2020250502195705.png)