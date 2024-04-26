# Designing a Ride-Sharing Service Like Uber

This article explores the object-oriented design and implementation of a Ride-Sharing Service similar to Uber using Python3. 

We focus on the various aspects of ride-sharing, including user and driver management, ride booking, and fare calculation.

## System Requirements

The Ride-Sharing Service should:

1. **User and Driver Account Management**: Handle the registration and profiles of users and drivers.
2. **Ride Booking**: Enable users to book rides and drivers to accept them.
3. **Fare Calculation**: Compute fares based on distance and other factors.
4. **Ride History Management**: Keep a record of past rides for users and drivers.

## Core Use Cases

1. **Registering and Managing User and Driver Accounts**
2. **Booking and Managing Rides**
3. **Calculating and Processing Fares**
4. **Maintaining Ride History**

## Key Classes:
- `RideSharingService`: Manages the overall system.
- `User`: Represents a service user.
- `Driver`: Represents a driver.
- `Ride`: Manages ride details.
- `FareCalculator`: Calculates ride fares.

## Python3 Implementation

### User Class

Manages user account information.

```python
class User:
    def __init__(self, user_id: str, name: str, phone: str):
        self.user_id = user_id
        self.name = name
        self.phone = phone

```
### Driver Class
Represents a driver in the system.
```python
class Driver:
    def __init__(self, driver_id: str, name: str, vehicle_details: str):
        self.driver_id = driver_id
        self.name = name
        self.vehicle_details = vehicle_details

```
### Ride Class
Manages ride details.
```python
from datetime import datetime

class Ride:
    def __init__(self, ride_id: str, user: User, driver: Driver, ride_date: datetime):
        self.ride_id = ride_id
        self.user = user
        self.driver = driver
        self.ride_date = ride_date
        self.fare = 0.0

    def set_fare(self, fare: float):
        self.fare = fare

```
### FareCalculator Class
Calculates the fare of a ride.
```python
class FareCalculator:
    BASE_FARE = 2.50  # Base fare for a ride
    COST_PER_MILE = 1.50  # Cost per mile
    COST_PER_MINUTE = 0.25  # Cost per minute

    def calculate_fare(self, distance_in_miles: float, duration_in_minutes: float) -> float:
        distance_charge = distance_in_miles * self.COST_PER_MILE
        time_charge = duration_in_minutes * self.COST_PER_MINUTE
        return self.BASE_FARE + distance_charge + time_charge

```
### RideSharingService Class
Manages the ride-sharing service operations.
```python
from typing import List
import time

class RideSharingService:
    def __init__(self):
        self.users: List[User] = []
        self.drivers: List[Driver] = []
        self.rides: List[Ride] = []

    def book_ride(self, user: User, driver: Driver, ride_date: datetime, distance: float, duration: float) -> Ride:
        new_ride = Ride(self.generate_ride_id(), user, driver, ride_date)
        fare_calculator = FareCalculator()
        fare = fare_calculator.calculate_fare(distance, duration)
        new_ride.set_fare(fare)
        self.rides.append(new_ride)
        return new_ride

    def generate_ride_id(self) -> str:
        return f"RIDE_{int(time.time() * 1000)}"

```

### Example Usage
``` python
def main():
    service = RideSharingService()

    # Add users and drivers
    user = User("U001", "Alice", "1234567890")
    driver = Driver("D001", "Bob", "Toyota Camry 2015")

    # Book a ride
    ride_date = datetime.now()
    new_ride = service.book_ride(user, driver, ride_date, distance=10.0, duration=15.0)

    # Output ride details
    print(f"Ride booked with ID: {new_ride.ride_id}")
    print(f"Driver: {new_ride.driver.name}, Vehicle: {new_ride.driver.vehicle_details}")
    print(f"User: {new_ride.user.name}, Fare: ${new_ride.fare:.2f}")

if __name__ == "__main__":
    main()

```