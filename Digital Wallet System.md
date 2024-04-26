# Designing a Digital Wallet System

In this article, we will explore the object-oriented design and implementation of a Digital Wallet System using Python3. 

This system facilitates online transactions, enabling users to store money digitally and make secure payments.

## System Requirements

The Digital Wallet System should:

1. **User Account Management**: Manage user account creation and maintenance.
2. **Wallet Management**: Allow users to add, withdraw, and check balances.
3. **Transaction Processing**: Handle transactions and maintain a history.
4. **Security and Authentication**: Ensure secure access and transaction integrity.

## Core Use Cases

1. **Creating and Managing User Accounts**
2. **Adding and Withdrawing Funds**
3. **Making and Receiving Payments**
4. **Viewing Transaction History**

## Key Classes:
- `DigitalWalletSystem`: Manages the system.
- `User`: Represents a user.
- `Wallet`: Manages a user's wallet.
- `Transaction`: Represents a transaction.

## Python3 Implementation

### User Class

Manages user account information.

```python
from typing import List, Optional
from datetime import datetime

class User:
    def __init__(self, user_id: str, name: str) -> None:
        self.user_id: str = user_id
        self.name: str = name
        self.wallet: 'Wallet' = Wallet(self)

class Wallet:
    def __init__(self, user: User) -> None:
        self.user: User = user
        self.balance: float = 0.0
        self.transaction_history: List['Transaction'] = []

    def add_funds(self, amount: float) -> bool:
        if amount <= 0:
            return False
        self.balance += amount
        self.transaction_history.append(Transaction("Deposit", amount, self.balance))
        return True

    def withdraw_funds(self, amount: float) -> bool:
        if amount > self.balance:
            return False
        self.balance -= amount
        self.transaction_history.append(Transaction("Withdrawal", amount, self.balance))
        return True
```
### Wallet Class
Represents a user's digital wallet.
```python
class Wallet:
    def __init__(self, user: User) -> None:
        self.user: User = user
        self.balance: float = 0.0
        self.transaction_history: List['Transaction'] = []

    def add_funds(self, amount: float) -> bool:
        if amount <= 0:
            return False
        self.balance += amount
        self.transaction_history.append(Transaction("Deposit", amount, self.balance))
        return True

    def withdraw_funds(self, amount: float) -> bool:
        if amount > self.balance:
            return False
        self.balance -= amount
        self.transaction_history.append(Transaction("Withdrawal", amount, self.balance))
        return True
```
### Transaction Class
Represents a financial transaction.
```python
class Transaction:
    def __init__(self, type_: str, amount: float, post_transaction_balance: float) -> None:
        self.type: str = type_
        self.amount: float = amount
        self.date: datetime = datetime.now()
        self.post_transaction_balance: float = post_transaction_balance

```
### DigitalWalletSystem Class
Manages the digital wallet system operations.
```python
class DigitalWalletSystem:
    def __init__(self) -> None:
        self.users: List[User] = []

    def add_user(self, user: User) -> None:
        self.users.append(user)

    def transfer_funds(self, sender_user_id: str, receiver_user_id: str, amount: float) -> bool:
        sender: Optional[User] = next((user for user in self.users if user.user_id == sender_user_id), None)
        receiver: Optional[User] = next((user for user in self.users if user.user_id == receiver_user_id), None)

        if sender and receiver:
            sender_wallet: Wallet = sender.wallet
            receiver_wallet: Wallet = receiver.wallet

            if sender_wallet.withdraw_funds(amount):
                receiver_wallet.add_funds(amount)
                return True
        return False
```