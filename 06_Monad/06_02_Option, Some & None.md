
# 1 总览


```scala
val findProductNameByIdUnsafe: Int => String = id =>
  if (id == 1) "T-Shirt"
  else null


val name = findProductNameByIdUnsafe(999)
println(name.toUpperCase)

val name = findProductNameById(999).getOrElse("Unknown")
println(name.toUpperCase)
```


- In vielen Fällen kann eine Funktion keinen sinnvollen Rückgabewert liefern – zum Beispiel wenn ein Kunde in der Datenbank nicht gefunden wird
- Im imperativen Stil braucht man viele `**if**`, `**try**` und `**catch**`, um solche Ausnahmen abzufangen und Null-Pointer-Exception zu vermeiden
- In Scala und Java existiert `**null**`, aber man benutzt es fast nie
- Stattdessen verwendet man den generischen Container **`Option[T]`** in Scala (oder `**Optional<T>**` in Java) für "Wert vorhanden oder nicht"


# 2 Option als alternative zu Null

![](image/Pasted%20image%2020250614211040.png)

- In Scala verwendet man `**Option[T]**`, um auszudrücken, dass ein Wert vorhanden sein kann, aber nicht muss
- Anstatt mit `**null**` zu arbeiten, kapselt Option das Vorhandensein oder Fehlen eines Wertes auf typsichere Weise
- `**Option[T]**` ist eine abstrakte Oberklasse, d.h. eine Variable vom Typ `**Option[T]**` verweist entweder auf einen Wert vom Typ `**Some[+T]**` oder `**None**`
- Beachte: `**Some[+T]**` ist immutable und kovariant
- `**Option[T]**` ist eines von mehreren Beispielen für **Monaden** (siehe Kapitel 5)


```scala
val findProductNameByIdUnsafe: Int => String = id =>
  if (id == 1) "T-Shirt"
  else null

val name = findProductNameById(999).getOrElse("Unknown")
println(name.toUpperCase)
```

- Wenn eine Variable vom Typ `**Option[T]**` einen Wert vom Typ `**Some[+T]**` enthält, bedeutet dies, dass ein Wert vom Typ `**T**` existiert und dieser durch `**Some[+T]**` gekapselt wird
- Wenn die Variable einen Wert vom Typ `**None**` enthält, bedeutet dies, dass kein Wert existiert
- `**Option[T]**` definiert zahlreiche Methoden, um an den inneren Wert vom Typ `**T**` zu gelangen oder um diesen Wert zu verarbeiten und als Ergebnis der Bearbeitung einen neuen Wert vom Typ `**Option[T]**` zu generieren
- `**getOrElse**` ist eine Methode, die entweder den Wert vom Typ `**T**` zurückgibt falls dieser existiert oder einen Standardwert, der beim Aufruf als Argument übergeben wurde



# 3 Beispielmethoden auf Option

![](image/Pasted%20image%2020250615091416.png)



```
val findProductNameById: Int => Option[String] = id =>
  if (id == 1) Some("T-Shirt")
  else None

```



## 3.1 使用 `getOrElse`

```
val name = findProductNameById(999).getOrElse("Unknown")
println(name.toUpperCase)
```

- 如果找不到产品名（返回 `None`），就使用默认值 `"Unknown"`。
    
- 然后把名称转换为大写输出。
    
- ✅ 安全，推荐使用。


## 3.2 使用 `get`（不推荐，危险）

`val name = findProductNameById(999).get`

- `get` 会强行提取 `Some` 中的值，如果是 `None` 会抛出异常。
    
- ❌ 不安全，**不推荐使用**。



## 3.3 `orElse`

提供一个后备的 `Option` 值（默认备用值）

```
val opt = None
val fallback = opt.orElse(Some("Default Shirt"))
println(fallback)  // Some("Default Shirt")

```


## 3.4 使用 `foreach`

`findProductNameById(1).foreach(p => println(s"YAOS verkauft: $p"))`

- 如果是 `Some`，就对其中的值执行 `println` 操作。
    
- 如果是 `None`，什么都不做。
    
- 用于需要对可选值执行副作用的情况

## 3.5 forall 

与 `exists` 相反，None 会返回 `true`，Some 需满足条件才为 `true`

```
val opt: Option[String] = None
println(opt.forall(_.startsWith("T")))  // true

```


## 3.6 使用 `filter`

```
val filtered = findProductNameById(1).filter(_.startsWith("T"))
println(filtered)

```
- 对 `Option` 中的值应用过滤条件。
    
- 如果值存在并满足条件（以 `"T"` 开头），保留 `Some`；
    
- 否则返回 `None`。



## 3.7 `map`
对 `Option` 中的值应用一个函数，返回新的 `Option`

```scala
val opt = Some("t-shirt")
val upperOpt = opt.map(_.toUpperCase)
println(upperOpt)  // Some("T-SHIRT")
```

如果是 `None`，什么也不会做，仍然返回 `None`。



## 3.8 `flatMap`

类似 `map`，但函数返回的是 `Option` 而不是普通值（避免嵌套）


```
val opt = Some("T-Shirt")
val result = opt.flatMap(name => Some(name.length))
println(result)  // Some(7)

```


## 3.9 collect

结合模式匹配进行处理，仅处理满足条件的情况

```
val opt = Some("T-Shirt")
val result = opt.collect { case s if s.startsWith("T") => s.toUpperCase }
println(result)  // Some("T-SHIRT")

```

说明：如果条件不匹配，则返回 `None`。



## 3.10 `fold`

提供一个默认值和一个函数，来统一处理 `None` 和 `Some` 的情况

```
val opt = Some("t-shirt")
val result = opt.fold("no item")(_.toUpperCase)
println(result)  // "T-SHIRT"

```

- 如果是 `None`，使用 `"no item"`
    
- 如果是 `Some(value)`，则对 `value` 应用函数


## 3.11 exists

检查是否满足某个条件

```
val opt = Some("T-Shirt")
val hasT = opt.exists(_.startsWith("T"))
println(hasT)  // true
```



# 4 SCALA OPTION VS JAVA OPTIONAL

```
val findProductNameById: Int => Option[String] = id =>
  if (id == 1) Some("T-Shirt")
  else None

val name = findProductNameById(1).getOrElse("Unknown")
println(name.toUpperCase)
```


```
import java.util.Optional;

Optional<String> findProductNameById(int id) {
    if (id == 1) {
        return Optional.of("T-Shirt");
    } else {
        return Optional.empty();
    }
}

var name = findProductNameById(1).orElse("Unknown");
System.out.println(name.toUpperCase());
```



- `**Optional<T>**` in Java ist wie `**Option[T]**` in Scala
- Beide dienen dazu, `**null**` zu vermeiden
- Scalas `**Option[T]**` ist kompakter, ausdrucksstärker und besser in den Sprachkern integriert
- Javas `**Optional<T>**` erfordert mehr Boilerplate, ist aber seit Java 8 ebenfalls typsicher


