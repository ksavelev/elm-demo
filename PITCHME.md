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

### ELM architecture

- Picture