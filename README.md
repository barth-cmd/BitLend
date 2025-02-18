# BitLend: Bitcoin-Backed Lending Protocol

BitLend is a secure, decentralized lending protocol built on Stacks Layer 2, leveraging Bitcoin's robust security through proof-of-work while enabling efficient DeFi operations.

## Overview

BitLend revolutionizes DeFi by bringing Bitcoin's security to decentralized lending through Stacks L2 technology. The protocol enables users to participate in a lending market that inherits Bitcoin's security properties while maintaining the programmability of Stacks.

## Key Features

- **Bitcoin-Secured Collateral**: Leverage Bitcoin's security for loan collateral
- **Dynamic Interest Rates**: Market-driven interest rate model
- **Risk-Adjusted Liquidations**: Automated position management
- **Layer 2 Scalability**: Efficient operations with Layer 1 security
- **Transparent Operations**: Verifiable state transitions

## Protocol Parameters

- Maximum Collateral Ratio: 500%
- Minimum Collateral Ratio: 110%
- Default Collateralization Ratio: 150%
- Liquidation Threshold: 130%
- Protocol Fee: 1% (Maximum: 10%)

## Core Functions

### User Operations

#### Deposit

```clarity
(define-public (deposit))
```

Allows users to deposit STX tokens as collateral. The amount is automatically determined from the user's balance.

#### Borrow

```clarity
(define-public (borrow (amount uint)))
```

Enables users to borrow STX against their deposited collateral, maintaining the required collateralization ratio.

#### Repay

```clarity
(define-public (repay (amount uint)))
```

Allows borrowers to repay their loans, reducing their borrowed amount.

#### Withdraw

```clarity
(define-public (withdraw (amount uint)))
```

Enables users to withdraw their collateral if maintaining sufficient collateralization.

### Liquidation Mechanism

```clarity
(define-public (liquidate (user principal)))
```

Handles liquidation of under-collateralized positions:

- Triggers at 130% collateral ratio
- Prevents self-liquidation
- Automatically transfers collateral to liquidator
- Clears user position after successful liquidation

### Query Functions

#### Get User Position

```clarity
(define-read-only (get-user-position (user principal)))
```

Returns user's current position:

- Total collateral
- Total borrowed amount
- Loan count

#### Get Protocol Stats

```clarity
(define-read-only (get-protocol-stats))
```

Returns protocol-wide statistics:

- Total deposits
- Total borrows
- Current parameters

### Administrative Controls

#### Set Minimum Collateral Ratio

```clarity
(define-public (set-minimum-collateral-ratio (new-ratio uint)))
```

Allows admin to adjust minimum collateral ratio within bounds (110% - 500%).

#### Set Liquidation Threshold

```clarity
(define-public (set-liquidation-threshold (new-threshold uint)))
```

Enables adjustment of liquidation trigger threshold.

#### Set Protocol Fee

```clarity
(define-public (set-protocol-fee (new-fee uint)))
```

Allows modification of protocol fee (maximum 10%).

## Security Features

1. **Row-Level Security**

   - Strict access controls
   - Position-specific permissions
   - Protected administrative functions

2. **Safety Checks**

   - Collateral ratio validation
   - Balance verification
   - Operation bounds checking

3. **Error Handling**
   - Comprehensive error codes
   - Graceful failure states
   - Clear error messages

## Technical Implementation

### Data Structures

#### Loans Map

```clarity
{
    loan-id: uint,
    borrower: principal,
    collateral-amount: uint,
    borrowed-amount: uint,
    interest-rate: uint,
    start-height: uint,
    last-interest-update: uint,
    active: bool
}
```

#### User Positions Map

```clarity
{
    user: principal,
    total-collateral: uint,
    total-borrowed: uint,
    loan-count: uint
}
```

### Interest Calculation

Interest is calculated per block using the formula:

```clarity
interest-per-block = (principal * rate) / 10000
total-interest = interest-per-block * blocks
```

## Best Practices for Integration

1. **Position Management**

   - Monitor collateral ratios regularly
   - Maintain safe buffer above liquidation threshold
   - Repay loans promptly to avoid liquidation

2. **Risk Management**

   - Start with small positions
   - Use conservative collateral ratios
   - Set up liquidation monitoring

3. **Transaction Optimization**
   - Batch related operations
   - Monitor gas costs
   - Implement proper error handling

## Development and Testing

To interact with the protocol:

1. Deploy the contract to Stacks testnet
2. Use the provided function calls to test operations
3. Monitor positions through read-only functions
4. Test liquidation scenarios safely

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request with detailed description
4. Ensure all tests pass
