# haven

My hobby programming langauge.

## `haven-as`
`haven-as` will be a pseudo assembly compiler.

### Grammar
```
<ID>        ::= [a-zA-Z_][a-zA-Z_0-9]*
<IntVal>    ::= (-?[1-9][0-9]*|0)
<RealVal>   ::= <IntVal>(\.[0-9]+)?r
<Val>       ::= <ID> | <IntVal> | <RealVal>

<MovOp>     ::= mov
<MathOp>    ::= add | sub | mult | div | mod | lt | gt | lte | gte | neg
<BitOp>     ::= not | flip | and | or | xor | rsh | lsh | eq | neq
<StackOp>   ::= push | pop
<CntrlOp>   ::= jump | return
<Op>        ::= <MathOp> | <BitOp> | <StackOp> | <CntrlOp>

<ValList>   ::= <Val> | <PropValList>, <Val>
<CallArgs>  ::= { <ValList>? }

<Attr>      ::= @INT | @REAL
<AttrList>  ::= <Attr> | <AttrList> <Attr>
<OperandList> ::= <Val> | <Val>, <Val> | <Val>, <Val>, <Val>

<Stmt>      ::= <Op> <AttrsList>? <OperandList>? 
            |   call (<ID>,)? <ID> <CallArgs>?

<Line>      ::= (<ID>:)? <Stmt>;

<IDPair>        ::= <Prim> <ID>
<IDPairBlock>   ::= { (<IDPair>;)* }
<Func>          ::= func <ID> 
                    <IDPairBlock>? (=> <PrimType>)?
                    (lcls <IDPairBlock>)?
                    (stack <IntVal>)?
                    text { <Line>+ }
```


### Operation Semantics

__Move :__ `mov`
    * Move a value into a destination. The given value is always casted to 
      the type of the destination.

__Simple Math :__ `add`, `sub`, `mult`, `div`, `neg`
    * If executing any of these operations requires the combination of a `real` and an `int`,
      the `int` value will be cast to a `real` before the operation begins.
    * If the destination has a type which differs from the resultant value. The resultant
      value will be cast to the destination type after evaluation. For example, let `int a; real b = 2.5r; int c = 2;`.
      The operation `mult a, b, c;` will store the integer value `5` in `a`.
    * When no operands are given (stack based), the user can provide a type attribute
      to specify whether floating point or integral math should be done.
      Use `@REAL` to specify a `real` operation. Use `@INT` to specify an integral operation.

__Mod Math :__  `mod`
    * Modulus requires only `int` operands.

__Math Comparisons :__ `lt`, `gt`, `lte`, `gte`
    * Destination must be an `int`. 
    * If one operand being compared is of type `real` and the other is of type `int`, the `int` value
      will be cast to a `real` before the comparison is executed.
    * Use `@REAL` or `@INT` attribute to specify when stack based.

__Bitwise Logic :__ `flip`, `and`, `or`, `xor`
    * Types of operands and destinations are ignored for these operations.

__Bitwise Shifts :__ `lsh`, `rsh`
    * The shift amount operand must be an `int`.

__Bitwise Comparisons :__ `eq`, `neq`, `not`
    * Destination must be an `int`. 
    * Operand(s) can be any type.

__Stack Based :__ `push`, `pop`, `peek`



