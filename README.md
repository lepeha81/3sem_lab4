# 3sem_lab4
## Homework
### Задание 1
Создайте класс MyMatrix, представляющий матрицу m на n.
Создайте конструктор, принимающий число строк и столбцов, заполняющий матрицу случайными числами в диапазоне, который пользователь вводит при запуске программы.
Определите операторы сложения, вычитания и умножения матриц, а также умножения и деления матрицы на число.
Создайте пользовательский индексатор матрицы для доступа к элементам матрицы по номеру строки и столбца.
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Linq.Expressions;
using System.Runtime.Remoting.Messaging;
using System.Text;
using System.Threading.Tasks;

public class MyMatrix
{
    private int[,] matrix;//двумерный массив
    private int numstroki;
    private int numstolbci;
    private static Random random = new Random();//создаём объект и выделяем память

    public MyMatrix(int stroki, int stolbci, int minRandomValue, int maxRandomValue)
    {
        numstroki = stroki;
        numstolbci = stolbci;
        matrix = new int[numstroki, numstolbci];

        for (int i = 0; i < numstroki; i++)
        {
            for (int j = 0; j < numstolbci; j++)
            {
                matrix[i, j] = random.Next(minRandomValue, maxRandomValue + 1);//диапазон
            }
        }
    }
    public int this[int stroka, int stolbec]
    {
        get
        {
            if (stroka >= 0 && stroka < numstroki && stolbec >= 0 && stolbec < numstolbci)
            {
                return matrix[stroka, stolbec];
            }
            else
            {
                throw new IndexOutOfRangeException("Index vne diapazona");
            }

        }
        set
        {
            if (stroka >= 0 && stroka < numstroki && stolbec >= 0 && stolbec < numstolbci)
            {
                matrix[stroka, stolbec] = value;
            }
            else
            {
                throw new IndexOutOfRangeException("Index vne diapazona");
            }
        }

    }
    public static MyMatrix operator +(MyMatrix matrix1, MyMatrix matrix2)//Возвращает true, если хотя бы один из операндов возвращает true.
    {
        
        
        if (matrix1.numstroki != matrix2.numstroki || matrix1.numstolbci != matrix2.numstolbci)
        {
            Console.WriteLine("Matrici dolchni imet odinakowii razmer");
            MyMatrix result = new MyMatrix(1, 1, 0, 0);

            for (int i = 0; i < 2; i++) for (int j = 0; j < 2; j++) result[i, j] = 0;

            return result;
        }
        else
        {
            MyMatrix result = new MyMatrix(matrix1.numstroki, matrix1.numstolbci, 0, 0);

            for (int i = 0; i < matrix1.numstroki; i++)
            {
                for (int j = 0; j < matrix1.numstolbci; j++)
                {
                    result[i, j] = matrix1[i, j] + matrix2[i, j];
                }
            }

            return result;
        }
    }
    public static MyMatrix operator -(MyMatrix matrix1, MyMatrix matrix2)
    {
        if (matrix1.numstroki != matrix2.numstroki || matrix1.numstolbci != matrix2.numstolbci)
        {
            Console.WriteLine("Matrici dolchni imet odinakowii razmer");
            MyMatrix result = new MyMatrix(1, 1, 0, 0);

            for (int i = 0; i < 2; i++) for (int j = 0; j < 2; j++) result[i, j] = 0;

            return result;
        }
        else
        {
            MyMatrix result = new MyMatrix(matrix1.numstroki, matrix1.numstolbci, 0, 0);

            for (int i = 0; i < matrix1.numstroki; i++)
            {
                for (int j = 0; j < matrix1.numstolbci; j++)
                {
                    result[i, j] = matrix1[i, j] - matrix2[i, j];
                }
            }

            return result;
        }
    }
    public static MyMatrix operator *(MyMatrix matrix1, MyMatrix matrix2)
    {
        if (matrix1.numstolbci != matrix2.numstroki)
        {
            Console.WriteLine("Matrici dolchni imet odinakowii razmer");
            MyMatrix result = new MyMatrix(1, 1, 0, 0);

            for (int i = 0; i < 2; i++) for (int j = 0; j < 2; j++) result[i, j] = 0;

            return result;
        }
        else
        {
            MyMatrix result = new MyMatrix(matrix1.numstroki, matrix1.numstolbci, 0, 0);

            for (int i = 0; i < matrix1.numstroki; i++)
            {
                for (int j = 0; j < matrix1.numstolbci; j++)
                {
                    int sum = 0;
                    for (int k = 0; k < matrix1.numstolbci; k++)
                    {
                        sum += matrix1[i, k] * matrix2[k, j];
                    }
                    result[i, j] = sum;
                }
            }

            return result;
        }
    }
    public static MyMatrix operator /(MyMatrix matrix1, int scalar)
    {
        if (scalar == 0)
        {
            throw new DivideByZeroException("Na 0 delit nelsa");
        }
        else
        {
            MyMatrix result = new MyMatrix(matrix1.numstroki, matrix1.numstolbci, 0, 0);

            for (int i = 0; i < matrix1.numstroki; i++)
            {
                for (int j = 0; j < matrix1.numstolbci; j++)
                {
                    result[i, j] = matrix1[i, j] / scalar;
                }
            }

            return result;
        }
    }
    public static MyMatrix operator *(MyMatrix matrix1, int scalar)
    {
        
            MyMatrix result = new MyMatrix(matrix1.numstroki, matrix1.numstolbci, 0, 0);

            for (int i = 0; i < matrix1.numstroki; i++)
            {
                for (int j = 0; j < matrix1.numstolbci; j++)
                {
                    result[i, j] = matrix1[i, j] * scalar;
                }
            }

            return result;
        
    }
    public void Print()
    {
        for (int i = 0; i < numstroki; i++)
        {
            for (int j = 0; j < numstolbci; j++)
            {
                Console.Write(matrix[i, j] + " ");
            }
            Console.WriteLine();
        }
    }

}
class vosmoi
{
    static void Main()
    {
        Console.WriteLine("Vvedite cilichestvo strok dlya matrix1:");
        int stroki1 = int.Parse(Console.ReadLine());

        Console.WriteLine("Vvedite cilichestvo stolbchov dlya matrix1:");
        int stolci1 = int.Parse(Console.ReadLine());

        Console.WriteLine("Vvedite cilichestvo strok dlya matrix2:");
        int stroki2 = int.Parse(Console.ReadLine());

        Console.WriteLine("Vvedite cilichestvo stolbchov dlya matrix2:");
        int stolci2 = int.Parse(Console.ReadLine());

        Console.WriteLine("minimum random value:");
        int minRandomValue = int.Parse(Console.ReadLine());

        Console.WriteLine("maximum random value:");
        int maxRandomValue = int.Parse(Console.ReadLine());

        MyMatrix matrix1 = new MyMatrix(stroki1, stolci1, minRandomValue, maxRandomValue);
        MyMatrix matrix2 = new MyMatrix(stroki2, stolci2, minRandomValue, maxRandomValue);

        Console.WriteLine("\nMatrix 1:");
        matrix1.Print();

        Console.WriteLine("\nMatrix 2:");
        matrix1.Print();

        Console.WriteLine("\nMatrix 1 + Matrix 2:");
        (matrix1 + matrix2).Print();

        Console.WriteLine("\nMatrix 1 - Matrix 2:");
        (matrix1 - matrix2).Print();

        Console.WriteLine("\nMatrix 1 * Matrix 2:");
        (matrix1 * matrix2).Print();

        Console.WriteLine("\nMatrix 1 * 2:");
        (matrix1 * 2).Print();

        Console.WriteLine("\nMatrix 1 / 2:");
        (matrix1 / 2).Print();
    }
}
```
![image](https://github.com/lepeha81/3sem_lab4/blob/main/1.PNG)
### Задание 2
Создайте класс Car с тремя авто-свойствами: Name, ProductionYear и MaxSpeed, соответствующими названию, году выпуска и максимальной скорости соответственно.
Создайте класс CarComparer, наследуемый от IComparer<Car> и реализуйте метод Compare таким образом, чтобы можно было сортировать массив элементов Car по названию, году выпуска или максимальной скорости по выбору.
Создайте массив элементов Car и продемонстрируйте сортировку различными способами.
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

class Car
{
    private string name;
    private int productionYear;
    private int maxSpeed;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public int ProductionYear
    {
        get { return productionYear; }
        set { productionYear = value; }
    }

    public int MaxSpeed
    {
        get { return maxSpeed; }
        set { maxSpeed = value; }
    }

    public Car(string name, int productionYear, int maxSpeed)
    {
        Name = name;
        ProductionYear = productionYear;
        MaxSpeed = maxSpeed;
    }
}

class CarComparer : IComparer<Car>//// Объявление класса CarComparer, который реализует интерфейс IEquatable<Car>
{
    private string sortBy;

    public CarComparer(string sortBy)
    {
        this.sortBy = sortBy;
    }

    public int Compare(Car x, Car y)
    {
        switch (sortBy)
        {
            case "Name":
                return x.Name.CompareTo(y.Name);
            case "ProductionYear":
                return x.ProductionYear.CompareTo(y.ProductionYear);
            case "MaxSpeed":
                return x.MaxSpeed.CompareTo(y.MaxSpeed);
            default:
                return 0;
        }
    }
}

class devyat
{
    static void Main()
    {
        Car[] cars = new Car[]
        {
            new Car("Porche", 2015, 250),
            new Car("Lada", 2020, 150),
            new Car("YAZ", 1980, 100)
        };

        Console.WriteLine("\nCars:");
        foreach (var car in cars)
        {
            Console.WriteLine($"{car.Name}, {car.ProductionYear}, {car.MaxSpeed}");
        }

        int i = 0; while (i != 1)
        {
            Console.WriteLine("\nViberi tip sortirovki"); string type = Console.ReadLine();
            switch (type)
            {
                case "Name":
                    Array.Sort(cars, new CarComparer("Name"));
                    i++;
                    break;
                case "Year":
                    Array.Sort(cars, new CarComparer("Year"));
                    i++;
                    break;
                case "Speed":
                    Array.Sort(cars, new CarComparer("Speed"));
                    i++;
                    break;
                default:
                    break;
            }
        }

        Console.WriteLine("\nSorted cars:");
        foreach (var car in cars)
        {
            Console.WriteLine($"{car.Name}, {car.ProductionYear}, {car.MaxSpeed}");
        }
    }
}
```
![image](https://github.com/lepeha81/3sem_lab4/blob/main/2.PNG)
### Задание 3
Используйте класс Car из задания №2, на его основе создайте класс CarCatalor, содержащий массив элементов типа Car. 
Для класса CarCatalog реализуйте возможность итерации по элементам массива Car с помощью оператора foreach различными способами: 
Прямой проход с первого элемента до последнего.
Обратный проход от последнего к первому.
Проход по элементам массива с фильтром по году выпуска.
Проход по элементам массива с фильтром по максимальной скорости.

Примечание: для выполнения задания необходимо реализовать различные итераторы, используя конструкцию yield return. Для п.3 и 4, итератор должен принимать год выпуска и скорость как параметр, чтобы возвращать только те элементы коллекции, которые удовлетворяют условию.
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

class Car
{
    private string name;
    private int productionYear;
    private int maxSpeed;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public int ProductionYear
    {
        get { return productionYear; }
        set { productionYear = value; }
    }

    public int MaxSpeed
    {
        get { return maxSpeed; }
        set { maxSpeed = value; }
    }

    public Car(string name, int productionYear, int maxSpeed)
    {
        Name = name;
        ProductionYear = productionYear;
        MaxSpeed = maxSpeed;
    }
}

class CarComparer : IComparer<Car>
{
    private string sortBy;

    public CarComparer(string sortBy)
    {
        this.sortBy = sortBy;
    }

    public int Compare(Car x, Car y)
    {
        switch (sortBy)
        {
            case "Name":
                return x.Name.CompareTo(y.Name);
            case "ProductionYear":
                return x.ProductionYear.CompareTo(y.ProductionYear);
            case "MaxSpeed":
                return x.MaxSpeed.CompareTo(y.MaxSpeed);
            default:
                return 0;
        }
    }
}

class CarCatalog
{
    private List<Car> cars = new List<Car>();

    public void AddCar(Car car)
    {
        cars.Add(car);
    }

    public IEnumerable<Car> ForwardIteration()
    {
        foreach (var car in cars)
        {
            yield return car;
        }
    }

    public IEnumerable<Car> ReverseIteration()
    {
        for (int i = cars.Count - 1; i >= 0; i--)
        {
            yield return cars[i];
        }
    }

    public IEnumerable<Car> YearFilter(int year)
    {
        foreach (var car in cars)
        {
            if (car.ProductionYear == year)
            {
                yield return car;
            }
        }
    }

    public IEnumerable<Car> SpeedFilter(int speed)
    {
        foreach (var car in cars)
        {
            if (car.MaxSpeed == speed)
            {
                yield return car;
            }
        }
    }
}

class decyat
{
    static void Main()
    {
        CarCatalog catalog = new CarCatalog();

        catalog.AddCar(new Car("Porche", 2015, 250));
        catalog.AddCar(new Car("Lada", 2020, 150));
        catalog.AddCar(new Car("YAZ", 1980, 100));

        int i = 0; while (i != 1)
        {
            Console.WriteLine("\nViberi tip sortirovki):"); string type = Console.ReadLine();
            switch (type)
            {
                case "Forward":

                    foreach (var car in catalog.ForwardIteration())
                    {
                        Console.WriteLine(car.Name);
                    }
                    i++;
                    break;
                case "Reverse":

                    foreach (var car in catalog.ReverseIteration())
                    {
                        Console.WriteLine(car.Name);
                    }
                    i++;
                    break;
                case "Year":
                    int a = int.Parse(Console.ReadLine());

                    foreach (var car in catalog.YearFilter(a))
                    {
                        Console.WriteLine(car.Name);
                    }
                    i++;
                    break;
                case "Speed":
                    int b = int.Parse(Console.ReadLine());

                    foreach (var car in catalog.SpeedFilter(b))
                    {
                        Console.WriteLine(car.Name);
                    }
                    i++;
                    break;
                default:
                    break;
            }
        }





    }
}
```
![image](https://github.com/lepeha81/3sem_lab4/blob/main/3.PNG)
