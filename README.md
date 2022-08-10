
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
## *Control Structure*

Most of the control structures known from object-oriented programming languages are available in Solidity:  The following are available in solidity:

* if else
* while
* do
* for
* break
* continue
* return
* Tenary operator(? :)


## *Modifiers: Custom and Access Modifiers*
Modifiers helps automatically check if certain pre-conditions have been met

**Custom Modifier**

As well as built-in Access Modifiers, you can also create your own.

```solidity
modifier onlyOwner {
    require(msg.sender == owner);
    _;
}
```

**Access modifiers**

* public - can be accessed from this contract, inherited contracts and externally
* private -can only be accessed from this contract
* internal -can be accessed only from this contract and contracts inheriting from it
* external - Cannot be accessed internally, only externally.



## *Interface*

An interface is the description of all functions that an object must have in order for it to operate. It includes function signatures without the implementation of the function definition.  

The following are the characteristics of an interface:

* An interface can be created with the “interface” keyword.
* All interface functions are implicitly virtual.
* To easily identifier an interface,  it’s naming can start with an “I” e.g IERC20
* Cannot have any functions implemented.
* Cannot inherit other contracts or interfaces.
* Cannot define constructor.
* Cannot declare state variables
* An interface function can be overridden.
* Functions of an interface can be only of type external

Example

```solidity
pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
}

contract Token {
    
    // state variable called ERC20ContractAddress that represents the address of the ERC20 contract
    address private constant ERC20ContractAddress = CONTRACTADDRESS;
    
    
    // to access the functions in the ERC20 contract
    // use the interface declared above and wrap the ERC20 contract address in ()  
    IERC20  TokenInstance = IERC20(ERC20ContractAddress) ;

    function getTotalSupply() external pure returns(uint) {
        return TokenInstance.totalSupply();
  }

  function getBalanceOf() external pure returns(uint) {
        return TokenInstance.balanceOf();
  }
  
}

```

## *Inheritance*

Inheritance is a way to extend functionality of a contract. Solidity supports both single as well as multiple inheritance.

Example

```solidity
contract Owner {
    address owner;
    function setOwner() public { 
        owner = msg.sender; 
    }
}

contract Suicide is Owner {
    function destroy() public {
        if (msg.sender == owner) selfdestruct(payable(owner));
    }
}

contract EndGame is Suicide {
    function kill() public { 
        super.destroy(); // Calls kill() of mortal.
    }
}
```

**Multiple inheritance**

```solidity
contract A {}
contract B {}
contract C is A, B {}


// Setting Constructor in base contract

contract A {
    uint a;
    constructor(uint _a) { a = _a; }
}

contract B is A {
    constructor(uint _b) A(_b) {
  }
}

```


## *Abstract Contract*

Abstract Contract is one which contains at least one function without any implementation i.e  in abstract contracts, at least one function lacks implementation. This type of contract cannot be compiled, but they can be used as base contracts.


```solidity
// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.8.0;

abstract contract HelloWorld {
   string public message;
    constructor(string memory _message){
       message = _message;
    }

    function getMessage() public virtual view returns (string memory){
        return message;
    }

    function setMessage(string memory _message) public virtual {}

    function defaultMessage() public  pure returns (string memory) 
    {
        return "Hello World";
    }
}

 contract UseHelloWorld is HelloWorld {

    uint public num;
    constructor(string memory _message, uint _num) HelloWorld(_message){
       num = _num;
    }

    function setNum(uint _num ) public {
        num = _num;
    }

    function getNum() public virtual view returns (uint256){
        return num;
    }

    function defaultNum() public pure returns (uint256){
         return 50;
     }

  function setMessage(string memory _message ) public override virtual {
        message = _message;
  }

 } 

 ```

## *Events*

Events allow the convenient usage of the EVM logging facilities. An event is emitted, and its arguments passed in transaction logs. 
Up to three parameters can receive the attribute indexed, which will cause the respective arguments to be searched for.
All non-indexed arguments will be stored in the data part of the log.

```solidity
pragma solidity ^0.8.0;

contract DepositEther {
    event Deposit(
        address indexed _from,
       	uint indexed _id,
        uint _value
    );

    function deposit(uint _id) payable {
        emit Deposit(msg.sender, _id, msg.value);
    }
}
```


## *Libraries and Using-For*

Libraries are similar to Contracts but are mainly intended for reuse. A Library contains functions which other contracts can call. 

Characteristics of a Solidity Library

* Library can not be destroyed as it is assumed to be stateless.
* A Library cannot have state variables.
* A Library cannot inherit any element.
* A Library cannot be inherited.

```solidity
library arithmetic {
    function multiply(uint _a, uint _b) returns (uint) {
        return _a  * _b;
    }
}

contract C {
    uint timesUp;

    function mul() {
       timesUp = arithmetic.multiply(5, 5);
    }
}
```

**Using-For**

using this for that; can be used to attach library functions to any type.


```solidity
library arithmetic {
    function add(uint _a, uint _b) returns (uint) {
        return _a + _b;
    }
}

contract C {
    using arithmetic for uint;
    
    uint sum;
    function sumUp(uint _a) {
        sum = _a.add(3);
    }
}
```

## *Error Handling and Custom Errors*

in Solidity, the assert(), require()and revert() functions are to check for conditions. In cases when conditions are not met, they throw exceptions.

* **require(bool condition, string memory message):** is used to validate inputs and conditions before execution.
* **assert(bool condition):** is used to check for code that should never be false. Failing assertion probably means that there is a bug.
* **revert():** is used abort execution and revert state changes

Example 


```solidity
pragma solidity ^0.8.0;

contract Donation {
    // input your default donor address
    address public donor = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
    uint constant DONATION_FEE = 2 ether;

    modifier onlyDonor() {
    require(msg.sender == donor,"Only buyer can call this.");
    _;
    }

    function setNewDonor(address _donor, address _recipient) public payable onlyDonor {
        uint bal = address(this).balance;
        uint amount = msg.value;
        if (msg.value <  DONATION_FEE) revert("Not enough Ether provided.");
        // Perform the buy operation.
        donor = _donor;
        payable(_recipient).transfer(amount);

        assert(address(this).balance == bal - amount);
    } 

}

```

## *Custom Errors*

Starting from Solidity v0.8.4 , there is now an additional way to revert a transaction.
With Custom Errors we can now revert transactions in a convenient and gas-efficient way and tell our users why a transaction failed. They are simply defined using the `error` statement. Also have similar syntax as events do. 

Example 

```solidity
contract CustomError {

    error InsufficientBalance(uint balance, uint withdrawAmount);

    function testCustomError(uint _withdrawAmount) public view {
        uint bal = address(this).balance;
        if (bal < _withdrawAmount) {
            revert InsufficientBalance({balance: bal, withdrawAmount: _withdrawAmount});
        }
    }

}

```

## *Global Variables and functions*

Special variables are globally available variables which provides information about the blockchain

* **block.blockhash**(uint blockNumber) returns (bytes32): hash of the given block - only works for the 256 most recent blocks excluding current
* **block.coinbase** (address): current block miner’s address
* **block.difficulty** (uint): current block difficulty
* **block.gaslimit** (uint): current block gaslimit
* **block.number** (uint): current block number
* **block.timestamp** (uint): current block timestamp as seconds since unix epoch
* **msg.data** (bytes): complete calldata
* **msg.gas** (uint): remaining gas
* **msg.sender** (address): sender of the message (current call)
* **msg.sig** (bytes4): first four bytes of the calldata (i.e. function identifier)
* **msg.value** (uint): number of wei sent with the message
* **tx.gasprice** (uint): gas price of the transaction
* **tx.origin** (address): sender of the transaction (full call chain)
* **addmod**(uint x, uint y, uint k) returns (uint): compute (x + y) % k where the addition is performed with arbitrary precision and does not wrap around at 2**256.
* **mulmod**(uint x, uint y, uint k) returns (uint): compute (x * y) % k where the multiplication is performed with arbitrary precision and does not wrap around at 2**256.
* **keccak256**(...) returns (bytes32): compute the Ethereum-SHA-3 (Keccak-256) hash of the (tightly packed) arguments
* **sha256**(...) returns (bytes32): compute the SHA-256 hash of the (tightly packed) arguments
* **sha3**(...) returns (bytes32): alias to keccak256
* **ripemd160**(...) returns (bytes20): compute RIPEMD-160 hash of the (tightly packed) arguments
* **ecrecover**(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address): recover the address associated with the public key from elliptic curve signature or return zero on error (example usage).
* **this** (current contract’s type): the current contract, explicitly convertible to Address
* **selfdestruct**(address recipient): destroy the current contract, sending its funds to the given Address.

## *Ether Units*

1 wei == 1
	
1 gwei == 1e9
	
1 szabo == 1e12
	
1 finney == 1e15
	
1 ether == 1e18

## *Other Important Keywords*

**Virtual:** A function marked as virtual allows a derived/inheriting contract to override its function behavior. This is suitable when you want to allow another contract that is inheriting from the current contract to override its functionality.

**Override:** A function that overrides a base function is marked as override. This occurs when you want to change the functionality of an inherited/derived contract function.

Example

```solidity
     contract A {
         function foo()  virtual public pure returns (string memory) {
             return "Foo from A";
         }
    

         function boo()  public pure returns (string memory) {
             return "Boo from A";
         }

     }
    

     contract B is A {
         function foo()  override public pure returns (string memory) {
             return "I override the Contract A's foo function";
         }
    

         function boo2()  public pure returns (string memory) {
             return "Boo from B";
         }
        
     }

```

**Variable packing:**

Whenever we declare a variable in our solidity code, the EVM stores it in a storage slot of 32 bytes (256 bits), which means there is a slot where the variable declared is stored. Simply put, whenever we declare a uint256 variable type, we have successfully filled a slot

Variable packing is literally checking the size of data types declared and fitting them sequentially. This is what is done under the hood by the EVM. In order to do this, we need to understand the size of each data type.

Uint256 is 32 bytes
Uint128 is 16bytes
Uint64 is 8bytes
Address and address payable is 20bytes
Bool is 1byte
String is 1byte per character


```solidity

contract VariablePacking {

// Example of an unpacked variable

Struct Unpacked {
	uint128 a,
	uint256 b,
	uint128 c
}

// Example of an packed variable

Struct Packed {
	uint128 a,
	uint128 b,
	uint256 c,
}

}
```

This approach is pretty much cool for gas optimisation.

**Immutable:**  Immutable state variables can be declared using the immutable keyword. State variable declared as immutable can only be assigned during contract creation, but will remain constant throughout the life-time of a deployed contract. 

uint256 immutable maxSupply = 6464;

**new:** The new keyword in Solidity deploys and creates a new contract instance. 

Example 

```solidity
contract Parent {
    function add(uint _a, uint _b) public pure returns (uint) {
        return _a + _b;
    }

     function getBalance() public view returns (uint) {
         return address(this).balance;
    }
}

contract Child {
    Parent newA = new Parent();

    function getAdd(uint _a, uint _b) public view returns (uint) {
       return newA.add(_a, _b);
    }

    function getBalance() public view returns (uint) {
       return newA.getBalance();
    }

}

```

**Constructor:** Constructor is a special function declared using constructor keyword. This function is used to initialise state variables of a contract. Following are the key characteristics of a constructor.
* A contract can have only one constructor.
* A constructor executed during contract deployment and it is used to initialise contract’s state.

Example: 

```solidity

contract Sample {
   uint num;
   constructor(uint _num) {
      data = _num;   
   }
}

```

**Receive function:** A contract can now have only one receive function that is declared with the syntax 
receive() external payable {…} (without the function keyword).

It executes on calls to the contract with no data (calldata), such as calls made via send() or transfer().

```solidity
contract ReceiveSample {
    event emitReceive(uint);

  receive() external payable {
        emit emitReceive(msg.value);
    }
}
```

**Fallback function:** fallback function does not take any arguments and does not return anything.

Characteristics 
* a function that does not exist is called or
* Ether is sent directly to a contract but receive() does not exist or msg.data is not empty
* Can be defined once per contract.
* It is mandatory to mark it external
* fallback has a 2300 gas limit when called by transfer or send.


```solidity
// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.8.0;

contract Fallback {
    event emitFallBack(uint, gasLeft);

    fallback() external payable {
        emit emitFallBack(msg.value, gasleft());
    }
}
```

**View functions:** Functions can be declared view in which case they promise not to modify the state, but can read from them.

```solidity
function sub(uint a) view returns (uint) {
    return a - b; //  where b is a storage variable
}
```

**Pure functions:** Functions can be declared pure in which case they promise not to read from or modify the state.

```solidity
function mul(uint a, uint b)pure returns (uint) {
    return a * b; //  where b is a storage variable
}
```

## *Developers Tool Kits and IDE’s*

**Solidity:** Most dominant Ethereum smart contracting language

**Foundry:** Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.

**Hardhat:** Hardhat is an Ethereum development environment. Compile your contracts and run them on a development network. Get Solidity stack traces, console.log and more.

**Truffle:** A world class development environment, testing framework and asset pipeline for blockchains using the Ethereum Virtual Machine (EVM), aiming to make life as a developer easier.


**Ethers js:** The ethers.js library aims to be a complete and compact library for interacting with the Ethereum Blockchain and its ecosystem.

**Web3 js:** web3.js is a collection of libraries that allow you to interact with a local or remote ethereum node using HTTP, IPC or WebSocket

**ApeWorx:** The Ethereum application development framework for Python Developers, Data Scientists, and Security Professionals.


**Metamask:** The leading crypto wallet available on browser extension and mobile. Trusted by over 30 million users to buy, store, send and swap crypto securely.

**Etherscan:** Etherscan is a Block Explorer and Analytics Platform for Ethereum, a decentralized smart contracts platform.

**Openzeppelin:** OpenZeppelin provides security products to build, automate, and operate decentralized applications.

**Brownie:** Brownie is a Python-based development and testing framework for smart contracts targeting the Ethereum Virtual Machine.

**Ethereum Stack Exchange:** Post and search questions to help your development life cycle.

**Biconomy:** Do gasless transactions in your dapp by enabling meta-transactions using simple to use SDK.

**Radicle:** Radicle is a new kind of code collab network built on Git. Radicle is a peer-to-peer stack for building software together.

**IPFS:** A peer-to-peer hypermedia protocol designed to preserve and grow humanity's knowledge by making the web upgradeable, resilient, and more open.

**Arweave:** Permanent decentralized data storage.

**Moralis:** Moralis provides a single workflow for building high performance dapps. Fully compatible with your favorite web3 tools and services.

**Ankr:** Access all the developer tools you need with remote node access, advanced APIs, and the largest multi-chain network of RPCs. Build with fast, reliable infrastructure.

**Spheron:** Decentralized web hosting which supports storage on Arweave, Skynet, IPFS & Filecoin.

**Remix:** Remix is a web IDE  that allows developing, deploying and administering smart contracts for Ethereum like blockchains. It can also be used as a learning platform.

**EthFiddle:** This browser-based Solidity IDE lets you compile test code, compiles the smart contract with helpful error handling, and lets you share the fiddles with friends and coworkers effortlessly.

**Vs Code:** Visual Studio Code extension that adds support for Solidity

**IntelliJ:**  Open-source plug-in for Jetbrains IntelliJ Idea IDE(free/commercial) with syntax highlighting, formatting, code completion etc.
