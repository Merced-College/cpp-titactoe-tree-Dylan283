# cpp_tictactoe_tree

// The Minimax function is used to determine the best possible move for the computer in the Tic-Tac-Toe game. It follows a recursive approach to evaluate all possible future states and assign scores based on whether the computer wins (+1), the human wins (-1), or the game ends in a draw (0). The function strategically minimizes the opponent’s best move while maximizing the computer’s chances of winning.

// The GameState class acts as a node in the game tree. Each possible board state represents a unique node, and the getAvailableMoves method provides the child nodes (next possible states). Whenever a move is made, a new GameState object is created, representing the next level of the tree. The tree branches out until a terminal state is reached (win, loss, or draw).

//Recursion in the minimax function stops under the following conditions:

A player has won – If the checkWinner function detects a win for either HUMAN or COMPUTER, the function returns the corresponding score.

The board is full – If there are no empty cells left, meaning no further moves can be made, the function returns a score of 0 (draw).

End of available moves – If there are no more moves to explore in a given path, the function stops calculating further states.

#include <iostream>
#include <vector>
#include <limits>
#include <cstdlib>
#include <ctime>

using namespace std;

const char HUMAN = 'X';
const char COMPUTER = 'O';
const char EMPTY = ' ';

class GameState {
private:
    vector<char> board; // 9 cells for Tic-Tac-Toe

public:
    GameState() : board(9, EMPTY) {}

    GameState(const vector<char>& b) : board(b) {}

    void printBoard() const {
        for (int i = 0; i < 9; ++i) {
            cout << (board[i] == EMPTY ? '-' : board[i]) << " ";
            if ((i + 1) % 3 == 0) cout << endl;
        }
    }

    bool isFull() const {
        for (char c : board)
            if (c == EMPTY) return false;
        return true;
    }

    bool isGameOver() const {
        return checkWinner() != EMPTY || isFull();
    }

    char checkWinner() const {
        const int winPatterns[8][3] = {
            {0,1,2}, {3,4,5}, {6,7,8},
            {0,3,6}, {1,4,7}, {2,5,8},
            {0,4,8}, {2,4,6}
        };

        for (auto& pattern : winPatterns) {
            if (board[pattern[0]] != EMPTY &&
                board[pattern[0]] == board[pattern[1]] &&
                board[pattern[1]] == board[pattern[2]]) {
                return board[pattern[0]];
            }
        }
        return EMPTY;
    }

    vector<int> getAvailableMoves() const {
        vector<int> moves;
        for (int i = 0; i < 9; ++i)
            if (board[i] == EMPTY) moves.push_back(i);
        return moves;
    }

    GameState makeMove(int index, char player) const {
        vector<char> newBoard = board;
        newBoard[index] = player;
        return GameState(newBoard);
    }

    const vector<char>& getBoard() const {
        return board;
    }
};

class TicTacToeTree {
public:
    int minimax(const GameState& state, bool isMaximizing) {
        char winner = state.checkWinner();
        if (winner == COMPUTER) return 1;
        if (winner == HUMAN) return -1;
        if (state.isFull()) return 0;
        if (depth >= 5) return 0;


        if (isMaximizing) {
            int bestScore = numeric_limits<int>::min();
            for (int move : state.getAvailableMoves()) {
                GameState newState = state.makeMove(move, COMPUTER);
                int score = minimax(newState, false);
                bestScore = max(bestScore, score);
            }
            return bestScore;
        } else {
            int bestScore = numeric_limits<int>::max();
            for (int move : state.getAvailableMoves()) {
                GameState newState = state.makeMove(move, HUMAN);
                int score = minimax(newState, true);
                bestScore = min(bestScore, score);
            }
            return bestScore;
        }
    }

    int findBestMove(const GameState& state) {
        int bestScore = numeric_limits<int>::min();
        int bestMove = -1;

        for (int move : state.getAvailableMoves()) {
            GameState newState = state.makeMove(move, COMPUTER);
            int score = minimax(newState, false);
            if (score > bestScore) {
                bestScore = score;
                bestMove = move;
            }
        }
        return bestMove;
    }
};

void playGame() {
    GameState state;
    TicTacToeTree ai;
    char currentPlayer = HUMAN;

    while (!state.isGameOver()) {
        state.printBoard();
        if (currentPlayer == HUMAN) {
            int move;
            cout << "Enter your move (0-8): ";
            cin >> move;
            if (move >= 0 && move < 9 && state.getBoard()[move] == EMPTY) {
                state = state.makeMove(move, HUMAN);
                currentPlayer = COMPUTER;
            } else {
                cout << "Invalid move. Try again.\n";
            }
        } else {
            int move = ai.findBestMove(state);
            state = state.makeMove(move, COMPUTER);
            cout << "Computer plays at position " << move << endl;
            currentPlayer = HUMAN;
        }
    }

    state.printBoard();
    char winner = state.checkWinner();
    if (winner == COMPUTER) cout << "Computer wins!\n";
    else if (winner == HUMAN) cout << "You win!\n";
    else cout << "It's a draw!\n";
}

int main() {
    playGame();
    return 0;
}
