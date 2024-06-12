# haven

My hobby programming langauge.

## `haven-as`
`haven-as` will be a pseudo assembly compiler.

### `haven-as` Grammar

A function will have arguments, local variables, and a stack?
What about return types?


```
<Prim>      ::= int | real
<ID>        ::= [a-zA-Z_][a-zA-Z_0-9]*
<IntVal>    ::= (-?[1-9][0-9]*|0)
<RealVal>   ::= <IntVal>(\.[0-9]+)?r
<Val>       ::= <ID> | <IntVal> | <RealVal>
<BinOp>     ::= add | sub | mult | div | mod | lt | gt | lte | gte | eq | neq | and | or | xor | rsh | lsh
<UnOp>      ::= neg | not | flip

<MovStmt>       ::= <ID> $ <Val>
<BinStmt>       ::= <ID> $ <Val> <BinOp> <Val>
<SelfBinStmt>   ::= <ID> $ <BinOp> <Val>
<UnStmt>        ::= <ID> $ <UnOp> <Val>
<SelfUnStmt>    ::= <ID> $ <UnOp>

<PropValList>   ::= <Val> | <PropValList>, <Val>
<ValList>       ::= e | <PropValList>

<CallStmt>      ::= <ID> $ <ID> {ValList}
<VoidCallStmt>  ::= <ID> {ValList}

<PushStmt>      ::= push <Val>
<PopStmt>       ::= pop <ID>?
<PeekStmt>      ::= peek <ID>
<DoStmt>        ::= (do|doip) <Prim>? (<BinOp>|<UnOp>)

<Label>         ::= @[a-zA-Z_0-9]+
<JumpStmt>      ::= jump <Label> (when <ID>)?
<ReturnStmt>    ::= return <Val>?

// Comments will just go until the newline than stop.
<Comment>       ::= #.*\n

<PartialStmt>   ::= <MovStmt>
                |   <BinStmt>
                |   <SelfBinStmt>
                |   <UnStmt>
                |   <SelfUnStmt>
                |   <CallStmt>
                |   <VoidCallStmt>
                |   <PushStmt>
                |   <PopStmt>
                |   <PeekStmt>
                |   <DoStmt>
                |   <JumpStmt>
                |   <ReturnStmt>

<FullStmt>      ::= <Label>? <PartialStmt>;

<IDPair>        ::= <Prim> <ID>
<IDPairBlock>   ::= { (<IDPair>;)* }
<Func>          ::= func <ID> 
                    <IDPairBlock>? (=> <PrimType>)?
                    lcls <IDPairBlock> 
                    stack <IntVal>
                    text { <FullStmt>* }

```

```
func isPrime
{ int num; } => int
lcls {
    int i;
    int cond;
    int modRes;
}
stack 0
text {
    i $ 2;

    @loop modRes $ num % i;
    jump @continue when modRes 

    # We only make it in here when modRes is 0.
    return 0;

    @continue i $ + 1;
    cond $ i >= num; 
    jump @loop_exit when cond;
    jump @loop;

    @loop_exit return 1;
}

func doSomeMath
{ int a; int b; int c; } => int
lcls { int retval; }
stack 12
text {
    // calc (b * (a + b + c)) / (a % b)
    push b
    push a
    do mod

    push c
    push b
    do add

    push a
    do add

    push b
    do mult

    do div

    pop retval; 
    return retval;
}
```
