# What is ELM

A delightful language for reliable webapps

---

### What is ELM

- Purely functional domain specific language
- Front-end only
- Compiles to JavaScript
- UI framework (functional reactive, virtual DOM)
- Runtime (managed effects)

---

### Why

- Whats is wrong with JavaScript?
    + null, undefined
    + runtime exceptions
- Maintainability

---

### How

- Statically-typed language
- ELM architecture

---

### Statically-typed language
#### Expressions

- list of tuples
```elm
> trades = [(100, 1.5), (130, 1.45)]
[(100,1.5),(130,1.45)] : List ( number, Float )
```
 
- pure function that takes tuple and returns number
```elm
> mv (nominal, price) = nominal * price
<function> : ( number, number ) -> number
```

- applying function to each item of list
```elm
> mvs = List.map mv trades
[150,188.5] : List Float
```

---

### Statically-typed language
#### Immutable values

- record value
```elm
> t = { isin = "DK1234", nominal = 100, price = 1.5 }
{ isin = "DK1234", nominal = 100, price = 1.5 }
    : { isin : String, nominal : number, price : Float }
```

- update field
```elm
> t2 = { t | price = 1.45 }
{ isin = "DK1234", nominal = 100, price = 1.45 }
    : { isin : String, nominal : number, price : Float }
```

---

### Statically-typed language
#### Persistent data structures

- persistent list
```elm
> trades = [(100, 1.5), (130, 1.45)]
[(100,1.5),(130,1.45)] : List ( number, Float )

> trades2 = (120, 1.52) :: trades
[(120,1.52),(100,1.5),(130,1.45)] : List ( number, Float )
```

- trades3 and trades2 "share" trades
```elm
> trades3 = (140, 1.66) :: trades
[(140,1.66),(100,1.5),(130,1.45)] : List ( number, Float )
```

---

### ELM architecture

- Picture