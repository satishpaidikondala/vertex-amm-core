# DEX AMM Project

## Overview
A simplified Decentralized Exchange (DEX) using the Automated Market Maker (AMM) model, similar to Uniswap V2. Allows users to provide liquidity and swap tokens using the Constant Product Formula.

## Features
- Initial and subsequent liquidity provision
- Liquidity removal with proportional share calculation
- Token swaps using constant product formula (x * y = k)
- 0.3% trading fee for liquidity providers
- LP token minting and burning

## Architecture
- **DEX.sol**: Core smart contract managing reserves, liquidity tokens, and swaps.
- **MockERC20.sol**: ERC20 token for testing.

## Mathematical Implementation

### Constant Product Formula
The core invariant is `x * y = k`, where `x` and `y` are the reserves of Token A and Token B. 
When swapping `dx` amount of Token A:
`y_new = k / (x + dx)`
`amountOut = y - y_new`

### Fee Calculation
A 0.3% fee is applied to the input amount *before* the constant product calculation.
`amountInWithFee = amountIn * 997`
Numerator: `amountInWithFee * reserveOut`
Denominator: `(reserveIn * 1000) + amountInWithFee`

### LP Token Minting
- **Initial Supply**: `sqrt(amountA * amountB)`
- **Subsequent Supply**: `min((amountA * totalLiquidity / reserveA), (amountB * totalLiquidity / reserveB))`

## Setup Instructions

### Prerequisites
- Docker and Docker Compose installed
- Git

### Installation

1. Clone the repository:
```bash
git clone <your-repo-url>
cd dex-amm
```

2. Start Docker environment:
```bash
docker-compose up -d
```

3. Compile contracts:
```bash
docker-compose exec app npm run compile
```

4. Run tests:
```bash
docker-compose exec app npm test
```

5. Check coverage:
```bash
docker-compose exec app npm run coverage
```

6. Stop Docker:
```bash
docker-compose down
```

## Running Tests Locally (without Docker)
```bash
npm install
npm run compile
npm test
```

## Contract Addresses
(Local Deployment)

## Known Limitations
- No slippage protection (minAmountOut)
- No deadline checks
- Tokens with transfer fees/rebasing not supported
- No unchecked math (using Solidity 0.8+ checked arithmetic)

## Security Considerations
- Checks for zero amounts
- Reentrancy protection (implied usage, though basic AMM typically safe if following checks-effects-interactions, ReentrancyGuard can be added)
- Transfer return value checks (using `require` on `transfer/transferFrom` results)