# Designing a Concert Ticket Booking System

In this article, we delve into the object-oriented design and implementation of a Concert Ticket Booking System using Python3. 

This system facilitates booking tickets for concerts and managing events.

## System Requirements

The Concert Ticket Booking System should:

1. **Event Management**: Manage concert details including dates, venues, and artists.
2. **User Account Management**: Handle user registrations and profiles.
3. **Ticket Booking Process**: Enable users to book tickets and select seats.
4. **Payment Processing**: Handle ticket payments and issue receipts.
5. **Ticket Cancellation and Refund**: Manage cancellations and refunds.

## Core Use Cases

1. **Creating and Managing Concert Events**
2. **Registering and Managing User Accounts**
3. **Booking and Canceling Tickets**
4. **Processing Payments and Issuing Tickets**
5. **Handling Refunds**

## UML/Class Diagrams

Key Classes:

- `ConcertTicketBookingSystem`: Manages the system.
- `User`: Represents a customer.
- `Concert`: Represents a concert event.
- `Ticket`: Manages ticket details.
- `Payment`: Handles payment transactions.

## Python3 Implementation

### Concert Class

Represents a concert event.

```python
from datetime import datetime
from typing import Optional

class Concert:
    def __init__(self, concert_id: str, title: str, date: datetime, venue: str, total_seats: int):
        self.concert_id: str = concert_id
        self.title: str = title
        self.date: datetime = date
        self.venue: str = venue
        self.total_seats: int = total_seats
        self.available_seats: int = total_seats

    def book_seats(self, number_of_seats: int) -> bool:
        if number_of_seats <= self.available_seats:
            self.available_seats -= number_of_seats
            return True
        return False

```
### User Class
Manages user account information.
```python
class User:
    def __init__(self, user_id: str, name: str, email: str):
        self.user_id: str = user_id
        self.name: str = name
        self.email: str = email

```
### Ticket Class
Represents a concert ticket.
```python
class Ticket:
    def __init__(self, ticket_id: str, concert: Concert, user: User, number_of_seats: int, price_per_seat: float):
        self.ticket_id: str = ticket_id
        self.concert: Concert = concert
        self.user: User = user
        self.number_of_seats: int = number_of_seats
        self.total_price: float = price_per_seat * number_of_seats

```
### Payment Class
Handles payment transactions.
```python
from enum import Enum

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
### ConcertTicketBookingSystem Class
Manages the concert ticket booking system operations.

```python
from typing import List, Optional

class ConcertTicketBookingSystem:
    def __init__(self):
        self.users: List[User] = []
        self.concerts: List[Concert] = []
        self.tickets: List[Ticket] = []

    def add_user(self, user: User):
        self.users.append(user)

    def add_concert(self, concert: Concert):
        self.concerts.append(concert)

    def book_ticket(self, user_id: str, concert_id: str, number_of_seats: int) -> Optional[Ticket]:
        user = self.find_user_by_id(user_id)
        concert = self.find_concert_by_id(concert_id)
        price_per_seat = 20.0  # Example fixed price

        if user and concert and concert.book_seats(number_of_seats):
            ticket = Ticket(self.generate_ticket_id(), concert, user, number_of_seats, price_per_seat)
            self.tickets.append(ticket)
            return ticket
        return None

    def find_user_by_id(self, user_id: str) -> Optional[User]:
        for user in self.users:
            if user.user_id == user_id:
                return user
        return None

    def find_concert_by_id(self, concert_id: str) -> Optional[Concert]:
        for concert in self.concerts:
            if concert.concert_id == concert_id:
                return concert
        return None

    def generate_ticket_id(self) -> str:
        import time
        return f"TICKET_{int(time.time())}"

```

### Example Usage
``` python
def main():
    system = ConcertTicketBookingSystem()
    system.add_concert(Concert("CON1", "The Big Show", datetime(2023, 5, 17), "Grand Arena", 500))
    system.add_user(User("U1", "Alice Johnson", "alice@example.com"))

    ticket = system.book_ticket("U1", "CON1", 3)
    if ticket:
        print(f"Ticket ID {ticket.ticket_id} booked for {ticket.number_of_seats} seats.")
    else:
        print("Failed to book ticket.")

if __name__ == "__main__":
    main()

```