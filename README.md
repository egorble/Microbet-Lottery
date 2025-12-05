# Up/Down Prediction Game on Linera
Frontend part - https://github.com/egorble/MicrobetFront
## Overview
This is a cross-chain prediction game built on the Linera blockchain platform. Players can predict whether a value will go "UP" or "DOWN" for each round and place bets using NAT tokens.

This implementation extends the native fungible token example with prediction game functionality, allowing users to place bets on price movements and win rewards based on correct predictions.

## Features
- Native token (NAT) transfers with prediction parameters
- Cross-chain token transfers with automatic bet placement
- Round-based prediction system (Active, Closed, Resolved)
- Automatic creation of new rounds when current round is closed
- Automatic reward distribution when a round is resolved
- Rewards are distributed from the resolver's owner balance
- Proportional reward distribution to winners
- GraphQL interface for queries and mutations

## How It Works

### Core Components
1. **Rounds**: Time-based prediction periods with three states:
   - Active: Players can place bets
   - Closed: No more bets, waiting for result
   - Resolved: Results announced, rewards automatically distributed

2. **Bets**: Players place bets on "UP" or "DOWN" predictions during active rounds

3. **Cross-chain transfers**: NAT tokens can be transferred between chains with prediction parameters

### Game Flow
1. Admin creates a round (becomes Active) or a new round is automatically created
2. Players transfer NAT tokens with prediction parameter (UP/DOWN)
3. Admin closes the round (provides closing price as decimal value)
4. System automatically creates a new active round
5. Admin resolves the closed round with a resolution price
6. System automatically distributes rewards to all winners from the resolver's owner balance

### Cross-chain Functionality
- Tokens transferred with prediction parameter automatically create bets
- Winners on other chains automatically receive rewards via cross-chain transfers
- All operations work seamlessly across multiple chains

### Key Operations
- `createRound`: Start a new prediction round
- `transfer`: Send tokens with prediction (automatically places bet)
- `closeRound`: Close active round for betting (accepts decimal price) and automatically creates new round
- `resolveRound`: Resolve closed round with result (accepts decimal price) and automatically distributes rewards from resolver's owner balance
- `claimWinnings`: Winner claims their rewards (for manual claiming if needed)

### GraphQL Endpoints
- Queries: Get rounds, active bets, resolved bets
- Mutations: All operations that change state

The system automatically handles bet placement when tokens are transferred with prediction parameters, making it easy for players to participate from any chain. Prices can now be specified as decimal values (e.g., "123.45") instead of just integers.

When a round is closed, a new active round is automatically created, ensuring continuous gameplay without manual intervention.

When a round is resolved, rewards are automatically distributed to all winners from the resolver's owner balance, eliminating the need for a separate reward distribution operation.

## Smart Contract Structure
- `contract.rs`: Main contract logic handling operations and messages
- `service.rs`: GraphQL service implementation for queries and mutations
- `state.rs`: State management using Linera's view system
- `lib.rs`: Data structures and ABI definitions

## Building and Deployment
This contract follows the standard Linera smart contract structure and can be built and deployed using the Linera SDK tooling.
