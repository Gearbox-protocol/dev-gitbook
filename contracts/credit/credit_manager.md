# CreditManager

Contract which implements basic credit account functions:

* Open credit account
* Close / Repay credit account
* Liquidate credit account
* Increase borrow amount

Interactions with 3rd party protocols should be implemented as [adapters](../adapters.md)

## Events

### OpenCreditAccount <a href="#event-open-credit-account" id="event-open-credit-account"></a>

```
event OpenCreditAccount(
    address indexed sender,
    address indexed onBehalfOf,
    address indexed creditAccount,
    uint256 amount,
    uint256 borrowAmount,
    uint256 referralCode
);
```

Emits each time when the credit account is opened

### CloseCreditAccount <a href="#event-close-credit-account" id="event-close-credit-account"></a>

```
event CloseCreditAccount(
   address indexed owner,
   address indexed to,
   uint256 remainingFunds
);
```

 Emits each time when the virtual account is closed

### LiquidateCreditAccount <a href="#event-liquidate-credit-account" id="event-liquidate-credit-account"></a>

```
event LiquidateCreditAccount(
    address indexed owner,
    address indexed liquidator,
    uint256 remainingFunds
);
```

Emits each time when the virtual account is liquidated

### RepayCreditAccount <a href="#event-repay-credit-account" id="event-repay-credit-account"></a>

```
event RepayCreditAccount(address indexed owner, address indexed to);
```

 Emits each time when the virtual account is repaid

### NewLimits

```
function repayCreditAccount(address to) external;
```

Emits each time when new limits are set

## State-Changing Functions <a href="#state-changing-functions" id="state-changing-functions"></a>

### openCreditAccount <a href="#open-credit-account" id="open-credit-account"></a>

```
function openCreditAccount(
    uint256 amount,
    address payable onBehalfOf,
    uint256 leverageFactor,
    uint256 referralCode
) external
```

| Parameter      | Description                                                                                                                                                                    |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| amount         | Borrower's own (initial) funds                                                                                                                                                 |
| onBehalfOf     | The address that we open virtual account. Same as msg.sender if the user wants to open it for  his own wallet, or a different address if the beneficiary is a different wallet |
| leverageFactor | Multiplier to borrowers own funds                                                                                                                                              |
| referralCode   | Code used to register the integrator originating the operation, for potential rewards. 0 if the action is executed directly by the user, without any middle-man                |

![](../../.gitbook/assets/Gearbox\_white\_high.014.jpeg)

* Opens credit account (take it from account factory^1)
* Transfers borrowers' initial funds to credit account
* Transfers borrowed leveraged amount from pool (= amount x leverageFactor) calling[ lendCreditAccount() ](../pools/pool-service.md#lendcreditaccount)on connected Pool contract.
* Emits [OpenCreditAccount](credit\_manager.md#event\_openvirtualaccount) event

To get reusable credit account, credit manager invokes [takeCreditAccount](../core/account-factory.md#takecreditaccount) method from [AccountFactory](../core/account-factory.md). If there is no credit accounts in stock, it deploys new [credit account contract](credit\_account.md) and pays gas compensation for borrower

### closeCreditAccount <a href="#close-credit-account" id="close-credit-account"></a>

```
function closeCreditAccount(address to, uint256 amountOutTolerance)
external
```

| Parameter          | Description                                                              |
| ------------------ | ------------------------------------------------------------------------ |
| to                 | Address to send remaining funds                                          |
| amountOutTolerance | Coefficient to amountOut during sale on default swap in percenatage math |

![](../../.gitbook/assets/Gearbox\_white\_high.015.jpeg)

* Swaps all assets to underlying one using Uniswap V2 compatible swap protocol
* Pays borrowed amount + interest accrued + fees back to the pool by calling [repayCreditAccount](../pools/pool-service.md#repaycreditaccount)
* Transfers remaining funds to borrower
* Closes the credit account and [returns](../core/account-factory.md#returncreditaccount) it to account factory
* Emits [CloseCreditAccount](credit\_manager.md#event\_closevirtualaccount) event

More information about closing credit account on [economy](economy.md#close-account) page.

### liquidateCreditAccount <a href="#liquidate-credit-account" id="liquidate-credit-account"></a>

```
function liquidateVirtualAccount(address borrower, address to) external
```

| Parameter | Description                                         |
| --------- | --------------------------------------------------- |
| borrower  | Borrower address                                    |
| to        | Address to transfer all assets from virtual account |

![](../../.gitbook/assets/Gearbox\_white\_high.016.jpeg)

Liquidate credit accounts with [health factor](economy.md#health-factor) < 1. Liquidator pays discounted total virtual account value and gets all assets from pool to his ("to") account:

* Transfers discounted total virtual account value from liquidators account
* Pays borrowed funds + interest + fees back to pool, than transfers remaining funds to virtual account owner
* Transfer all assets from virtual account to liquidator ("to") account
* Returns virtual account to factory
* Emits [LiquidateCreditAccount](credit\_manager.md#event-liquidate-credit-account) event

More information about liquidation on [economy](economy.md#liquidation) page.

### repayCreditAccount <a href="#repay-credit-account" id="repay-credit-account"></a>

```
function repayVirtualAccount(address to)
    external
```

| Parameter | Description                                    |
| --------- | ---------------------------------------------- |
| to        | Address to send all asstets of virtual account |

Borrower can repay the borrowed amount + interest rate + fee and the protocoland get all assets from credit account to his own (provided as "to" param)

![](<../../.gitbook/assets/Gearbox\_white\_high.017 (1).jpeg>)

* Transfers borrowed amount + interest accrued + fee from borrower
* Transfers all assets from credit account to trader ("to") account
* Returns creditl account to account factory

More information can be found on [economy](economy.md#repay) page.

### increaseBorrowedAmount <a href="#increase-borrowed-amount" id="increase-borrowed-amount"></a>

```
function increaseBorrowedAmount(uint256 amount)
```

| Parameter | Description      |
| --------- | ---------------- |
| borrower  | Borrower account |

![](../../.gitbook/assets/Gearbox\_white.009.jpeg)

Increases borrowed amount by transferring additional funds from the pool if after that HealthFactor > minimum health factor.&#x20;

Maximum available health factor equals health factor at opening account with maximum leverage:

### provideCreditAccountAllowance

```
function provideCreditAccountAllowance(
    address creditAccount,
    address toContract,
    address token
) external 
```

| Parameter     | Description                 |
| ------------- | --------------------------- |
| creditAccount | Credit account address      |
| toContract    | Contract to check allowance |
| token         | Token address of contract   |

Approve tokens for credit accounts. Restricted for adapters only

### executeOrder

```
function executeOrder(
    address borrower,
    address target,
    bytes memory data
)
    external
```

| Parameter | Description           |
| --------- | --------------------- |
| borrower  | Borrower address      |
| target    | Target smart-contract |
| data      | Call data for call    |

Executes filtered order on credit account which is connected with particular borrower

### setLimits

```
function setLimits(uint256 _minAmount, 
uint256 _maxAmount) external;
```

 Set minimal and maximum allowed amount for initial funds at opening virtual account

## Getters

### hasOpenedCreditAccount&#x20;

```
function hasOpenedCreditAccount(address borrower) 
extenal 
view 
returns (bool)
```

| Parameters | Descriptions     |
| ---------- | ---------------- |
| borrower   | Borrower account |

Returns true if the borrower has opened a virtual account

### getCreditAccountOrRevert

```
function getCreditAccountOrRevert(address borrower)
    external
    view
```

Returns address of borrower's credit account and reverts of borrower has no one

### calcRepayAmount

```
function calcRepayAmount(address borrower, bool isLiquidated)
    external
    view
    override
    returns (uint256)
```

| Parameters   | Description                                   |
| ------------ | --------------------------------------------- |
| borrower     | Borrower account                              |
| isLiquidated | True if called for compute liquidation amount |

Calculates repay / liquidate amount. More info on [economy](economy.md#repay) page.

###

## &#x20;<a href="#state-changing-functions" id="state-changing-functions"></a>



