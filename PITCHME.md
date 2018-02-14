# What is ELM

A delightful language for reliable webapps

---

### What is ELM

- Purely functional domain specific language
- Front-end only
- Compiles to JavaScript
- UI framework (functional, virtual DOM)
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

- Expressions
- Immutable values
- Persistent data structures
- Union types (aka algebraic data types)
- No nulls or undefined
- Pattern matching
- Managed effects (commands and subscriptions)

---

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

#### Persistent data structures

- persistent list
```elm
> trades = [(100, 1.5), (130, 1.45)]
[(100,1.5),(130,1.45)] : List ( number, Float )
```

- added value and created trades 2
```elm
> trades2 = (120, 1.52) :: trades
[(120,1.52),(100,1.5),(130,1.45)] : List ( number, Float )
```

- added value and created trades 3: trades3 and trades2 "share" trades
```elm
> trades3 = (140, 1.66) :: trades
[(140,1.66),(100,1.5),(130,1.45)] : List ( number, Float )
```

---

#### Union types

```elm
type Msg
    = Isin String
    | Nominal String
    | Price String
    | CreateNew
```

---

#### ELM architecture - Model - Update - View

![ELM architecture](https://github.com/ksavelev/elm-demo/raw/master/ELM%20Architecture%201.jpg)

---

### Demo - Model - Update - View

[Try ELM](http://elm-lang.org/try)

---

### Code - Model - View

```elm
import Html exposing (..)
import Html.Attributes exposing (..)
import Html.Events exposing (..)

type Msg
    = Isin String
    | Nominal String
    | Price String
    | CreateNew

view : String -> Html Msg
view model =
    div []
        [ input [ placeholder "ISIN", onInput Isin ] []
        , input [ placeholder "Nominal", onInput Nominal ] []
        , input [ placeholder "Price", onInput Price ] []
        , button [ onClick CreateNew ] [ text "New Trade" ]
        ]
      
main = 
  view ""
```

@[5-9]
@[11-18]
@[20-21]

---

### Code - Model - View - Update

```elm
import Html exposing (..)
import Html.Attributes exposing (..)
import Html.Events exposing (..)

type Msg
    = Isin String
    | Nominal String
    | Price String
    | CreateNew

type FloatOrString = FloatValue Float | StringValue String
type alias Model =  { isin : String
                   , nominal : FloatOrString
                   , price : FloatOrString
                   , result: ValidationResult }
type alias ValidationResult = (String, String)

view : Model -> Html Msg
view model =
    let
      toStringOrEmpty = \x -> case x of
                                FloatValue v -> toString v
                                StringValue v -> v
      nominal = toStringOrEmpty model.nominal
      price = toStringOrEmpty model.price
    in
    div []
        [ input [ value model.isin, placeholder "ISIN", onInput Isin ] []
        , input [ value nominal, placeholder "Nominal", onInput Nominal ] []
        , input [ value price, placeholder "Price", onInput Price ] []
        , button [ onClick CreateNew ] [ text "New Trade" ]
        , showValidate model
        ]
        
showValidate : Model -> Html msg
showValidate model =
    let
        (color, message) = model.result
    in
    div [ style [ ( "color", color ) ] ] [ text message ]

toStringOrFloat str =
  case String.toFloat str of
    Ok v -> FloatValue v
    Err _ -> StringValue str

update : Msg -> Model -> Model
update msg model =
    case msg of
        Isin isin ->
            { model | isin = isin } |> validate
        Nominal str ->
            { model | nominal = toStringOrFloat str } |> validate
        Price str ->
            { model | price = toStringOrFloat str } |> validate
        CreateNew ->
            model
            
validate : Model -> Model
validate model =
  let result =
    case (model.isin, model.nominal, model.price) of
    ("", _, _) -> ( "red", "enter isin" )
    (_, StringValue _, _) -> ( "red", "enter nominal" )
    (_, _, StringValue _) -> ( "red", "enter price" )
    (_, _, _) -> ( "green", "OK" )
  in
    { model | result = result }
     
main = 
  let
    initialModel = Model "" (StringValue "") (StringValue "") ("blue", "enter trade")
  in
    Html.beginnerProgram { model = initialModel, view = view, update = update }
```

@[5-9]
@[11-16]
@[18-33]
@[34-40]
@[46-57]

---

### ELM architecture - Commands

![ELM architecture - Commands](https://github.com/ksavelev/elm-demo/raw/master/ELM%20Architecture%202.jpg)

---

### Demo - Commands

[Try ELM](http://elm-lang.org/try)

---

### Commands

```elm
import Html exposing (..)
import Html.Attributes exposing (..)
import Html.Events exposing (..)

type Msg
    = Isin String
    | Nominal String
    | Price String
    | CreateNew

type FloatOrString = FloatValue Float | StringValue String
type alias Model =  { isin : String
                   , nominal : FloatOrString
                   , price : FloatOrString
                   , result: ValidationResult }
type alias ValidationResult = (String, String)

view : Model -> Html Msg
view model =
    let
      toStringOrEmpty = \x -> case x of
                                FloatValue v -> toString v
                                StringValue v -> v
      nominal = toStringOrEmpty model.nominal
      price = toStringOrEmpty model.price
    in
    div []
        [ input [ value model.isin, placeholder "ISIN", onInput Isin ] []
        , input [ value nominal, placeholder "Nominal", onInput Nominal ] []
        , input [ value price, placeholder "Price", onInput Price ] []
        , button [ onClick CreateNew ] [ text "New Trade" ]
        , showValidate model
        ]
        
showValidate : Model -> Html msg
showValidate model =
    let
        (color, message) = model.result
    in
    div [ style [ ( "color", color ) ] ] [ text message ]

toStringOrFloat str =
  case String.toFloat str of
    Ok v -> FloatValue v
    Err _ -> StringValue str

update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
    case msg of
        Isin isin ->
            ({ model | isin = isin } |> validate, Cmd.none)
        Nominal str ->
            ({ model | nominal = toStringOrFloat str } |> validate, Cmd.none)
        Price str ->
            ({ model | price = toStringOrFloat str } |> validate, Cmd.none)
        CreateNew ->
            (model, Cmd.none)
            
validate : Model -> Model
validate model =
  let result =
    case (model.isin, model.nominal, model.price) of
    ("", _, _) -> ( "red", "enter isin" )
    (_, StringValue _, _) -> ( "red", "enter nominal" )
    (_, _, StringValue _) -> ( "red", "enter price" )
    (_, _, _) -> ( "green", "OK" )
  in
    { model | result = result }
     
main = 
  let
    initialModel = Model "" (StringValue "") (StringValue "") ("blue", "enter trade")
  in
    Html.program
      { init = (initialModel, Cmd.none)
      , view = view
      , update = update
      , subscriptions = \_ -> Sub.none }
```

@[48-59]
@[72-80]

---

### What is not covered

- Subscriptions (websockets)
- Time-travel debugger (immutability + managed effects)
- Interop with JavaScript (ports)
- Package manger (elm-package)
- Automatic recompilation (elm-reactor)

---

### Conclusion

- Is this production ready? Not quite
    - One person development (Evan Czaplicki)
- So, why?
    - Fun
    - Good architecture

---

### Sources and Links

- [Home](http://elm-lang.org/)
- [Try ELM](http://elm-lang.org/try)
- [Build With ELM](http://builtwithelm.co/)
- [A curated list of useful Elm tutorials, libraries and software](https://github.com/isRuslan/awesome-elm)
- [ELM with Bootstrap 4](http://elm-bootstrap.info)
- Sample code is based on [Form Example](http://elm-lang.org/examples/form)

---

### Q & A

Questions
