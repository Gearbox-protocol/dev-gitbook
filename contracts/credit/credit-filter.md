# CreditFilter

Implements filter logic for allowed tokens & contract-adapters&#x20;

* Sets/Gets tokens for allowed tokens list
* Sets/Gets adapters & allowed contracts
* Calculates total value for credit account
* Calculates threshold weighted value for credit account
* Keeps enabled tokens for credit accounts

## Code

[CreditFilter.sol](https://github.com/Gearbox-protocol/gearbox-contracts/blob/master/contracts/credit/CreditFilter.sol)

## Events

### TokenAllowed

```
event TokenAllowed(address indexed token, uint256 liquidityThreshold);
```

Emits each time token is allowed or liquidtion threshold changed

### ContractAllowed

```
event ContractAllowed(address indexed protocol, address indexed adapter);
```

Emits each time contract is allowed or adapter changed

## State-Changing Functions

### allowToken

```
function allowToken(address token, uint256 liquidationThreshold) external
```

Adds token to the list of allowed tokens

### allowContract

```
function allowContract(address allowedContract, address adapter)
external
```

 Adds contract to the list of allowed contracts

### connectCreditManager

```
function connectCreditManager(address _poolService)
```

Connects credit managaer, checks that all needed price feeds exists and finalize config

### checkCollateralChange

```
function checkCollateralChange(
    address creditAccount,
    address tokenIn,
    address tokenOut,
    uint256 amountIn,
    uint256 amountOut
) external
```

Checks that tokenOut is allowed and credit account has enough collateral after operation.

### initEnabledTokens

```
function initEnabledTokens(address creditAccount)
```

Initializes enabled tokens

## Getters

### calcTotalValue

```
function calcTotalValue(address creditAccount)
    external
    view
    returns (uint256 total)
```

Calculates total value for provided address. More info check on [economy](economy.md#total-value) page

### calcThresholdWeightedTotalValue

```
function calcThresholdWeightedValue(address creditAccount)
    external
    view
    override
    returns (uint256 total)
```

Calculates Threshold Weighted Total Value. More info check on [vam economy page](economy.md#total-value).

### allowedTokens

```
address[] public override allowedTokens
```

Gets of token address from allowed list by its id

### allowedTokensCount

```
function allowedTokensCount() 
external 
view 
returns (uint256)
```

 Counts tokens in the allowed list

### revertIfTokenNotAllowed

```
function revertIfTokenNotAllowed(address token) external view
```

 Reverts if the token isn't in the token allowed list

###  allowedContracts

```
address[] public override allowedContracts;
```

 Gets of contract address from the allowed list by its id

### allowedContractsCount

```
function allowedContractsCount() 
external 
view 
returns (uint256)
```

 Counts contracts in the allowed list

### revertIfContractNotAllowed

```
function revertIfContractNotAllowed(address contractToCheck) 
external 
view
```

 Reverts if the contract isn't in contract allowed list

### getCreditAccountTokenById

```
function getCreditAccountTokenById(address creditAccount, uint256 id)
    external
    view
    override
    returns (
        address token,
        uint256 balance,
        uint256 tv,
        uint256 tvw
    )
```



Returns address & balance of token by the id of allowed token in the list

### calcCreditAccountAccruedInterest

```
function calcCreditAccountAccruedInterest(address creditAccount)
    external
    view
    override
    returns (uint256)
```

Calculates credit account interest accrued. More info check on [economy](economy.md#interest-rate-accrued) page

### calcCreditAccountHealthFactor

```
function calcCreditAccountHealthFactor(address creditAccount)
    external
    view
    override
    returns (uint256)
```

Calculates health factor for the credit account. More info check on [economy](economy.md#health-factor) page



###





