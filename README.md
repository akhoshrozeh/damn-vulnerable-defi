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

### Challenge 5: The-Rewarder
We can steal all the reward tokens by waiting until the next round offchain, then taking a very large flashloan and depositing it into the rewards pool, get the rewards token, then pay back the flashloan. See `contracts/the-rewarder/Exploit6.sol`.

### Challenge 6: Selfie
My favorite one so far. The key problem here is that the pool uses the same ERC20 for governance tokens as it does for flashloans. This allows us to take a large flashloan giving us the majority of governance tokens during the transaction. We then craft a payload that will drain the tokens to us, and queue it in the governance contract. After the timelock has been completed, we can then execute the action. See `contracts/selfie/Exploit6.sol`.


### Challege 7: Compromised
I like how this one incorporated a bit of offchain work as well. The server has leaked private keys to the accounts that are 'oracles'. By gaining access to the oracle accounts, we can set the price of NFT very low, buy one, then set the price of the NFT very high, then sell it for profit. The exact parameters can be adjusted such that we drain the entire exchange of Eth and we solve the level.


### Challenge 8: Puppet
Also really liked this one for using Uniswap, a real protocol. Basically, the vulnerability is that the lending relied on the Uniswap pair as a price oracle. This pair has very little liquidity, so we can easily manipulate the price of the DVT token. By depositing a lot of DVT into the pair, we make it so that the price of DVT is now very cheap. We can the then borrow 100,000 DVT for less than 10 eth, whereas before the swap we would have to collateralize 200,000 eth. Neattt. 
