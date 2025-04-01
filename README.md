#include <iostream>
using namespace std;

class vect {
    int dim; // Размерность вектора
    double* b; // Массив для хранения элементов вектора
    static int count; // Статический счетчик объектов
    int num; // Номер текущего объекта

public:
    // Конструктор
    vect(int d = 3) : dim(d), num(++count) {
        b = new double[dim]();
        cout << "Vector " << num << " created\n";
    }

    // Деструктор
    ~vect() {
        delete[] b;
        cout << "Vector " << num << " destroyed\n";
    }

    // Копирующий конструктор
    vect(const vect& other) : dim(other.dim), num(++count) {
        b = new double[dim];
        for (int i = 0; i < dim; ++i)
            b[i] = other.b[i];
        cout << "Vector " << num << " copied from Vector " << other.num << "\n";
    }

    // Оператор присваивания
    vect& operator=(const vect& other) {
        if (this != &other) {
            delete[] b;
            dim = other.dim;
            b = new double[dim];
            for (int i = 0; i < dim; ++i)
                b[i] = other.b[i];
            cout << "Vector " << num << " assigned from Vector " << other.num << "\n";
        }
        return *this;
    }

    // Оператор сложения
    vect operator+(const vect& other) const {
        vect temp(dim);
        for (int i = 0; i < dim; ++i)
            temp.b[i] = b[i] + other.b[i];
        cout << "Vector " << num << " + Vector " << other.num << "\n";
        return temp;
    }

    // Оператор вычитания
    vect operator-(const vect& other) const {
        vect temp(dim);
        for (int i = 0; i < dim; ++i)
            temp.b[i] = b[i] - other.b[i];
        cout << "Vector " << num << " - Vector " << other.num << "\n";
        return temp;
    }

    // Оператор умножения на скаляр
    vect operator*(double k) const {
        vect temp(dim);
        for (int i = 0; i < dim; ++i)
            temp.b[i] = k * b[i];
        cout << "Vector " << num << " * " << k << "\n";
        return temp;
    }

    // Оператор скалярного произведения
    double operator*(const vect& other) const {
        double sum = 0;
        for (int i = 0; i < dim; ++i)
            sum += b[i] * other.b[i];
        cout << "Vector " << num << " * Vector " << other.num << "\n";
        return sum;
    }

    // Унарный оператор минус
    vect operator-() const {
        vect temp(dim);
        for (int i = 0; i < dim; ++i)
            temp.b[i] = -b[i];
        cout << "-Vector " << num << "\n";
        return temp;
    }

    // Индексатор для доступа к элементам вектора
    double& operator[](int index) {
        return b[index];
    }

    const double& operator[](int index) const {
        return b[index];
    }

    // Getter for num
    int getNum() const { return num; }
};

int vect::count = 0;

class matr {
    int dim; // Размерность матрицы
    double** a; // Двумерный массив для хранения элементов матрицы
    static int count; // Статический счетчик объектов
    int num; // Номер текущего объекта

public:
    // Конструктор
    matr(int d = 3) : dim(d), num(++count) {
        a = new double*[dim];
        for (int i = 0; i < dim; ++i)
            a[i] = new double[dim]();
        cout << "Matrix " << num << " created\n";
    }

    // Деструктор
    ~matr() {
        for (int i = 0; i < dim; ++i)
            delete[] a[i];
        delete[] a;
        cout << "Matrix " << num << " destroyed\n";
    }

    // Копирующий конструктор
    matr(const matr& other) : dim(other.dim), num(++count) {
        a = new double*[dim];
        for (int i = 0; i < dim; ++i) {
            a[i] = new double[dim];
            for (int j = 0; j < dim; ++j)
                a[i][j] = other.a[i][j];
        }
        cout << "Matrix " << num << " copied from Matrix " << other.num << "\n";
    }

    // Оператор присваивания
    matr& operator=(const matr& other) {
        if (this != &other) {
            for (int i = 0; i < dim; ++i)
                delete[] a[i];
            delete[] a;

            dim = other.dim;
            a = new double*[dim];
            for (int i = 0; i < dim; ++i) {
                a[i] = new double[dim];
                for (int j = 0; j < dim; ++j)
                    a[i][j] = other.a[i][j];
            }
            cout << "Matrix " << num << " assigned from Matrix " << other.num << "\n";
        }
        return *this;
    }

    // Оператор сложения
    matr operator+(const matr& other) const {
        matr temp(dim);
        for (int i = 0; i < dim; ++i)
            for (int j = 0; j < dim; ++j)
                temp.a[i][j] = a[i][j] + other.a[i][j];
        cout << "Matrix " << num << " + Matrix " << other.num << "\n";
        return temp;
    }

    // Оператор вычитания
    matr operator-(const matr& other) const {
        matr temp(dim);
        for (int i = 0; i < dim; ++i)
            for (int j = 0; j < dim; ++j)
                temp.a[i][j] = a[i][j] - other.a[i][j];
        cout << "Matrix " << num << " - Matrix " << other.num << "\n";
        return temp;
    }

    // Оператор умножения на скаляр
    matr operator*(double k) const {
        matr temp(dim);
        for (int i = 0; i < dim; ++i)
            for (int j = 0; j < dim; ++j)
                temp.a[i][j] = k * a[i][j];
        cout << "Matrix " << num << " * " << k << "\n";
        return temp;
    }

    // Оператор матричного умножения
    matr operator*(const matr& other) const {
        matr temp(dim);
        for (int i = 0; i < dim; ++i)
            for (int j = 0; j < dim; ++j)
                for (int k = 0; k < dim; ++k)
                    temp.a[i][j] += a[i][k] * other.a[k][j];
        cout << "Matrix " << num << " * Matrix " << other.num << "\n";
        return temp;
    }

    // Оператор умножения матрицы на вектор
    vect operator*(const vect& v) const {
        vect temp(dim);
        for (int i = 0; i < dim; ++i) {
            double sum = 0;
            for (int j = 0; j < dim; ++j)
                sum += a[i][j] * v[j];
            temp[i] = sum;
        }
        cout << "Matrix " << num << " * Vector " << v.getNum() << "\n";
        return temp;
    }

    // Индексатор для доступа к элементам матрицы
    double* operator[](int index) {
        return a[index];
    }

    const double* operator[](int index) const {
        return a[index];
    }
};

int matr::count = 0;

int main() {
    // Создание векторов
    vect v1(3), v2(3), v3;
    // Заполнение векторов значениями
    v1[0] = 1.0; v1[1] = 2.0; v1[2] = 3.0;
    v2[0] = 4.0; v2[1] = 5.0; v2[2] = 6.0;

    // Операции с векторами
    v3 = v1 + v2; // Сложение векторов
    v3 = v1 - v2; // Вычитание векторов
    double dot_product = v1 * v2; // Скалярное произведение
    vect scaled_v1 = v1 * 2.0; // Умножение вектора на скаляр

    // Создание матриц
    matr m1(3), m2(3), m3;
    // Заполнение матриц значениями
    m1[0][0] = 1.0; m1[0][1] = 2.0; m1[0][2] = 3.0;
    m1[1][0] = 4.0; m1[1][1] = 5.0; m1[1][2] = 6.0;
    m1[2][0] = 7.0; m1[2][1] = 8.0; m1[2][2] = 9.0;

    m2[0][0] = 9.0; m2[0][1] = 8.0; m2[0][2] = 7.0;
    m2[1][0] = 6.0; m2[1][1] = 5.0; m2[1][2] = 4.0;
    m2[2][0] = 3.0; m2[2][1] = 2.0; m2[2][2] = 1.0;

    // Операции с матрицами
    m3 = m1 + m2; // Сложение матриц
    m3 = m1 - m2; // Вычитание матриц
    matr scaled_m1 = m1 * 2.0; // Умножение матрицы на скаляр
    vect result_vector_from_matrix_vector_multiplication = m1 * v1; // Умножение матрицы на вектор

    return 0;
}
