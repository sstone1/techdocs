---
id: version-0.30.1-logic-simple-type
title: Introducing Types
original_id: logic-simple-type
---

We have so far talked about types only informally. When we wrote earlier:
```ergo
    "John Smith" // a String literal
    1            // an Integer literal
    ...
```
the comments mention that `"John Smith"` is of type `String`, and that `1` is of type `Integer`.

In reality, the Ergo compiler understands which types your expressions have and can detect whether those expressions apply to the right type(s) or not.

Ergo types are based on the [Concerto Modeling](model-concerto.md) Language.

## Primitive types

The simplest of types are primitive types which describe the various kinds of literal values we saw in the previous section. Those primitive types are:

```ergo
    Boolean
    String
    Double
    Integer
    Long
    DateTime
```

:::note
The two primitive types `Integer` and `Long` are currently treated as the same type by the Ergo compiler.
:::

## Type errors

The Ergo compiler understand types and can detect type errors when you write expressions. For instance, if you write: `1.0 + 2.0 * 3.0`, the Ergo compiler knows that the expression is correct since all parameters for the operators `+` and `*` are of type `Double`, and it knows the result of that expression will be a `Double` as well.

If you write `1.0 + 2.0 * "some text"` the Ergo compiler will detect that `"some text"` is of type `String`, which is not of the right type for the operator `*` and return a type error.

> Typing `return 1.0 + 2.0 * "some text"` in the [Ergo REPL](https://ergorepl.netlify.com), should answer a type error:
> ```text
> Type error (at line 1 col 13). This operator received
> unexpected arguments of type Double  and String.
> return 1.0 + 2.0 * "some text"
>              ^^^^^^^^^^^^^^^^^
> ```

## Type annotations

In a let bindings, you can also use a _type annotation_ to indicate which type you expect it to have.

```ergo
    let name : String = "John"; // declares and initialize a string variable
    name ++ " Smith"            // rest of the expression
```
or
```ergo
    let x : Double = 3.1416     // declares and initialize a double variable
    sqrt(x)                     // rest of the expression
```

This can be useful to document your code, or to remember what type you expect from an expression.

The Ergo compiler will return a type error if the annotation is not consistent with the expression that computes the value for that let binding. For instance, the following will return a type error since `"pi!"` is not of type `Double`.

```ergo
    let x : Double = "pi!"; // TYPE ERROR: "pi!" is not a Double
    sqrt(x)
```

> Typing `return let x : Double = "pi!"; sqrt(x)` in the [Ergo REPL](https://ergorepl.netlify.com), should answer a type error:
> ```text
> Type error (at line 1 col 7). The let type annotation Double for
> the name x does not match the actual type String.
> return let x : Double = "pi!"; sqrt(x)
>        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
> ```

This becomes particularly useful as your code becomes more complex. For instance the following expression will also trigger a type error:

```ergo
    let rate = 3.5;
    let name : String =
      if rate > 0.0
      then 3.14         // TYPE ERROR: 3.14 is not a String
      else "John";
    name ++ " Smith"
```

Since not all the cases of the `if ... then ... else ...` expressions return a value of type `String` which is the type annotation for the `name` variable.

