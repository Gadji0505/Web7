import java.util.Scanner;

class DrobnoeChislo {
    private int celayaChast; // Целая часть
    private double drobnayaChast; // Дробная часть (в диапазоне от 0 до 1)

    // Конструктор
    public DrobnoeChislo(int celayaChast, double drobnayaChast) {
        this.celayaChast = celayaChast;
        this.drobnayaChast = drobnayaChast;
    }

    // Метод сложения
    public DrobnoeChislo add(DrobnoeChislo other) {
        int newCelaya = this.celayaChast + other.celayaChast;
        double newDrob = this.drobnayaChast + other.drobnayaChast;

        if (newDrob >= 1) {
            newCelaya += 1;
            newDrob -= 1;
        }

        return new DrobnoeChislo(newCelaya, newDrob);
    }

    // Метод вычитания
    public DrobnoeChislo subtract(DrobnoeChislo other) {
        int newCelaya = this.celayaChast - other.celayaChast;
        double newDrob = this.drobnayaChast - other.drobnayaChast;

        if (newDrob < 0) {
            newCelaya -= 1;
            newDrob += 1;
        }

        return new DrobnoeChislo(newCelaya, newDrob);
    }

    // Метод умножения
    public DrobnoeChislo multiply(DrobnoeChislo other) {
        double totalSelf = this.celayaChast + this.drobnayaChast;
        double totalOther = other.celayaChast + other.drobnayaChast;
        double result = totalSelf * totalOther;

        return new DrobnoeChislo((int) result, result % 1);
    }

    // Метод для сравнения
    public boolean isLessThan(DrobnoeChislo other) {
        return (this.celayaChast + this.drobnayaChast) < (other.celayaChast + other.drobnayaChast);
    }

    @Override
    public String toString() {
        return celayaChast + (drobnayaChast > 0 ? " + " + drobnayaChast : "");
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Введите количество дробных чисел: ");
        int n = scanner.nextInt();
        DrobnoeChislo[] drobnyeChisla = new DrobnoeChislo[n];

        // Ввод данных от пользователя
        for (int i = 0; i < n; i++) {
            System.out.print("Введите целую часть " + (i + 1) + ": ");
            int celayaChast = scanner.nextInt();
            System.out.print("Введите дробную часть " + (i + 1) + " (например, 0.5): ");
            double drobnayaChast = scanner.nextDouble();
            drobnyeChisla[i] = new DrobnoeChislo(celayaChast, drobnayaChast);
        }

        // Сортировка массива по возрастанию, используя пузырьковую сортировку
        for (int i = 0; i < drobnyeChisla.length - 1; i++) {
            for (int j = 0; j < drobnyeChisla.length - 1 - i; j++) {
                if (drobnyeChisla[j].isLessThan(drobnyeChisla[j + 1])) {
                    // Обмен местами
                    DrobnoeChislo temp = drobnyeChisla[j];
                    drobnyeChisla[j] = drobnyeChisla[j + 1];
                    drobnyeChisla[j + 1] = temp;
                }
            }
        }

        // Вывод отсортированного массива
        System.out.println("Отсортированные дробные числа:");
        for (DrobnoeChislo chislo : drobnyeChisla) {
            System.out.println(chislo);
        }

        scanner.close();
    }
}