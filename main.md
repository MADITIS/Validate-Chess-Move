# Classes

**Chessboard:**
Represents the chessboard on which the game is played.

**Piece:**
Base class representing a chess piece.

**Pawn (inherits Piece):**
Represents a pawn chess piece.

**Rook (inherits Piece):**
Represents a rook chess piece.

**Knight (inherits Piece):**
Represents a knight chess piece.

**Bishop (inherits Piece):**
Represents a bishop chess piece.

**Queen (inherits Piece):**
Represents a queen chess piece.

**King (inherits Piece):**
Represents a king chess piece.


## Class Diagram

![Chessboard](/Validate%20Chess%20Moves%20(1).png)


## Validate move code

```python
from abc import ABC, abstractmethod
from typing import Optional, Tuple, List


class ChessBoard:
    def __init__(self):
        self.board: List[List[Optional['Piece']]] = [
            [None] * 8 for _ in range(8)]

    def get_piece(self, position: Tuple[int, int]) -> Optional['Piece']:
        row, col = position
        return self.board[row][col]

    def set_piece(self, position: Tuple[int, int], piece: 'Piece') -> None:
        row, col = position
        self.board[row][col] = piece

```
## Abstract base class of pieces
```python
class Piece(ABC):
    def __init__(self, color):
        self.color = color

    @abstractmethod
    def can_move(self, source: Tuple[int, int],
                 destination: Tuple[int, int], chessboard: ChessBoard): ...

```

```python
class Pawn(Piece):
    def can_move(self, source: Tuple[int, int], destination: Tuple[int, int], chessboard: ChessBoard) -> bool:
        src_row, src_col = source
        dest_row, dest_col = destination

        if ChessBoard.get_piece(destination) is None:
            if self.color == 'white':
                if src_col == dest_col and dest_row - src_row == 1:
                    return True
            elif self.color == 'black':
                if src_col == dest_col and dest_row - src_row == -1:
                    return True

        return False


class Rook(Piece):
    def can_move(self, source: Tuple[int, int], destination: Tuple[int, int], chessboard: ChessBoard) -> bool:
        src_row, src_col = source
        dest_row, dest_col = destination

        dest_piece = ChessBoard.get_piece(destination)
        if dest_piece is None or dest_piece.color != self.color:
            if src_row == dest_row or src_col == dest_col:
                return True

        return False


class Knight(Piece):
    def can_move(self, source: Tuple[int, int], destination: Tuple[int, int], chessboard: ChessBoard) -> bool:
        src_row, src_col = source
        dest_row, dest_col = destination

        dest_piece = ChessBoard.get_piece(destination)
        if dest_piece is None or dest_piece.color != self.color:
            if abs(src_row - dest_row) == 2 and abs(src_col - dest_col) == 1:
                return True
            elif abs(src_row - dest_row) == 1 and abs(src_col - dest_col) == 2:
                return True

        return False


class Bishop(Piece):
    def can_move(self, source: Tuple[int, int], destination: Tuple[int, int], chessboard: ChessBoard) -> bool:
        src_row, src_col = source
        dest_row, dest_col = destination

        dest_piece = ChessBoard.get_piece(destination)
        if dest_piece is None or dest_piece.color != self.color:
            if abs(src_row - dest_row) == abs(src_col - dest_col):
                return True

        return False
##...king and queen
```
