# Designing an Online Stock Brokerage System

In this article, we'll examine the object-oriented design and implementation of an Online Stock Brokerage System using Python3. 

This system simulates key functionalities of stock trading platforms, enabling users to engage in buying and selling stocks, manage their portfolios, and stay updated with stock prices.

## System Requirements

The Stock Brokerage System needs to:

1. **User Account Management:** Manage user registrations and profiles.
2. **Stock Trading:** Enable stock buying and selling.
3. **Portfolio Management:** Allow users to manage their stock holdings.
4. **Stock Price Feed:** Provide real-time stock price updates.

## Core Use Cases

1. **Registering and Managing User Accounts**
2. **Buying and Selling Stocks**
3. **Managing Portfolio**
4. **Viewing Stock Prices**

## Key Classes:
- `StockBrokerageSystem`: Manages the entire system.
- `User`: Represents a system user.
- `Stock`: Represents a stock in the market.
- `Portfolio`: Manages a user's stock holdings.
- `Trade`: Handles stock trade transactions.

## Python3 Implementation

### User Class
Manages user account information.

```python
import uuid

class User:
    def __init__(self, name: str):
        self.user_id = str(uuid.uuid4())
        self.name = name
        self.portfolio = Portfolio()

    def execute_trade(self, stock, quantity, trade_type, system):
        trade = Trade(self, stock, quantity, trade_type)
        system.execute_trade(trade)

```
### Stock Class
Represents a stock in the market.
```python
class Stock:
    def __init__(self, symbol: str, price: float):
        self.symbol = symbol
        self.price = price

    def update_price(self, new_price: float):
        self.price = new_price

```
### Portfolio Class
Manages a user's stock holdings.
```python
class Portfolio:
    def __init__(self):
        self.holdings = {}

    def add_stock(self, stock, quantity):
        if stock in self.holdings:
            self.holdings[stock] += quantity
        else:
            self.holdings[stock] = quantity

    def remove_stock(self, stock, quantity):
        if stock in self.holdings and self.holdings[stock] >= quantity:
            self.holdings[stock] -= quantity
            if self.holdings[stock] == 0:
                del self.holdings[stock]

```
### Trade Class
Represents a stock trade transaction.
```python
from enum import Enum

class TradeType(Enum):
    BUY = 'BUY'
    SELL = 'SELL'

class Trade:
    def __init__(self, user: User, stock: Stock, quantity: int, trade_type: TradeType):
        self.user = user
        self.stock = stock
        self.quantity = quantity
        self.trade_type = trade_type

```
### StockBrokerageSystem Class
Main class managing the brokerage system.
```python
class StockBrokerageSystem:
    def __init__(self):
        self.users = []
        self.stocks = []

    def execute_trade(self, trade: Trade):
        if trade.trade_type == TradeType.BUY:
            self.process_buy_trade(trade)
        elif trade.trade_type == TradeType.SELL:
            self.process_sell_trade(trade)

    def process_buy_trade(self, trade: Trade):
        trade.user.portfolio.add_stock(trade.stock, trade.quantity)

    def process_sell_trade(self, trade: Trade):
        trade.user.portfolio.remove_stock(trade.stock, trade.quantity)

```

### Example Usage
``` python
def main():
    # Create system, users, and stocks
    system = StockBrokerageSystem()
    user = User("Alice")
    stock = Stock("AAPL", 150.00)
    system.stocks.append(stock)

    # User buys stock
    user.execute_trade(stock, 10, TradeType.BUY, system)

    # User sells stock
    user.execute_trade(stock, 5, TradeType.SELL, system)

    # Print portfolio details
    for stock, quantity in user.portfolio.holdings.items():
        print(f"{stock.symbol}: {quantity}")

if __name__ == "__main__":
    main()

```