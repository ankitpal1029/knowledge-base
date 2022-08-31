# Protostar Walkthrough

## Project init

```bash
$ protostar init
```

## protostar.toml

```toml
["protostar.config"]
protostar_version = "0.1.0"

["protostar.project"]
libs_path = "./lib"         # a path to the dependency directory

# List all contracts that you want to compile here
["protostar.contracts"]
main = [
  "./src/main.cairo",
]
```

### Command config

List all your dependencies here, say you installed openzeppelin contracts

```toml
["protostar.build"]
cairo-path = ["./lib/cairo_contracts/src"]

# to add as arguement for multiple commands
["protostar.shared_command_configs"]
cairo-path = ["./lib/cairo_contracts/src"]
```

### Running profiles

```toml
["profile.ci.protostar.shared_command_configs"]
no-color = true
```

Both these work

```protostar
$ protostar -p ci ...

$ protostar -- profile ci ...
```

## Dependencies

```bash
$ protostar install https://github.com/OpenZeppelin/cairo-contracts
```

gets stored in `lib/` folder

### Installing dependencies after cloning protostar project

If you clone without using `--recursive-submodules` then you have to run

```bash
$ protostar install
```

### Updating dependencies

```bash
$ protostar update cairo-contracts
```

### Remove dependency

```bash
$ protostar remove cairo-contracts
```

## Declaring/Deploying contracts

### Declaring contracts

```bash
$ protostar declare ./build/main.json --network alpha-goerli
```

### Deploying contracts

```bash
$ protostar deploy ./build/main.json --network alpha-goerli
```

### Config Profiles

```toml
[profile.devnet.protostar.deploy]
gateway-url="http://127.0.0.1:5050/"

[profile.testnet.protostar.deploy]
network="alpha-goerli"

[profile.mainnet.protostar.deploy]
network="alpha-mainnet"
```

To run this config use

```bash
$ protostar -p devnet deploy ./build/main.json
```

## Migrations

[Read about migrations later](https://docs.swmansion.com/protostar/docs/tutorials/deploying/migrations)

## Testing

- All files starting with `test_` in `tests` folder are considered seperate test cases
- To do setting up work before testing run `__setup__` hook

```rust
%lang starknet

@external
func __setup__():
    %{ context.contract_a_address =
    deploy_contract("./tests/integration/testing_hooks/basic_contract.cairo")
    .contract_address %}
    return ()
end

@external
func test_something():
    tempvar contract_address
    %{ ids.contract_address = context.contract_a_address %}

    # ...

    return ()
end
```

- You can pass `context` variables to pass data from `__setup__` to the test
- Use hints to do the testing
