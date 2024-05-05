# Designing a Car Rental System

In this article, we will explore the object-oriented design and implementation of a Car Rental System (CRS) using Python3. 

This system will handle the renting of cars to customers, managing car inventory, and tracking rentals.

## System Requirements

The CRS should support:

1. **Car Inventory Management:** Keep track of available cars for rent.
2. **Rental Process Management:** Handle the process of renting a car to a customer.
3. **Rental Tracking:** Track ongoing and past rentals.
4. **Customer Management:** Manage customer information.

## Core Use Cases

1. **Renting a Car:** Customers can rent available cars.
2. **Returning a Car:** Handle the return process of rented cars.
3. **Tracking Rentals:** View current and past rental records.
4. **Managing Car Inventory:** Add, update, and remove cars from the inventory.

## Key Classes:
- `CarRentalSystem`: Manages the overall operations of the car rental system.
- `Car`: Represents a car in the system.
- `Rental`: Manages details about a car rental.
- `Customer`: Stores information about customers.

## Python3 Implementation

### Car Class

```python
class Car:
    def __init__(self, license_plate: str, make: str):
        self.license_plate: str = license_plate
        self.make: str = make
        self.is_available: bool = True

    def rent_out(self):
        self.is_available = False

    def return_car(self):
        self.is_available = True

```
### Customer Class
```python
class Customer:
    def __init__(self, customer_id: str, name: str):
        self.customer_id: str = customer_id
        self.name: str = name

```
### Rental Class
```python
from datetime import date

class Rental:
    def __init__(self, car: Car, customer: Customer, rental_date: date):
        self.car: Car = car
        self.customer: Customer = customer
        self.rental_date: date = rental_date
        self.return_date: date = None
        self.car.rent_out()

    def complete_rental(self, return_date: date):
        self.return_date = return_date
        self.car.return_car()

```
### CarRentalSystem Class
```python
from typing import List, Optional

class CarRentalSystem:
    def __init__(self):
        self.cars: List[Car] = []
        self.rentals: List[Rental] = []

    def add_car(self, car: Car):
        self.cars.append(car)

    def rent_car(self, license_plate: str, customer: Customer, rental_date: date) -> Optional[Rental]:
        car = self.find_available_car(license_plate)
        if car:
            rental = Rental(car, customer, rental_date)
            self.rentals.append(rental)
            return rental
        return None

    def find_available_car(self, license_plate: str) -> Optional[Car]:
        for car in self.cars:
            if car.license_plate == license_plate and car.is_available:
                return car
        return None

```

### Running the Program
Sample to run/demo the system
```python
from datetime import date

def main():
    system = CarRentalSystem()
    system.add_car(Car("123ABC", "Toyota Camry"))
    system.add_car(Car("456DEF", "Honda Accord"))

    customer = Customer("C001", "John Doe")
    today = date.today()

    rental = system.rent_car("123ABC", customer, today)
    if rental:
        print(f"Car rented to {rental.customer.name} on {rental.rental_date}")
    else:
        print("Car not available")

    # Return the car
    rental.complete_rental(today)
    print(f"Car returned on {rental.return_date}")

if __name__ == "__main__":
    main()

```
### More information
- **Design Pattern**: Composite Pattern
- **Benefits**:
  - **Uniformity**: Treats single objects (like a specific car) and collections (like all cars or rentals) with the same management approach.
  - **Simplicity**: Simplifies client interactions with the system by handling different tasks (renting, returning) uniformly.
  - **Scalability**: Easily adaptable and expandable for adding new features or managing more complex operations as the system grows.
