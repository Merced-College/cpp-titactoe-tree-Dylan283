# cpp_tictactoe_tree

// The Minimax function is used to determine the best possible move for the computer in the Tic-Tac-Toe game. It follows a recursive approach to evaluate all possible future states and assign scores based on whether the computer wins (+1), the human wins (-1), or the game ends in a draw (0). The function strategically minimizes the opponent’s best move while maximizing the computer’s chances of winning.

// The GameState class acts as a node in the game tree. Each possible board state represents a unique node, and the getAvailableMoves method provides the child nodes (next possible states). Whenever a move is made, a new GameState object is created, representing the next level of the tree. The tree branches out until a terminal state is reached (win, loss, or draw).

//Recursion in the minimax function stops under the following conditions:

A player has won – If the checkWinner function detects a win for either HUMAN or COMPUTER, the function returns the corresponding score.

The board is full – If there are no empty cells left, meaning no further moves can be made, the function returns a score of 0 (draw).

End of available moves – If there are no more moves to explore in a given path, the function stops calculating further states.

// Recursion and trees are powerful tools for decision-making in AI. The game tree structure allowed the AI to explore possible moves systematically, while recursion helped it evaluate outcomes efficiently. The AI was smart in that it could determine optimal moves using minimax, but its depth limitation meant it wasn’t always perfect. Additionally, adding randomness made it less predictable but prevented it from always playing the exact same game. Given more time, I would improve it by incorporating alpha-beta pruning to further optimize performance and make the AI smarter while keeping it fast. Enhanced heuristics could also improve decision-making at lower depths.