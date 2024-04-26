# Designing a Parking Lot System

This article explores the low-level design of a parking lot system, a common problem in object-oriented design. 

We'll cover essential components and functionalities, breaking down requirements, use cases, and providing a Python3 implementation.

## Understanding the Requirements
A parking lot system manages vehicles parking in and out, with different parking spot sizes and rates. Key requirements include:
- **Parking Space Management:** Track the availability of parking spaces.
- **Vehicle Management:** Handle the entry and exit of vehicles.
- **Fee Calculation:** Calculate parking fees based on parking duration.
- **Parking Lot Capacity:** Support different types of vehicles with designated spots (e.g., compact, large, handicapped).

## Core Use Cases
1. **Parking a Vehicle:** Assigning spots to vehicles and recording entry time.
2. **Unparking a Vehicle:** Removing a vehicle and calculating the fee.
3. **Spot Availability Check:** Checking for available spots for specific vehicles.
4. **Handling Different Vehicle Types**

## Key Classes:
- `ParkingLot`: Manages the entire parking lot, including floors.
- `ParkingFloor`: Represents individual floors with parking spots.
- `ParkingSpot`: Represents an individual parking spot.
- `Vehicle`: Abstract class for various vehicle types (e.g., `Car`).
- `ParkingTicket`: Represents a parking ticket issued to a vehicle.
- `FeeCalculator`: Calculates parking fees.

## Python3 Implementation
Here's a simplified version of Python3 code:

### Vehicle Class
```python
from enum import Enum

class VehicleType(Enum):
    COMPACT = 'COMPACT'
    LARGE = 'LARGE'
    HANDICAPPED = 'HANDICAPPED'

class Vehicle:
    def __init__(self, license_plate: str, vehicle_type: VehicleType):
        self.license_plate: str = license_plate
        self.vehicle_type: VehicleType = vehicle_type

class Car(Vehicle):
    def __init__(self, license_plate: str):
        super().__init__(license_plate, VehicleType.COMPACT)

```
### ParkingSpot Class
```python
class ParkingSpot:
    def __init__(self, spot_id: str, spot_type: VehicleType):
        self.spot_id: str = spot_id
        self.spot_type: VehicleType = spot_type
        self.is_occupied: bool = False

    def occupy_spot(self):
        self.is_occupied = True

    def free_spot(self):
        self.is_occupied = False

class CarSpot(ParkingSpot):
    def __init__(self, spot_id: str):
        super().__init__(spot_id, VehicleType.COMPACT)

```
### ParkingTicket Class
```python
from datetime import datetime

class ParkingTicket:
    def __init__(self, ticket_id: str):
        self.ticket_id: str = ticket_id
        self.issued_at: datetime = datetime.now()
        self.paid_at: datetime = None
        self.fee: float = 0.0

    def mark_paid(self, fee: float):
        self.paid_at = datetime.now()
        self.fee = fee

```
### FeeCalculator Class
```python
class FeeCalculator:
    def calculate_fee(self, issued_at: datetime, paid_at: datetime) -> float:
        duration = paid_at - issued_at
        hours = duration.total_seconds() / 3600
        return 2.0 * hours  # Assume $2 per hour

```
### ParkingLot Class
```python
from typing import List, Optional

class ParkingLot:
    def __init__(self):
        self.parking_spots: List[ParkingSpot] = []
        self.issued_tickets: List[ParkingTicket] = []

    def find_available_spot(self, vehicle_type: VehicleType) -> Optional[ParkingSpot]:
        for spot in self.parking_spots:
            if spot.spot_type == vehicle_type and not spot.is_occupied:
                return spot
        return None

    def issue_ticket(self, vehicle: Vehicle) -> Optional[ParkingTicket]:
        spot = self.find_available_spot(vehicle.vehicle_type)
        if spot:
            spot.occupy_spot()
            ticket = ParkingTicket(self.generate_ticket_id())
            self.issued_tickets.append(ticket)
            return ticket
        return None

    def process_payment(self, ticket: ParkingTicket, fee: float):
        ticket.mark_paid(fee)

    def generate_ticket_id(self) -> str:
        return f"TICKET_{int(time.time() * 1000)}"

```
### ParkingFloor
```python
class ParkingFloor:
    def __init__(self):
        self.spots: List[ParkingSpot] = []

    def add_spot(self, spot: ParkingSpot):
        self.spots.append(spot)

    def find_free_spot(self, vehicle_type: VehicleType) -> Optional[ParkingSpot]:
        return next((spot for spot in self.spots if spot.spot_type == vehicle_type and not spot.is_occupied), None)

    def occupy_spot(self, spot_id: str) -> bool:
        spot = next((spot for spot in self.spots if spot.spot_id == spot_id and not spot.is_occupied), None)
        if spot:
            spot.occupy_spot()
            return True
        return False

    def free_spot(self, spot_id: str) -> bool:
        spot = next((spot for spot in self.spots if spot.spot_id == spot_id and spot.is_occupied), None)
        if spot:
            spot.free_spot()
            return True
        return False

```

### Example Usage
``` python
def main():
    # Initialize parking lot and floor
    parking_lot = ParkingLot()
    parking_floor = ParkingFloor()

    # Add spots to the floor
    parking_floor.add_spot(CarSpot("C1"))
    parking_floor.add_spot(ParkingSpot("H1", VehicleType.HANDICAPPED))
    parking_floor.add_spot(ParkingSpot("L1", VehicleType.LARGE))

    # Add this floor's spots to the parking lot (simulate a single floor lot)
    parking_lot.parking_spots.extend(parking_floor.spots)

    # Create a vehicle
    car = Car("ABC-1234")

    # Attempt to park the car
    ticket = parking_lot.issue_ticket(car)
    if ticket:
        print(f"Ticket issued with ID {ticket.ticket_id} for vehicle with license plate {car.license_plate}.")
    else:
        print("Failed to issue ticket: no available spots.")

    # Simulate duration and payment
    if ticket:
        fee_calculator = FeeCalculator()
        # Simulating time passing before payment
        from datetime import timedelta
        ticket.paid_at = ticket.issued_at + timedelta(hours=2)
        fee = fee_calculator.calculate_fee(ticket.issued_at, ticket.paid_at)
        parking_lot.process_payment(ticket, fee)
        print(f"Payment processed for ${fee:.2f}, ticket ID: {ticket.ticket_id}")

        # Free the spot after payment
        parking_floor.free_spot("C1")
        print(f"Spot 'C1' is now {'occupied' if parking_floor.spots[0].is_occupied else 'free'}.")

if __name__ == "__main__":
    main()

```
