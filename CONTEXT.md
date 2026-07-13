# Benji

Benji provides long-term investment guidance and manages a simulated Brokerage Account for an Investor.

## Language

**Investor**:
The person Benji advises and for whom one simulated Brokerage Account is maintained.
_Avoid_: User, client, customer

**Benji**:
The wealth manager that advises an Investor and proposes changes to their simulated Brokerage Account during Investor-initiated conversations. Outside financial circumstances may inform its advice but are not managed records.
_Avoid_: Advisor, chatbot, assistant

**Investor Approval**:
The Investor's explicit consent for a specific change to their Brokerage Account. Benji cannot execute a change without it.
_Avoid_: Permission, confirmation

**Investor-Directed Market Order**:
A Market Order whose security, side, and quantity are explicitly chosen by the Investor. Benji may execute one without an Investment Strategy after Investor Approval.
_Avoid_: Requested trade, manual trade

**Market Order**:
An instruction to immediately buy or sell an exact whole-share quantity of a Tradable Security at the current quote. Investor Approval authorizes the side, Ticker, and quantity, not a guaranteed fill price.
_Avoid_: Trade, transaction

**Tradable Security**:
An active US-listed stock or ETF supported by Paper Trade. A supported ETF may provide exposure to an otherwise unsupported asset class, but Benji cannot manage the underlying assets directly.
_Avoid_: Asset, investment, instrument

**Investment Philosophy**:
Benji's default long-term approach: broad diversification through broad-market ETFs, low-cost indexing, disciplined rebalancing, and resistance to market timing. Individual stocks are considered when requested, with their concentration risk made explicit. The philosophy guides judgment without overriding the Investment Strategy.
_Avoid_: Doctrine, rules

**Investment Strategy**:
The optional single current record of an Investor's investment goals, time horizon, risk tolerance, liquidity needs, and preferences or restrictions that guides Benji's recommendations when present. Unknown dimensions remain explicit rather than inferred.
_Avoid_: Portfolio strategy notes, plan, profile

**Material Strategy Change**:
A proposed Investment Strategy change that could alter Benji's recommendations or that depends on an assumption by Benji. It requires Investor confirmation before becoming current.
_Avoid_: Large change, major update

**Strategy Clarification**:
An explicit change that preserves the meaning of the Investment Strategy. Benji may apply it immediately and then notify the Investor.
_Avoid_: Small change, minor update

**Brokerage Account**:
An Investor's simulated account containing cash, Positions, and Account Activity. It is created with an Investor-chosen Starting Cash amount after Investor Approval.
_Avoid_: Portfolio, trading account

**Starting Cash**:
The simulated USD cash balance established when a Brokerage Account is created.
_Avoid_: Initial deposit, opening balance

**Position**:
The whole shares of one Tradable Security held in a Brokerage Account, together with their average cost basis.
_Avoid_: Holding, investment

**Account Activity**:
A recorded Brokerage Account event such as Starting Cash, a deposit, a withdrawal, or a Market Order fill.
_Avoid_: Transaction, history entry

**Portfolio Valuation**:
A point-in-time USD view that combines a Brokerage Account with current quotes to value its cash and Positions, gains or losses, and allocation.
_Avoid_: Account snapshot, portfolio performance

**Chat**:
One durable conversation between an Investor and Benji. An Investor may have multiple Chats, all sharing the same Brokerage Account and Investment Strategy.
_Avoid_: Thread, session, conversation
