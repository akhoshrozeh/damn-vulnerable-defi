![](cover.png)

**A set of challenges to learn offensive security of smart contracts in Ethereum.**

Featuring flash loans, price oracles, governance, NFTs, lending pools, smart contract wallets, timelocks, and more!

## Play

Visit [damnvulnerabledefi.xyz](https://damnvulnerabledefi.xyz)

## Disclaimer

All Solidity code, practices and patterns in this repository are DAMN VULNERABLE and for educational purposes only.

DO NOT USE IN PRODUCTION.


## My Solutions (akhoshrozeh)

### Challenge 1: Unstoppable:
The vulnerability is this line inside UnstoppableLender.sol: `assert(poolBalance == balanceBefore);` <br/>
We can transfer erc20 tokens to the contract which updates the balance in the erc20 contract, but not in the lending contract's state variable `poolBalance`. This leads to the assertion always failing. 


### Challenge 2: Naive-Receiver
The vulnerability is that the receiver contract doesn't verify that the call was initiated by the owner. This means anyone can call the `receiveEther` function. Since there's a large fee for flashLoans, we can drain the receiver's wallet by making them the `borrower`, forcing them to pay the high fee as many times as we want. We can complete the exploit in a single transaction by making a simple exploit contract. See `contracts/naive-receiver/Exploit2.sol`.


