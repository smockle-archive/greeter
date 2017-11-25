# Commands

```Bash
# Install ethereum, geth & solidity compiler
# - Requires Homebrew
$ brew update && brew upgrade
$ brew tap ethereum/ethereum
$ brew install ethereum solidity
$ brew linkapps solidity

# Create directories & files
$ mkdir ~/.ethereum
$ mkdir ~/.ethereum/private
$ touch ~/.ethereum/genesis.json
$ echo '{
    "config": {
        "chainId": 333,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "difficulty": "0x400",
    "gasLimit": "0x2100000",
    "alloc": {}
}' > ~/.ethereum/genesis.json

# Create a private chain
$ geth --datadir ~/.ethereum/private init ~/.ethereum/genesis.json

# Create & start a private network
# - Creates IPC file
$ geth --datadir ~/.ethereum/private --networkid 333

# Connect to private network
# - Opens geth console
$ geth attach ~/.ethereum/private/geth.ipc

# Create personal account
> personal.newAccount()
# - Enter password
# - Confirm password
# - View account address

# Check personal account balance
> eth.getBalance("0xACCOUNT_ADDRESS")
# e.g. > eth.getBalance(personal.listAccounts[0])
# - View account balance

# Unlock account
# - For validation
> personal.unlockAccount(eth.accounts[0])

# Start mining for Ether
> miner.start()

# Stop mining for Ether
> miner.stop()

# Exit console
> exit

# Compile a contract
$ echo "var compiled=$(solc --optimize --combined-json abi,bin,interface script.sol)" > script.js

# Load & deploy a contract
> loadScript("script.js");
> var contract = eth.contract(JSON.parse(compiled.contracts["script.sol:Script"].abi));
# Unlock account
> var obj = contract.new("_agrument", { from: eth.accounts[0], data: "0x" + compiled.contracts["script.sol:Script"].bin, gas: 300000 }, function (e, contract){console.log(e, contract); if (typeof contract.address !== 'undefined'){console.log('Contract mined! address: '+contract.address+' transactionHash: '+contract.transactionHash);}});
# NB: To deploy, a miner must be running

# Run a contract
# - Run
> obj.doSomething();
# - Connect & run
> eth.contract(JSON.parse(compiled.contracts["script.sol:Script"])).at(obj.address).doSomething();

# Destroy a contract
> obj.kill.sendTransaction({ from: eth.accounts[0] });
# NB: To destroy, a miner must be running

# Restart private network
# - Reusing IPC file
$ geth --ipcpath ~/.ethereum/private/geth.ipc --networkid 333 --datadir ~/.ethereum/private

# Delete private chain
$ geth removedb --datadir ~/.ethereum/private
```

# Additional Resources

* https://medium.com/@WWWillems/how-to-set-up-a-private-ethereum-testnet-blockchain-using-geth-and-homebrew-1106a27e8e1e

* https://hackernoon.com/heres-how-i-built-a-private-blockchain-network-and-you-can-too-62ca7db556c0

* https://ethereum.org/cli

* https://medium.com/@gus_tavo_guim/deploying-a-smart-contract-the-hard-way-8aae778d4f2a
