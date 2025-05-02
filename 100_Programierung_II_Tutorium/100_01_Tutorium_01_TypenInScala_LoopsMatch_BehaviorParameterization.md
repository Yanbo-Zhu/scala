

Freitag Tutorium 14-16 tazin 的课件和代码 

![](image/Pasted%20image%2020250502153614.png)

https://git.tu-berlin.de/tenzin-lham/prog2sose25#


# 1 Aufgabe 1.1  Daten Type 


![](image/Pasted%20image%2020250502143552.png)


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
