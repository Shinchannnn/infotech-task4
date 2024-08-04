#include <iostream>
#include <vector>

const int SIZE = 9;

// Function to print the Sudoku grid
void printGrid(const std::vector<std::vector<int>>& grid) {
    for (int i = 0; i < SIZE; ++i) {
        for (int j = 0; j < SIZE; ++j) {
            std::cout << grid[i][j] << " ";
        }
        std::cout << std::endl;
    }
}

// Function to check if placing a number in a given position is valid
bool isValid(const std::vector<std::vector<int>>& grid, int row, int col, int num) {
    // Check the row
    for (int i = 0; i < SIZE; ++i) {
        if (grid[row][i] == num) return false;
    }

    // Check the column
    for (int i = 0; i < SIZE; ++i) {
        if (grid[i][col] == num) return false;
    }

    // Check the 3x3 sub-grid
    int startRow = row - row % 3;
    int startCol = col - col % 3;
    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            if (grid[i + startRow][j + startCol] == num) return false;
        }
    }

    return true;
}

// Backtracking function to solve the Sudoku
bool solveSudoku(std::vector<std::vector<int>>& grid) {
    for (int row = 0; row < SIZE; ++row) {
        for (int col = 0; col < SIZE; ++col) {
            if (grid[row][col] == 0) {
                for (int num = 1; num <= SIZE; ++num) {
                    if (isValid(grid, row, col, num)) {
                        grid[row][col] = num;
                        if (solveSudoku(grid)) {
                            return true;
                        }
                        grid[row][col] = 0; // Backtrack
                    }
                }
                return false; // Trigger backtracking
            }
        }
    }
    return true; // Puzzle solved
}

int main() {
    // Example Sudoku puzzle (0 represents empty cells)
    std::vector<std::vector<int>> grid = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };

    std::cout << "Original Sudoku Puzzle:" << std::endl;
    printGrid(grid);

    if (solveSudoku(grid)) {
        std::cout << "\nSolved Sudoku Puzzle:" << std::endl;
        printGrid(grid);
    } else {
        std::cout << "No solution exists for the given Sudoku puzzle." << std::endl;
    }

    return 0;
}
