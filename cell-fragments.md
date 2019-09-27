---
backends:
  - ocaml
#  - java  # Incorrect output format
  - llvm
#  - haskell
tests:
  - selector: ".collect-foos"
---

K allows matching fragments of the configuration and using them to construct
terms and use as function parameters.

```k
module CELL-FRAGMENTS
    imports BOOL
    imports SET
    imports INT
```

This configuration contains a `<foo>` cell with multiplicity `*`.

```k
    syntax KItem ::= "#init"
    configuration <t>
                    <k> #init ~> #collectOddFoos ~> $PGM </k>
                    <foos>
                      <foo multiplicity="*" type="Set"> 1 </foo>
                    </foos>
                  </t>
```

The `#collectOddFoos` construct grabs the entire content of the `<foos>` cell.
We may also match on only a portion of its content. Note that the fragment
must be wrapped in a `<foo>` cell at the call site.

```k
    syntax KItem ::= "#collectOddFoos"
    rule <k> #collectOddFoos => collectOddFoos(<foos> FOOS </foos>) ... </k>
         <foos> FOOS </foos>
```

Next the `collectOddFoos` function collects the items it needs

```k
    syntax Set ::= collectOddFoos(FoosCell) [function]
    rule collectOddFoos(<foos> <foo> I </foo> REST </foos>)
      => SetItem(I) collectOddFoos(<foos> REST </foos>)
      requires I %Int 2 ==Int 1
    rule collectOddFoos(<foos> <foo> I </foo> REST </foos>)
      => collectOddFoos(<foos> REST </foos>)
      requires I %Int 2 ==Int 0
    rule collectOddFoos(<foos> .Bag </foos>) => .Set
```

```k
    rule <t>
           <k> #init => .K ... </k>
           <foos> _
               => <foo> 3 </foo>
                  <foo> 6 </foo>
                  <foo> 9 </foo>
                  <foo> 7 </foo>
           </foos>
         </t>
```

```k
endmodule
```

``` {.collect-foos .input}
1
```

``` {.collect-foos .expected}
<t>
  <k>
    SetItem ( 3 )
    SetItem ( 7 )
    SetItem ( 9 ) ~> 1
  </k>
  <foos>
    <foo>
      3
    </foo> <foo>
      6
    </foo> <foo>
      7
    </foo> <foo>
      9
    </foo>
  </foos>
</t>
```
