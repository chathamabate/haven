# haven

My hobby programming langauge.

## `haven-as`
`haven-as` will be a pseudo assembly compiler.

### `haven-as` Grammar

A function will have arguments, local variables, and a stack?
What about return types?

```
Types:
    real: 64-bit floating point number.
    int: 64-bit signed integer.

Operations:
  Simple Arithmetic:
    +   add 
    -   subtract
    *   multiply 
    /   divide 
    %   modulus

  Comparisons:
    =   Eq
    !=  Neq
    <   Lt
    >   Gt
    <=  Lte
    >=  gte

* For all operations, if one argument is real
the second argument will be converted to real (if needed)
* Modulus will only work on integral values.
If either is real, an error will be thrown.

Instruction Set:

 Simple Variable Based:
    <Id> $ <Id>    // Transfer values from IDs. (Casting is done if possible)
    <Id> $ <Cnst>  // Assign a constant to an ID. 
    <Id> $ <Id> <Op> <Id>  // Perform a simple operation and store result.
    
 Stack Based:
    pushq <Id>     // Push 64-bit values onto the operand stack.
    pushq <Cnst>

    popq <Id>      // Pop 64-bit value into an ID.  (With the stack, little type checking can really be done)
    popq           // Just pop a 64-bit value and throw it out.

    peekq <Id>     // Simply store the top of the stack into an ID.

    doq int  <Op>  // Perform a 64-bit integral stack operation.
    doq real <Op>  // Performa a 64-bit floating point stack operation.
    
    doipq int <Op>  // These are the same as the 2 above, just without popping
    doipq real <Op> // any values from the operand stack.

 Control Flow: 



func myFunc(args...) => return type 
lcls {
    int x2;
    real x3; 
}
code {
    push64 

    x $ x + y;  // Simple addition here...

    ... Instructions ??
    ... Stack based stuff??
    ... Different operators for different types??
    ... Labels and stuff???

}
```
