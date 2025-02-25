---
title: Transfer of Reserve Tokens
sidebar_position: 2
---

```javascript
function depositReserveToken(
    bool isSourceNative,
    bytes memory swapData,
    bytes memory executeData
) external payable;
```

Reserve tokens are the tokens that have been kept in reserve by the Voyager protocol. The Voyager keeps reserves of some selected tokens on each chain so that it is able to provide fund transfers by locking funds on source chain and unlocking the funds on the destination chain. These include some stable tokens, wrapped native tokens for that chain as well as other tokens according to the needs as well as partnerships of projects with the Voyager .

When a user wants to transfer any of these reserve tokens from source chain to any other token on the destination chain, this function is to be called. After this function is called, the Voyager’s infrastructure will handle the transfer of tokens to the other chain.

**Parameters:**

| isSourceNative | If the source token is the native token for that chain, this should be true. |
| --- | --- |
| swapData | abi encoded data for the required swap which is created by an API  |
| executeData | abi encoded data for the execution on the destination chain which is created by an API |

The **Swap Data** includes: 

1. **srcStableTokenAmount:** Amount of the reserve token the user wants to transfer to the other chain.
2. **srcStableTokenAddress:** Address of the reserve token the user wants to transfer to the other chain.
3. **srcTokenAddress:** Address of the token the user wants to transfer to the other chain. This will be the same as srcStableTokenAddress in case of reserve tokens.
4. **destChainIdBytes:** The Chain ID identifier for the destination chain.

The **Execute Data** includes: 

1. **destTokenAmount:** Amount of destination tokens user will receive. This will be checked in the middleware and if this amount (USD Value) is larger than what user deposited, the transaction will be reverted.
2. **destTokenAddress:** Address of the token user will receive on the destination chain.
3. **isDestNative:** If the token to be received on the destination chain is native token for that chain, this is set to 1 else 0.
4. **destStableTokenAddress:** Address of the stable token in which USD value for the tokens to be received will be calculated on the middleware.
5. **recipient:** Address of the recipient on the destination chain.
6. **dataTx:** Data for the transaction for swap on destination chain received from the API. If the token to be received on the destination chain is a reserve token or an LP token for that chain, the dataTx will be an empty bytes array.
7. **path:** Path for the swap on destination chain received from the API. If the token to be received on the destination chain is a reserve token or an LP token for that chain, the path will be an empty address array.
8. **flags:** The identifiers of DEXes that will be used for swap on destination chain received from the API. If the token to be received on the destination chain is a reserve token or an LP token for that chain, the flags will be an empty uint256 array.
9. **widgetID:** Widget ID of the partner who has integrated the Voyager Widget. If you are not a partner, you can pass uint256 0 as widgetID.

:::info
All these data will directly be generated by the API. You don’t need to worry about generating this data.
:::
