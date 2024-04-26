# Designing an ATM System

This article covers the design and implementation of an Automated Teller Machine (ATM) system using object-oriented programming in Python3.

## System Requirements
Our ATM system is designed to handle:
1. **Authentication:** Verifying user identity using a card number and PIN.
2. **Account Management:** Managing different account types.
3. **Balance Inquiry:** Checking the account balance.
4. **Cash Withdrawal and Deposit:** Handling money transactions.
5. **Transaction History:** Providing a record of past transactions.

## Core Use Cases
- **User Authentication**
- **Performing Transactions:** Withdrawals, deposits, balance inquiries.
- **Printing Transaction Receipts** (optional for this implementation)

## Key Classes:
- `ATM`: The main class to interact with users.
- `Account`: To manage account details.
- `Bank`: Represents the bank system that verifies transactions.
- `CardReader`: To read user's card data.
- `CashDispenser`: To manage cash dispensing.

## Python3 Implementation
### Account Class
```python
from typing import Dict

class Account:
    def __init__(self, account_number: str, initial_balance: float):
        self.account_number: str = account_number
        self.balance: float = initial_balance

    def withdraw(self, amount: float) -> bool:
        if amount > self.balance:
            return False  # Insufficient balance
        self.balance -= amount
        return True

    def deposit(self, amount: float):
        self.balance += amount

    def get_balance(self) -> float:
        return self.balance

```
### ATM Class
```python
class ATM:
    def __init__(self, bank: 'Bank'):
        self.current_account: Account = None
        self.bank: Bank = bank

    def authenticate_user(self, account_number: str, pin: str) -> bool:
        self.current_account = self.bank.verify_account(account_number, pin)
        return self.current_account is not None

    def check_balance(self) -> float:
        if self.current_account:
            return self.current_account.get_balance()
        return 0.0  # Default return when no account is active

    def withdraw(self, amount: float) -> bool:
        if self.current_account:
            return self.current_account.withdraw(amount)
        return False

    def deposit(self, amount: float):
        if self.current_account:
            self.current_account.deposit(amount)

```

### Bank Class
```python
class Bank:
    def __init__(self):
        self.accounts: Dict[str, Account] = {}  # Simulated database of accounts

    def add_account(self, account_number: str, account: Account):
        self.accounts[account_number] = account

    def verify_account(self, account_number: str, pin: str) -> Account:
        # Here we assume all pins are '1234' for simplicity, replace with actual verification
        account = self.accounts.get(account_number)
        if account and pin == '1234':  # Always ensure the PIN is correct in a real system
            return account
        return None

```

### Running the Program
Sample to run/demo the system
```python
def main():
    # Initialize the bank and add some accounts
    bank = Bank()
    bank.add_account("123456", Account("123456", 1000.0))
    bank.add_account("789101", Account("789101", 500.0))

    # Initialize ATM with the bank
    atm = ATM(bank)

    # Authenticate user
    if atm.authenticate_user("123456", "1234"):
        print("User authenticated successfully.")
        print(f"Current balance: ${atm.check_balance():.2f}")
        
        # Deposit money
        atm.deposit(200.0)
        print(f"Balance after deposit: ${atm.check_balance():.2f}")
        
        # Withdraw money
        if atm.withdraw(50.0):
            print(f"Balance after withdrawal: ${atm.check_balance():.2f}")
        else:
            print("Failed to withdraw money due to insufficient funds.")
    else:
        print("Authentication failed.")

if __name__ == "__main__":
    main()

```