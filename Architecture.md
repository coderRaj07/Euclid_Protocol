# Euclid Protocol - Liquidity Sync & Messaging Protocol

## Overview

The **Euclid Protocol** enables seamless interaction with liquidity pools across multiple blockchains and exchanges by leveraging the **Liquisync Model** and **Euclid Messaging Protocol (EMP)**. It ensures optimized routes, low slippage, and transaction finality across chains.

---

## Key Features

- **Virtual Liquidity Pools**: Access liquidity across multiple chains.
- **Euclid Messaging Protocol (EMP)**: Guarantees transaction finality.
- **Liquisync Model**: Dynamically provides liquidity where needed.
- **Optimized Routes**: Implements graph algorithms for the best slippage and prices.

---

## Key Components

### Constant Product Formula for Liquidity Pools

The constant product formula maintains balance in pools by keeping the product of token quantities constant:

```
x × y = k
```

### Example Calculation with Two Pools

Assuming there are **two separate liquidity pools**:

- **Pool 1:**
  - Initial X tokens (`x1`): 100
  - Initial Y tokens (`y1`): 100

- **Pool 2:**
  - Initial X tokens (`x2`): 100
  - Initial Y tokens (`y2`): 100

**Total in VSL:**
- Total X tokens: `x1 + x2 = 200`
- Total Y tokens: `y1 + y2 = 200`
- Constant Product (`k`): `200 × 200 = 40,000`

**To Swap 10 X tokens for Y tokens:**

1. **New X tokens after swap:**
   ```
   New X = Total X + 10 = 200 + 10 = 210
   ```

2. **Calculate new Y tokens:**
   ```
   Y = k / (New X) = 40,000 / 210 ≈ 190.48
   ```

3. **Tokens received by the trader:**
   ```
   Y received = Total Y - New Y = 200 - 190.48 ≈ 9.52 Y tokens
   ```

### Virtual Settlement Layer (VSL)

The VSL consists of two main components:

1. **Virtual Liquidity Pools (VLP)**: Responsible for performing swap calculations for token pairs using the Constant Product formula.

2. **Virtual Balances**: Keeps track of balances for users and Virtual Liquidity Pools (VLPs), ensuring accurate accounting.

---

## Transaction Workflow

1. **User Sends Request**: The user initiates a swap request to the **Factory**.
2. **Factory to Router**: The **Factory** forwards the request to the **Router** through a dedicated communication channel.
3. **Router to VSL**: The **Router** sends the swap request to the **Virtual Settlement Layer (VSL)** for calculation.
4. **VSL Acknowledgment**: The **VSL** returns an acknowledgment of the completed calculations to the **Router**.
5. **Router to Factory**: The **Router** forwards the acknowledgment to the **Factory**.
6. **Factory to Escrow**: The **Factory** instructs the **Escrow** to release the appropriate tokens to the user.
7. **Escrow**: The **Escrow** releases the tokens, completing the swap.

---

## Escrow Smart Contract

- Each **Escrow Smart Contract** holds one type of token.
- Multiple escrows can exist for a chain.
- **Successful Transaction**: An escrow holds Token A, and another escrow releases Token B to the user.
- **Failed Transaction**: If the swap fails, Token A is returned to the user.

---

## Relayers

**Relayers** facilitate communication between the **Virtual Settlement Layer (VSL)** and integrated chains, ensuring smooth execution of transactions and liquidity updates.

### Key Functions of Relayers:

1. **Trade Requests**: Handle messages sent from the chain to the VSL for processing.
2. **Execution Messages**: Relay execution instructions from the VSL back to the originating chain.
3. **Public and Private Relayers**: 
   - **Public Relayers** manage high volumes of Inter-Blockchain Communication (IBC) messages.
   - **Private Relayers** operate within Euclid's private ecosystem, ensuring secure and efficient operations. 

---

## Router

The **Router** performs essential tasks for facilitating cross-chain operations:

1. **Initialization**: Initializes the **Virtual Balance Contract** and registers new **Factory Contracts** on integrated chains.
   
2. **Communication**: Sends and receives packets to and from the **Virtual Settlement Layer (VSL)**, coordinating liquidity and swap operations across chains.

3. **Handles Replies**: Processes replies from sub-messages and packets, forwarding results to the appropriate destination, ensuring proper state updates and request outcomes.

---

## Conclusion

The **Euclid Protocol** streamlines cross-chain liquidity management and trade execution, significantly reducing slippage and improving user experience across decentralized finance (DeFi) applications. With the combined strength of its components, it offers an efficient and robust solution for liquidity synchronization and messaging across multiple ecosystems.
