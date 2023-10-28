# Gas optimization for Solidity contracts

Gas: unit of measurement for the amount of computational effort required to carry out specific operations on the Ethereum network

When a smart contract is compiled in Solidity it is converted into a series of operation codes (opcodes).
Each opcode is assigned a predefined amount of gas which represents the computing work necessary to carry out that operation.

Bytecodes provide the hexadecimal version of opcodes and are executed on the EVM.

Optimization aims to reduce the overall number of operations needed to run a smart contract.

## Cheaper operations
- Reading constants and immutable variables
- Reading and writing local variables
- Reading and writing memory variables like memory arrays and structs
- Reading calldata variables like calldata arrays and structs
- Internal function calls

## Expensive operations
- Reading and writing state variables stored in contract storage
- External function calls
- Loops

## Use mappings instead of arrays


## Optimizing state reads and writes in loops
- Replace state variable reads and writes in loops with local variable reads and writes

Unoptimized code:
```
function doIt() external {                
  for(uint256 i; i < myArray.length; i++) { // state reads
    myCounter++; // state reads and writes
  }        
}
```

Optimized code:
```
function doIt() external {
    uint256 length = myArray.length; // one state read
    uint256 local_mycounter = myCounter; // one state read
    for(uint256 i; i < length; i++) { // local reads
        local_mycounter++; // local reads and writes  
    }
    myCounter = local_mycounter; // one state write
}
```
- Optimized code ensures state reads and writes are done only once instead of multiple times.

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

## Packing structs
- When values are stored in a smart contract, 256 bits are stored at a time.
- It is cheaper if multiple variables can be fit into one storage slot of 256 bits.
- Smaller data types such as uint128 and uint96 should be placed next to each other.
- Packing structs reduces both contract storage reads and writes because you can get the values of multiple variables in one state read or state write.

Code:
```
struct MyStruct {
  uint96 time;
  address storedAddress;
}
```
- In the above the data types for `time` and `storedAddress` (160 bits) both add up to 256 bits.

## Solidity Gas Optimizer
- Solidity's gas optimizer helps to reduce gas costs.
- Optimize for contract deployment (cost incurred while deploying a contract): set the optimizer to a low number
- Optimize for run-time gas cost (cost incurred when functions are called on a contract): set the optimizer to a high number

## Reducing multi-contract gas costs


# References
- [Alchemy: 10 Expert Solidity Gas Optimization Techniques (2022)](https://www.alchemy.com/overviews/solidity-gas-optimization)
- [EIP-2535 Diamonds Substack: Basics of Smart Contract Gas Optimization with Solidity (2022)](https://eip2535diamonds.substack.com/p/smart-contract-gas-optimization-with)
- [EIP-2535 Diamons Substack: How EIP2535 Diamonds Reduces Gas Costs for Smart Contract Systems (2022)](https://eip2535diamonds.substack.com/p/how-eip2535-diamonds-reduces-gas) (TBR)
