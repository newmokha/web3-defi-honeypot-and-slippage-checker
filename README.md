# web3-defi-honeypot-and-slippage-checker

Cross-chain deployed Smart-contract to detect Honeypot and Slippage for DeFi tokens.

### How it works:

There is no magic involved; only EVM (Ethereum Virtual Machine) features are used. 
The contract simulates a **buy/approve/sell** execution in a single transaction and evaluates the results. It works with any fork of UniSwap2 Router interface.

### Community deployed contracts:

```
  Ethereum => '0xe7e07a2281f1e66e938ae7feefc69db181329f12'
  Arbitrum => '0x0aa2037E40a78A169B5214418D66377ab828cb23'
  Binance chain => '0x385826FBd70DfBB0a7188eE790A36E1fe4f6fc34' // PancakeSwap '0x52689BA8e1D164A16fb06918A18978d03fF6EB3F'
  Cronos chain => '0xb5BAA7d906b985C1A1eF0e2dAd19825EbAb5E9fc' // PhenixDex '0x37495E34de11F8Ee72DBb0a71e60C1bd312674fE'
  Fantom Chain => '0x4208B737e8f3075fD2dCB9cE3358689452f98dCf'
  Polygon Chain => '0xc817b3a104B7d48e3B9C4fbfd624e5D5F03757e0'
  Avalanche  => '0xf3af9a948f275c2c3b9c61ade16540e66158a1d5' // Trader Joe '0x2B30ddE904B22c0Bba6019543231c857e0Be1DfB'
  Astar Network => '0x0aa2037E40a78A169B5214418D66377ab828cb23'
  DogeChain => '0x7c0612357771f6599e8e1a046a02f4beb9496de1' // DogeSwap '0xDB2135662F55C241EEEef9424B68f661d5c0D298'
  PulseChain => '0xBe4A121B0fa604438B61e49a4a818A00F50c09e1'
```

### How to use?

There is a TypeScript code snippet (example/index.ts)

```
const RunHoneyContract = async (
  from: string, // Any existing address on the blockchain e.g. 0x573fbc5996bfb18b3f9b9f8e96b774905bcdc8b6 (find one from the Top Accounts https://cronoscan.com/accounts)
  to: string, // The Honeypot checker contract Address e.g. 0xb5BAA7d906b985C1A1eF0e2dAd19825EbAb5E9fc
  token: string, // the address of the token e.g. 0x062E66477Faf219F25D27dCED647BF57C3107d52 (wBTC)
  router: string, // the DEX router address e.g.  0x145677fc4d9b8f19b5d56d1820c48e0443049a30 (MMfinance router on Cronos)
  rpcAddress: string // Provide your EVM node e.g. https://evm-cronos.crypto.org
)

Result:
{
  buyTax: 0,
  sellTax: 0.3, // Passed 0.3% Tax detected
  buyGasCost: 0,
  sellGasCost: 0,
  isHoneypot: 0
}
```

### A Token Contract is failing on the Honeypot check why?

1. Required to have a native currency trading pair available (wETH,wBNB,wCro...). Why? because it make no sense to support route like WrappedCoin -> USDT -> AnyToken.
2. The available liquidity is lower than your simulation required
3. The Contract is broken or not satisfy the Uniswap2 de facto requirement. (Whitelisting, Blacklisting, Trade Disable, MaxTx, Disallow Buy/Sell in same transaction, MaxWallet...)

### Is this safe?

1. Slippage calculation already tested over 1.000.000 different Token pair.
2. Honeypot checking can be bypassed since it's still EVM. However, the aim here is to reduce risk and avoid broken contracts.
