# Designing an Elevator System

This article explores the design and implementation of an Elevator System using object-oriented principles in Python3, focusing on functionality, scalability, and user interaction.

## System Requirements

The Elevator System is designed to:

1. **Handle Multiple Requests:** Manage requests from different floors efficiently.
2. **Optimize Elevator Movement:** Allocate elevators based on requests to improve efficiency.
3. **Track Elevator Status:** Monitor the state and position of each elevator.
4. **Incorporate Safety Features:** Ensure key safety mechanisms are in place.

## Core Use Cases

1. **Requesting an Elevator:** Users can call elevators to their current floor.
2. **Transporting Passengers:** Elevators carry passengers to their desired floors.
3. **Managing Idle Elevators:** Efficiently allocate available elevators.

## Key Classes:
- `ElevatorSystem`: Manages multiple elevators.
- `Elevator`: Represents an individual elevator.
- `ElevatorControlPanel`: Interface for users to interact with an elevator.

## Python3 Implementation

### Elevator Class

This class represents an individual elevator.

```python
from enum import Enum, auto

class ElevatorState(Enum):
    IDLE = auto()
    MOVING = auto()

class Elevator:
    def __init__(self) -> None:
        self.current_floor: int = 0  # Starting at ground floor
        self.state: ElevatorState = ElevatorState.IDLE

    def move_to_floor(self, floor: int) -> None:
        # Simulate elevator movement
        self.current_floor = floor
        self.state = ElevatorState.MOVING
        # Assume elevator reaches the floor instantly
        self.state = ElevatorState.IDLE

    def get_current_floor(self) -> int:
        return self.current_floor

    def get_state(self) -> ElevatorState:
        return self.state

```
### ElevatorSystem Class
```python
from typing import List, Optional

class ElevatorSystem:
    def __init__(self, number_of_elevators: int) -> None:
        self.elevators: List[Elevator] = [Elevator() for _ in range(number_of_elevators)]

    def request_elevator(self, floor: int) -> None:
        closest_elevator = self.find_closest_elevator(floor)
        if closest_elevator:
            closest_elevator.move_to_floor(floor)

    def find_closest_elevator(self, floor: int) -> Optional[Elevator]:
        closest: Optional[Elevator] = None
        min_distance: int = float('inf')
        for elevator in self.elevators:
            distance = abs(elevator.get_current_floor() - floor)
            if distance < min_distance and elevator.get_state() == ElevatorState.IDLE:
                closest = elevator
                min_distance = distance
        return closest
```
### ElevatorControlPanel Class
```python
class ElevatorControlPanel:
    def __init__(self, elevator: Elevator) -> None:
        self.elevator: Elevator = elevator

    def go_to_floor(self, floor: int) -> None:
        self.elevator.move_to_floor(floor)

```

### Example Usage

``` python
def main():
    # Initialize an elevator system with 3 elevators
    elevator_system = ElevatorSystem(number_of_elevators=3)

    # Simulate several elevator requests from different floors
    elevator_system.request_elevator(5)  # Request from floor 5
    elevator_system.request_elevator(1)  # Request from floor 1
    elevator_system.request_elevator(8)  # Request from floor 8

    # Output the state of each elevator after handling the requests
    for i, elevator in enumerate(elevator_system.elevators):
        print(f"Elevator {i + 1} is at floor {elevator.get_current_floor()} and is {elevator.get_state().name}.")

    # Example of a passenger using the control panel inside an elevator
    # Let's assume this passenger is in the first elevator and wants to go to the 10th floor
    control_panel = ElevatorControlPanel(elevator_system.elevators[0])
    control_panel.go_to_floor(10)

    # Output the state of the first elevator after the control panel use
    print(f"After using control panel, Elevator 1 is now at floor {elevator_system.elevators[0].get_current_floor()} and is {elevator_system.elevators[0].get_state().name}.")

if __name__ == "__main__":
    main()

```