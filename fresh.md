---
alias: fresh
backends:
  - ocaml
#  - java
#  - llvm
#  - haskell
tests:
  - selector: ".fresh-foo"
---

This module demonstrates how to use the `!` operator generate a fresh instance
of a given sort.

```k
module FRESH
  imports INT
  imports STRING
  syntax Foo ::= r"[a-z][A-Za-z\\-0-9'\\#\\_]*" [token, autoReject]
```

The hook `STRING.string2token` allows conversion of any string to a token:

```k
  syntax Foo ::= String2Foo(String) [function, functional, hook(STRING.string2token)]
```

A construct tagged with `freshGenerator` will create a fresh instance when using
the `!` operator. In this case, a fresh instance of sort `Foo` will prepend
"fresh_" to an integer, for which it keeps a running count.

```k
  syntax Foo ::= freshFoo(Int) [freshGenerator, function, functional]
  rule freshFoo(I:Int) => String2Foo("fresh_" +String Int2String(I))
```

`getNewFoo` calls `!` operator which calls the above function to create a fresh instance.

```k
  syntax Foo ::= "getFreshFoo"
  rule getFreshFoo => !F1:Foo ~> !F2:Foo
```

```k
endmodule
```

``` {.fresh-foo .input}
getFreshFoo
```

``` {.fresh-foo .expected}
<k>
  fresh_1 ~> fresh_0
</k>
```
