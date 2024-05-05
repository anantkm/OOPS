# Designing an Airline Management System

This article delves into the object-oriented design and implementation of an Airline Management System using Python3. 

This system will handle various aspects of airline operations including managing flights, passengers, crew, and aircraft.

## System Requirements

The Airline Management System should:

1. **Flight Management:** Create and schedule flights.
2. **Passenger Management:** Manage passenger bookings and check-ins.
3. **Crew Management:** Assign crew members to flights.
4. **Aircraft Management:** Track aircraft and maintenance schedules.

## Core Use Cases

1. **Scheduling and Managing Flights**
2. **Booking and Managing Passenger Seats**
3. **Assigning Crew to Flights**
4. **Managing Aircraft**

## Key Classes:
- `AirlineManagementSystem`: Manages the entire system.
- `Flight`: Represents a flight.
- `Passenger`: Represents a passenger.
- `CrewMember`: Represents a flight crew member.
- `Aircraft`: Represents an aircraft.

## Python Implementation

### Flight Class

Represents a flight.

```python
from datetime import datetime
class Flight:
    def __init__(self, flight_number, aircraft, departure_time, origin, destination):
        self._flight_number = flight_number
        self._aircraft = aircraft
        self._departure_time = departure_time  # Assuming datetime object
        self._origin = origin
        self._destination = destination

    # Property decorators for getters
    @property
    def flight_number(self):
        return self._flight_number

    @property
    def aircraft(self):
        return self._aircraft

    @property
    def departure_time(self):
        return self._departure_time

    @property
    def origin(self):
        return self._origin

    @property
    def destination(self):
        return self._destination
```
### Passenger Class
Manages passenger information.
```python
class Passenger:
    def __init__(self, name, passport_number):
        self._name = name
        self._passport_number = passport_number

    # Property decorators for getters
    @property
    def name(self):
        return self._name

    @property
    def passport_number(self):
        return self._passport_number

```
### CrewMember Class
Represents a crew member.
```python
class CrewMember:
    def __init__(self, name, employee_id):
        self._name = name
        self._employee_id = employee_id

    # Property decorators for getters
    @property
    def name(self):
        return self._name

    @property
    def employee_id(self):
        return self._employee_id

```
### Aircraft Class
Represents an aircraft.
```python
class Aircraft:
    def __init__(self, registration_number, model, total_seats):
        self._registration_number = registration_number
        self._model = model
        self._total_seats = total_seats

    # Property decorators for getters
    @property
    def registration_number(self):
        return self._registration_number

    @property
    def model(self):
        return self._model

    @property
    def total_seats(self):
        return self._total_seats

```
### AirlineManagementSystem Class
Manages overall airline operations.
```python
class AirlineManagementSystem:
    def __init__(self):
        self.flights = []
        self.passengers = []
        self.crew_members = []
        self.aircrafts = []

    def add_flight(self, flight):
        self.flights.append(flight)

    def add_passenger(self, passenger):
        self.passengers.append(passenger)

    def add_crew_member(self, crew_member):
        self.crew_members.append(crew_member)

    def add_aircraft(self, aircraft):
        self.aircrafts.append(aircraft)
```
### Running the Program
Sample to run/demo the system
```python
def main():
    # Create some aircrafts
    aircraft1 = Aircraft("REG123", "Boeing 737", 180)
    aircraft2 = Aircraft("REG456", "Airbus A320", 160)

    # Define departure times
    from datetime import datetime
    departure_time1 = datetime(2024, 5, 20, 15, 30)
    departure_time2 = datetime(2024, 5, 22, 16, 45)

    # Create some flights
    flight1 = Flight("FL123", aircraft1, departure_time1, "New York", "London")
    flight2 = Flight("FL456", aircraft2, departure_time2, "Tokyo", "Sydney")

    # Create some passengers
    passenger1 = Passenger("John Doe", "P12345678")
    passenger2 = Passenger("Jane Smith", "P87654321")

    # Create a crew member
    crew_member1 = CrewMember("Alice Johnson", "E1234")

    # Initialize the airline management system
    airline_system = AirlineManagementSystem()

    # Add entities to the airline management system
    airline_system.add_aircraft(aircraft1)
    airline_system.add_aircraft(aircraft2)
    airline_system.add_flight(flight1)
    airline_system.add_flight(flight2)
    airline_system.add_passenger(passenger1)
    airline_system.add_passenger(passenger2)
    airline_system.add_crew_member(crew_member1)

    # Print some outputs to verify that objects are managed correctly
    print(f"Managed flights: {[f.flight_number for f in airline_system.flights]}")
    print(f"Managed passengers: {[p.name for p in airline_system.passengers]}")
    print(f"Managed crew members: {[c.name for c in airline_system.crew_members]}")
```
### More information
- **Design Pattern**: Composite Pattern
- **Benefits**:
  - **Uniformity**: Treats single objects and collections with the same methods.
  - **Simplicity**: Simplifies client interactions with the system.
  - **Scalability**: Easily adaptable and expandable for new types of operations.



if __name__ == "__main__":
    main()
```

