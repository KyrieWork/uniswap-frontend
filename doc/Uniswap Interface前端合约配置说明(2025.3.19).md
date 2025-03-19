# Uniswap Interface前端合约配置说明

环境:

- arm64
- node vetsion v16.20.2
- yarn version v1.22.22

---

## 拉取指定版本的Uniswap前端文件;

```jsx
git clone --branch v4.5.0 git@github.com:Uniswap/interface.git
```

---

## 使用 node verison = v16.20.2 ,并安装依赖;

```jsx
nvm use 16
npm install -g yarn@latest
yarn
```

---

## 修改代码内的配置(例如新增：BSC_TESTNET);

- `/src/constants/chains.ts`

```jsx
export enum SupportedChainId {
 // ......
  BSC_TESTNET = 97,
}
```

```jsx
export const NETWORK_LABELS: { [chainId in SupportedChainId | number]: string } = {
  // ......
  [SupportedChainId.BSC_TESTNET]: 'BNBChain Testnet',
}
```

- `/src/constants/addresses.ts`

```jsx
export const MULTICALL2_ADDRESSES: AddressMap = {
  ...constructSameAddressMap('0x5BA1e12693Dc8F9c48aAD8770482f4739bEeD696'),
  [SupportedChainId.ARBITRUM_ONE]: '0x021CeAC7e681dBCE9b5039d2535ED97590eB395c',
  [SupportedChainId.ARBITRUM_RINKEBY]: '0x334f67349c1cB3A8fF1268c3eC43FF1D3De246C6',
  [SupportedChainId.BSC_TESTNET]: '0x642050B432c1107A15047D5De5294BdB761e8e00',
}
```

```jsx
export const V3_CORE_FACTORY_ADDRESSES: AddressMap = {
  ...constructSameAddressMap(V3_FACTORY_ADDRESS, [SupportedChainId.ARBITRUM_ONE, SupportedChainId.ARBITRUM_RINKEBY]),
  [SupportedChainId.BSC_TESTNET]: '0xdaD5a51f02b60e7d67A7083Db9B077B1DA5b5907',
}
export const QUOTER_ADDRESSES: AddressMap = {
  ...constructSameAddressMap('0xb27308f9F90D607463bb33eA1BeBb41C27CE5AB6', [
    SupportedChainId.ARBITRUM_ONE,
    SupportedChainId.ARBITRUM_RINKEBY,
  ]),
  [SupportedChainId.BSC_TESTNET]: '0x4224fc5e8675E8d43f575699FDfE767BaEDe3b87',
}
export const NONFUNGIBLE_POSITION_MANAGER_ADDRESSES: AddressMap = {
  ...constructSameAddressMap('0xC36442b4a4522E871399CD717aBDD847Ab11FE88', [
    SupportedChainId.ARBITRUM_ONE,
    SupportedChainId.ARBITRUM_RINKEBY,
  ]),
  [SupportedChainId.BSC_TESTNET]: '0x7899b665DE3D23FAEfbF0022655439797a0F408d',
}
```

```jsx
export const SWAP_ROUTER_ADDRESSES: AddressMap = {
  ...constructSameAddressMap('0xE592427A0AEce92De3Edee1F18E0157C05861564', [
    SupportedChainId.ARBITRUM_ONE,
    SupportedChainId.ARBITRUM_RINKEBY,
  ]),
  [SupportedChainId.BSC_TESTNET]: '0x18Cbe8F47B0D6DAa3c005DD10cC583ab1d047F9C',
}
export const V3_MIGRATOR_ADDRESSES: AddressMap = {
  ...constructSameAddressMap('0xA5644E29708357803b5A882D272c41cC0dF92B34', [
    SupportedChainId.ARBITRUM_ONE,
    SupportedChainId.ARBITRUM_RINKEBY,
  ]),
  [SupportedChainId.BSC_TESTNET]: '0x7a8bEE82F7e297138a478f029B0680de32C4Ee31',
}
```

- `/src/connectors/index.ts`

```jsx
const NETWORK_URLS: {
  [chainId in SupportedChainId]: string
} = {
  // ......
  [SupportedChainId.BSC_TESTNET]: 'https://bsc-testnet.nodereal.io/v1/cd23340661a64776a3995ffea4c22539',
}
const SUPPORTED_CHAIN_IDS: SupportedChainId[] = [
  // ......
  SupportedChainId.BSC_TESTNET,
]
```

- `/src/constants/tokens.ts`

```jsx
export const USDT_BSC_TESTNET = new Token(
  SupportedChainId.BSC_TESTNET,
  '0x65c2ae1FA5e7f12452F55D71d5dFbe482Feb9db9',
  18,
  'USDT',
  'Tether USD'
)
```

```jsx
export const WETH9_EXTENDED: { [chainId: number]: Token } = {
	// ......
  [SupportedChainId.BSC_TESTNET]: new Token(
    SupportedChainId.BSC_TESTNET,
    '0x4Ae32D12b1c30e482A26Bad11d0E0682d91b93a5',
    18,
    'WETH',
    'Wrapped Ether'
  ),
}
```

- `/src/utils/getExplorerLink.ts`

```jsx
export function getExplorerLink(chainId: number, data: string, type: ExplorerDataType): string {
  // ......
  if (chainId === SupportedChainId.BSC_TESTNET) {
    switch (type) {
      case ExplorerDataType.TRANSACTION:
        return `https://testnet.bscscan.com/tx/${data}`
      case ExplorerDataType.ADDRESS:
        return `https://testnet.bscscan.com/address/${data}`
      case ExplorerDataType.BLOCK:
        return `https://testnet.bscscan.com/block/${data}`
      default:
        return `https://testnet.bscscan.com/`
    }
  }
  // ......
}
```

- `/src/utils/getLibrary.ts`

```jsx

const NETWORK_POLLING_INTERVALS: { [chainId: number]: number } = {
  // ......
  [SupportedChainId.BSC_TESTNET]: 1_000,
}
```

- `/src/components/PositionListItem/index.tsx`

```jsx
import { DAI, USDC, USDT_BSC_TESTNET, USDT, WBTC, WETH9_EXTENDED } from '../../constants/tokens'
// ......

export function getPriceOrderingFromPositionForUI(position?: Position): {
  priceLower?: Price<Token, Token>
  priceUpper?: Price<Token, Token>
  quote?: Token
  base?: Token
} {
  // ......
  // if token0 is a dollar-stable asset, set it as the quote token
  const stables = [DAI, USDC, USDT, USDT_BSC_TESTNET]
  // ......
}
```

- `/src/constants/routing.ts`

```jsx
import {
  // ......
  USDT_BSC_TESTNET,
} from './tokens'

const WETH_ONLY: ChainTokenList = {
  // ......
  [SupportedChainId.BSC_TESTNET]: [WETH9_EXTENDED[SupportedChainId.BSC_TESTNET]],
}
// used to construct intermediary pairs for trading
export const BASES_TO_CHECK_TRADES_AGAINST: ChainTokenList = {
  // ......
  [SupportedChainId.BSC_TESTNET]: [...WETH_ONLY[SupportedChainId.BSC_TESTNET], USDT_BSC_TESTNET],
}
```

```jsx
export const COMMON_BASES: ChainCurrencyList = {
  // ......
  [SupportedChainId.BSC_TESTNET]: [
    ExtendedEther.onChain(SupportedChainId.BSC_TESTNET),
    WETH9_EXTENDED[SupportedChainId.BSC_TESTNET],
    USDT_BSC_TESTNET,
  ],
}
```

- `/src/hooks/useUSDCPrice.ts`

```jsx
import { USDC, USDC_ARBITRUM, USDT_BSC_TESTNET } from '../constants/tokens'
  // ......
const STABLECOIN_AMOUNT_OUT: { [chainId: number]: CurrencyAmount<Token> } = {
  // ......
  [SupportedChainId.BSC_TESTNET]: CurrencyAmount.fromRawAmount(USDT_BSC_TESTNET, 10_000e6),
}
```

---