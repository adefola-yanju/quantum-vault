# Quantum Vault - Advanced Collateralized Lending Engine

[![Clarity Version](https://img.shields.io/badge/Clarity-v3-blue)](https://docs.stacks.co/clarity)
[![License](https://img.shields.io/badge/License-ISC-green.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen.svg)](#testing)

## Overview

Quantum Vault revolutionizes decentralized finance by creating a trustless, intelligent lending ecosystem where digital asset holders can unlock instant liquidity while maintaining ownership of their appreciating assets. Built on cutting-edge risk assessment algorithms and autonomous market mechanisms, this protocol establishes a fully autonomous lending marketplace that transforms illiquid digital assets into productive capital.

## Key Features

### 🚀 **Core Functionality**

- **Instant Collateral-Backed Lending** - Zero counterparty risk with immediate loan execution
- **Adaptive Interest Rate Mechanisms** - Dynamic rates based on real-time supply/demand dynamics
- **Predictive Liquidation Protection** - AI-powered risk assessment with automated safeguards
- **Multi-Asset Support** - Currently supports BTC and STX with extensible architecture
- **Institutional-Grade Security** - Professional security standards with retail accessibility

### 🛡️ **Risk Management**

- **Dynamic Collateral Optimization** - Real-time collateral ratio monitoring
- **Automated Liquidation System** - Autonomous position management
- **Price Oracle Integration** - Real-time market data for accurate valuations
- **Comprehensive Audit Trail** - Full transaction and position tracking

### 📊 **Protocol Economics**

- **Self-Sustaining Engine** - Competitive yields for lenders, instant access for borrowers
- **Protocol Fee Structure** - Transparent 1% platform revenue fee
- **Capital Efficiency** - Maximum utilization of deposited assets
- **Governance Controls** - Owner-managed parameter adjustments

## Smart Contract Architecture

### Core Components

#### **State Management**

```clarity
;; Protocol configuration
minimum-collateral-ratio: 150%  // Minimum collateral requirement
liquidation-threshold: 120%     // Automatic liquidation trigger
platform-fee-rate: 1%          // Protocol revenue fee
```

#### **Data Structures**

- **Loans Registry** - Comprehensive loan tracking with borrower details, amounts, and status
- **User Portfolio** - Individual loan management up to 10 active loans per user
- **Price Oracle** - Real-time asset pricing for accurate collateral valuation

#### **Security Framework**

- **Input Validation** - Comprehensive parameter checking and bounds validation
- **Access Control** - Owner-only administrative functions
- **Error Handling** - Detailed error codes for all failure scenarios

## Getting Started

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) - Stacks smart contract development tool
- [Node.js](https://nodejs.org/) v18+ - For running tests
- [Stacks Wallet](https://www.hiro.so/wallet) - For contract interaction

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/adefola-yanju/quantum-vault.git
   cd quantum-vault
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Verify installation**

   ```bash
   clarinet check
   ```

### Development Setup

1. **Initialize the development environment**

   ```bash
   clarinet console
   ```

2. **Deploy to local testnet**

   ```bash
   clarinet integrate
   ```

3. **Run the test suite**

   ```bash
   npm test
   ```

## Usage Guide

### Basic Operations

#### 1. Initialize Platform

```clarity
;; Deploy and initialize the protocol (owner only)
(contract-call? .quantum-vault initialize-platform)
```

#### 2. Set Asset Prices

```clarity
;; Update price feeds for supported assets
(contract-call? .quantum-vault update-price-feed "BTC" u50000000000) ;; $50,000 BTC
(contract-call? .quantum-vault update-price-feed "STX" u200000000)   ;; $2.00 STX
```

#### 3. Deposit Collateral

```clarity
;; Deposit BTC as collateral
(contract-call? .quantum-vault deposit-collateral u100000000) ;; 1 BTC
```

#### 4. Request Loan

```clarity
;; Request loan against collateral
(contract-call? .quantum-vault request-loan 
  u100000000  ;; 1 BTC collateral
  u20000000000) ;; $20,000 loan amount
```

#### 5. Repay Loan

```clarity
;; Repay loan with interest
(contract-call? .quantum-vault repay-loan 
  u1           ;; loan ID
  u21000000000) ;; repayment amount (principal + interest)
```

### Advanced Features

#### Portfolio Management

```clarity
;; Check user's active loans
(contract-call? .quantum-vault get-user-loans tx-sender)

;; Get detailed loan information
(contract-call? .quantum-vault get-loan-details u1)
```

#### Protocol Analytics

```clarity
;; View platform statistics
(contract-call? .quantum-vault get-platform-stats)

;; Check supported assets
(contract-call? .quantum-vault get-valid-assets)
```

## Testing

### Running Tests

```bash
# Run all tests
npm test

# Run tests with coverage report
npm run test:report

# Watch mode for development
npm run test:watch
```

### Test Coverage

The test suite covers:

- ✅ Platform initialization and configuration
- ✅ Collateral deposit and withdrawal
- ✅ Loan origination and validation
- ✅ Interest calculation and compounding
- ✅ Liquidation triggers and execution
- ✅ Error handling and edge cases
- ✅ Access control and permissions

### Contract Validation

```bash
# Check contract syntax and types
clarinet check

# Analyze contract for potential issues
clarinet analyze
```

## API Reference

### Public Functions

#### Administrative Functions

| Function | Description | Access |
|----------|-------------|---------|
| `initialize-platform()` | Initialize the lending protocol | Owner only |
| `update-collateral-ratio(uint)` | Adjust minimum collateral requirements | Owner only |
| `update-liquidation-threshold(uint)` | Modify liquidation trigger point | Owner only |
| `update-price-feed(string, uint)` | Update asset price oracle | Owner only |

#### Core Operations

| Function | Description | Parameters |
|----------|-------------|------------|
| `deposit-collateral(uint)` | Deposit assets as collateral | `amount` - Collateral amount |
| `request-loan(uint, uint)` | Request loan against collateral | `collateral`, `loan-amount` |
| `repay-loan(uint, uint)` | Repay existing loan | `loan-id`, `amount` |

#### Query Functions

| Function | Description | Returns |
|----------|-------------|---------|
| `get-loan-details(uint)` | Retrieve loan information | Loan details object |
| `get-user-loans(principal)` | Get user's active loans | List of loan IDs |
| `get-platform-stats()` | Platform health metrics | Statistics object |
| `get-valid-assets()` | Supported asset list | Asset array |

## Security Considerations

### Risk Mitigation

- **Collateral Requirements** - 150% minimum ratio prevents under-collateralization
- **Liquidation Protection** - 120% threshold provides safety buffer
- **Price Validation** - Sanity checks prevent oracle manipulation
- **Access Controls** - Critical functions restricted to contract owner

### Best Practices

- Always verify collateral ratios before borrowing
- Monitor liquidation thresholds during market volatility
- Keep loans well-collateralized above minimum requirements
- Regular repayments reduce accumulated interest

### Known Limitations

- Maximum 10 active loans per user
- Currently supports BTC and STX only
- Owner-controlled price feeds (future: decentralized oracles)
- Fixed interest rates (future: dynamic rate models)

## Deployment

### Testnet Deployment

1. **Configure network settings**

   ```toml
   # Clarinet.toml
   [network]
   name = "testnet"
   deployment_fee_rate = 10
   ```

2. **Deploy contract**

   ```bash
   clarinet deployments apply --network testnet
   ```

3. **Verify deployment**

   ```bash
   clarinet deployments check --network testnet
   ```

### Mainnet Deployment

⚠️ **Mainnet deployment requires thorough testing and security audits**

1. Complete comprehensive security audit
2. Perform extensive testnet validation
3. Configure production parameters
4. Execute phased mainnet deployment

## Contributing

We welcome contributions to Quantum Vault! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

### Development Workflow

1. **Fork the repository**
2. **Create feature branch** (`git checkout -b feature/amazing-feature`)
3. **Write tests** for new functionality
4. **Ensure all tests pass** (`npm test`)
5. **Submit pull request** with detailed description

### Code Standards

- Follow Clarity best practices and conventions
- Maintain 100% test coverage for new features
- Include comprehensive documentation
- Perform security analysis for all changes

## Roadmap

### Phase 1: Core Protocol ✅

- [x] Basic lending and borrowing functionality
- [x] Collateral management system
- [x] Liquidation mechanisms
- [x] Test suite and documentation

### Phase 2: Enhanced Features 🚧

- [ ] Dynamic interest rate models
- [ ] Flash loan capabilities
- [ ] Cross-chain asset support
- [ ] Governance token integration

### Phase 3: Advanced Features 📋

- [ ] Decentralized price oracles
- [ ] Insurance fund mechanism
- [ ] Yield farming integration
- [ ] Mobile application interface

## License

This project is licensed under the ISC License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Stacks Foundation for blockchain infrastructure
- Clarity language development team
- DeFi community for inspiration and feedback
- Security auditors and contributors
