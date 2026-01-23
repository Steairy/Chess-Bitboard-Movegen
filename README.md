# Chess-Bitboard-Movegen
Legal move generation using bitboards for chess. Written in C++

# How it works
It uses something called magic bitboards, along with a few other optimizations to generate legal moves for chess quickly.

It detects repetition using something called zobrist hashing.

# Usage
To use this, include board.h in a project you want and create a Board object

Each time you want to generate the legal moves(including when you first create the board), run the board.generateLegal function.

After running that function, you can see the legal moves by iterating through indexes 0 to board.legalMoveCount. For each index the legal move is board.legalMoves[index]

The reason we have to do this is because legalMoves is an array with a fixed size.

When compiling the project, compile board.cpp along with the project.

# Moves
Each move is an unsigned 32 bit integer. When making moves, the user needs to pass this move integer into the makeMove function.

To make a move, you need to call the makeMove function like this: board.makeMove(uint32_t move, bool outcomeCheck). If outcomeCheck is true, the board will check if the game is over and what the outcome is. For perft testing, this is false since it doesn't matter for perft.

To unmake a move, you can just call board.unmakeMove()

To extract extra info while making a chess engine or making the moves human readable, here is how the move integer is generated:
- Bits 0-5 (6 bits) = Starting square (0-63, 0 = a1, 63 = h8)
- Bits 6-11 (6 bits) = Ending square
- Bits 12-15 (4 bits) = Moving piece (0 = white pawn, 1 = white knight, 2 = white bishop, 3 = white rook, 4 = white queen, 5 = white king. The black versions are the same as white but with 6 added)
- Bit 16 (1 bit) = Boolean for if the move is a capture
- Bits 17-20 (4 bits) = Captured piece
- Bit 21 (1 bit) = Shows if move is an en passant
- Bit 22 (1 bit) = Shows if move is castling
- Bit 23 (1 bit) = Shows the castle side (0 = kingside, 1 = queenside)
- Bit 24 (1 bit) = Shows if the move is a promotion
- Bits 25-28 (4 bits) = Which piece the pawn promoted to

# Outcome Checking
To check the outcome, we can use the gameOver and outcome variables of the board. If board.gameOver is false, the game hasn't ended yet. When the game ends, you can check board.outcome for the result. -1 = Black wins, 0 = Draw, 1 = White Wins.

# Performance
Here are the perft times for this bitboard engine, you can replicate the perft times using the built in perft(Board board, int depth) function.

Note: The perft tests were done with g++ and using -O3 optimization. In the perft tests, zobrist hashing was still done but terminal state checking wasn't done. The perft results are the average of 100 runs.

- Perft 4: 3.7 ms
- Perft 5: 84.6 ms
- Perft 6: 2200 ms

# Extra Notes
In this board implementation, white is 0 and black is 1. So when checking board.turn, keep this in mind. You can also use board.WHITE or board.BLACK to remove any confusion.

To access the current zobrist hash, you can do board.zobrist.hash



