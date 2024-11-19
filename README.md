#include <iostream>
#include <vector>
#include <thread>
#include <mutex>

const int SIZE = 9;
std::mutex mtx;

// Определим структуру для представления судоку
using Sudoku = std::vector<std::vector<int>>;

// Функция для проверки правильности числа в заданной позиции
bool isValid(const Sudoku& board, int row, int col, int num) {
    for (int x = 0; x < SIZE; x++) {
        if (board[row][x] == num || board[x][col] == num || 
            board[row - row % 3 + x / 3][col - col % 3 + x % 3] == num) {
            return false;
        }
    }
    return true;
}

// Функция для решения судоку
bool solveSudoku(Sudoku& board) {
    int row, col;
    bool empty = true;

    // Поиск пустой ячейки
    for (row = 0; row < SIZE; row++) {
        for (col = 0; col < SIZE; col++) {
            if (board[row][col] == 0) {
                empty = false;
                break;
            }
        }
        if (!empty) break;
    }

    if (empty) return true; // Судоку уже решено

    for (int num = 1; num <= 9; num++) {
        if (isValid(board, row, col, num)) {
            board[row][col] = num; // Пробуем данное число

            // Рекурсивный вызов для следующей пустой ячейки
            if (solveSudoku(board)) {
                return true;
            }
            board[row][col] = 0; // Возвращаемся, откат
        }
    }
    return false;
}

// Функция для работы одного потока
void threadSolve(Sudoku& board, bool& foundSolution) {
    std::lock_guard<std::mutex> guard(mtx);
    if (!foundSolution) {
        foundSolution = solveSudoku(board);
    }
}

int main() {
    Sudoku board = {
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

    bool foundSolution = false;
    
    // Создаем два потока для решения судоку
    std::thread thread1(threadSolve, std::ref(board), std::ref(foundSolution));
    std::thread thread2(threadSolve, std::ref(board), std::ref(foundSolution));
    
    thread1.join();
    thread2.join();

    if (foundSolution) {
        std::cout << "Решение найдено:\n";
        for (const auto& row : board) {
            for (int num : row) {
                std::cout << num << " ";
            }
            std::cout << std::endl;
        }
    } else {
        std::cout << "Решение не найдено." << std::endl;
    }

    return 0;
}