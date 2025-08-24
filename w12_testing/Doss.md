# 11.3 ChessTest

We have the following method used in a chess game: boolean canMoveTo(int x, int y). This method is part of the Piece class and returns whether or not the piece can move to position (x, y). Explain how you would test this method.

## Chess Piece Types

There are 6 different type of pieces in Chess, King, Queen, Rook, Bishop, Knight, Pawn. They can move to 4 each different directions. So we have to test with 4 directions for each pieces. In the case of Rook and Bishop, I propose to move to end of the board for each 4 directions.

## Piece Design

Basically, I prefer to design a classic with interface. Because inheritance way is not flexible as much as interface. So I propose to design a Piece interface, and create each Piece classes with it.

```java
interface Piece {
    boolean canMoveTo(int x, int y);
    void moveTo(int x, int y);
}

class Knight implements Piece {
    private int x, y;

    public Knight(int startX, int startY) {
        this.x = startX;
        this.y = startY;
    }

    @Override
    public boolean canMoveTo(int x, int y) {
        return (Math.abs(this.x - x) == 2 && Math.abs(this.y - y) == 1) ||
               (Math.abs(this.x - x) == 1 && Math.abs(this.y - y) == 2);
    }

    @Override
    public void moveTo(int x, int y) {
        if (canMoveTo(x, y)) {
            this.x = x;
            this.y = y;
        }
    }
}
```

## Test

Let's think about test cases - what cases should we consider? First, we should have boundary value testing, and we can think about valid moves, invalid moves, etc. Also, in chess, each piece has different characteristics. For example, pawns cannot move backwards, knights can move without being obstructed by other pieces, and so on.

So I propose dividing the testing into the following cases:

```java
class PieceTest {
    private ChessBoard board;

    @BeforeEach
    void setUp() {
        board = new ChessBoard();
    }

    @Test
    void testOutOfBound() {
        Piece pawn = new Pawn(1, 1);

        assertFalse(pawn.canMoveTo(-1, 0));
        assertFalse(pawn.canMoveTo(0, -1));
    }

    @Test
    void testValidMove() {
        Piece pawn = new Pawn(1, 1);

        assertTrue(pawn.canMoveTo(1, 2));
    
        board.setPiece(2, 2, new Pawn(2, 2));
        assertTrue(pawn.canMoveTo(2, 2));
    }

    @Test
    void testInvalidMove() {
        Piece pawn = new Pawn(1, 1);

        assertFalse(pawn.canMove(1, 0));
        assertFalse(pawn.canMove(0, 1));
    }

    @Test
    void testPathBlocked() {
        Piece pawn = new Pawn(1, 1);

        board.setPiece(new Pawn(1, 2));
        assertFalse(pawn.canMove(1, 2));

        Piece knight = new Knight(1, 0);
        assertTrue(knight.canMove(2, 2));
    }
}
```