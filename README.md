// tictactoe.cpp
#include <iostream>
#include <vector>

void printBoard(const std::vector<char>& b) {
    std::cout << "\n";
    for (int i = 0; i < 9; i++) {
        char c = b[i];
        if (c == ' ') std::cout << (i+1); else std::cout << c;
        if (i % 3 != 2) std::cout << " | ";
        if (i % 3 == 2 && i != 8) std::cout << "\n---------\n";
    }
    std::cout << "\n\n";
}

bool checkWin(const std::vector<char>& b, char p) {
    const int wins[8][3] = {
        {0,1,2},{3,4,5},{6,7,8}, // rows
        {0,3,6},{1,4,7},{2,5,8}, // cols
        {0,4,8},{2,4,6}          // diagonals
    };
    for (auto &w : wins) {
        if (b[w[0]] == p && b[w[1]] == p && b[w[2]] == p) return true;
    }
    return false;
}

bool boardFull(const std::vector<char>& b) {
    for (char c : b) if (c == ' ') return false;
    return true;
}

int main() {
    std::vector<char> board(9, ' ');
    char player = 'X';
    std::cout << "Tic-Tac-Toe (Two players). Players take turns entering 1-9.\n";
    printBoard(board);

    while (true) {
        int move;
        std::cout << "Player " << player << ", enter your move (1-9): ";
        if (!(std::cin >> move)) {
            std::cin.clear();
            std::cin.ignore(10000, '\n');
            std::cout << "Invalid input. Try again.\n";
            continue;
        }
        if (move < 1 || move > 9) {
            std::cout << "Choose a number between 1 and 9.\n";
            continue;
        }
        if (board[move-1] != ' ') {
            std::cout << "Cell taken. Pick another.\n";
            continue;
        }

        board[move-1] = player;
        printBoard(board);

        if (checkWin(board, player)) {
            std::cout << "Player " << player << " wins! 🎉\n";
            break;
        }
        if (boardFull(board)) {
            std::cout << "It's a draw!\n";
            break;
        }
        player = (player == 'X') ? 'O' : 'X';
    }

    std::cout << "Game over. Thanks for playing!\n";
    return 0;
}
