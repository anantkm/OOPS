# Designing a Hotel Management System

In this article, we will explore the design and implementation of a Hotel Management System (HMS) using object-oriented principles in Python3. 

The HMS is designed to streamline hotel operations including room booking, customer management, and service provision.

## System Requirements

The HMS will facilitate:

1. **Room Booking Management:** Manage bookings for various types of rooms.
2. **Customer Management:** Handle customer information and booking history.
3. **Room Service Management:** Manage orders for food and other services.
4. **Billing:** Generate bills for customers based on their usage of services.

## Core Use Cases

1. **Booking a Room:** Customers can book different types of rooms.
2. **Managing Customer Profiles:** Storing and retrieving customer details.
3. **Ordering Room Services:** Placing orders for room-related services.
4. **Generating Bills:** Calculating and producing bills for customers.

## Key Classes:
- `Hotel`: Manages the overall hotel operations.
- `Room`: Represents individual rooms in the hotel.
- `Customer`: Manages information about customers.
- `Booking`: Represents a room booking by a customer.
- `Service`: Represents additional services provided to customers.

## Python3 Implementation

### Room Class

This class represents a hotel room.

```python
class Room:
    def __init__(self, room_number: str, room_type: str):
        self.room_number: str = room_number
        self.room_type: str = room_type
        self.is_booked: bool = False

    def book_room(self):
        self.is_booked = True

    def vacate_room(self):
        self.is_booked = False

```

### Customer Class
```python
class Customer:
    def __init__(self, customer_id: str, name: str, phone: str):
        self.customer_id: str = customer_id
        self.name: str = name
        self.phone: str = phone

```
### Booking Class
```python
from datetime import date

class Booking:
    def __init__(self, room: Room, customer: Customer, check_in_date: date, check_out_date: date):
        self.room: Room = room
        self.customer: Customer = customer
        self.check_in_date: date = check_in_date
        self.check_out_date: date = check_out_date
        room.book_room()

    def complete_booking(self):
        self.room.vacate_room()

```
### Hotel Class
```python
from typing import List, Optional

class Hotel:
    def __init__(self, hotel_name: str):
        self.hotel_name: str = hotel_name
        self.rooms: List[Room] = []
        self.bookings: List[Booking] = []

    def add_room(self, room: Room):
        self.rooms.append(room)

    def book_room(self, room_number: str, customer: Customer, check_in: date, check_out: date) -> Optional[Booking]:
        room = next((r for r in self.rooms if r.room_number == room_number and not r.is_booked), None)
        if room:
            booking = Booking(room, customer, check_in, check_out)
            self.bookings.append(booking)
            return booking
        return None

```

### Example Usage
``` python
def main():
    # Initialize hotel and add rooms
    hotel = Hotel("Grand Plaza")
    hotel.add_room(Room("101", "Single"))
    hotel.add_room(Room("102", "Double"))

    # New customer
    customer = Customer("C001", "John Doe", "555-0123")

    # Booking a room
    check_in_date = date(2024, 1, 1)
    check_out_date = date(2024, 1, 5)
    booking = hotel.book_room("101", customer, check_in_date, check_out_date)
    if booking:
        print(f"Booking successful for {booking.customer.name}")
        print(f"Room {booking.room.room_number} is now booked from {booking.check_in_date} to {booking.check_out_date}")
        booking.complete_booking()
        print(f"Room {booking.room.room_number} status after checkout: {'Booked' if booking.room.is_booked else 'Available'}")
    else:
        print("Booking failed: Room not available or does not exist.")

if __name__ == "__main__":
    main()

```