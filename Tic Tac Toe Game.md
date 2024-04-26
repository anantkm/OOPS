# Designing a Tic Tac Toe Game

This article explores the design and implementation of a Tic Tac Toe game using object-oriented principles in Python3. 

It's a classic game that provides a great opportunity to apply fundamental OOP concepts.

## System Requirements

The Tic Tac Toe game will:

1. **Handle Player Moves:** Allow two players to alternately place their marks (X or O) on a 3x3 grid.
2. **Check for Win or Draw:** Determine the outcome of the game – a win for one player, a draw, or continuation.
3. **Reset the Game:** Enable starting a new game after one round finishes.

## Core Use Cases

1. **Making a Move:** Players take turns to place their marks on the grid.
2. **Checking Game Status:** After each move, check if the game is won, drawn, or still ongoing.
3. **Resetting the Game:** Clear the board for a new round.

## Key Classes:
- `TicTacToe`: Manages the overall game.
- `Board`: Represents the game board.
- `Player`: Enum to represent the players (X and O).

## Python3 Implementation

### Player Enum

Represents the players in the game.

```python
from enum import Enum

class Player(Enum):
    X = 'X'
    O = 'O'

```
### Board Class
```python
class Board:
    def __init__(self):
        self.size = 3
        self.board = [[None for _ in range(self.size)] for _ in range(self.size)]

    def make_move(self, player: Player, row: int, col: int) -> bool:
        if 0 <= row < self.size and 0 <= col < self.size and self.board[row][col] is None:
            self.board[row][col] = player
            return True
        return False

    def check_winner(self) -> Player:
        # Check rows, columns and diagonals
        for i in range(self.size):
            if self.board[i][0] is not None and all(self.board[i][j] == self.board[i][0] for j in range(1, self.size)):
                return self.board[i][0]
            if self.board[0][i] is not None and all(self.board[j][i] == self.board[0][i] for j in range(1, self.size)):
                return self.board[0][i]

        # Check diagonals
        if self.board[0][0] is not None and all(self.board[i][i] == self.board[0][0] for i in range(1, self.size)):
            return self.board[0][0]
        if self.board[0][self.size - 1] is not None and all(self.board[i][self.size - i - 1] == self.board[0][self.size - 1] for i in range(1, self.size)):
            return self.board[0][self.size - 1]

        return None

    def is_full(self) -> bool:
        return all(all(cell is not None for cell in row) for row in self.board)

    def reset(self):
        self.board = [[None for _ in range(self.size)] for _ in range(self.size)]

```
### TicTacToe Class
```python
from enum import Enum

class GameState(Enum):
    PLAYING = 'PLAYING'
    X_WON = 'X_WON'
    O_WON = 'O_WON'
    DRAW = 'DRAW'


class TicTacToe:
    def __init__(self):
        self.board = Board()
        self.current_player = Player.X
        self.game_state = GameState.PLAYING

    def play_move(self, row: int, col: int):
        if self.game_state != GameState.PLAYING:
            print("Game is already over.")
            return

        if not self.board.make_move(self.current_player, row, col):
            print("Invalid move. Try again.")
            return

        winner = self.board.check_winner()
        if winner:
            self.game_state = GameState.X_WON if winner == Player.X else GameState.O_WON
            print(f"Player {winner.value} wins!")
        elif self.board.is_full():
            self.game_state = GameState.DRAW
            print("Game is a draw.")
        else:
            self.current_player = Player.O if self.current_player == Player.X else Player.X

    def reset_game(self):
        self.board.reset()
        self.current_player = Player.X
        self.game_state = GameState.PLAYING
        print("Game reset. Player X starts.")

```

### Example Usage
``` python
def main():
    game = TicTacToe()

    moves = [(0, 0), (0, 1), (1, 1), (1, 0), (2, 2)]
    for move in moves:
        row, col = move
        print(f"Player {game.current_player.value} plays to ({row}, {col})")
        game.play_move(row, col)
        if game.game_state != GameState.PLAYING:
            break

    if game.game_state == GameState.PLAYING:
        print("Continuing game...")
    else:
        print(f"Game ended: {game.game_state.value}")

if __name__ == '__main__':
    main()

```