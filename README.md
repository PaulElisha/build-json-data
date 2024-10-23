## CreateJsonStructure

This is a tool useful for creating data using JSON structure and format.

## Why use it

- For data gathering which can be stored in a merkle tree for efficient hashing and storage in a test environment.

- For building immutable dynamic data, stored in strings efficiently without consuming storage space, such that it can be read for verification of data passed into a contract.

- For building and managing NFT metadata associated with an ERC721 token such that reliance on an external centralized system is removed. 

- For easier integration with off-chain services that expects or handles data in this format.

- For dynamic data storage and management.

## Requirements

- Solidity v0.8.0^

## Installation

```shell
forge install PaulElisha/CreateJsonStructure
```

## Functions

- startMainObject - Entry point to creating a Json data.
- startObject - Entry point to creating an object.
- addKeyValuePairWith<dataType> - Used for add key-value pair.
- startNestedObjectInArray - Used for creating nested object in an array.
- addArrayElementWith<dataType> - Used to add array element.
- StartArray - Used to start an array.

To every 'start' method, ensure you close them with the required functions.

## How to use

- Create a target file in your script folder. In this case, I used this path: "script/target/input.json"
- Assign the target file to a variable like this: `string private constant INPUT_PATH = "script/target/input.json"`;
- Copy the fs_permissions configuration in the foundry.toml file and paste in your foundry config file to enable write permission to access your project directory, it works for read permission too.

- Example: 

```solidity
// SPDX-License-Identifier: SEE LICENSE IN LICENSE
pragma solidity ^0.8.0;

import "forge-std/Script.sol";
import "forge-std/StdJson.sol";
import "../src/CreateJsonStruct.sol";

contract CreateJsonStructScript is Script {
    CreateJsonStruct createJsonStruct = new CreateJsonStruct();
    string private constant INPUT_PATH = "script/target/input.json";

    function run() public {
        string memory jsonObject = getJson();
        vm.writeFile(INPUT_PATH, jsonObject);
    }

    function getJson() public returns (string memory jsonObject) {
        createJsonStruct.startMainObject();

        createJsonStruct.startObject("kaia");

        createJsonStruct.addKeyValuePairWithString("network", "testnet");
        createJsonStruct.addKeyValuePairWithString("chainid", "1001");

        createJsonStruct.startArray("developers");
        createJsonStruct.addArrayElementWithString("xx");
        createJsonStruct.addArrayElementWithString("oo");
        createJsonStruct.addArrayElementWithString("ppp");
        createJsonStruct.closeArray();

        createJsonStruct.closeObject();

        createJsonStruct.startObject("base");
        createJsonStruct.addKeyValuePairWithString("network", "testnet");
        createJsonStruct.addKeyValuePairWithString("chainid", "8354");
        createJsonStruct.closeObject();
        createJsonStruct.closeMainObject();

        jsonObject = createJsonStruct.getJson();

        console.log("file has been written to script/target/input.json");

    }
}


```
## Output

```shell
{
    "kaia": {
        "network": "testnet",
        "chainid": "1001",
        "developers": [
            "xx",
            "oo",
            "ppp"
        ]
    },
    "base": {
        "network": "testnet",
        "chainid": "8354"
    }
}
```
## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```