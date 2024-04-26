# Designing a Logging Framework

In this article, we will explore the design and development of a Logging Framework in Python3, using object-oriented programming principles. 

A Logging Framework is crucial for effective monitoring, debugging, and auditing of applications.

## System Requirements

The Logging Framework should:

1. **Support Multiple Log Levels:** Including INFO, DEBUG, WARN, and ERROR.
2. **Flexible Log Destination:** Enable logging to various outputs like the console, files, or external services.
3. **Configurable Formatting:** Allow for custom log message formats.
4. **Performance Efficiency:** Ensure minimal impact on application performance.

## Core Use Cases

1. **Logging Messages:** Ability to log messages at different levels.
2. **Configuring Loggers:** Setup loggers with varying settings and outputs.
3. **Managing Log Output:** Direct messages to appropriate destinations based on configurations.

## Key Classes:
- `Logger`: Main interface for logging messages.
- `LogLevel`: Enum representing log levels.
- `LogDestination`: Interface for different log output destinations.
- `ConsoleDestination`, `FileDestination`: Implementations of the `LogDestination` interface.

## Python3 Implementation

### LogLevel Enum

Defines different levels of logging.

```python
from enum import Enum

class LogLevel(Enum):
    INFO = 1
    DEBUG = 2
    WARN = 3
    ERROR = 4

```
### LogDestination Interface
Interface for different log destinations.
```python
from abc import ABC, abstractmethod

class LogDestination(ABC):
    @abstractmethod
    def write_log(self, message: str):
        pass

```
### ConsoleDestination Class
Implementation for logging to the console.
```python
class ConsoleDestination(LogDestination):
    def write_log(self, message: str):
        print(message)

```
### FileDestination Class
Implementation for logging to a file.
```python
import os

class FileDestination(LogDestination):
    def __init__(self, filename: str):
        self.filename = filename

    def write_log(self, message: str):
        with open(self.filename, 'a') as file:
            file.write(message + '\n')

```
### Logger Class
Main class for logging operations.
```python
from datetime import datetime

class Logger:
    def __init__(self, level: LogLevel, destination: LogDestination):
        self.level = level
        self.destination = destination

    def log(self, level: LogLevel, message: str):
        if level.value >= self.level.value:
            timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
            formatted_message = f"{timestamp} [{level.name}] {message}"
            self.destination.write_log(formatted_message)

```

### Example Usage
``` python
def main():
    # Create a console destination logger
    console_logger = Logger(LogLevel.DEBUG, ConsoleDestination())

    # Log some messages
    console_logger.log(LogLevel.INFO, "This is an informational message.")
    console_logger.log(LogLevel.ERROR, "This is an error message.")

    # Create a file destination logger
    file_logger = Logger(LogLevel.WARN, FileDestination('log.txt'))

    # Log a warning message
    file_logger.log(LogLevel.WARN, "This is a warning message.")
    file_logger.log(LogLevel.DEBUG, "This debug message will not be logged, because the level is too low.")

if __name__ == "__main__":
    main()

```