# AddressProvider

Keeps addresses of core contracts. Used for smart contract address discovery

## Code

[AddressProvider.sol](https://github.com/Gearbox-protocol/gearbox-contracts/blob/master/contracts/core/AddressProvider.sol)

## Events

### AddressSet

```
event AddressSet(bytes32 indexed service, address newAddress);
```

Emits each time when a new address is set

## State-Changing Functions

### setPoolRegistry

```
function setPoolRegistry(address _address) 
external
```

 Sets address of PoolRegistry

### setACL

```
function setACL(address _address) 
external
```

 Sets address of ACL contract

### setPriceOracle

```
function setPriceOracle(address _address) 
external
```

 Sets address of PriceOracle

### setAccountFactory

```
function setAccountFactory(address _address) 
external
```

 Sets address of AccountFactory. Used for test purposes only

### setTreasuryContract

```
function setTreasuryContract(address _address) 
external
```

 Sets address of Treasury Contract

### setGearToken

```
function setGearToken(address _address) 
external
```

 Sets address of GEAR token

### setWethToken

```
function setWethToken(address _address) 
external
```

 Sets address of WETH token

### setWETHGateway

```
function setWETHGateway(address _address) external
```

 set WETHGateway address

## Getters

### getPoolRegistry

```
function getPoolRegistry() 
external 
view 
returns (address)
```

 Gets address of PoolRegistry

### getACL

```
function getACL() 
external 
view 
returns (address)
```

 Gets address of ACL contract

### getPriceOracle

```
function getPriceOracle() 
external 
view 
returns (address)
```

 Gets address of PriceOracle

### getAccountFactory

```
function getAccountFactory() 
external 
view 
returns (address)
```

 Gets address of Account Factory. For testing purposes only.

### getTreasuryContract

```
function getTreasuryContract() 
external 
view 
returns (address)
```

 Gets address of Treasury contract

### getGearToken

```
function getGearToken() 
external 
view 
returns (address)
```

 Gets address of GEAR token

### getWethToken

```
function getWethToken() 
external 
view 
returns (address)
```

 Gets address of WETH token

### getWETHGateway

```
function getWETHGateway() 
external 
view 
returns (address)
```

 Gets address of WETH gateway

##
