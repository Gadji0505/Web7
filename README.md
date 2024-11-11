🤖️ gpt-4o

Конечно! Вот пример реализации класса «Телефонный справочник» на языке Java. Этот класс будет включать методы для добавления, удаления и поиска записей, а также для сортировки по различным полям.

import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

class PhoneBookEntry {
    private String name;
    private String phone;
    private String email;

    public PhoneBookEntry(String name, String phone, String email) {
        this.name = name;
        this.phone = phone;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public String getPhone() {
        return phone;
    }

    public String getEmail() {
        return email;
    }

    @Override
    public String toString() {
        return "Имя: " + name + ", Телефон: " + phone + ", Email: " + (email != null ? email : "Нет");
    }
}

class PhoneBook {
    private List<PhoneBookEntry> entries;

    public PhoneBook() {
        entries = new ArrayList<>();
    }

    public void addEntry(String name, String phone, String email) {
        PhoneBookEntry entry = new PhoneBookEntry(name, phone, email);
        entries.add(entry);
    }

    public void removeEntry(String name) {
        entries.removeIf(entry -> entry.getName().equalsIgnoreCase(name));
    }

    public PhoneBookEntry findEntry(String name) {
        for (PhoneBookEntry entry : entries) {
            if (entry.getName().equalsIgnoreCase(name)) {
                return entry;
            }
        }
        return null;
    }

    public void sortEntriesByName() {
        entries.sort(Comparator.comparing(PhoneBookEntry::getName));
    }

    public void sortEntriesByPhone() {
        entries.sort(Comparator.comparing(PhoneBookEntry::getPhone));
    }

    public void display() {
        for (PhoneBookEntry entry : entries) {
            System.out.println(entry);
        }
    }
}
public class PhoneBookDemo {
    public static void main(String[] args) {
        PhoneBook phoneBook = new PhoneBook();

        // Добавление записей
        phoneBook.addEntry("Иванов Иван", "123-456", "ivanov@example.com");
        phoneBook.addEntry("Петров Петр", "789-012", "petrov@example.com");
        phoneBook.addEntry("Сидоров Сидор", "345-678", null);

        System.out.println("Телефонный справочник:");
        phoneBook.display();

        // Поиск записи
        System.out.println("\nПоиск записи 'Иванов Иван':");
        PhoneBookEntry entry = phoneBook.findEntry("Иванов Иван");
        if (entry != null) {
            System.out.println(entry);
        } else {
            System.out.println("Запись не найдена.");
        }

        // Удаление записи
        System.out.println("\nУдаление записи 'Петров Петр'.");
        phoneBook.removeEntry("Петров Петр");

        System.out.println("Телефонный справочник после удаления:");
        phoneBook.display();

        // Сортировка записей
        System.out.println("\nСортировка по имени:");
        phoneBook.sortEntriesByName();
        phoneBook.display();

        System.out.println("\nСортировка по телефону:");
        phoneBook.sortEntriesByPhone();
        phoneBook.display();
    }
}


▎Описание кода:

1. PhoneBookEntry: Класс, представляющий одну запись телефонного справочника. Содержит имя, телефон и, при желании, email. Реализует метод toString() для удобного отображения информации.

2. PhoneBook: Класс, представляющий телефонный справочник. Он содержит список записей и методы для работы с ни   - addEntry: добавляет новую запись.

   • removeEntry: удаляет запись по имени.

   • findEntry: ищет запись по имени.

   • sortEntriesByName: сортирует записи по имени.

   • sortEntriesByPhone: сортирует записи по телефону.

   • display: отображает все записи в справочнике.

3. PhoneBookDemo: Класс с методом main, который демонстрирует работу с классом PhoneBook. В нем создается экземпляр PhoneBook, добавляются записи, производится поиск, удаление и сортировка записей, а также выводится текущее состояние справочника.

Этот код можно расширять, добавляя дополнительные функции, такие как сохранение и загрузка записей из файла или работа с пользовательским вводом.