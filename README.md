# codealphatask3
A Sudoku solver program in C++ uses a backtracking algorithm to solve Sudoku puzzles. Sudoku is a number puzzle game played on a 9x9 grid, divided into 3x3 subgrids called "regions." The goal is to fill the grid with digits from 1 to 9, such that each row, each column, and each 3x3 subgrid contains all the digits from 1 to 9 without repetition.
#include <iostream>
#include <vector>

using namespace std;

const int N = 9;


void displayGrid(const vector<vector<int>> &grid) {
    for (int i = 0; i < N; ++i) {
        for (int j = 0; j < N; ++j) {
            cout << grid[i][j] << " ";
            if ((j + 1) % 3 == 0 && j != N - 1) {
                cout << "| ";
            }
        }
        cout << endl;
        if ((i + 1) % 3 == 0 && i != N - 1) {
            cout << "------+-------+------" << endl;
        }
    }
}


bool isSafe(const vector<vector<int>> &grid, int row, int col, int num) {
    for (int i = 0; i < N; ++i) {
        if (grid[row][i] == num || grid[i][col] == num) {
            return false;
        }
    }

    int startRow = row - row % 3;
    int startCol = col - col % 3;

    for (int i = startRow; i < startRow + 3; ++i) {
        for (int j = startCol; j < startCol + 3; ++j) {
            if (grid[i][j] == num) {
                return false;
            }
        }
    }

    return true;
}
bool solveSudoku(vector<vector<int>> &grid) {
    int row = -1, col = -1;
    bool isEmpty = false;

    for (int i = 0; i < N && !isEmpty; ++i) {
        for (int j = 0; j < N && !isEmpty; ++j) {
            if (grid[i][j] == 0) {
                row = i;
                col = j;
                isEmpty = true;
            }
        }
    }

    if (!isEmpty) {
        return true;
    }

    for (int num = 1; num <= N; ++num) {
        if (isSafe(grid, row, col, num)) {
            grid[row][col] = num;

            if (solveSudoku(grid)) {
                return true;
            }

            grid[row][col] = 0;
        }
    }

    return false;
}

int main() {
    vector<vector<int>> grid = {
        {4, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {6, 0, 0, 0, 6, 2, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 5},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 8}
    };

    cout << "Sudoku Puzzle:" << endl;
    displayGrid(grid);
    cout << endl;

    if (solveSudoku(grid)) {
        cout << "Sudoku Solution:" << endl;
        displayGrid(grid);
    } else {
        cout << "No solution exists." << endl;
    }

    return 0;
}
