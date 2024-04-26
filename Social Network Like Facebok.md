# Designing a Social Network Like Facebook

This article explores designing and implementing a social network platform similar to Facebook using Python3. 

We focus on user profiles, friendships, posting updates, and generating a feed of posts.

## System Requirements

The Social Network platform should support:

1. **User Profile Management:** Enabling creation and management of user profiles.
2. **Friendship Management:** Allowing users to connect as friends.
3. **Posting Updates:** Permitting users to post updates and view others' updates.
4. **Feed Generation:** Displaying a feed composed of friends' posts.

## Core Use Cases

1. **Creating and Updating User Profiles**
2. **Managing Friendships**
3. **Creating Posts**
4. **Viewing the Feed**

## Key Classes:
- `SocialNetworkSystem`: Manages the overall operations.
- `User`: Represents a user on the network.
- `Post`: Represents a user's post.
- `Friendship`: Manages the friendships between users.

## Python3 Implementation

### User Class

Handles user profiles and interactions.

```python
import time
from typing import List

class User:
    def __init__(self, name: str):
        self.user_id: str = self.generate_user_id()
        self.name: str = name
        self.friends: List['User'] = []
        self.posts: List['Post'] = []

    def add_friend(self, user: 'User'):
        self.friends.append(user)

    def post_update(self, content: str):
        self.posts.append(Post(self, content))

    def generate_user_id(self) -> str:
        return "USR_" + str(int(time.time() * 1000))

    # Getters for friends and posts to be used in other methods
    def get_friends(self) -> List['User']:
        return self.friends

    def get_posts(self) -> List['Post']:
        return self.posts

```
### Post Class
Represents a post on the social network.
```python
class Post:
    def __init__(self, author: User, content: str):
        self.author: User = author
        self.content: str = content
        self.timestamp: float = time.time()

    # Getter methods for author and timestamp if needed
    def get_author(self) -> User:
        return self.author

    def get_content(self) -> str:
        return self.content

    def get_timestamp(self) -> float:
        return self.timestamp

```
### Friendship Class
Manages connections between users.
```python
class Friendship:
    def __init__(self, user1: User, user2: User):
        self.user1: User = user1
        self.user2: User = user2
        self.establish_friendship()

    def establish_friendship(self):
        self.user1.add_friend(self.user2)
        self.user2.add_friend(self.user1)

```
### SocialNetworkSystem Class
Main class for managing the network.
```python
from typing import List

class SocialNetworkSystem:
    def __init__(self):
        self.users: List[User] = []
        self.friendships: List[Friendship] = []

    def add_user(self, user: User):
        self.users.append(user)

    def add_friendship(self, friendship: Friendship):
        self.friendships.append(friendship)

    def get_feed(self, user: User) -> List[Post]:
        feed: List[Post] = []
        for friend in user.get_friends():
            feed.extend(friend.get_posts())
        return feed

```

### Example Usage:
``` python
def main():
    # Initialize the social network system
    system = SocialNetworkSystem()

    # Create users
    alice = User("Alice")
    bob = User("Bob")
    charlie = User("Charlie")

    # Add users to the system
    system.add_user(alice)
    system.add_user(bob)
    system.add_user(charlie)

    # Establish friendships
    system.add_friendship(Friendship(alice, bob))
    system.add_friendship(Friendship(bob, charlie))

    # Users post updates
    alice.post_update("Hello, this is Alice!")
    bob.post_update("Bob's first post!")
    charlie.post_update("Good day from Charlie!")

    # Get and print Alice's feed
    alices_feed = system.get_feed(alice)
    print(f"Alice's Feed:")
    for post in alices_feed:
        print(f"{post.get_author().name}: {post.get_content()}")

if __name__ == "__main__":
    main()

```