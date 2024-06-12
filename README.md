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

<Attr>      ::= @INT | @REAL | @NO_POP
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

<MovOp>     ::= mov
<MathOp>    ::= add | sub | mult | div | mod | lt | gt | lte | gte | neg
<BitOp>     ::= not | flip | and | or | xor | rsh | lsh | eq | neq
<StackOp>   ::= push | pop
<CntrlOp>   ::= jump | return

__Move :__ `mov`
    * Move a value into a destination. The given value is always casted to 
      the type of the destination.

__Simple Math :__ `add`, `sub`, `mult`, `div`, `neg`
    * These will always cast operands to the type of the destination.
    * When no operands are given (stack based), the user can provide a type attribute
      to specify whether floating point or integral math should be done.
      Use `@REAL` to specify a `real` operation. Use `@INT` to specify an integral operation.

__Math Comparisons :__ `lt`, `gt`, `lte`, `gte`
    * Destination must be an `int`. 
    * If one operand being compared is of type `real` and the other is of type `int`, the `int` value
      will be cast to a `real` before the comparison is executed.
    * Use `@REAL` or `@INT` attribute to specify when stack based.

__Bitwise Math :__ `flip`


