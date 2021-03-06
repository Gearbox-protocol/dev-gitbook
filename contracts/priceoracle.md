# PriceOracle

PriceOracle is connected to each pool via [VAMFilter](credit/credit-filter.md). It is used to evaluate balance on credit accounts converting to the underlying asset.

![](../.gitbook/assets/Gearbox\_white.008.jpeg)

Price oracle gets prices to ETH from Chainlink oracles. Gathering the data, PriceOracle computes the rate between two tokens and supply it to Token Filter. TokenFilter computes the total balance converted to the underlying asset and supply it to the pool contract.&#x20;

[VAMFilter](credit/credit-filter.md)  uses this data to compute [Threshold Weighted Value](credit/economy.md#threshold-weighted-value), which is used in AbstractVirtualAccountManager to calculate [the health factor](credit/economy.md#health-factor).

PriceOracle implements [IPriceOracle](https://github.com/Gearbox-protocol/gearbox-v2/blob/master/contracts/interfaces/IPriceOracle.sol) interface.

## Code

[PriceOracle.sol](https://github.com/Gearbox-protocol/gearbox-contracts/blob/master/contracts/oracles/PriceOracle.sol)

##  State-Changing Functions

### addPriceFeed

```
function addPriceFeed(address token, address priceFeedToken)
    external
```

 Sets price feed if it doesn't exist

## Getters

### convert

```
function convert(
    uint256 amount,
    address tokenFrom,
    address tokenTo
) external view returns (uint256)
```

 Converts one asset into another using rate. Reverts if price feed doesn't exist

### getLastPrice

```
function getLastPrice(address token1, address token2)
    public
    view
    returns (uint256)
```

Gets token rate with 18 decimals. Reverts if priceFeed doesn't exist



