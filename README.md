Программа на C++ для умножения матрицы на вектор с использованием OpenMP

#include <iostream>
#include <vector>
#include <ctime>
#include <omp.h>

void multiplyMatrixVector(const std::vector<std::vector<int>>& matrix, const std::vector<int>& vector, std::vector<int>& result) {
    #pragma omp parallel for
    for (size_t i = 0; i < matrix.size(); ++i) {
        result[i] = 0;
        for (size_t j = 0; j < matrix[i].size(); ++j) {
            result[i] += matrix[i][j] * vector[j];
        }
    }
}

int main() {
    int n = 1000; // размерность матрицы и вектора
    std::vector<std::vector<int>> matrix(n, std::vector<int>(n, 1)); // создание матрицы (все элементы = 1)
    std::vector<int> vector(n, 2); // создание вектора (все элементы = 2)
    std::vector<int> result(n, 0); // вектор результата

    // Замер времени
    clock_t start = clock();
    multiplyMatrixVector(matrix, vector, result);
    clock_t end = clock();

    double time_taken = double(end - start) / CLOCKS_PER_SEC;
    std::cout << "Время выполнения: " << time_taken << " секунд." << std::endl;

    return 0;
}


Инструкции по запуску программы:
1. Убедитесь, что у вас установлен компилятор C++ с поддержкой OpenMP.
2. Скомпилируйте программу с флагом -fopenmp:
      g++ -fopenmp -o matrix_vector matrix_vector.cpp
   
3. Запустите программу:
      ./matrix_vector
   

Примечание: Вы можете изменять размерность n в программе, чтобы провести тесты для разных значений матрицы и вектора.