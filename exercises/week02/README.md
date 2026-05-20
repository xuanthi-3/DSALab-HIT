# Tuần 2: Mảng & Con Trỏ — Bài tập

## 🎯 Mục tiêu tuần này
Thành thạo mảng 1D/2D, con trỏ, cấp phát động trong C++.

---

### Bài 1: Mảng cơ bản ⭐
Nhập mảng n phần tử. Tính min, max, trung bình, tổng. Không dùng STL.
#include <iostream>
using namespace std;

void inputArray(int arr[], int n)
{
    for (int i = 0; i < n; i++)
    {
        cout << "arr[" << i << "] = ";
        cin >> arr[i];
    }
}

int sumArray(int arr[], int n)
{
    int sum = 0;

    for (int i = 0; i < n; i++)
    {
        sum += arr[i];
    }

    return sum;
}

int findMin(int arr[], int n)
{
    int minValue = arr[0];

    for (int i = 1; i < n; i++)
    {
        if (arr[i] < minValue)
        {
            minValue = arr[i];
        }
    }

    return minValue;
}

int findMax(int arr[], int n)
{
    int maxValue = arr[0];

    for (int i = 1; i < n; i++)
    {
        if (arr[i] > maxValue)
        {
            maxValue = arr[i];
        }
    }

    return maxValue;
}

double averageArray(int arr[], int n)
{
    int sum = sumArray(arr, n);

    return (double)sum / n;
}

void outputArray(int arr[], int n)
{
    cout << "\nMang: ";

    for (int i = 0; i < n; i++)
    {
        cout << arr[i] << " ";
    }

    cout << endl;
}

int main()
{
    int n;

    cout << "Nhap n = ";
    cin >> n;

    int arr[100];

    inputArray(arr, n);

    outputArray(arr, n);

    cout << "Tong = " << sumArray(arr, n) << endl;
    cout << "Min = " << findMin(arr, n) << endl;
    cout << "Max = " << findMax(arr, n) << endl;
    cout << "Trung binh = " << averageArray(arr, n) << endl;

    return 0;
}
### Bài 2: Mảng 2D ⭐⭐
Nhân 2 ma trận n×n. Tính định thức ma trận 3×3. Hiển thị đẹp.
#include <iostream>
using namespace std;

void inputMatrix(int arr[][100], int n)
{
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            cout << "a[" << i << "][" << j << "] = ";
            cin >> arr[i][j];
        }
    }
}

void outputMatrix(int arr[][100], int n)
{
    cout << "\nMa tran:\n";

    for (int i = 0; i < n; i++)
    {
        cout << "| ";

        for (int j = 0; j < n; j++)
        {
            cout << arr[i][j] << " ";
        }

        cout << "|" << endl;
    }
}

int main()
{
    int n;
    int arr[100][100];

    cout << "Nhap n = ";
    cin >> n;

    inputMatrix(arr, n);

    outputMatrix(arr, n);

    int sumDiagonal = 0;

    for (int i = 0; i < n; i++)
    {
        sumDiagonal += arr[i][i];
    }

    cout << "\nTong duong cheo chinh = "
         << sumDiagonal << endl;

    return 0;
}
### Bài 3: Con trỏ & cấp phát động ⭐⭐
Cài đặt mảng động tự resize (như `std::vector` đơn giản). Hỗ trợ push_back, pop_back, at(i).
#include <iostream>
using namespace std;

struct DynamicArray
{
    int* data;
    int size;
    int capacity;
};

void init(DynamicArray &arr)
{
    arr.capacity = 2;
    arr.size = 0;
    arr.data = new int[arr.capacity];
}

void resize(DynamicArray &arr)
{
    arr.capacity *= 2;

    int* newData = new int[arr.capacity];

    for (int i = 0; i < arr.size; i++)
    {
        newData[i] = arr.data[i];
    }

    delete[] arr.data;

    arr.data = newData;
}

void push_back(DynamicArray &arr, int value)
{
    if (arr.size == arr.capacity)
    {
        resize(arr);
    }

    arr.data[arr.size] = value;
    arr.size++;
}

void pop_back(DynamicArray &arr)
{
    if (arr.size > 0)
    {
        arr.size--;
    }
}

int at(DynamicArray arr, int index)
{
    if (index < 0 || index >= arr.size)
    {
        return -1;
    }

    return arr.data[index];
}

void output(DynamicArray arr)
{
    for (int i = 0; i < arr.size; i++)
    {
        cout << arr.data[i] << " ";
    }

    cout << endl;
}

void destroy(DynamicArray &arr)
{
    delete[] arr.data;
}

int main()
{
    DynamicArray arr;

    init(arr);

    push_back(arr, 10);
    push_back(arr, 20);
    push_back(arr, 30);

    output(arr);

    pop_back(arr);

    output(arr);

    cout << "arr[0] = " << at(arr, 0) << endl;

    destroy(arr);

    return 0;
}
### Bài 4: 🔥 Dự Án Mini — Student Score Manager ⭐⭐⭐
> **Cảm hứng:** BaiTapTongHop — Quản lý sinh viên (DSALab)

Xây dựng hệ thống quản lý điểm sinh viên bằng **mảng động**:
- Thêm / xóa / sửa sinh viên (tên, MSSV, điểm)
- Sắp xếp theo điểm (dùng Selection Sort hoặc Bubble Sort)
- Tìm kiếm theo tên hoặc MSSV (Linear Search)
- Thống kê: điểm cao nhất, thấp nhất, trung bình lớp
- Xuất danh sách ra file `diem_sinhvien.txt`
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

struct Student
{
    string name;
    string id;
    float score;
};

Student students[100];
int countStudent = 0;

void addStudent()
{
    Student s;

    cin.ignore();

    cout << "Nhap ten: ";
    getline(cin, s.name);

    cout << "Nhap MSSV: ";
    getline(cin, s.id);

    cout << "Nhap diem: ";
    cin >> s.score;

    students[countStudent] = s;
    countStudent++;

    cout << "Da them sinh vien!\n";
}

void showStudents()
{
    cout << "\n===== DANH SACH =====\n";

    for (int i = 0; i < countStudent; i++)
    {
        cout << students[i].id << " | "
             << students[i].name << " | "
             << students[i].score << endl;
    }
}

void deleteStudent()
{
    string id;

    cin.ignore();

    cout << "Nhap MSSV can xoa: ";
    getline(cin, id);

    int pos = -1;

    for (int i = 0; i < countStudent; i++)
    {
        if (students[i].id == id)
        {
            pos = i;
            break;
        }
    }

    if (pos == -1)
    {
        cout << "Khong tim thay!\n";
        return;
    }

    for (int i = pos; i < countStudent - 1; i++)
    {
        students[i] = students[i + 1];
    }

    countStudent--;

    cout << "Da xoa!\n";
}

void sortStudents()
{
    for (int i = 0; i < countStudent - 1; i++)
    {
        for (int j = i + 1; j < countStudent; j++)
        {
            if (students[i].score > students[j].score)
            {
                Student temp = students[i];
                students[i] = students[j];
                students[j] = temp;
            }
        }
    }

    cout << "Da sap xep!\n";
}

void searchStudent()
{
    string id;

    cin.ignore();

    cout << "Nhap MSSV can tim: ";
    getline(cin, id);

    for (int i = 0; i < countStudent; i++)
    {
        if (students[i].id == id)
        {
            cout << students[i].id << " | "
                 << students[i].name << " | "
                 << students[i].score << endl;

            return;
        }
    }

    cout << "Khong tim thay!\n";
}

void exportFile()
{
    ofstream file("diem_sinhvien.txt");

    for (int i = 0; i < countStudent; i++)
    {
        file << students[i].id << " | "
             << students[i].name << " | "
             << students[i].score << endl;
    }

    file.close();

    cout << "Da xuat file!\n";
}

int main()
{
    int choice;

    do
    {
        cout << "\n===== QUAN LY SINH VIEN =====\n";
        cout << "1. Them sinh vien\n";
        cout << "2. Xoa sinh vien\n";
        cout << "3. Tim kiem\n";
        cout << "4. Sap xep diem\n";
        cout << "5. Hien thi danh sach\n";
        cout << "6. Xuat file\n";
        cout << "0. Thoat\n";

        cout << "Nhap lua chon: ";
        cin >> choice;

        switch (choice)
        {
            case 1:
                addStudent();
                break;

            case 2:
                deleteStudent();
                break;

            case 3:
                searchStudent();
                break;

            case 4:
                sortStudents();
                break;

            case 5:
                showStudents();
                break;

            case 6:
                exportFile();
                break;

            case 0:
                cout << "Tam biet!\n";
                break;

            default:
                cout << "Lua chon khong hop le!\n";
        }

    } while (choice != 0);

    return 0;
}
```
=== QUẢN LÝ ĐIỂM SINH VIÊN ===
1. Thêm sinh viên
2. Xóa sinh viên
3. Tìm kiếm
4. Xếp hạng lớp
5. Xuất báo cáo
0. Thoát
```

---
📁 Tham khảo: `Chuong1_TongQuan/Chuong1_TongQuan.cpp`
