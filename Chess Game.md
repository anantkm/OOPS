# Designing a Chess Game

In this article, we explore the object-oriented design and implementation of a Chess game using Python3. 

Chess offers an excellent platform to apply object-oriented programming principles due to its complex rules and variety of pieces.

## System Requirements

The Chess game should:

1. **Handle Game Setup:** Set up the chessboard with pieces in their initial positions.
2. **Manage Player Turns:** Alternate turns between two players, white and black.
3. **Enforce Chess Rules:** Validate moves based on chess rules.
4. **Check Game Status:** Identify check, checkmate, and stalemate conditions.

## Core Use Cases

1. **Moving a Piece:** Players move pieces on the board.
2. **Capturing Pieces:** Implement logic for capturing opponent's pieces.
3. **Checking Game Status:** Detect check, checkmate, or stalemate.
4. **Ending the Game:** Conclude the game based on the outcome.

## Key Classes:
- `ChessGame`: Manages overall gameplay.
- `Board`: Represents the chessboard.
- `Piece`: Abstract class for different types of chess pieces.
- `Player`: Represents a player.
- `Move`: Represents a move.

## Python3 Implementation

### Piece Class
Abstract class for chess pieces.
```python
from abc import ABC, abstractmethod

class Piece(ABC):
    def __init__(self, is_white: bool):
        self.is_white = is_white

    @abstractmethod
    def can_move(self, board: 'Board', start: 'Box', end: 'Box') -> bool:
        pass

```
### Board Class
Represents the chessboard.
```python
class Box:
    def __init__(self, x: int, y: int, piece: Piece = None):
        self.x = x
        self.y = y
        self.piece = piece

    def get_piece(self):
        return self.piece

    def set_piece(self, piece: Piece):
        self.piece = piece

class Board:
    def __init__(self):
        self.boxes = [[Box(i, j) for j in range(8)] for i in range(8)]
        self.reset_board()

    def get_box(self, x: int, y: int) -> Box:
        return self.boxes[x][y]

    def reset_board(self):
        # Initialize the board with pieces at the correct positions
        pass

```
### Player Class
Represents a player in the game.
```python
class Player:
    def __init__(self, white_side: bool, human_player: bool):
        self.white_side = white_side
        self.human_player = human_player

```
### Move Class
Represents a move in the game.
```python
class Move:
    def __init__(self, player: Player, start: Box, end: Box):
        self.player = player
        self.start = start
        self.end = end
        self.piece_moved = start.get_piece()

```
### ChessGame Class
Manages the overall game.
```python
from typing import List

class ChessGame:
    def __init__(self):
        self.players = [Player(True, True), Player(False, False)]  # Assuming two players: one white, one black
        self.board = Board()
        self.moves_played: List[Move] = []
        self.current_turn = self.players[0]  # White starts the game

    def player_move(self, player: Player, start_x: int, start_y: int, end_x: int, end_y: int) -> bool:
        start_box = self.board.get_box(start_x, start_y)
        end_box = self.board.get_box(end_x, end_y)
        move = Move(player, start_box, end_box)
        return self.make_move(move, player)

    def make_move(self, move: Move, player: Player) -> bool:
        # Implement move logic, including validation and piece capture
        return True  # Assuming move is valid for demonstration

```

### Example Usage:
```python
def main():
    game = ChessGame()

    # Make a valid move with the white king
    if game.player_move(game.players[0], 0, 0, 1, 1):
        print("White King moved successfully.")
    else:
        print("White King move failed.")

    # Try to make an invalid move with the white pawn
    if game.player_move(game.players[0], 0, 1, 0, 3):
        print("White Pawn moved successfully.")
    else:
        print("White Pawn move failed.")

    # Make a valid move with the black king
    if game.player_move(game.players[1], 7, 7, 6, 6):
        print("Black King moved successfully.")
    else:
        print("Black King move failed.")

if __name__ == "__main__":
    main()

```


### More information
- **Design Pattern**: Strategy Pattern
- **Reason**:
  - **Flexible Behavior**: Each piece type (like a pawn, knight, bishop, etc.) can have different moving strategies, which are implemented as behaviors that can be switched out at runtime if necessary.
  - **Encapsulation of Algorithms**: The movement rules for each chess piece are encapsulated in their respective classes, allowing the algorithms to be independently varied from the clients that use them.
  - **Interchangeability**: The Strategy Pattern allows the chess pieces' movement behaviors to be set dynamically during runtime, which is useful for potentially altering game dynamics or adding new piece types with unique behaviors.
