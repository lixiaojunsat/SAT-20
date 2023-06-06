# SAT-20 Protocol

## Introduction

SAT-20 is an experimental token standard and NFT collection specification for Bitcoin ordinals. You should exercise caution and do your own research before testing it. Use at your own risk.

SAT-20 does not rely on any third-party indexers. It extends the definition of Rare Satoshi. SAT-20 provides a unified characteristic for an ordinal range of satoshis as tokens or NFT collections. Token transfer refers to the transfer of a specific amount of satoshis within a designated ordinal range. NFT transfer involves the transfer of a satoshi with a specific number within a particular ordinal range. Satoshi with ordinal number is similar to Semi-Fungible Token (SFT).

The tools for transferring a single satoshi or a specific quantity of satoshis within a certain ordinal number range are currently limited.

## Protocol Spec

### Basics

| Key | Required | Description |
|:--- | :---| :---|
|p| Yes| Protocol: helps other systems identify and process SAT-20 events|
|name| Yes| Name: name of SAT-20 in any size|
|symbol| Yes| Symbol: symbol of SAT-20 in any size and case-insensitive|
|max| No| Max Supply: set max token supply，equal to the number of satoshi from "range"|
|dec| No |Decimal: decimal precision must be <=8, default to 0. *e.g.* dec=2, Indicates that 1 SAT-20 Token can be divided into 100 satoshis.
|range| Yes| Range: ordinal numbers of satoshis by assigned one property, such as NFTs, security tokens or accounts. Represented by an array. The elements of the array can be a string of single or serial numbers in a certain range. *e.g.* ["0","1","5-10","20-99","2100"]. Other element forms are not supported|
|id| No| Identifier: deploy any domain names, like .sats, requires relevant third-party indexer support|
|description| No| Description: an introduction, explanation or description|

**Example of New SAT-20:**  Deploy SAT-20 Token SATS, max supply of 2100000000000000. 

```
{
  "p": "SAT-20",
  "name":"Satoshi",
  "symbol":"SATS", 
  "max":"2100000000000000",
  "range":["0-2099999999999999"]  
}
```
**Example of New SAT-20:**  Deploy SAT-20 Token BTC, max supply of 21000000. 

```
{
  "p": "SAT-20",
  "name":"Bitcoin",
  "symbol": "BTC", 
  "max":"21000000",
  "dec":"8",
  "range":["0-2099999999999999"]  
}
```

**Example of New SAT-20:**  Deploy SAT-20 Token or NFT collection: MYTHIC. The first sat of the genesis block.
```
{
 "p":"SAT-20",
 "name":"Mythic Satoshi"
 "symbol":"MYTHIC",
 "max":"1", 
 "range":["0"],
 "description":"The first sat of the genesis block"
}
```
**Example of New SAT-20:**  Deploy SAT-20 Token or NFT collection: PUNKS, max supply of 10000. *Note: Due to space limitations, not all the ordinal numbers of Satoshi are fully listed here.It needs to be completed when deploying.Ellipses are not supported*
```
{
 "p":"SAT-20",
 "name":"Bitcoin Punks",
 "symbol":"PUNKS",
 "max":"10000",
 "range":["974156770921434","1787393705565114","515386258041706",...,"1912486022083367"]
 "description":"The first 10k NFT collection on Bitcoin"
}
```

### Advanced

| Key | Required | Description |
|:--- | :---| :---|
|p| Yes| Protocol: helps other systems identify and process SAT-20 events|
|name| Yes| Name: name of SAT-20 in any size|
|symbol| Yes| Symbol: symbol of SAT-20 in any size and case-insensitive|
|max| No| Max Supply: set max token supply，equal to the number of satoshi from "range"|
|dec| No |Decimal: decimal precision must be <=8, default to 0. *e.g.* dec=2, Indicates that 1 SAT-20 Token can be divided into 100 satoshis.
|range| Yes| Range: ordinal numbers of satoshis by assigned one property, such as NFTs, security tokens or accounts. Represented by an object. The key of the object are "inputs" and "ouputs",the value of the object can be a string of single , serial numbers in a certain range, or interface standard function. *e.g.* "0","1","5-10","20-99","2100","transfer()","transferFrom()". Other element forms are not supported|
|id| No| Identifier: deploy any domain names, like .sats, requires relevant third-party indexer support|
|description| No| Description: an introduction, explanation or description|

### interface standard 

| Function | Parameter | Description |
|:--- | :---| :---|
|transfer | address_to block_start block_end | Returns the ordinal numbers of all satoshis sent to address_to between block_start ( block number, optional ) and block block_end ( block number, optional ) |
|transferFrom|address_from address_to address_to block_start block_end |Returns the ordinal numbers of all satoshis from address_from sent to address_to between block_start ( block number, optional ) and block block_end ( block number, optional ) |
|hodlOf|UTXO_output| Returns the ordinal numbers of all satoshis in the output of UXTO |
|valueSupply| None | Returns the ordinal numbers of all valid satoshis for SAT-20 token, A loose analogy with UTXO model: "inputs" = "outputs", valueSupply() = "inputs" - others of "outputs".|
|event|address_freeze| After the satoshi sent by address_freeze, return the ordinal number of all satoshi from the receiving address at that block height|

**Example of New SAT-20:**  Deploy SAT-20 Token PIZZA, max supply of 1000000000000, an interesting experiment.
```
{
 "p":"SAT-20",
 "name":"Bitcoin Pizza",
 "symbol":"PIZZA",
 "max":"1000000000000",  
 "range":{"inputs":["hodlOf(a1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d:0)"],"outputs":["valueSupply()"] },
 "description":"Sat from the 10000 Bitcoin used to purchase two Papa John's pizzas on May 22 2010" 
}
```

**Example of New SAT-20:**  Deploy SAT-20 Token HKDC, a hypothetical stablecoin.

```
{      
 "p":"SAT-20",
 "name":"Hongkong Dollar Coin",
 "symbol":"HKDC",
 "decimal":"2",
 "range":{"inputs":["transferFrom(address_from,address_to)","event(address_unfreeze)"],"outputs":["valueSupply()", "transfer(address_to)","event(address_freeze)"]},
 "description":"a hypothetical stablecoin"
}
```

## Multiple Value Attributes

The total amount of satoshi is fixed, so a satoshi may have multiple value attributes at the same time, such as rarity, NFT, and token. The choice of which value may be determined by the owner or the use environment. This is not a bad thing. Each satoshi becomes more valuable.

## Reference

- https://docs.ordinals.com/
- https://docs.sats.id/
- https://domo-2.gitbook.io/brc-20-experiment/
- https://docs.orc20.org/
- https://github.com/hydren-crypto/stampchain/blob/main/docs/src20.md
- https://www.brc721.com/
- https://github.com/DerpHerpenstein/src-721
- https://bitcoinpunks.com/
