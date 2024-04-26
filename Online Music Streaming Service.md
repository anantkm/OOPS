# Designing an Online Music Streaming Service Like Spotify

This article focuses on developing an object-oriented design for an Online Music Streaming Service similar to Spotify using Python3. 

The system aims to deliver a comprehensive music streaming experience.

## System Requirements

The Online Music Streaming Service should:

1. **User Account Management**: Manage user registrations, profiles, and subscriptions.
2. **Music Library Management**: Maintain a library of songs, artists, and albums.
3. **Streaming and Playback**: Enable streaming of music and manage playback settings.
4. **Playlist Management**: Allow users to create and manage personalized playlists.
5. **User Recommendation System**: Offer music suggestions based on preferences and listening history.

## Core Use Cases

1. **Registering and Managing User Accounts**
2. **Browsing and Streaming Music**
3. **Creating and Editing Playlists**
4. **Recommending Music**
5. **Handling Subscriptions and Payments**

## UML/Class Diagrams

Key Classes:

- `MusicStreamingService`: Manages the system.
- `User`: Represents a subscriber.
- `Song`: Represents an individual music track.
- `Playlist`: Manages a collection of songs.
- `Subscription`: Handles subscription details.

## Python3 Implementation

### User Class

Manages user account information and preferences.

```python
from typing import List

class User:
    def __init__(self, user_id: str, name: str):
        self.user_id: str = user_id
        self.name: str = name
        self.playlists: List['Playlist'] = []
        self.subscription: Subscription = Subscription()

    def create_playlist(self, playlist_name: str):
        new_playlist = Playlist(playlist_name)
        self.playlists.append(new_playlist)

```
### Song Class
Represents an individual music track.
```python
class Song:
    def __init__(self, song_id: str, title: str, artist: str, album: str, duration: float):
        self.song_id: str = song_id
        self.title: str = title
        self.artist: str = artist
        self.album: str = album
        self.duration: float = duration

```
### Playlist Class
Manages a collection of songs.
```python
class Playlist:
    def __init__(self, name: str):
        self.name: str = name
        self.songs: List[Song] = []

    def add_song(self, song: Song):
        self.songs.append(song)

```
### Subscription Class
Handles user subscription details.
```python
from enum import Enum

class SubscriptionType(Enum):
    FREE = 'FREE'
    PREMIUM = 'PREMIUM'

class Subscription:
    def __init__(self):
        self.type: SubscriptionType = SubscriptionType.FREE
        self.price: float = 0.0

    def upgrade_subscription(self, new_type: SubscriptionType):
        self.type = new_type
        self.price = 9.99 if new_type == SubscriptionType.PREMIUM else 0.0

```
### MusicStreamingService Class
```python
from typing import List, Optional

class MusicStreamingService:
    def __init__(self):
        self.users: List[User] = []
        self.songs: List[Song] = []

    def add_user(self, user: User):
        self.users.append(user)

    def add_song(self, song: Song):
        self.songs.append(song)

    def search_songs(self, title: str) -> List[Song]:
        return [song for song in self.songs if song.title.lower() == title.lower()]

    def subscribe_user(self, user_id: str, subscription_type: SubscriptionType):
        user = self.find_user_by_id(user_id)
        if user:
            user.subscription.upgrade_subscription(subscription_type)

    def find_user_by_id(self, user_id: str) -> Optional[User]:
        for user in self.users:
            if user.user_id == user_id:
                return user
        return None

```

### Example Usage
``` python
def main():
    service = MusicStreamingService()
    user1 = User("001", "Alice")
    service.add_user(user1)

    song1 = Song("S001", "Shape of You", "Ed Sheeran", "Divide", 3.54)
    service.add_song(song1)

    user1.create_playlist("Favorites")
    user1.playlists[0].add_song(song1)

    found_songs = service.search_songs("Shape of You")
    print(f"Found songs: {', '.join(song.title for song in found_songs)}")

    service.subscribe_user("001", SubscriptionType.PREMIUM)
    print(f"User {user1.name} subscription type: {user1.subscription.type.name}")

if __name__ == "__main__":
    main()

```