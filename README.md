# Csharp_lab05
## Цели работы:
  1. Научиться реализации интерфейса IEnumerable и работе с массивами и коллекциями языка C#.

## Задание №1
Создайте класс MyMatrix, представляющий матрицу m на n.
Создайте конструктор, принимающий число строк и столбцов, заполняющий матрицу случайными числами в диапазоне, который пользователь вводит при запуске программы.
Создайте метод Fill, перезаполняющий матрицу случайными значениями.
Создайте метод ChangeSize, изменяющий число строк и/или столбцов с копированием значений существующей матрицы. Если новая матрица больше существующий, то метод должен дозаполнить новую матрицу случайными числами.
Создайте метод ShowPartialy, принимающий в качестве параметров начальные и конечные значения строк и столбцов, значения матрицы внутри которых нужно вывести на консоль.
Создайте метод Show, выводящий все значения матрицы на консоль.
Создайте индексатор для матрицы вида this[int index1, int index2] с аксессором и мутатором.

## Задание №2
Создайте класс MyList<T>.
Реализуйте в простейшем приближении возможность использования его экземпляра аналогично экземпляру класса List<T>.
Минимально требуемый интерфейс взаимодействия с экземпляром должен включать метод добавления элемента, индексатор для получения значения элемента по указанному индексу, свойство получения общего количества элементов и поддержку инициализатора коллекции.
При выполнении нельзя использовать коллекции, только массивы.

## Задание №3
Создайте коллекцию MyDictionary<TKey,TValue>.
Реализуйте в простейшем приближении возможность использования ее экземпляра аналогично экземпляру класса Dictionary<TKey,TValue>.
Минимально требуемый интерфейс взаимодействия с экземпляром должен включать метод добавления элемента, индексатор для получения значения элемента по указанному индексу и свойство только для чтения для получения общего количества элементов.
Реализуйте возможность перебора элементов коллекции в цикле foreach. При выполнении нельзя использовать коллекции, только массивы.

## Код задания № 1
    using System;
    
    class MyMatrix
    {
        // Поля для хранения матрицы и размеров
        private int[,] matrix; // Двумерный массив
        private int rows; // Число строк
        private int cols; // Число столбцов
        private Random random = new Random(); // Для генерации случайных чисел
    
        // Конструктор, создающий матрицу с заданными размерами и заполняющий её случайными числами
        public MyMatrix(int rows, int cols, int minValue, int maxValue)
        {
            this.rows = rows; // Устанавливаем число строк
            this.cols = cols; // Устанавливаем число столбцов
            matrix = new int[rows, cols]; // Создаем матрицу
            Fill(minValue, maxValue); // Заполняем матрицу случайными значениями
        }
    
        // Метод для заполнения матрицы случайными значениями
        public void Fill(int minValue, int maxValue)
        {
            for (int i = 0; i < rows; i++) // Цикл по строкам
            {
                for (int j = 0; j < cols; j++) // Цикл по столбцам
                {
                    matrix[i, j] = random.Next(minValue, maxValue + 1); // Присваиваем случайное значение элементу матрицы
                }
            }
        }
    
        // Метод для изменения размера матрицы с копированием существующих значений
        public void ChangeSize(int newRows, int newCols, int minValue, int maxValue)
        {
            int[,] newMatrix = new int[newRows, newCols]; // Создаем новую матрицу с новыми размерами
    
            // Копируем значения из старой матрицы в новую
            for (int i = 0; i < Math.Min(rows, newRows); i++) // Проходим по минимальному числу строк
            {
                for (int j = 0; j < Math.Min(cols, newCols); j++) // Проходим по минимальному числу столбцов
                {
                    newMatrix[i, j] = matrix[i, j]; // Копируем значения
                }
            }
    
            // Заполняем новые элементы матрицы случайными значениями, если размер увеличился
            for (int i = 0; i < newRows; i++) // Проходим по всем строкам новой матрицы
            {
                for (int j = 0; j < newCols; j++) // Проходим по всем столбцам новой матрицы
                {
                    if (i >= rows || j >= cols) // Если мы вышли за пределы старой матрицы
                    {
                        newMatrix[i, j] = random.Next(minValue, maxValue + 1); // Заполняем случайными значениями
                    }
                }
            }
    
            // Обновляем матрицу и её размеры
            matrix = newMatrix;
            rows = newRows;
            cols = newCols;
        }
    
        // Метод для вывода части матрицы, задаваемой диапазоном строк и столбцов
        public void ShowPartially(int startRow, int endRow, int startCol, int endCol)
        {
            for (int i = startRow; i <= endRow && i < rows; i++) // Проходим по заданным строкам
            {
                for (int j = startCol; j <= endCol && j < cols; j++) // Проходим по заданным столбцам
                {
                    Console.Write(matrix[i, j] + "\t"); // Выводим элемент матрицы
                }
                Console.WriteLine(); // Переход на новую строку после каждого ряда
            }
        }
    
        // Метод для вывода всей матрицы
        public void Show()
        {
            for (int i = 0; i < rows; i++) // Проходим по строкам
            {
                for (int j = 0; j < cols; j++) // Проходим по столбцам
                {
                    Console.Write(matrix[i, j] + "\t"); // Выводим элемент матрицы
                }
                Console.WriteLine(); // Переход на новую строку после каждого ряда
            }
        }
    
        // Индексатор для доступа к элементам матрицы по индексу
        public int this[int row, int col]
        {
            get
            {
                return matrix[row, col]; // Возвращаем элемент по указанным индексам строки и столбца
            }
            set
            {
                matrix[row, col] = value; // Устанавливаем новое значение элементу по указанным индексам
            }
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            // Вводим параметры для создания матрицы
            Console.WriteLine("Введите количество строк:");
            int rows = int.Parse(Console.ReadLine());
    
            Console.WriteLine("Введите количество столбцов:");
            int cols = int.Parse(Console.ReadLine());
    
            Console.WriteLine("Введите минимальное значение для элементов матрицы:");
            int minValue = int.Parse(Console.ReadLine());
    
            Console.WriteLine("Введите максимальное значение для элементов матрицы:");
            int maxValue = int.Parse(Console.ReadLine());
    
            // Создаем объект MyMatrix
            MyMatrix matrix = new MyMatrix(rows, cols, minValue, maxValue);
    
            // Выводим начальную матрицу
            Console.WriteLine("\nСгенерированная матрица:");
            matrix.Show();
    
            // Меняем размер матрицы
            Console.WriteLine("\nИзменение размера матрицы...");
            Console.WriteLine("Введите новое количество строк:");
            int newRows = int.Parse(Console.ReadLine());
    
            Console.WriteLine("Введите новое количество столбцов:");
            int newCols = int.Parse(Console.ReadLine());
    
            matrix.ChangeSize(newRows, newCols, minValue, maxValue);
    
            // Выводим измененную матрицу
            Console.WriteLine("\nМатрица после изменения размера:");
            matrix.Show();
    
            // Выводим часть матрицы
            Console.WriteLine("\nЧастичный вывод матрицы:");
            Console.WriteLine("Введите начальную строку (нумерация с 0):");
            int startRow = int.Parse(Console.ReadLine());
    
            Console.WriteLine("Введите конечную строку (нумерация с 0):");
            int endRow = int.Parse(Console.ReadLine());
    
            Console.WriteLine("Введите начальный столбец (нумерация с 0):");
            int startCol = int.Parse(Console.ReadLine());
    
            Console.WriteLine("Введите конечный столбец (нумерация с 0):");
            int endCol = int.Parse(Console.ReadLine());
    
            matrix.ShowPartially(startRow, endRow, startCol, endCol);
    
            // Использование индексатора для изменения элемента
            Console.WriteLine("\nИзменение элемента матрицы с помощью индексатора:");
            Console.WriteLine("Введите номер строки для изменения:");
            int rowToChange = int.Parse(Console.ReadLine());
    
            Console.WriteLine("Введите номер столбца для изменения:");
            int colToChange = int.Parse(Console.ReadLine());
    
            Console.WriteLine("Введите новое значение:");
            int newValue = int.Parse(Console.ReadLine());
    
            matrix[rowToChange, colToChange] = newValue; // Изменяем элемент с помощью индексатора
    
            // Выводим матрицу после изменения элемента
            Console.WriteLine("\nМатрица после изменения элемента:");
            matrix.Show();
        }
    }
## Код задания № 2
    using System.Collections;
    using System.Runtime.InteropServices;
    
    class MyList<T> : IEnumerable<T>  // Класс MyList<T>, реализующий интерфейс IEnumerable<T> для поддержки перечисления элементов
    {
        private T[] values;  // Массив для хранения элементов
        private int capacity;  // Текущий размер массива, равный количеству добавленных элементов
    
        // Конструктор, принимающий переменное количество аргументов (params)
        public MyList(params T[] valuesInit)
        {
            values = new T[valuesInit.Length];  // Инициализация массива размером с переданные значения
            Array.Copy(valuesInit, values, valuesInit.Length);  // Копирование переданных значений в массив
            capacity = valuesInit.Length;  // Установка емкости массива на количество переданных элементов
        }
    
        // Метод для добавления нового элемента в список
        public void Add(T value)
        {
            // Если массив заполнен, увеличиваем его размер
            if (capacity == values.Length)
            {
                int newCapacity = capacity == 0 ? 4 : capacity * 2;  // Увеличиваем размер массива в 2 раза или до 4, если он был пуст
                var newValues = new T[newCapacity];  // Создаем новый массив с увеличенной емкостью
                Array.Copy(values, newValues, values.Length);  // Копируем старые значения в новый массив
                values = newValues;  // Присваиваем ссылку на новый массив
            }
            values[capacity] = value;  // Добавляем новый элемент
            capacity++;  // Увеличиваем текущий размер
        }
    
        // Альтернативный конструктор, принимающий коллекцию элементов
        public MyList(IEnumerable<T> collect)
        {
            values = new T[0];  // Инициализация пустого массива
            foreach (var item in collect)  // Перебираем коллекцию
            {
                Add(item);  // Добавляем каждый элемент в массив
            }
        }
    
        // Индексатор для доступа к элементам по индексу
        public T this[int index]
        {
            get
            {
                return values[index];  // Возвращает элемент по индексу
            }
            set
            {
                values[index] = value;  // Устанавливает элемент по индексу
            }
        }
    
        // Метод для получения количества элементов в списке
        public int Count()
        {
            return capacity;  // Возвращает количество элементов
        }
    
        // Реализация метода GetEnumerator для поддержки foreach
        public IEnumerator<T> GetEnumerator()
        {
            for (int i = 0; i < capacity; i++)  // Перебираем элементы до текущего размера
            {
                yield return values[i];  // Возвращаем текущий элемент
            }
        }
    
        // Реализация неуниверсального метода GetEnumerator для поддержки IEnumerable
        IEnumerator IEnumerable.GetEnumerator()
        {
            return GetEnumerator();  // Возвращаем универсальный перечислитель
        }
    
        // Метод для печати всех элементов списка
        public void Print()
        {
            for (int i = 0; i < capacity; i++)  // Перебираем элементы до текущего размера (capacity)
            {
                Console.Write(values[i] + " ");  // Выводим каждый элемент
            }
            Console.WriteLine();  // Переход на новую строку после вывода
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            // Создание списка с начальными значениями
            MyList<int> list = new MyList<int> { 1, 2, 3, 4, 5, 6 };
            list.Print();  // Печать начальных значений
            list.Add(13);  // Добавление нового элемента
            list.Print();  // Печать после добавления
    
            // Печать общего количества элементов
            Console.WriteLine($"\nTotal number of elements: {list.Count()}");  // Выводим количество элементов
        }
    }
## Код задания № 3
    using System.Collections;  // Подключение пространства имен для поддержки интерфейса IEnumerable.
    using System.Collections.Generic;  // Подключение пространства имен для поддержки обобщенных коллекций, таких как List<> и KeyValuePair<>.
    
    class MyDictionary<TKey, TValue> : IEnumerable<KeyValuePair<TKey, TValue>>
    {
        // Внутренний список, который будет хранить пары ключ-значение.
        private List<KeyValuePair<TKey, TValue>> items = []; // Инициализация списка пар ключ-значение.
    
        // Метод для добавления новой пары ключ-значение в словарь.
        public void Add(TKey key, TValue value)
        {
            items.Add(new KeyValuePair<TKey, TValue>(key, value));  // Добавление объекта KeyValuePair<TKey, TValue> в список.
        }
    
        // Индексатор для доступа к значению по ключу.
        public TValue this[TKey key]
        {
            get
            {
                // Поиск элемента в списке, ключ которого равен переданному ключу.
                var item = items.Find(i => EqualityComparer<TKey>.Default.Equals(i.Key, key));
    
                // Если элемент найден (не равен значению по умолчанию для пары ключ-значение), возвращаем его значение.
                if (!item.Equals(default(KeyValuePair<TKey, TValue>)))
                {
                    return item.Value;  // Возвращаем значение, соответствующее ключу.
                }
                // Если ключ не найден, выбрасываем исключение.
                throw new KeyNotFoundException($"The key '{key}' was not found in the dictionary.");
            }
        }
    
        // Свойство для получения количества элементов в словаре.
        public int Count
        {
            get { return items.Count; }  // Возвращаем количество элементов в списке.
        }
    
        // Реализация метода для поддержки перебора коллекции с использованием foreach.
        public IEnumerator<KeyValuePair<TKey, TValue>> GetEnumerator()
        {
            return items.GetEnumerator();  // Возвращаем перечислитель для перебора элементов списка.
        }
    
        // Неявная реализация интерфейса IEnumerable для поддержки перебора коллекции.
        IEnumerator IEnumerable.GetEnumerator()
        {
            return GetEnumerator();  // Используем основной перечислитель, реализованный выше.
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            // Создание словаря с ключами типа string и значениями типа int, инициализация с использованием инициализатора коллекции.
            MyDictionary<string, int> myDict = new()
            {
                { "one", 1 },   // Добавление пары ключ-значение в словарь.
                { "two", 2 },
                { "three", 3 }
            };
    
            // Вывод количества элементов в словаре.
            Console.WriteLine("Count: " + myDict.Count);
    
            // Получение значения по ключу "two" с помощью индексатора.
            Console.WriteLine("\nValue for key 'two': " + myDict["two"]);
    
            // Перебор всех элементов словаря с использованием foreach.
            Console.WriteLine("\nIterating through the dictionary:");
            foreach (var item in myDict)
            {
                // Вывод ключа и значения каждой пары.
                Console.WriteLine($"{item.Key}: {item.Value}");
            }
        }
    }

C# lab05
