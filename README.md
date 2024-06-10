# haven

My hobby programming langauge.

## `haven-as`
`haven-as` will be a pseudo assembly compiler.

### `haven-as` Grammar

A function will have arguments, local variables, and a stack?
What about return types?

```
func myFunc
{
    int x;
    real y;
} => int
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
