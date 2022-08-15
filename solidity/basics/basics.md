## Hello World

- Mention license at very top
- Solidity version (setting it to be compiled by version greater than 0.8.7)

```ts
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract HelloWorld{
    string public myString = "hello world";
}
```

## Value Types

```
Solidity
|
|----- values
|
|----- references
```

Types in solidity include:

- bool
- uint `uint256: 0 to 2**256 -1` `uint8: 0 to 2**8 -1`
- int

so that you don't have to remember max and min values

```ts
contract ValueTypes{
    int public minInt = type(int).min;
    int public maxInt = type(int).max;
}
```

- address
- bytes32: output when you use keccak256

## Functions

```ts
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract FunctionIntro{
    // external: only accounts outside the contract can call this function
    // pure: function doesn't change state of blockchain
    function add(uint256 x, uint 256 y) external pure return(uint256) {
        return x + y;
    }
}
```

## State Variables

These store data on the blockchain

```ts
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract StateVariables{
    uint256 public myUint = 123; // state variable
    function foo() external {
        uint256 notStateVariable = 234; // not a state variable
    }
}
```

## Local Variables

These store data on the blockchain

```ts
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract StateVariables{
    uint256 public myUint = 123; // state variable
    function foo() external {
        uint256 notStateVariable = 234; // not a state variable
    }
}
```

## Global Variables

List of global variables by default:

- msg.sender (address)
- block.timestamp (uint): current unix time
- block.number (uint): current block

```ts
contract GlobalVariables{
    function globalVars() external view returns(address, uint256, uint256){
        address sender = msg.sender;
        uint256 time = block.timestamp;
        uint256 number = block.number;
        return(sender, time, number)
    }
}
```

## View and Pure Functions

View functions can read state variable of blockchain (storage) pure can't

```ts
contract ViewAndPureFunctions{
    uint public num;
    function viewFunc() external view returns(uint){
        return num;
    }

    function pureFunc() external pure returns(uint){
        return 1;
    }
}
```

## Default Values

```ts
contract DefaultValues{
    bool public b; // default: false
    uint public u; // default: 0
    int public i; // default: 0
    address public a; // default: 0x0..(40 zeros)..0
    bytes32 public bt; // default: 0x0..(64 zeros)..0

}
```

## Constants

Set variables as constants to reduce gas fees

```ts
contract Constants{
    address public constant MY_ADDRESS = 0x657D3C03e450E4815f3411Aa26713A2A90e9Ad83
    uint public constant MY_UINT = 123;
}
```
