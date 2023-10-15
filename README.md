# Gas optimization for Solidity contracts

## Use bit shift operators for arithmetic operations
- Bit shift operators `<<` and `>>` are likely to be cheaper.
- Note: Bit shift operators do not revert on overflow.

Code:
```
x >> 4; // x / 16
x << 4; // x * 16
x & 0xff; // x % 256
```

## External modifier is cheaper than public
- `public` modifier : function can be called both internally and externally
- `external` modifier : function can only be called externally


