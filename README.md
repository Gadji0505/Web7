#include <iostream>
#include <ctime>
#include <omp.h>
#include <vector>
int main()
{
	setlocale(LC_ALL, "RU");
	long long p = 1;
	//генерация матрицы
	for (int i = 0; i < n; i++)
		for (int j = 0; j < n; j++)
		{
			int s = i + j + 2;
			int k = 0;
			for (int d = 1; d <= s; d++)
				if (s % d == 0)
					k++;
			if (k == 2)
				p *= a[i][j];
		}
}
