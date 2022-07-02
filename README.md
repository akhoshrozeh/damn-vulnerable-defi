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


### Challenge 3: Truster
The vulnerability is that an attacker can let the `data` parameter be anything. This includes the `approve` function of the ERC20 token. By having the flashloan contract call the `approve` function, we can authorize ourselves to steal all the funds from the lending pool. See `contracts/truster/Exploit3.sol` to see how you can do in a single transaction.


### Challenge 4: Side-Entrance
The vulnerability is that flashloans are repaid by deposting ether back into the caller's balance (state variable in the pool contract). This means we can set our balance in the state variable as the contract's entire balance using a valid flashloan. After that, Ww can then withdraw all the funds since our balance has been updated. See `contacts/side-entrance/Exploit4.sol` for the solution on how to do this.
