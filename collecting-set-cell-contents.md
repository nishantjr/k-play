---
backends:
  - ocaml
tests:
  - selector: ".x"
---


```k
module COLLECTING-SET-CELL-CONTENTS
    imports BOOL
    imports SET
    imports INT

    configuration <k> #init ~> #collectFoos ~> $PGM </k>
                  <foos> <foo multiplicity="*" type="Set"> 1 </foo> </foos>

    syntax KItem ::= "#init"
                   | "#collectFoos"
    rule <k> #init => .K ... </k>
         <foos> _
             => <foo> 3 </foo>
                <foo> 6 </foo>
                <foo> 9 </foo>
                <foo> 7 </foo>
         </foos>

    syntax KItem ::= "#collectFoos"
    rule <k> #collectFoos => collectFoos(.Set) ... </k>

    syntax Set ::= collectFoos(Set) [function]
    rule [[ collectFoos(S) => collectFoos(SetItem(I) S) ]]
        <foo> I </foo>
      requires notBool(I in S)
       andBool I %Int 2 ==Int 1
    rule collectFoos(S) => S [owise]
endmodule
```

``` {.x .input}
1
```

``` {.x .expected}
TODO: WRITEME
```
