# Designing a Pub-Sub System in Python3

This article explores the design and implementation of a basic Publish-Subscribe (Pub-Sub) system using Python3, following object-oriented design principles.

The Pub-Sub model is a widely used pattern in messaging systems, allowing for scalable and decoupled communication. This guide outlines the design and implementation of such a system.

## Understanding the Requirements
The system will enable:
- Publishers to send messages to topics.
- Subscribers to receive messages from topics they are subscribed to.
- Scalability and decoupling between publishers and subscribers.

## Core Use Cases
- **Subscribing to Topics**: Users can subscribe to topics of interest.
- **Publishing Messages**: Publishers can send messages to topics.
- **Receiving Messages**: Subscribers receive messages from their subscribed topics.

## Key Classes:
- **Publisher Class**: Responsible for publishing messages.
- **Subscriber Interface**: Interface for subscribers to receive messages.
- **Topic Class**: Represents a topic in the system.
- **PubSubService Class**: Manages the publication and subscription logic.

## Python3 Implementation

### Topic Class 
```python
class Topic:
    def __init__(self, name: str):
        self.name = name

    def get_name(self) -> str:
        return self.name

```

### Message Class
```python
class Message:
    def __init__(self, content: str, topic: Topic):
        self.content = content
        self.topic = topic

    def get_content(self) -> str:
        return self.content

    def get_topic(self) -> Topic:
        return self.topic

```

### Subscriber and Publisher Classes

```python
from abc import ABC, abstractmethod

class Subscriber(ABC):
    @abstractmethod
    def receive(self, message: Message):
        pass


class Publisher:
    def publish(self, message: Message, service: 'PubSubService'):
        service.add_message_to_queue(message)

```

### Publisher Service
```python
from typing import List, Dict, Deque
from collections import deque

class PubSubService:
    def __init__(self):
        self.message_queue: Deque[Message] = deque()
        self.subscribers_topic_map: Dict[str, List[Subscriber]] = {}

    def add_message_to_queue(self, message: Message):
        self.message_queue.append(message)

    def add_subscriber(self, topic_name: str, subscriber: Subscriber):
        if topic_name not in self.subscribers_topic_map:
            self.subscribers_topic_map[topic_name] = []
        self.subscribers_topic_map[topic_name].append(subscriber)

    def remove_subscriber(self, topic_name: str, subscriber: Subscriber):
        if topic_name in self.subscribers_topic_map:
            self.subscribers_topic_map[topic_name].remove(subscriber)

    def broadcast(self):
        while self.message_queue:
            message = self.message_queue.popleft()
            topic_name = message.get_topic().get_name()
            subscribers_of_topic = self.subscribers_topic_map.get(topic_name, [])
            for subscriber in subscribers_of_topic:
                subscriber.receive(message)

```
### Example Usage
``` python
class SampleSubscriber(Subscriber):
    def receive(self, message: Message):
        print(f"Received message on topic '{message.get_topic().get_name()}': {message.get_content()}")

def main():
    # Initialize the service and entities
    service = PubSubService()
    tech_topic = Topic("Technology")
    sports_topic = Topic("Sports")

    # Create subscribers
    tech_subscriber = SampleSubscriber()
    sports_subscriber = SampleSubscriber()

    # Register subscribers
    service.add_subscriber(tech_topic.get_name(), tech_subscriber)
    service.add_subscriber(sports_topic.get_name(), sports_subscriber)

    # Create a publisher
    publisher = Publisher()

    # Publish messages
    publisher.publish(Message("Hello World in Tech!", tech_topic), service)
    publisher.publish(Message("Hello World in Sports!", sports_topic), service)

    # Broadcast messages to all subscribers
    service.broadcast()

if __name__ == "__main__":
    main()

```