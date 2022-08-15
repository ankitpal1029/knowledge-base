# Solidity New Features

New features of solidity:

- safe math
  If you want your numbers to overflow/underflow use unchecked  
  the below function won't throw an error upon underflow:

```java
    contract{
        function uncheckedUnderflow() public pure returns (uint){
            uint x = 0;
            unchecked {x--};
            return x;
        }
    }
```

- custom errors  
  Majorly for gas savings

```java
    error Unauthorized(address caller);
    contract{
        function withdraw() public pure returns (){
            if(msg.sender != owner){
                revert Unauthorized(msg.sender);
            }
        }
    }
```

- functions outside contracts  
  can't have state variables or the `this` keyword outside contract

```java
    function helper(uint x ) view returns (uint){
        return x*2;
    }
    contract{
        function math() public pure returns (uint){
            return helper(123);
        }
    }
```

- new import styles

```ts
    import { Unauthorized, helper as h1} from "./Sol.sol";

    function helper(uint x ) view returns (uint){
        return x*2;
    }

    contract{
        function math() public pure returns (uint){
            return helper(123);
        }
    }
```

- salted contract creations

For deploying contracts from other contracts  
address obtained from `getAddress` function and the address by deploying this way is the same

```ts
contract D {
    uint public x;
    constructor(uint a){
        x = a;
    }
}

contract Create2{
    function getBytes32(uint salt) external pure returns(bytes32){
        return bytes32(salt);
    }

    function getAddress(bytes32 salt, uint arg) external view returns(address){
        address addr = address(uint160(uint(keccak256(abi.encodePacked(
            bytes1(0xff),
            address(this),
            salt,
            keccak256(abi.encodePacked(
                type(D).creationCode,
                arg
            ))
        )))))
        return addr;
    }
    function createDSalted(bytes32 salt, uint arg) public {
        D d = new D{salt: salt}(arg);
        deployedAddress = address(d);
    }
}
```
