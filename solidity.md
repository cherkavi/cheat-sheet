# ethereum
[app example](https://github.com/StephenGrider/EthereumCasts)


# smart contracts
[Linux Foundation Hyberledger Fabric](https://www.hyperledger.org/)
[hyperledger composer playground](http://composer-playground.mybluemix.net/login)
[hyperledger composer playground tutorial](https://hyperledger.github.io/composer/latest/tutorials/playground-tutorial.html)
```
# application schema
Business Application -> Hyperledger Composer -> Blockchain ( Hyperledger Fabric)
```
[documentation](http://solidity.readthedocs.io/en/latest/)

---
user app to communicate with Ethereum
* Metamask ( chrome extension )
* Mist browser

---
[Rikneby ethereum account charger](rinkeby-faucet.com)

---

## account address types:
* external ( user account, common for all networks )
* internal ( contract account, specific for each network )
```
balance
storage - data storage
code - compiled machine code 
```

## [smart contract playground](http://remix.ethereum.org)


## SmartContract API collaboration via nodejs app example
* NodeJS
```
# ganache-cli 
const Web3 = require('web3')
const web3 = new Web3("http://localhost:8545")

console.log(web3.eth.getAccounts().then(e=>console.log(e)))
```
* [Java](https://docs.web3j.io/getting_started.html)
* [SpringBoot, gradle example, maven plugin](https://github.com/web3j/)

http://solidity.readthedocs.io/en/latest/

# syntax
## function types
* public/private
* view/constant ( return field, return constant)
* pure ( don't use any contract-variables to manipulate )
* payable 
