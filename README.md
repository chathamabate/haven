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

<MathOp>    ::= add | sub | mult | div | mod | lt | gt | lte | gte | neg
<BitOp>     ::= not | flip | and | or | xor | rsh | lsh | eq | neq
<StackOp>   ::= push | pop
<CntrlOp>   ::= jump | return
<Op>        ::= <MathOp> | <BitOp> | <StackOp> | <CntrlOp>

<ValList>   ::= <Val> | <PropValList>, <Val>
<CallArgs>  ::= { <ValList>? }

<Attr>      ::= @INT | @REAL | @NO_MUT
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


### Valid Statement Attributes
* `@REAL` or `@INT` attributes:

