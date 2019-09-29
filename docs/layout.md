---
title: Custom comments
backends:
  - ocaml
  - java
  - llvm
#  - haskell #  Duplicated name: 'Lbl'Unds'LAYOUT'UndsHash'Layout'.
tests:
  - selector: ".semicolon-comments"
---

Productions for the `#Layout` sort are used to describe tokens that are considered "whitespace".
For example, below, we use it to define lines begining with `;` (semicolon) as comments.

```k
module LAYOUT
  syntax #Layout ::= r"(;[^\\n\\r]*)"
                   | r"([\\ \\n\\r\\t])" // Whitespace
                   
  syntax KItem ::= "foo"
endmodule
```

``` {.semicolon-comments .input}
foo ; buzz
; bar
```

``` {.semicolon-comments .expected}
<k>
  foo
</k>
```
