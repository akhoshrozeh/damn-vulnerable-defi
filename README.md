![](cover.png)

**A set of challenges to learn offensive security of smart contracts in Ethereum.**

Featuring flash loans, price oracles, governance, NFTs, lending pools, smart contract wallets, timelocks, and more!

## Play

Visit [damnvulnerabledefi.xyz](https://damnvulnerabledefi.xyz)

## Disclaimer

All Solidity code, practices and patterns in this repository are DAMN VULNERABLE and for educational purposes only.

DO NOT USE IN PRODUCTION.


## My Solutions (akhoshrozeh)

### Unstoppable:
The vulnerability is this line inside UnstoppableLender.sol: `assert(poolBalance == balanceBefore);` <br\>
We can transfer erc20 tokens to the contract which updates the balance in the erc20 contract, but not in the lending contract's state variable `poolBalance`. This leads to the assertion always failing. 

