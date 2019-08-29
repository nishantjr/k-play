---
alias: conversions
backends:
  - ocaml
#  - java
#  - llvm
#  - haskell
tests:
  - selector: ".foo-to-string"
  - selector: ".string-to-bar"
  - selector: ".random-string-to-bar"
  - selector: ".foo-to-bar"
---

You can convert between tokens of one sort via `String`s by defining functions that are
implemented by builtin hooks.

```k
module CONVERSIONS
  imports STRING
  syntax Foo ::= "foo" [token]
  syntax Bar ::= "bar" [token]
```

The hook `STRING.token2string` allows conversion of any token to a string:

```k
  syntax String ::= FooToString(Foo)  [function, functional, hook(STRING.token2string)]
```

``` {.foo-to-string .input}
FooToString(foo)
```

``` {.foo-to-string .expected}
<k>
  "foo"
</k>
```

Similarly, the hook `STRING.string2Token` implements the inverse:

```k
  syntax Bar ::= StringToBar(String) [function, functional, hook(STRING.string2token)]
```

``` {.string-to-bar .input}
StringToBar("bar")
```

``` {.string-to-bar .expected}
<k>
  bar
</k>
```

WARNING: This sort of conversion does *NOT* do any sort of parsing or validation.
Thus, we can create arbitary tokens of any sort:

``` {.random-string-to-bar .input}
StringToBar("The sun rises in the west.")
```

``` {.random-string-to-bar .expected}
<k>
  The sun rises in the west.
</k>
```

Composing these two functions lets us convert from `Foo` to `Bar`

```k
  syntax Bar ::= FooToBar(Foo) [function]
  rule FooToBar(F) => StringToBar(FooToString(F))
```

``` {.foo-to-bar .input}
FooToBar(foo)
```

``` {.foo-to-bar .expected}
<k>
  foo
</k>
```

```k
endmodule
```
