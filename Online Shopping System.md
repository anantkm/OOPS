# Designing an Online Shopping System Like Amazon

In this article, we're going to design and implement an Online Shopping System resembling Amazon, using Python3. 

This system will cover product listings, user accounts, shopping carts, and order processing.

## System Requirements

The Online Shopping System should:

1. **Product Management:** Manage a catalog of products.
2. **User Account Management:** Handle user registrations and logins.
3. **Shopping Cart Management:** Allow users to add and remove products from their shopping cart.
4. **Order Processing:** Process user orders and maintain order history.

## Core Use Cases

1. **Browsing Products**
2. **Managing User Accounts**
3. **Handling Shopping Carts**
4. **Processing Orders**

## Key Classes:
- `OnlineShoppingSystem`: Manages the overall system.
- `Product`: Represents a product in the catalog.
- `User`: Represents a user of the system.
- `ShoppingCart`: Manages the shopping cart.
- `Order`: Represents a user's order.

## Python3 Implementation

### Product Class

Represents a product in the store.

```python
class Product:
    def __init__(self, product_id, name, price, description):
        self._product_id = product_id
        self._name = name
        self._price = price
        self._description = description

    # Getters and setters...
    @property
    def product_id(self):
        return self._product_id

    @property
    def name(self):
        return self._name

    @property
    def price(self):
        return self._price

    @property
    def description(self):
        return self._description

```
### User Class
Handles user account information.
```python
class User:
    def __init__(self, user_id, name, email):
        self._user_id = user_id
        self._name = name
        self._email = email
        self._cart = ShoppingCart(self)
        self._order_history = []

    def add_product_to_cart(self, product, quantity):
        self._cart.add_product(product, quantity)

    def checkout(self):
        order = self._cart.checkout()
        self._order_history.append(order)
        return order

    # Getters and setters...

```
### ShoppingCart Class
Manages shopping cart operations.
```python
class ShoppingCart:
    def __init__(self, owner):
        self._owner = owner
        self._items = {}

    def add_product(self, product, quantity):
        if product in self._items:
            self._items[product] += quantity
        else:
            self._items[product] = quantity

    def checkout(self):
        new_order = Order(self._owner, dict(self._items))
        self._items.clear()
        return new_order

```
### Order Class
Represents a user's order.
```python
from enum import Enum

class OrderStatus(Enum):
    PROCESSING = 1
    SHIPPED = 2
    DELIVERED = 3

class Order:
    def __init__(self, user, ordered_items):
        self._user = user
        self._ordered_items = ordered_items
        self._status = OrderStatus.PROCESSING

    def update_status(self, new_status):
        self._status = new_status

```
### OnlineShoppingSystem Class
Main class for managing the online shopping platform.
```python
class OnlineShoppingSystem:
    def __init__(self):
        self._catalog = []
        self._users = []

    def add_product_to_catalog(self, product):
        self._catalog.append(product)

    def add_user(self, user):
        self._users.append(user)

```

### Running the Program
Sample to run/demo the system
```python
def main():
    # Create an instance of the online shopping system
    shopping_system = OnlineShoppingSystem()

    # Add products to the catalog
    product1 = Product("P001", "Laptop", 1200.00, "High-performance laptop suitable for gaming and professional work.")
    product2 = Product("P002", "Smartphone", 700.00, "Latest model smartphone with advanced features.")
    shopping_system.add_product_to_catalog(product1)
    shopping_system.add_product_to_catalog(product2)

    # Add users to the system
    user1 = User("U001", "John Doe", "johndoe@example.com")
    user2 = User("U002", "Jane Smith", "janesmith@example.com")
    shopping_system.add_user(user1)
    shopping_system.add_user(user2)

    # Simulate adding products to the cart
    user1.add_product_to_cart(product1, 1)
    user1.add_product_to_cart(product2, 2)
    user2.add_product_to_cart(product2, 1)

    # Simulate checkout
    order1 = user1.checkout()
    order2 = user2.checkout()

    # Print out details of the orders
    print(f"Order 1: User {order1._user._name}, Status: {order1._status.name}, Items: {[(product._name, quantity) for product, quantity in order1._ordered_items.items()]}")
    print(f"Order 2: User {order2._user._name}, Status: {order2._status.name}, Items: {[(product._name, quantity) for product, quantity in order2._ordered_items.items()]}")

if __name__ == "__main__":
    main()

```