
# Tic-Tac-Toe-
Tic Tac Toe is a simple two-player game developed to demonstrate basic programming concepts. Players take turns marking a 3×3 grid with X or O. The game checks for winning conditions, validates moves, and declares a win or draw, helping beginners understand logic and control structures.
Code
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Player class to manage player details.
class Player {
public:
    string name;
    char symbol;

    Player(string playerName, char playerSymbol)
        : name(playerName), symbol(playerSymbol) {}
};

// Board class to represent the Tic-Tac-Toe board.
class Board {
private:
    vector<vector<char>> board;
public:
    Board() : board(4, vector<char>(4, ' ')) {}  // Change size to 4x4

    // Initialize the board with grid display.
    void display() {
        cout << "   1   2   3   4\n";  // Column headers starting from 1
        for (int i = 0; i < 4; ++i) {
            cout << " +---+---+---+---+\n"; // Top border of the grid
            cout << i + 1 << "|";  // Row header starting from 1
            for (int j = 0; j < 4; ++j) {
                cout << " " << board[i][j] << " |";
            }
            cout << '\n';
        }
        cout << " +---+---+---+---+\n"; // Bottom border of the grid
    }

    // Place a move on the board.
    bool placeMove(int row, int col, char symbol) {
        if (row < 1 || row > 4 || col < 1 || col > 4 || board[row - 1][col - 1] != ' ') // Adjust for 1-based indexing
            return false;
        board[row - 1][col - 1] = symbol; // Adjust for 1-based indexing
        return true;
    }

    // Check for win condition.
    bool checkWin(char symbol) {
        for (int i = 0; i < 4; ++i) {
            // Check rows and columns.
            if ((board[i][0] == symbol && board[i][1] == symbol && board[i][2] == symbol && board[i][3] == symbol) ||
                (board[0][i] == symbol && board[1][i] == symbol && board[2][i] == symbol && board[3][i] == symbol))
                return true;
        }
        // Check diagonals.
        return (board[0][0] == symbol && board[1][1] == symbol && board[2][2] == symbol && board[3][3] == symbol) ||
               (board[0][3] == symbol && board[1][2] == symbol && board[2][1] == symbol && board[3][0] == symbol);
    }

    // Check for tie condition.
    bool checkTie() {
        for (const auto& row : board) {
            for (char cell : row) {
                if (cell == ' ')
                    return false;
            }
        }
        return true;
    }

    // Reset the board.
    void reset() {
        for (auto& row : board) {
            fill(row.begin(), row.end(), ' ');
        }
    }
};

// Game class to manage the game flow.
class Game {
private:
    Board board;
    Player player1;
    Player player2;
    Player* currentPlayer;

public:
    Game(string name1, string name2)
        : player1(name1, 'X'), player2(name2, 'O'), currentPlayer(&player1) {}

    // Start the game.
    void start() {
        while (true) {
            board.display();
            int row, col;

            // Get a valid move from the player.
            while (true) {
                cout << currentPlayer->name << "'s turn (" << currentPlayer->symbol << ").\n";
                cout << "Enter row (1-4): ";
                cin >> row;
                cout << "Enter column (1-4): ";
                cin >> col;

                if (cin.fail() || !board.placeMove(row, col, currentPlayer->symbol)) {
                    cin.clear(); // Clear input stream
                    cin.ignore(numeric_limits<streamsize>::max(), '\n'); // Discard invalid input
                    cout << "Invalid move. Try again.\n";
                } else {
                    break; // Valid move
                }
            }

            // Check for win or tie.
            if (board.checkWin(currentPlayer->symbol)) {
                board.display();
                cout << currentPlayer->name << " wins!\n";
                break;
            }
            if (board.checkTie()) {
                board.display();
                cout << "It's a tie!\n";
                break;
            }

            // Switch players.
            currentPlayer = (currentPlayer == &player1) ? &player2 : &player1;
        }

        // Offer to play again.
        char choice;
        cout << "Do you want to play again? (y/n): ";
        cin >> choice;
        if (choice == 'y' || choice == 'Y') {
            board.reset();
            start();
        }
    }
};

int main() {
    string player1Name, player2Name;
    cout << "Enter player 1 name: ";
    getline(cin, player1Name);
    cout << "Enter player 2 name: ";
    getline(cin, player2Name);

    Game game(player1Name, player2Name);
    game.start();

    return 0;
}