# Designing a Traffic Signal Control System

In this article, we will explore the object-oriented design and implementation of a Traffic Signal Control System using Python. 

This system is crucial for managing traffic flow and ensuring safety at intersections.

## System Requirements

The Traffic Signal Control System should:

1. **Signal Timing Management**: Control the timing of traffic lights.
2. **Intersection Management**: Manage multiple traffic signals at an intersection.
3. **Mode Configuration**: Support different traffic modes.
4. **Emergency Override**: Allow manual control in emergencies.

## Core Use Cases

1. **Switching Signals**: Change signals from red to green and vice versa.
2. **Timing Adjustment**: Modify the duration of each signal.
3. **Mode Selection**: Choose operational modes.
4. **Emergency Control**: Manual override for emergencies.

## Key Classes:
- `TrafficSignalSystem`: Manages the system.
- `TrafficLight`: Represents an individual traffic light.
- `IntersectionController`: Manages lights at an intersection.
- `ControlPanel`: For manual control and emergency overrides.


## Python Implementation

### TrafficLight Class
```Python
from enum import Enum

class LightState(Enum):
    RED = 1
    GREEN = 2
    YELLOW = 3

class TrafficLight:
    def __init__(self, id):
        self.id = id
        self.state = LightState.RED

    def change_state(self, state):
        self.state = state
```

### IntersectionController Class
Manages traffic lights at an intersection.

```python
class IntersectionController:
    def __init__(self):
        self.traffic_lights = {}

    def add_traffic_light(self, light):
        self.traffic_lights[light.id] = light

    def change_signal(self, light_id, state):
        if light_id in self.traffic_lights:
            self.traffic_lights[light_id].change_state(state)
    
    #Additional methods...
```


### ControlPanel Class
For manual control and emergencies.

```Python
class ControlPanel:
    def __init__(self, controller):
        self.controller = controller

    def override_signal(self, light_id, state):
        self.controller.change_signal(light_id, state)

    #Additional methods...
```

### TrafficSignalSystem Class
Main class managing the traffic signal system.

```python
class TrafficSignalSystem:
    def __init__(self):
        self.intersection_controllers = []

    def add_intersection_controller(self, controller):
        self.intersection_controllers.append(controller)
    
    #additional methods to update the system config
```

### Example Usage
``` python
def main():
    # Initialize the traffic signal system
    signal_system = TrafficSignalSystem()

    # Create intersection controller
    intersection = IntersectionController()

    # Create traffic lights and add them to the intersection
    traffic_light1 = TrafficLight("001")
    traffic_light2 = TrafficLight("002")
    intersection.add_traffic_light(traffic_light1)
    intersection.add_traffic_light(traffic_light2)

    # Add the intersection controller to the traffic signal system
    signal_system.add_intersection_controller(intersection)

    # Create a control panel for the intersection
    control_panel = ControlPanel(intersection)

    # Demonstration of changing signals
    print("Initial state of traffic lights:")
    for light_id, light in intersection.traffic_lights.items():
        print(f"Light {light_id}: {light.state.name}")

    # Change signal to GREEN at traffic light 1
    intersection.change_signal("001", LightState.GREEN)
    print("\nAfter changing state to GREEN:")
    print(f"Light 001: {intersection.traffic_lights['001'].state.name}")

    # Override signal to RED at traffic light 1 using the control panel
    control_panel.override_signal("001", LightState.RED)
    print("\nAfter overriding state to RED using Control Panel:")
    print(f"Light 001: {intersection.traffic_lights['001'].state.name}")

    # Change traffic light 2 to YELLOW
    intersection.change_signal("002", LightState.YELLOW)
    print("\nAfter changing state to YELLOW:")
    print(f"Light 002: {intersection.traffic_lights['002'].state.name}")

if __name__ == "__main__":
    main()
```
