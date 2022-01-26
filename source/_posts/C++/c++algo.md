---
title: C++algorithm
date: 2022-01-26 14:43:17
tags:
- C++algorithm
categories:
- [C++]
---



##  排序算法

###  工具函数

```c++
#pragma once
#include <iostream>
#include <cassert>
#include <ctime>
using namespace std;

namespace SortTestHelper {
	
	//生成n个随机数，每个元素的范围：[l,r]
	int* generateRandomArray(int n, int l, int r) {
		assert(l < r);//满足条件才继续执行  要引入<cassert>

		int* arr = new int[n];//注意在堆区生成，需要delete[]

		srand(time(NULL));//生成随机数的种子， 需要引入<ctime>
		for (int i = 0; i < n; i++) {
			arr[i] = rand() % (r - l + 1) + l;//rand生成的是随机整数
		}
		return arr;
	}

	//打印数组的所有元素,n是数组长度
	template<typename T>
	void printArray(T arr[], int n) {
		for (int i = 0; i < n; i++) {
			cout << arr[i] << " ";
		}
		cout << endl;
		return;
	}

	//检查排序是否正确
	template<typename T>
	bool isSorted(T arr[], int n) {
		for (int i = 0; i < n - 1; i++) {
			if (arr[i] > arr[i + 1])
				return false;
		}
		return true;
	}

	//通用函数执行时间计算函数
	template<typename T>           //sort是函数指针
	void testSort(string sortName, void(*sort)(T[], int), T arr[], int n) {
		clock_t startTime = clock(); //需要引入<ctime>
		sort(arr, n);//调用传入的函数
		clock_t endTime = clock();


		//判断函数执行排序是否正确
		assert(isSorted(arr, n));
		                            //计算时钟周期个数            //宏定义 1秒有几个时钟周期
		cout << sortName << " : " << double(endTime - startTime) / CLOCKS_PER_SEC << " s" << endl;

		return;

	}
}
```



###  选择排序

> 时间复杂度：O(n*n)

从数组第一个元素开始，在其后选择一个最小的元素与之交换，然后后移一格，再在其后选择一个最小的元素与之交换，以此类推

```c++
#include <iostream>
#include "Student.h"
#include "SortTestHelper.h"

using namespace std;
template<typename T>
void selectSort(T arr[], int n) {
	for (int i = 0; i < n; i++) {
		int minIndex = i;
		for (int j = i + 1; j < n; j++) {
			if (arr[j] < arr[minIndex]) {
				minIndex = j;
				
			}
		}
		swap(arr[i], arr[minIndex]);
	}
}

int main() {
	int a[10] = { 10,9,8,7,6,5,4,3,2,1 };
	selectSort(a, 10);
	for (int i = 0; i < 10; i++) {
		cout << a[i] ;
	}
	cout << endl;

	string c[3] = { "C","B", "A" };
	selectSort(c, 3);
	for (int i = 0; i < 3; i++) {
		cout << c[i];
	}
	cout << endl;

	Student s[4] = { {"D", 100},{"C",95},{"B", 95},{"A", 91} };
	selectSort(s, 4);
	for (int i = 0; i < 4; i++) {
		cout << s[i];
	}
	cout << endl;

	int n = 10000;
	int* arr = SortTestHelper::generateRandomArray(n, 0, n);
	selectSort(arr, n);
	SortTestHelper::printArray(arr, n);
	delete[] arr; //注意删除堆区的临时变量
	return 0;
}
```

```c++
//Student.h
#include <iostream>

using namespace std;

struct Student {
	string name;
	int score;

	bool operator<(const Student& otherStudent) {
		return score != otherStudent.score ? score < otherStudent.score : name < otherStudent.name;
	}

	friend ostream& operator<<(ostream& os, const Student& student) {
		os << "Student: " << student.name << " " << student.score << endl;
		return os;
	}

};
```

###  插入排序

> * 时间复杂度：O（n*n）
> * 相对选择排序的优势： 内层循环可以提前终止
> * 相对选择排序优势更明显的情况： 数组的有序性本身很高，这样内层循环就会更早提前终止
> * 对于近乎有序的排序情况，插入排序比O(n*logn)的排序算法还要好
> * 数组本身有序时，插入排序时间复杂度：O(N)

从第二个元素开始，依次往后移动，每次将当前元素插入到前面已经排序好的序列中正确的位置，即比自己大的就往后移动

```c++
#include <iostream>
#include "Student.h"
#include "SortTestHelper.h"

using namespace std;

//选择排序
template<typename T>
void selectSort(T arr[], int n) {
	for (int i = 0; i < n; i++) {
		int minIndex = i;
		for (int j = i + 1; j < n; j++) {
			if (arr[j] < arr[minIndex]) {
				minIndex = j;
				
			}
		}
		swap(arr[i], arr[minIndex]);
	}
}

//插入排序
template<typename T>
void insertSort(T arr[], int n) {
	for (int i = 1; i < n; i++) {
		T temp = arr[i];
		int j;//在循环之外还要使用
		for ( j = i; j > 0 && arr[j-1]>temp ; j--) {//arr[j-1]>temp确保提前终止
			arr[j] = arr[j - 1];
		}
		arr[j] = temp;
	}
}

int main() {

	int n = 10000;
	/*int* arr = SortTestHelper::generateRandomArray(n, 0, n);
	int* arr2 = SortTestHelper::copyIntArray(arr, n);*/
	int* arr = SortTestHelper::generateNearlyOrderedArray(n, 10);//生成近乎有序的数组
	int* arr2 = SortTestHelper::copyIntArray(arr, n);
	/*selectSort(arr, n);
	SortTestHelper::printArray(arr, n);*/
	SortTestHelper::testSort("selectSort", selectSort, arr, n);
	SortTestHelper::testSort("insertSort", insertSort, arr2, n);
	delete[] arr; //注意删除堆区的临时变量
	delete[] arr2;
	return 0;
}
```

