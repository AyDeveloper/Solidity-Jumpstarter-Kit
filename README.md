
# Solidity-Jumpstarter-Kit

Do you need a solidity-refresher or want your solidity-basic-muscles revitalised? 
This guide is intended to help beginners, developer(that seeks refresher) get familiar or acquainted with solidity syntax and smart contract in general.

## Inspiration

There are times where we want to look up on basic solidity concepts or need some refreshers? What if we could jump-start this process.



## Solidity Definition

Simply put, Solidity is an object-oriented, high-level language for implementing smart contracts. It has resemblance with other object-oriented programming language like C++, Javascript, Python et al but specifically designed to target the Ethereum Virtual Machine(EVM).

So if you write one of the highlighted languages, you can easily pick up solidity

## *Smart Contract Definition*

Smart contracts are simply programs stored on a blockchain that run when predetermined conditions are met.

## *Contract Declaration in Solidity*

A contract in this context is a collection of code(its function) and data (it’s state) which resides at a specific address on the Ethereum blockchain.

A Simple Smart Contract

```solidity

// This line tells us that the source code is licensed under the GPL version 3.0.
// SPDX-License-Identifier: GPL-3.0

// This line specifies that the source code is written for Solidity version 0.7.0 
// or a newer version of the language up to, but not including version 0.9.0.
pragma solidity >=0.7.0 <0.9.0;

// A contract is declared using a contract keyword followed by the contract name

contract Storage {

    // This line declares a state variable 
    // called number of type uint (unsigned integer of 256 bits).
    uint256 number;

    // This function store() can be used to modify the value of the variable
    function store(uint256 num) public {
        number = num;
    }

    // This function retrieve() can be used to retrieve the value of the variable.
    function retrieve() public view returns (uint256){
        return number;
    }
}

```



## *Data Types*

Solidity is a statically typed language, which means that the type of each variable (state and local) needs to be specified. The following explains useful data types to store variables.

**Boolean**

bool:  true or false

**Integer**

Unsigned : uint8,uint16,uint32,uint64,uint128,uint256(uint)

Signed : int8, int16, int32, int64, int128, int256(int)

**Address**

address: Holds a 20 byte value (size of an Ethereum address).

address payable : Same as address, but includes additional members transfer and send

*Methods:*

**balance**
* <address>.balance (uint256): balance of the Address in Wei

**transfer and send**
* <address>.transfer(uint256 amount): send given amount of Wei to Address, throws on failure.
* <address>.send(uint256 amount) returns (bool): send given amount of Wei to Address, returns false on failure.

**Mappings**

Mappings uses the syntax mapping(KeyType => ValueType) with its variable name in front.

Example Mapping(address => uint) balanceOf

Note: The KeyType can be any built-in value type, bytes, string, or any contract or enum type. Other user-defined or complex types, such as mappings, structs or array types are not allowed. 
ValueType can be any type, including mappings, arrays and structs.

**Struct**

We can define new types using structs as provided by Solidity.

```solidity
struct Funder {
    address addr;
    uint amount;
}

Funder funders;

```

**Arrays**

Arrays can be of fixed size, or they can have a dynamic size.

- uint[] dynamicSizeArray;

- uint[7] fixedSizeArray;

**Array members**: 
push, pop, length

## *Data Locations - Storage, Memory and Calldata*

Each variable declared and used in a contract has a data location. It specifies where the value of that variable should be stored. 

**Storage:** A storage variable is stored in the state of a smart contract and is persistent between function calls. It is the permanent residence of data which makes it accessible by all functions within a contract.

**Memory:** The memory location is stores data temporarily and cheaper than the storage location. It can only be accessible within the function. It is important to note that once the function gets executed, its values are discarded. 

**Calldata:** Calldata is non-modifiable and non-persistent data location where all the passing values to the function are stored. Also, Calldata is the default location of parameters (not return parameters) of external functions.

## *Functions* 

Functions are notated as follows:

function (<parameter types>) {internal|external} [pure|view|payable] [returns (<return types>)].

Input Parameters: are declared just like variables.

```solidity
function add(uint _x, uint _y) {}

```

Output Parameters: are declared after the return keyword

```solidity
function add(uint _x, uint _y) returns (uint _sum) {
   _sum = _x + _y;
}

```

Note: In contrast to the parameter types, the return types cannot be empty - if the function type should not return anything, the whole returns (<return types>) part has to be omitted.
