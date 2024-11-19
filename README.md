#include <iostream>
#include <vector>
#include <thread>
#include <mutex>
using namespace std;
const int SIZE = 9;
mutex mtx;

// Структура для представления судоку
using Sudoku = vector<vector<int>>;

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
    lock_guard<mutex> guard(mtx);
    if (!foundSolution) {
        foundSolution = solveSudoku(board);
    }
}

int main() {
    setlocale(LC_ALL, "RU");
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
    thread thread1(threadSolve, ref(board), ref(foundSolution));
    thread thread2(threadSolve, ref(board), ref(foundSolution));

    thread1.join();
    thread2.join();

    if (foundSolution) {
        cout << "Решение найдено:\n";
        for (const auto& row : board) {
            for (int num : row) {
                cout << num << " ";
            }
            cout << endl;
        }
    }
    else {
        cout << "Решение не найдено." << endl;
    }

    return 0;
}
