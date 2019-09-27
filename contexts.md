---
backends:
  - ocaml
tests:
  - selector: ".pp-term"
  - selector: ".pp-term-amb"
  - selector: ".ambFirst"
---


```k
module CONTEXTS
    imports STRING
    
    syntax KItem ::= KItem "->" KItem [left]

    syntax Term ::= "a" | "b" | const(String)
                  | apply(Term, Term)
                  | amb(Term, Term)
    rule a => const("a") [macro]
    rule b => const("b") [macro]

    syntax KItem ::= visit(Term, Visitor)
    syntax Term ::= "#hole"
    syntax Term ::= VisitorResult
    syntax VisitorResult ::= Visitor "(" K ")" 
    
    rule <k> visit(apply(T1, T2), V)
          => visit(T1, V) ~> visit(apply(#hole, T2), V) 
             ...
         </k>
      requires notBool isVisitorResult(T1)
    rule <k> V1:VisitorResult ~> visit(apply(#hole, T2), V)
          => visit(apply(V1, T2), V)
             ...
         </k>
    rule <k> visit(apply(T1:VisitorResult, T2), V)
          => visit(T2, V) ~> visit(apply(T1, #hole), V) 
             ...
         </k>
      requires notBool isVisitorResult(T2)
    rule <k> V2:VisitorResult ~> visit(apply(T1, #hole), V)
          => visit(apply(T1, V2), V)
             ...
         </k>
         
    rule <k> visit(amb(T1, T2), V)
          => visit(T1, V) ~> visit(amb(#hole, T2), V) 
             ...
         </k>
      requires notBool isVisitorResult(T1)
    rule <k> V1:VisitorResult ~> visit(amb(#hole, T2), V)
          => visit(amb(V1, T2), V)
             ...
         </k>
    rule <k> visit(amb(T1:VisitorResult, T2), V)
          => visit(T2, V) ~> visit(amb(T1, #hole), V) 
             ...
         </k>
      requires notBool isVisitorResult(T2)
    rule <k> V2:VisitorResult ~> visit(amb(T1, #hole), V)
          => visit(amb(T1, V2), V)
             ...
         </k>
         
    rule <k> P:Term ~> V:Visitor
          => visit(P, V)
         ...
         </k>
         
    rule <k> _:Visitor(P) ~> V:Visitor => visit(P, V)
         ...
         </k>

    syntax Visitor ::= "pp"
    rule <k> visit(const(S), pp) => pp(S) ... </k>
    rule <k> visit(apply(pp(S1), pp(S2)), pp)
          => pp("(" +String S1 +String " " +String S2 +String ")")
             ...
         </k>
    rule <k> visit(amb(pp(S1), pp(S2)), pp)
          => pp("[" +String S1 +String " \\/ " +String S2 +String "]")
             ...
         </k>

    syntax Visitor ::= "ambFirst"
    rule <k> visit(const(_) #as C, ambFirst)
          => ambFirst(C)
             ...
         </k>
    rule <k> visit(apply(ambFirst(S1), ambFirst(S2)), ambFirst)
          => ambFirst(apply(S1, S2))
             ...
         </k>
    rule <k> visit(amb(ambFirst(S1), ambFirst(S2)), ambFirst)
          => ambFirst(S1)
             ...
         </k>
    
endmodule
```

``` {.pp-term .input}
visit(apply(apply(a, b), apply(b, a)), pp)
```

``` {.pp-term .expected}
<k>
  pp ( "((a b) (b a))" )
</k>
```

``` {.pp-term-amb .input}
visit(apply(apply(a, b), apply(amb(b, a), a)), pp)
```

``` {.pp-term-amb .expected}
<k>
  pp ( "((a b) ([b \\/ a] a))" )
</k>
```

``` {.ambFirst .input}
visit(apply(apply(const("a"), const("b")), apply(amb(const("b"), const("a")), const("a"))), ambFirst)
```

``` {.ambFirst .expected}
<k>
  ambFirst ( apply ( apply ( const ( "a" ) , const ( "b" ) ) , apply ( const ( "b" ) , const ( "a" ) ) ) )
</k>
```

``` {.ambFirst .input}
apply(apply(const("a"), const("b")), apply(amb(const("b"), const("a")), const("a"))) -> ambFirst -> pp
```
