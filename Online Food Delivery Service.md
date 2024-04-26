# Designing an Online Food Delivery Service Like Swiggy

In this article, we explore the object-oriented design and implementation of an Online Food Delivery Service similar to Swiggy using Python3. 

This system encompasses functionalities essential for online food ordering and delivery.

## System Requirements

The Online Food Delivery System should:

1. **Restaurant Management**: Manage restaurant profiles, menus, and availability.
2. **User Account Management**: Handle customer and delivery agent profiles.
3. **Order Processing**: Enable customers to place orders and track their status.
4. **Delivery Management**: Assign orders to delivery agents and manage the delivery process.
5. **Payment Processing**: Handle various modes of payment.

## Core Use Cases

1. **Adding and Managing Restaurants**
2. **Registering and Managing User and Delivery Agent Accounts**
3. **Placing and Tracking Orders**
4. **Assigning and Managing Deliveries**
5. **Processing Payments**

## UML/Class Diagrams

Key Classes:

- `OnlineFoodDeliveryService`: Manages the overall system.
- `User`: Represents a customer.
- `Restaurant`: Manages a restaurant's profile and menu.
- `Order`: Represents a food order.
- `DeliveryAgent`: Manages a delivery agent's information.
- `Payment`: Handles payment transactions.

## Python3 Implementation

### Restaurant Class

Manages restaurant information and menus.

```python
from typing import Dict

class Restaurant:
    def __init__(self, restaurant_id: str, name: str):
        self.restaurant_id: str = restaurant_id
        self.name: str = name
        self.menu: Dict[str, float] = {}

    def add_item_to_menu(self, item_name: str, price: float):
        self.menu[item_name] = price

```
### User Class
Manages customer account information.
```python
class User:
    def __init__(self, user_id: str, name: str, address: str):
        self.user_id: str = user_id
        self.name: str = name
        self.address: str = address

```
### Order Class
Represents a customer's food order.
```python
from datetime import datetime
from enum import Enum
from typing import Dict

class OrderStatus(Enum):
    PLACED = 'PLACED'
    PREPARING = 'PREPARING'
    OUT_FOR_DELIVERY = 'OUT_FOR_DELIVERY'
    DELIVERED = 'DELIVERED'
    CANCELLED = 'CANCELLED'

class Order:
    def __init__(self, order_id: str, user: User, restaurant: Restaurant, ordered_items: Dict[str, int], order_time: datetime):
        self.order_id: str = order_id
        self.user: User = user
        self.restaurant: Restaurant = restaurant
        self.ordered_items: Dict[str, int] = ordered_items
        self.order_time: datetime = order_time
        self.status: OrderStatus = OrderStatus.PLACED

    def update_status(self, new_status: OrderStatus):
        self.status = new_status

```
### DeliveryAgent Class
Manages delivery agent information.
```python
class DeliveryAgent:
    def __init__(self, agent_id: str, name: str):
        self.agent_id: str = agent_id
        self.name: str = name
        self.is_available: bool = True

    def set_availability(self, is_available: bool):
        self.is_available = is_available

```
### Payment Class
Handles payment transactions.
```python
class PaymentStatus(Enum):
    PENDING = 'PENDING'
    COMPLETED = 'COMPLETED'
    FAILED = 'FAILED'

class Payment:
    def __init__(self, payment_id: str, amount: float):
        self.payment_id: str = payment_id
        self.amount: float = amount
        self.status: PaymentStatus = PaymentStatus.PENDING

    def complete_payment(self):
        self.status = PaymentStatus.COMPLETED

```
### OnlineFoodDeliveryService Class
Manages the online food delivery service operations.
```python
from typing import List

class OnlineFoodDeliveryService:
    def __init__(self):
        self.users: List[User] = []
        self.restaurants: List[Restaurant] = []
        self.orders: List[Order] = []
        self.delivery_agents: List[DeliveryAgent] = []

    def add_user(self, user: User):
        self.users.append(user)

    def add_restaurant(self, restaurant: Restaurant):
        self.restaurants.append(restaurant)

    def add_order(self, order: Order):
        self.orders.append(order)

    def assign_delivery_agent(self, order_id: str, agent_id: str):
        order = self.find_order_by_id(order_id)
        agent = self.find_agent_by_id(agent_id)
        if order and agent and agent.is_available:
            agent.set_availability(False)
            order.update_status(OrderStatus.OUT_FOR_DELIVERY)

    def find_order_by_id(self, order_id: str) -> Order:
        for order in self.orders:
            if order.order_id == order_id:
                return order
        return None

    def find_agent_by_id(self, agent_id: str) -> DeliveryAgent:
        for agent in self.delivery_agents:
            if agent.agent_id == agent_id:
                return agent
        return None

```

### Example Usage:
``` python
def main():
    service = OnlineFoodDeliveryService()

    # Adding users and restaurants
    user = User("U1", "John Doe", "123 Elm Street")
    restaurant = Restaurant("R1", "Good Eats")
    restaurant.add_item_to_menu("Pizza", 9.99)
    service.add_user(user)
    service.add_restaurant(restaurant)

    # Creating and adding an order
    order = Order("O1", user, restaurant, {"Pizza": 2}, datetime.now())
    service.add_order(order)

    # Delivery Agent
    agent = DeliveryAgent("D1

```