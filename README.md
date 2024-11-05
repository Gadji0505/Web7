#include <iostream>
#include <ctime>
#include <omp.h>

void multiplyMatrixVector(int matrix[][1000], int vector[], int result[], int n) {
    #pragma omp parallel for
    for (int i = 0; i < n; ++i) {
        result[i] = 0;
        for (int j = 0; j < n; ++j) {
            result[i] += matrix[i][j] * vector[j];
        }
    }
}

int main() {
    const int n = 1000; // размерность матрицы и вектора
    int matrix[1000][1000]; // создание матрицы
    int vector[1000]; // создание вектора
    int result[1000]; // вектор результата

    // Инициализация матрицы и вектора
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            matrix[i][j] = 1; // все элементы матрицы = 1
        }
        vector[i] = 2; // все элементы вектора = 2
    }

    // Замер времени
    clock_t start = clock();
    multiplyMatrixVector(matrix, vector, result, n);
    clock_t end = clock();

    double time_taken = double(end - start) / CLOCKS_PER_SEC;
    std::cout << "Время выполнения: " << time_taken << " секунд." << std::endl;

    return 0;
}