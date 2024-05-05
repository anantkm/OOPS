# Designing a Cricket Information System

In this article, we're going to explore the object-oriented design and implementation of a cricket information system, similar to Cricinfo, using Python3. 

This system will focus on delivering real-time cricket match updates, managing player statistics, and maintaining team information.

## System Requirements

The system needs to:

1. **Manage Match Details:** Provide real-time updates of cricket matches.
2. **Manage Players and Teams:** Store and update information about players and teams.
3. **Track Statistics:** Keep records of player and team statistics.
4. **Schedule Matches:** Organize and update upcoming cricket matches and series.

## Core Use Cases:

1. **Updating Match Details:** Real-time updates of match scores and events.
2. **Managing Player/Team Profiles:** Adding and updating profiles of players and teams.
3. **Viewing Statistics:** Accessing statistical data of players and teams.
4. **Scheduling Matches:** Planning and updating upcoming matches and series.

## Key Classes:
- `CricketSystem` : Central system managing all functionalities.
- `Match`: Represents a cricket match.
- `Player`: Represents a cricket player.
- `Team`: Represents a cricket team.
- `Statistics`: Manages statistical data.

## Python3 Implementation

### Player Class

Manages player information and statistics.

```python
public class Player {
from typing import Optional

class Player:
    def __init__(self, name: str, team: 'Team'):
        self.name: str = name
        self.team: Team = team
        self.stats: Statistics = Statistics()

    def update_statistics(self, runs: int, wickets: int):
        self.stats.update_stats(runs, wickets)

```
### Team Class
Represents a cricket team.
```python
from typing import List

class Team:
    def __init__(self, name: str):
        self.name: str = name
        self.players: List[Player] = []

    def add_player(self, player: Player):
        self.players.append(player)

```
### Match Class
Manages cricket match details.
```python
from enum import Enum

class MatchStatus(Enum):
    SCHEDULED = 'SCHEDULED'
    LIVE = 'LIVE'
    COMPLETED = 'COMPLETED'

class Match:
    def __init__(self, team_a: Team, team_b: Team, location: str):
        self.team_a: Team = team_a
        self.team_b: Team = team_b
        self.location: str = location
        self.status: MatchStatus = MatchStatus.SCHEDULED
        self.team_a_score: int = 0
        self.team_b_score: int = 0

    def update_match_details(self, team_a_score: int, team_b_score: int, status: MatchStatus):
        self.team_a_score = team_a_score
        self.team_b_score = team_b_score
        self.status = status

```
### Statistics Class
Manages a player's cricket statistics.
```python
class Statistics:
    def __init__(self):
        self.matches_played: int = 0
        self.runs: int = 0
        self.wickets: int = 0

    def update_stats(self, runs_scored: int, wickets_taken: int):
        self.matches_played += 1
        self.runs += runs_scored
        self.wickets += wickets_taken

```
### CricketSystem Class
Main class for the cricket information system.
```python
from typing import List, Optional

class CricketSystem:
    def __init__(self):
        self.matches: List[Match] = []
        self.teams: List[Team] = []

    def add_match(self, match: Match):
        self.matches.append(match)

    def add_team(self, team: Team):
        self.teams.append(team)

    def update_match(self, match_id: str, team_a_score: int, team_b_score: int, status: MatchStatus):
        match = self.find_match_by_id(match_id)
        if match:
            match.update_match_details(team_a_score, team_b_score, status)

    def find_match_by_id(self, match_id: str) -> Optional[Match]:
        for match in self.matches:
            # Assuming a unique identifier `match_id` exists or is simulated by another attribute
            # Here we simply simulate finding a match
            return match
        return None

```

### Example Usage
``` python
def main():
    cricket_system = CricketSystem()
    team_a = Team("Team A")
    team_b = Team("Team B")
    cricket_system.add_team(team_a)
    cricket_system.add_team(team_b)

    player = Player("John Doe", team_a)
    team_a.add_player(player)

    match = Match(team_a, team_b, "Cricket Ground")
    cricket_system.add_match(match)

    player.update_statistics(50, 3)  # Player scored 50 runs and took 3 wickets
    match.update_match_details(250, 200, MatchStatus.COMPLETED)

    print(f"Match status: {match.status.value}, Team A score: {match.team_a_score}, Team B score: {match.team_b_score}")

if __name__ == "__main__":
    main()

```
### More information
- **Design Pattern**: Observer Pattern
- **Reason**:
  - **Real-Time Updates**: Ensures that changes in match scores, player performances, or team compositions are propagated in real time to all relevant parts of the system.
  - **Decoupling of Components**: Allows the system to maintain a clean separation between the data models (matches, players, teams) and the components that need updates, enhancing modularity and maintainability.
  - **Dynamic Interaction**: Supports dynamic interactions between the core system and its various components, allowing for scalable and flexible software architecture that can handle the dynamic nature of live sports updates effectively.
