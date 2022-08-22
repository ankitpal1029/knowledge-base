# White Paper Simple

## Problem

Problems in current DeFi as compared to tradefi

- Over collateralisation is a downgrade from tradefi
- Massively fluctuating interest rates

What Hashstack does:

- Set apr, apy depending on commitment periods
- Effective asset utilization
- Under collateralised loans

![](assets/2022-08-22-10-22-00.png)

## Open Protocol

- Primary market: deposits and loans here
- Secondary market: borrowed loans can be swapped to and from secondary market

1 epoch = 3 secs

### Deposit Flow

#### Fixed Deposits

1. Connect Account
2. Choose deposit market, deposit type(fixed, flexible) minimum commitment period
3. Transfer funds to reserve

Protocol offers dividends to fixed deposit when net difference of monthly yield with monthly percentage yield is positive

#### Flexible Deposits

don't offer dividends, earn a fixed apy of 7.8% since no lock in period

### Borrower Flow

- Fixed loans: pay 15%
- Flexible loans: 18%

protocol deploys collateral as a fixed deposit with mcp of 2 weeks earning 10% apy on collateral, no dividends here

[hashstack](https://blog.hashstack.finance/deconstructing-hashstacks-dynamic-interest-algorithm-dial/)

[Potocol](https://github.com/0xHashstack/whitepaper/tree/main/Open%20protocol/v1.0%5Bdraft%5D)
