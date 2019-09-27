---
backends:
#  - java # Fails because of different output (`SetItem()` around cells)
  - haskell
  - ocaml
  - llvm
tests:
  - selector: ".amb-underbar"
  - selector: ".amb-sharp"
---

Consider the following naive attempt at creating a language whos syntax
allows two types of variables: Names that contain underbars, and names that
contain sharps/hashes/pound-signs:

```k
module TOKENS-AND-PARSING-NAIVE
  syntax NameWithUnderbar ::= r"[a-zA-Z][A-Za-z0-9_]*"  [token]
  syntax NameWithSharp    ::= r"[a-zA-Z][A-Za-z0-9_#]*" [token]
  syntax Pgm ::= underbar(NameWithUnderbar)
               | sharp(NameWithSharp)
endmodule
```

Although, it seems that K has enough information to parse the programs
`underbar(foo)` and `sharp(foo)` with, the lexer does not take into account
whether a token is being parsed for the `sharp` or for the `underbar`
production. It chooses an arbitary sort for the token `foo` (perhaps
`NameWithUnderbar`). Thus, during paring it is unable to construct a valid term
for one of those programs (`sharp(foo)`) and produces the error message:
`Inner Parser: Parse error: unexpected token 'foo'.`

Since calculating inclusions and intersections between regular expressions is
difficult, we must provide this information to K. We do this via the `prec(N)`
attribute that specifies the order in which the lexer tries tokens. Token
productions with higher precedence are tried first.

We also need to make sorts with more specific tokens subsorts of ones with more
general tokens. We add the token attribute to this production so that all tokens
of a particular sort are marked with the sort it is parsed as, and not a subsort
thereof. e.g. we get `underbar(#token("foo", "NameWithUnderbar"))` instead of
`underbar(#token("foo", "#LowerId"))`

```k
module TOKENS-AND-PARSING
  imports BOOL
```

The `BUILTIN-ID-TOKENS` module defines `#UpperId` and `#LowerId` with attributes `prec(2)`

```k
  imports BUILTIN-ID-TOKENS
```

```k
  syntax NameWithUnderbar ::= r"[a-zA-Z][A-Za-z0-9_]*" [prec(1), token]
                            | #UpperId                [token]
                            | #LowerId                [token]

  syntax NameWithSharp ::= r"[a-zA-Z][A-Za-z0-9_#]*" [prec(1), token]
                         | #UpperId                 [token]
                         | #LowerId                 [token]

  syntax Pgm ::= underbar(NameWithUnderbar)
               | sharp(NameWithSharp)
endmodule 
```

``` {.amb-underbar .input}
underbar(foo)
```

``` {.amb-underbar .expected}
<k>
  underbar ( foo )
</k>
```

``` {.amb-sharp .input}
sharp(foo)
```

``` {.amb-sharp .expected}
<k>
  sharp ( foo )
</k>
```
