# haven

My hobby programming langauge.

## `haven-as`
`haven-as` will be a pseudo assembly compiler.
### Instructions

Note that in the below definitions, a `<Val>` is either an `<ID>`, `<RealVal>`, or `<IntVal>`.

#### `mov`
##### Forms
 * `mov` `<ID>`, `<Val>`
   * Equivelant to `<ID> <- <Val>`.

#### `add`, `sub`, `mult`, `div`
##### Forms
 * `add`  
.. Maybe it's time to go to bed..

#### `mod`
#### `lt`
#### `gt`
#### `lte`
#### `gte`
#### `neg`

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
<IDPairBlock>   ::= { (<IDPair>;)+ }
<Func>          ::= func <ID> 
                    <IDPairBlock>? (=> <PrimType>)?
                    (lcls <IDPairBlock>)?
                    (stack <IntVal>)?
                    text { <Line>+ }
<Sig>           ::= extern func <ID> <IDPairBlock>? (=> <PrimType>)?

<Program>       ::= (<Func>|<Sig>)*
```

### Lexemes
```
<ID>        ::= [a-zA-Z_][a-zA-Z_0-9]*
<IntVal>    ::= (-?[1-9][0-9]*|0)
<RealVal>   ::= <IntVal>(\.[0-9]+)?r
<ResWords>  ::= mov, add, sub, mult, dif, mod, lt, gt, lte, gte, neg
                not, flip, and, or, xor, rsh, lsh, eq, neq, push, pop,
                jump, return, call, int, real, func, lcls, stack, text, extern
<Attrs>     ::= @INT, @REAL
<Symbols>   ::= \, {, }, :, ;, =>
```

### Example Programs
```
func isPrime
{ int num; } => int
lcls {
    int i;
    int rem;
    int cond;
}
text {
    mov i, 2;
loop:
    mod rem, num, i; 
    jump continue, rem;

    return 1;

continue:
    add i, 1;
    lt cond, i, num;    
    not cond;
    jump exit, cond; 
    jump loop;
    
exit:
    return 0;
}

# This function will return a random real number between 0.0 and 1.0.
extern func rand => real

# Montecarlo pi calculation.
func calcPi
{ int trials; } => real
lcls {
    int hits;
    int t;
    
    real x;
    real y;
    real mag;

    int cond;

    real pi;
} 
stack 32
text {

    mov t, 0;
loop:
    call x, rand;
    call y, rand;

    push x;
    push x;
    mult @REAL;
    
    push y;
    push y;
    mult @REAL;
    
    add @REAL;

    pop mag;
    gt cond, mag, 1.0r;

    jump skip, cond;
    add hits, 1;    

skip: 
    add t, 1;
    gte cond, t, numTrials;
    jump exit, cond;
    jump loop;

exit:
    push @REAL trials;
    push @REAL hits;
    div @REAL;
    push 4.0r;
    mult @REAL;

    pop pi;
    return pi;
}
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
    * When operands are given, default casting can be overriden with a type attribute. For example,
      `div @real x, 1, 2;` will convert `1` and `2` to `real`s before the division occurs. `0.5r` will
      be stored in `x` given its type is `real`.

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
    * Type attributes can be given in `push` to perform a cast before pushing
      values onto the stack. For example, `push @INT 1;` would push `1.0r` onto 
      the stack.


