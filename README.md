# LevanaPerpsTypes

Types from [Rust documentation of Levana Perps](https://apidocs.levana.finance/msg/doc/levana_perpswap_cosmos_msg/index.html) in TypeScript

## Install

### NPM

```
npm i @nktkas/levana-perps-types
```

### Deno, Yarn, pnpm, Bun from [JSR](https://jsr.io/)

Visit https://jsr.io/@nktkas/levana-perps-types to learn about all the ways to install a package in your package manager

## Usage

```typescript
import type { LevanaCosmWasmClient, LevanaSigningCosmWasmClient } from "@nktkas/levana-perps-types";
import { CosmWasmClient } from "@cosmjs/cosmwasm-stargate";

const RPC_ENDPOINT = "https://osmosis-rpc.publicnode.com:443";
const FACTORY_ADDRESS = "osmo1ssw6x553kzqher0earlkwlxasfm2stnl3ms3ma2zz4tnajxyyaaqlucd45";

const levanaCosmWasmClient = await CosmWasmClient.connect(RPC_ENDPOINT) as LevanaCosmWasmClient;

// TypeScript automatically recognizes a query, offers certain hints, and returns the appropriate response type for the query

const marketsResp = await levanaCosmWasmClient.queryContractSmart(FACTORY_ADDRESS, {
    markets: {},
});
console.log("All market ids:", marketsResp);

const marketInfoResponse = await levanaCosmWasmClient.queryContractSmart(FACTORY_ADDRESS, {
    market_info: {
        market_id: marketsResp.markets[0],
    },
});
console.log("Market info:", marketInfoResponse);

const statusResp = await levanaCosmWasmClient.queryContractSmart(marketInfoResponse.market_addr, {
    status: {},
});
console.log("Market status:", statusResp);

const tokensResponse = await levanaCosmWasmClient.queryContractSmart(marketInfoResponse.market_addr, {
    nft_proxy: {
        nft_msg: {
            tokens: {
                owner: "osmo13euuwxeg62w2xw3ul8pyk2s4fq6awnh8tqgs5y",
            },
        },
    },
});
console.log("Tokens:", tokensResponse);

// TypeScript will also notify you if you try to make a query that does not exist in the documentation

const marketLimitOrder = await levanaCosmWasmClient.queryContractSmart(marketInfoResponse.market_addr, {
    // TS Error: Property 'owner' is missing in type '{}' but required in type '{ owner: string; start_after?: Option<string> | undefined; limit?: Option<number> | undefined; order?: Option<OrderInMessage> | undefined; }'.
    limit_orders: {},
});

const marketOraclePrice = await levanaCosmWasmClient.queryContractSmart(marketInfoResponse.market_addr, {
    oracle_price: {
        // TS Error: Type 'number' is not assignable to type 'boolean'.
        validate_age: 0,
    },
});

const numTokensResponse = await levanaCosmWasmClient.queryContractSmart(marketInfoResponse.market_addr, {
    nft_proxy: {
        nft_msg: {
            num_tokens: {
                // TS Error: Type 'string' is not assignable to type 'never'.
                direction: "long",
            },
        },
    },
});
```

## License

This project is licensed under the MIT License. Check the [License file](LICENSE) for more info.
