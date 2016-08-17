State Machine Parsers
=====================================

## Match Sets

A parser may have common sets of characters it handles in different states.
The most efficient way to match the characters depends on how many sets they are,
whether they are contiguous, which encoding you are parsing, etc.  So instead
of putting the match logic in the parser, this logic can be shared and generalized.
A match set is a list of character sets.  The implementation can decide
how the sets are matched outside of the parser.



Example:
```
// T = 
statemachine(T, bool checkOverload)
{
    T value;
    INITIAL_STATE {
        c = consumeChar();
        match(c) {
            '0'     : value = 0; goto DONE;
            '1'-'9' : value = c - '0'; goto STATE2;
            default : goto BAD_CHAR;
        }
    }
    STATE2 {
        c = peekChar();
        match(c) {
            '0'-'9' :
                consumePeek();
                value *= 10;      // TODO: Use checkOverload version if set
                value += c - '0'; // TODO: Use checkOverload version if set
                goto STATE2;
            default : discardPeek(); goto DONE;
        }
    }
}

```

