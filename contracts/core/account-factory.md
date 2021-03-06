# AccountFactory

### Reusable credit accounts

Reusable Credit Accounts is an innovative technology that makes Gearbox gas-efficient by keeping user balances with minimal overhead. Users "rent" deployed credit account smart contracts from protocol to save deployment costs.&#x20;

![](../../.gitbook/assets/Gearbox\_white\_high.021.png)

Each time, when a user opens a credit account in Gearbox protocol, it takes a pre-deployed credit account contract from the AccountFactory and when the user closes the credit account returns it.

If account factory has no pre-deployed contracts, it clones it using [https://eips.ethereum.org/EIPS/eip-1167](https://eips.ethereum.org/EIPS/eip-1167)

## Advantages

### Gas efficiency

This solution is much more gas-efficient, cause it doesn't require creating a new contract each time and has minimal operational overhead.

### Hacker-proof

Contract funds are allocated on isolated contracts which makes a possible attack more complex and less economically reasonable.  Furthermore, the gearbox protocol uses anomaly detection to pause contracts and keep funds safe if suspicious behavior is found.

User can protect their funds by splitting them between a few virtual accounts, and it makes the attack less economic reasonable.

### Balance transparency on Etherscan

Trader or farmer could check all their transactions on Etherscan if they know at which block they start and finish virtual account renting.

### Ethereum network ecology

It generates significantly less data in comparison with deployment credit contract for each new customer, and consume significantly less gas than keeping all balances in one place. As result it makes less impact on Ethereum infrastructure.

## Implementation

[Account Factory](broken-reference) supplies credit account for CM contracts on demand and keeps them when they are not used.

‌The account factory uses a list to keep credit accounts and two pointers: head and tail.

![](../../.gitbook/assets/va\_list.jpeg)

When some pool asks for a virtual account it takes it from the head pointer, when the pool returns a credit account, it adds it to the tail.

## Code

[AccountFactory.sol](https://github.com/Gearbox-protocol/gearbox-contracts/blob/master/contracts/core/AccountFactory.sol)

## State-Changing Functions

### takeCreditAccount

```
function takeCreditAccount(address payable borrower) external
```

 Provides a new credit account contract to credit manager. Creates a new one, if needed

### returnCreditAccount

```
function returnCreditAccount(address usedAccount) external
```

Takes credit account back and adds it to the stock

### addCreditAccount

```
function addCreditAccount() public
```

Deploys new credit account and adds it to list tail

## Getters

### getHead

```
function getHead() 
external 
view
returns (address)
```

 Gets head of the list of unused credit accounts

### getTail

```
function getTail() 
external 
view 
returns (address)
```

Gets tail of the list of unused credit accounts

### countCreditAccountsInStock

```
function countCreditAccountsInStock() 
external 
view 
returns (uint256)
```

 Counts how many credit accounts are in stock

### creditAccounts

```
function creditAccounts(uint256 id) external view returns (address);
```

Returns address of credit account by its id.

### countCreditAccounts

```
function countVirtualAccounts() external view returns (uint256);
```

Returns quantity of credit accounts







