Программа на C++ для умножения матрицы на вектор с использованием OpenMP

#include <iostream>
#include <ctime>
#include <omp.h>

const int n = 1000; // размерность матрицы и вектора

int main() {
    setlocale(LC_ALL, "RU");

    long long p = 1;
    int a[n][n]; // матрица
    int vector[n]; // вектор
    int result[n]; // вектор результата

    // Генерация матрицы и вектора
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int s = i + j + 2;
            int k = 0;
            for (int d = 1; d <= s; d++)
                if (s % d == 0)
                    k++;
            if (k == 2) // если число простое
                a[i][j] = 1; // например, присваиваем 1 для простых чисел
            else
                a[i][j] = 0; // остальные элементы 0
        }
        vector[i] = i + 1; // инициализация вектора
    }

    // Умножение матрицы на вектор и замер времени
    clock_t start = clock();
    
    #pragma omp parallel for
    for (int i = 0; i < n; i++) {
        result[i] = 0; // обнуляем результат
        for (int j = 0; j < n; j++) {
            result[i] += a[i][j] * vector[j];
        }
    }

    clock_t end = clock();

    double time_taken = double(end - start) / CLOCKS_PER_SEC;
    std::cout << "Время выполнения: " << time_taken << " секунд." << std::endl;

    return 0;
}


Инструкции по запуску программы:
1. Сохраните код в файл, например, matrix_vector.cpp.
2. Убедитесь, что у вас установлен компилятор C++ с поддержкой OpenMP.
3. Скомпилируйте программу с флагом -fopenmp:
      g++ -fopenmp -o matrix_vector matrix_vector.cpp
   
4. Запустите программу:
      ./matrix_vector
   

Примечание: 
- Размер матрицы n установлен в 1000, вы можете изменить его на любое другое значение.
- Код создает матрицу, в которой элементы инициализируются в зависимости от простоты чисел, и вектор инициализируется последовательными числами от 1 до n.