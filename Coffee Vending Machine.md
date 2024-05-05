# Designing a Coffee Vending Machine

This article explores the object-oriented design and implementation of a Coffee Vending Machine using Python3, a project that demonstrates managing coffee selections, inventory, and customer interactions.

## System Requirements

The Coffee Vending Machine is designed to:

1. **Offer Multiple Coffee Types:** Provide a selection of coffee types such as Espresso, Latte, and Cappuccino.
2. **Manage Inventory:** Track ingredients like coffee, milk, sugar, and water.
3. **Process Orders:** Handle customer coffee selections and dispense the correct beverage.
4. **Payment Handling:** Manage simplified payment transactions for the coffee.

## Core Use Cases

1. **Selecting a Coffee Type:** Customers choose their preferred type of coffee.
2. **Checking Ingredient Availability:** Ensure sufficient ingredients are available for the selected coffee.
3. **Dispensing Coffee:** Prepare and dispense the chosen coffee.
4. **Processing Payment:** Handle the transaction for the coffee purchase.

## Key Classes:
- `CoffeeVendingMachine`: Central class managing the vending machine.
- `Coffee`: Enum representing different types of coffee.
- `Ingredient`: Manages individual ingredients.
- `Inventory`: Tracks all available ingredients.
- `PaymentProcessor`: Handles payment transactions.

## Python3 Implementation

### Coffee Enum

Represents different coffee types available.

```python
from enum import Enum

class Coffee(Enum):
    ESPRESSO = 1
    LATTE = 2
    CAPPUCCINO = 3

```
### Ingredient Class
```python
class Ingredient:
    def __init__(self, name: str, quantity: int):
        self.name: str = name
        self.quantity: int = quantity

    def use_ingredient(self, amount: int) -> bool:
        if self.quantity >= amount:
            self.quantity -= amount
            return True
        return False

    def get_quantity(self) -> int:
        return self.quantity

```
### Inventory Class
```python
from typing import Dict

class Inventory:
    def __init__(self):
        self.ingredients: Dict[str, Ingredient] = {}
        # Initialize with default ingredients
        self.add_ingredient('water', 1000)
        self.add_ingredient('coffee', 500)
        self.add_ingredient('milk', 500)

    def check_ingredient_availability(self, ingredient_name: str, amount: int) -> bool:
        ingredient = self.ingredients.get(ingredient_name)
        return ingredient is not None and ingredient.get_quantity() >= amount

    def use_ingredient(self, ingredient_name: str, amount: int):
        if self.check_ingredient_availability(ingredient_name, amount):
            self.ingredients[ingredient_name].use_ingredient(amount)

    def add_ingredient(self, name: str, quantity: int):
        self.ingredients[name] = Ingredient(name, quantity)

```
### CoffeeVendingMachine Class
```python
class CoffeeVendingMachine:
    def __init__(self):
        self.inventory = Inventory()
        self.payment_processor = PaymentProcessor()

    def make_coffee(self, coffee_type: Coffee) -> Coffee:
        # Example ingredient checks (simplified)
        if self.inventory.check_ingredient_availability('coffee', 10) and \
           self.inventory.check_ingredient_availability('water', 50):
            if self.payment_processor.process_payment(2.50):  # Assuming each coffee costs $2.50
                self.inventory.use_ingredient('coffee', 10)
                self.inventory.use_ingredient('water', 50)
                print(f"Dispensing {coffee_type.name}")
                return coffee_type
        print("Failed to make coffee.")
        return None

```
### PaymentProcessor Class
```python
class PaymentProcessor:
    def process_payment(self, amount: float) -> bool:
        # Simulate payment processing
        return True  # Assume payment is successful

```

### Example Usage
``` python
def main():
    machine = CoffeeVendingMachine()
    coffee = machine.make_coffee(Coffee.ESPRESSO)
    if coffee:
        print(f"Enjoy your {coffee.name.lower()}!")
    else:
        print("Please try again!")

if __name__ == "__main__":
    main()

```

### More information
- **Design Pattern**: Facade Pattern
- **Reason**:
  - **Simplification of Interfaces**: The `CoffeeVendingMachine` class acts as a facade, simplifying the complex subsystems of inventory management, coffee preparation, and payment processing for the client.
  - **Centralized Control**: This pattern allows the machine to handle various operations like selecting coffee, managing inventory, and processing payments through a single interface, making the system easier to use and maintain.
  - **Decoupling of System Components**: The pattern helps in separating the interface exposed to the customer from the internal complexities involved in managing inventory and processing payments, thereby decoupling the system components for better modularity and easier troubleshooting or upgrades.
