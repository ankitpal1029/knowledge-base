# Assembly

To start off use `assembly` inside any function

```ts
contract A{
    function foo() external {
        uint a;
        uint b;
        uint c;

        c = a + b;
        uint size;
        address addr = msg.sender;

        assembly{
            // you can define local varibles by using
            let d;
            // and reassign using
            c := add(1,2) // no semicolons
            // to access certain variables use load
            let a:= mload(0x40)
            // to store a variable in memory
            mstore(a, 1) // a is address of destination, payload
            // to store to storage
            sstore(a, 10)

            // return size of code
            size := extcodesize(addr) // if output is 0 it is a regular address, else it is
            // a smartcontract
        }
        if(size > 0){
            return true;
        }else{
            return false;
        }

    }
}
```

Every data type occupies slots which are 256 bytes big, arrays and mapping take more slots

## Dealing with bytes32

```ts
contract A{
    function foo() external {
        uint a;
        uint b;
        uint c;

        bytes memory data = new bytes(10);
        // bytes32 dataB32 = bytes32(data);
        // will error out cause bytes32 is smaller in size can use assembly to save us

        bytes32 dataB32;
        assembly{
            // first memory slot is address of the byte, so we start from second
            dataB32 := mload(add(data, 32))
        }


        assembly{

    }
}
```
