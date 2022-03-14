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

###  归并排序

> * 时间复杂度：O(n*logn)
> * 自顶向下 
> * 自底向上 适合链表的归并，因为对数组的索引操作比较少
> * 优化：
>   * 1： 拆分的最小单元用插入排序进行排序
>   * 2： 已经有序的数组则不需要进行合并操作

```c++
#include <iostream>
using namespace std;

//将arr[l,mid]和arr[mid+1, r]两部分进行排序
template<typename T>
void __merge(T arr[], int l, int mid, int r) {
	T* aux = new int[r - l + 1];

	for (int i = l; i <= r; i++) {
		aux[i - l] = arr[i];
	}

	int i = l;
	int j = mid + 1;
	for (int k = l; k <= r; k++) {
		if (i > mid) {  // 如果左半部分元素已经全部处理完毕
			arr[k] = aux[j - l]; j++;
		}
		else if (j > r) {  // 如果右半部分元素已经全部处理完毕
			arr[k] = aux[i - l]; i++;
		}
		else if (aux[i - l] < aux[j - l]) {  // 左半部分所指元素 < 右半部分所指元素
			arr[k] = aux[i - l]; i++;
		}
		else {  // 左半部分所指元素 >= 右半部分所指元素
			arr[k] = aux[j - l]; j++;
		}
	}

	delete[] aux;
}


//递归使用归并排序，对arr[l....r]范围进行排序
template<typename T>
void __mergeSort(T arr[], int l, int r) {

	//递归结束条件
	/*if (l >= r) {
		return;
	}*/

	// 优化2: 对于小规模数组, 使用插入排序
	if (r - l <= 15) {
		insertSort(arr, l, r);
		return;
	}

	int mid = (l + r) / 2;
	__mergeSort(arr, l, mid);
	__mergeSort(arr, mid + 1, r);

	// 优化1: 对于arr[mid] <= arr[mid+1]的情况,不进行merge
	// 对于近乎有序的数组非常有效,但是对于一般情况,有一定的性能损失(增加了条件判断)
	if (arr[mid] > arr[mid + 1])
		__merge(arr, l, mid, r);
}


//自顶向下
template<typename T>
void mergeSort(T arr[], int n) {
	__mergeSort(arr, 0, n - 1);
}


//自底向上
template<typename T>
void mergeSortBU(T arr[], int n) {

	//for (int sz = 1; sz < n; sz += sz ) {//sz: 1 2 4 8 16
	//	for (int i = 0; i+sz < n; i += sz + sz) { //i+sz < n: 第二部分存在才需要合并
	//		__merge(arr, i, i + sz - 1, min(i+sz+sz-1,n-1));//注意越界问题
	//	}
	//}

	//对于小数组使用插入排序
	for (int i = 0; i < n; i += 16) {
		insertSort(arr, i, min(i + 15, n - 1));
	}

	
	for (int sz = 16; sz < n; sz += sz) {//sz: 1 2 4 8 16
		for (int i = 0; i + sz < n; i += sz + sz) { //i+sz < n: 第二部分存在才需要合并
			// 对于arr[mid] <= arr[mid+1]的情况,不进行merge
			if(arr[i+sz-1]>arr[i+sz])
			  __merge(arr, i, i + sz - 1, min(i + sz + sz - 1, n - 1));//注意越界问题
		}
	}
}

```

###  快速排序

> * 时间复杂度：O(n*logn)

#### 快排优化版本

* 优化1：最小单元的排序使用插入排序
* 优化2：分割位置的元素随机选择, (不随机的情况下，近乎有序数组的排序时间复杂度就会退化到O(n*n))

![image-20220219221043450](https://gitee.com/ChuckieWill/picture/raw/master/img/202202192210160.png)

```c++
#include <iostream>
#include <ctime>

using namespace std;

// 对arr[l...r]部分进行partition操作
// 返回p, 使得arr[l...p-1] < arr[p] ; arr[p+1...r] > arr[p]
template<typename T>
int __partition(T arr[], int l, int r) {

	//优化2： 随机在arr[l...r]的范围中, 选择一个数值作为标定点pivot， 避免有序数组拆分时形成链式树（深度退化为n）,时间复杂度也会退化为n*n
	swap(arr[l], arr[rand() % (r - l + 1) + l]);
	T v = arr[l];
	int j = l; // arr[l+1...j] < v ; arr[j+1...i) > v
	for (int i = l + 1; i <= r; i++) {
		if (arr[i] < v) {
			swap(arr[j + 1], arr[i]);
			j++;
		}
	}
	swap(arr[l], arr[j]);
	return j;
	
}

// 对arr[l...r]部分进行快速排序
template<typename T>
void __quickSort(T arr[], int l, int r) {
	/*if (l >= r) {
		return;
	}*/

	//优化1： 最小单元用插入排序 
	if (r - l <= 15) {
		insertSort(arr, l, r);
		return;
	}

	int p = __partition(arr, l, r);
	__quickSort(arr, l, p - 1);
	__quickSort(arr, p + 1, r);
}



template<typename T>
void quickSort(T arr[], int n) {
	srand(time(NULL));
	__quickSort(arr, 0, n - 1);
}
```

####  两路快排

* 用于解决重复元素很多的情况
  * 重复元素很多的情况下，划分的结果也会很不平衡，导致树的深度接近n,时间复杂度就会退化到O(n*n))

![image-20220219221627897](https://gitee.com/ChuckieWill/picture/raw/master/img/202202192216456.png)

* 二路快排示意图

![image-20220219225308292](https://gitee.com/ChuckieWill/picture/raw/master/img/202202192253159.png)

* 排序完成后，两边都包含了等于v的部分，v正好放在中间，两边划分就比较平衡

![image-20220219225757590](https://gitee.com/ChuckieWill/picture/raw/master/img/202202192257525.png)

```c++
//二路快排
#include <iostream>

using namespace std;

// 对arr[l...r]部分进行partition操作
// 返回p, 使得arr[l...p-1] < arr[p] ; arr[p+1...r] > arr[p]
template<typename T>
int __partition2(T arr[], int l, int r) {

	//优化2： 随机在arr[l...r]的范围中, 选择一个数值作为标定点pivot， 避免有序数组拆分时形成链式树（深度退化为n）,时间复杂度也会退化为n*n
	swap(arr[l], arr[rand() % (r - l + 1) + l]);
	T v = arr[l];
	int i = l + 1;//移动标志位
	int j = r; // arr[l+1...j) < v ; arr(j...r] > v
	while (true) {
		while(i <= r && arr[i] < v) {
			i++;
		}
		while(j>= l+1 && arr[j] > v) {
			j--;
		}
		if (i> j)
			break;

		swap(arr[i], arr[j]);
		i++;
		j--;
	}
	swap(arr[l], arr[j]);
	return j;

}

// 对arr[l...r]部分进行快速排序
template<typename T>
void __quickSort2(T arr[], int l, int r) {
	/*if (l >= r) {
		return;
	}*/

	//优化1： 最小单元用插入排序 
	if (r - l <= 15) {
		insertSort(arr, l, r);
		return;
	}

	int p = __partition2(arr, l, r);
	__quickSort2(arr, l, p - 1);
	__quickSort2(arr, p + 1, r);
}



template<typename T>
void quickSort2(T arr[], int n) {
	srand(time(NULL));
	__quickSort2(arr, 0, n - 1);
}

```

####  三路快排

* 三路快排示意图
  * 中间等于v的部分不再参与进一步划分的排序

![image-20220219225917118](https://gitee.com/ChuckieWill/picture/raw/master/img/202202192259784.png)

```c++
#pragma once
//三路快排
#include <iostream>

using namespace std;



// 对arr[l...r]部分进行快速排序
template<typename T>
void __quickSort3(T arr[], int l, int r) {
	/*if (l >= r) {
		return;
	}*/

	//优化1： 最小单元用插入排序 
	if (r - l <= 15) {
		insertSort(arr, l, r);
		return;
	}

	//优化2： 随机在arr[l...r]的范围中, 选择一个数值作为标定点pivot， 避免有序数组拆分时形成链式树（深度退化为n）,时间复杂度也会退化为n*n
	swap(arr[l], arr[rand() % (r - l + 1) + l]);
	T v = arr[l];
	int lt = l;     // arr[l+1...lt] < v
	int gt = r + 1; // arr[gt...r] > v
	int i = l + 1;    // arr[lt+1...i) == v
	while (i < gt) {
		if (arr[i] < v) {
			swap(arr[i], arr[lt + 1]);
			i++;
			lt++;
		}
		else if (arr[i] > v)
		{
			swap(arr[gt - 1], arr[i]);
			gt--;
		}
		else
		{
			i++;
		}
	}
	swap(arr[l], arr[lt]);

	__quickSort3(arr, l, lt - 1);  //[lt....gt-1]的部分不用再参与排序，因为都等于v
	__quickSort3(arr, gt, r);
}



template<typename T>
void quickSort3(T arr[], int n) {
	srand(time(NULL));
	__quickSort3(arr, 0, n - 1);
}

```


###  堆排序

####  构建堆

![image-20220220215808534](https://gitee.com/ChuckieWill/picture/raw/master/img/202202202158199.png)

* 通常用数组来表示堆，以**广度优先遍历**顺序存储到数组中
* 左侧子节点的位置(数组中的索引值)：`2*index+1`
* 右侧子节点的位置(数组中的索引值)：`2*index+2`
* 父节点的位置： `floor((index-1)/2)` 向下取整

####  堆排序

> 将n个元素逐个插入到一个空堆中，时间复杂度是O(nlogn)
> heapify的过程，时间复杂度为O(n)

```c++
#include <iostream>
#include <cassert>

using namespace std;


template<typename Item>
class MaxHeap {

private:
	Item* data;
	int count;
	int capacity;

	void shiftUp(int k) {
		while (k > 0 && data[k / 2] < data[k]) {
			swap(data[k / 2], data[k]);
			k /= 2;
		}
	}

	void shiftDown(int k) {
		while (2 * k <= count-1) {
			int j = 2 * k;
			if (j + 1 <= count-1 && data[j + 1] > data[j]) j++;
			if (data[k] >= data[j]) break;
			swap(data[k], data[j]);
			k = j;
		}
	}

public:

	// 构造函数, 构造一个空堆, 可容纳capacity个元素
	MaxHeap(int capacity) {
		data = new Item[capacity ];
		count = 0;
		this->capacity = capacity;
	}

	// 构造函数, 通过一个给定数组创建一个最大堆
	// 该构造堆的过程, 时间复杂度为O(n)
	MaxHeap(Item arr[], int n) {
		data = new Item[n];
		capacity = n;

		for (int i = 0; i < n; i++)
			data[i] = arr[i];
		count = n;

		for (int i = count-1 / 2; i >= 0; i--)//不是插入，而是就地构建堆，所以时间复杂度比插入小
			shiftDown(i);
	}

	~MaxHeap() {
		delete[] data;
	}

	// 返回堆中的元素个数
	int size() {
		return count;
	}

	// 返回一个布尔值, 表示堆中是否为空
	bool isEmpty() {
		return count == 0;
	}

	// 像最大堆中插入一个新的元素 item
	void insert(Item item) {
		assert(count  < capacity);
		data[count] = item;
		shiftUp(count);
		count++;
	}

	// 从最大堆中取出堆顶元素, 即堆中所存储的最大数据
	Item extractMax() {
		assert(count > 0);
		Item ret = data[0];
		swap(data[0], data[count-1]);
		count--;
		shiftDown(0);
		return ret;
	}

	// 获取最大堆中的堆顶元素
	Item getMax() {
		assert(count > 0);
		return data[0];
	}
};


// heapSort1, 将所有的元素依次添加到堆中, 在将所有元素从堆中依次取出来, 即完成了排序
// 无论是创建堆的过程, 还是从堆中依次取出元素的过程, 时间复杂度均为O(nlogn)
// 整个堆排序的整体时间复杂度为O(nlogn)
template<typename T>
void heapSort1(T arr[], int n) {
	MaxHeap<T> maxheap = MaxHeap<T>(n);
	for (int i = 0; i < n; i++) {
		maxheap.insert(arr[i]);
	}
	for (int i = n - 1; i >= 0; i--) {
		arr[i] = maxheap.extractMax();
	}

}

// heapSort2, 借助我们的heapify过程创建堆
// 此时, 创建堆的过程时间复杂度为O(n), 将所有元素依次从堆中取出来, 时间复杂度为O(nlogn)
// 堆排序的总体时间复杂度依然是O(nlogn), 但是比上述heapSort1性能更优, 因为创建堆的性能更优
template<typename T>
void heapSort2(T arr[], int n) {
	MaxHeap<T> maxheap = MaxHeap<T>(arr, n);
	for (int i = n - 1; i >= 0; i--) { 
		arr[i] = maxheap.extractMax();
	}
}
```



####  索引堆

> 引入索引堆的原因
>
> * 不是int型的数据，交换位置很耗费性能，例如string类型
> * 堆排序后，查找元素不好索引，只能遍历数组查找，时间复杂度上升为O(n)
>
> 索引堆的实现
>
> * 堆排序的所有操作都用int型的索引（index）来实现，数据本身(data)位置不变

![image-20220220210719390](https://gitee.com/ChuckieWill/picture/raw/master/img/202202202107515.png)

* reverse的引入

  * 引入reverse的意义： 当data中的数据修改后，就需要重新调整堆，那么就需要知道修改的这元素在index中是在什么位置，以便调用shiftUp()或shiftDown()来调整堆

  * 例如修改了data[4] = 26, 那么就需要知道data[4]的索引`4`在index中存储的位置，而reverse就保存了这个位置，即reverse[4]=9,

    data[index[9]] = 26,  所以拿到索引`9`后执行`shiftUp(9)或shiftDown(9)`即可完成堆的调整

    

![image-20220221214847006](https://gitee.com/ChuckieWill/picture/raw/master/img/202202212148699.png)

```c++
#include <iostream>
#include <cassert>

using namespace std;


template<typename Item>
class MaxHeap {

private:
	Item* data;
	int* index;
	int count;
	int capacity;

	void shiftUp(int k) {
		while (k > 0 && data[index[k / 2]] < data[index[k]]) {
			swap(index[k / 2], index[k]);
			reverse[index[k / 2]] = k / 2;
			reverse[index[k]] = k;
			k /= 2;
		}
	}

	void shiftDown(int k) {
		while (2 * k <= count - 1) {
			int j = 2 * k;
			if (j + 1 <= count - 1 && data[index[j + 1]] > data[index[j]]) j++;
			if (data[index[k]] >= data[index[j]]) break;
			swap(index[k], index[j]);
			reverse[index[k]] = k;
			reverse[index[j]] = j;
			k = j;
		}
	}

	//查看索引i这个位置的元素是否在堆中
	bool isInHeap(int i) {
		assert(i >= 0 && i < capacity);//首先i这个元素不能超过data中存储元素的个数
		return reverse[i] != -1; // = -1表示这个元素不在堆中

	}

public:

	// 构造函数, 构造一个空堆, 可容纳capacity个元素
	MaxHeap(int capacity) {
		data = new Item[capacity];
		index = new int[capacity];
		reverse = new int[capacity];
		for (int i = 0; i < capacity; i++) {
			reverse = -1;
		}
		count = 0;
		this->capacity = capacity;
	}

	// 构造函数, 通过一个给定数组创建一个最大堆
	// 该构造堆的过程, 时间复杂度为O(n)
	MaxHeap(Item arr[], int n) {
		data = new Item[n];
		index = new int[n];
		capacity = n;

		for (int i = 0; i < n; i++) {
			data[i] = arr[i];
			index[i] = i;
		}
			
		count = n;

		for (int i = count - 1 / 2; i >= 0; i--)
			shiftDown(i);
	}

	~MaxHeap() {
		delete[] data;
		delete[] index;
		delete[] reverse;
	}

	// 返回堆中的元素个数
	int size() {
		return count;
	}

	// 返回一个布尔值, 表示堆中是否为空
	bool isEmpty() {
		return count == 0;
	}

	// 向最大堆中插入一个新的元素 item
	void insert(int i, Item item) {
		assert(count < capacity);
		assert(i >= 0 && i < capacity);
		data[i] = item;
		index[count] = i;
		reverse[i] = count;
		shiftUp(count);
		count++;
	}

	// 从最大堆中取出堆顶元素, 即堆中所存储的最大数据
	Item extractMax() {
		assert(count > 0);
		Item ret = data[index[0]];
		swap(index[0], index[count - 1]);
		reverse(index[0]) = 0;
		reverse(index[count - 1]) = -1;
		count--;
		shiftDown(0);
		return ret;
	}


	// 获取最大堆中的堆顶元素
	Item getMax() {
		assert(count > 0);
		return data[index[0]];
	}

	//获取最大堆堆顶的索引
	int getMaxIndex() {
		assert(count > 0);
		return index[0];
	}

	// 获取最大索引堆中索引为i的元素
	Item getItem(int i) {
		assert( isInHeap(i));
		return data[i];
	}

	// 将最大索引堆中索引为i的元素修改为newItem
	void change(int i, Item newItem) {
		assert(isInHeap(i));
		data[i] = newItem;
		// 找到indexes[j] = i, j表示data[i]在堆中的位置
	    // 之后shiftUp(j), 再shiftDown(j)
		/*for (int j = 0; j < count; j++) {
			if (index[j] = i) {
				shiftUp(j);
				shiftDown(j);
				break;
			}
		}*/

		int j = reverse[i];
		shiftUp(j);
		shiftDown(j);
	}
};


// heapSort1, 将所有的元素依次添加到堆中, 在将所有元素从堆中依次取出来, 即完成了排序
// 无论是创建堆的过程, 还是从堆中依次取出元素的过程, 时间复杂度均为O(nlogn)
// 整个堆排序的整体时间复杂度为O(nlogn)
template<typename T>
void heapSort1(T arr[], int n) {
	MaxHeap<T> maxheap = MaxHeap<T>(n);
	for (int i = 0; i < n; i++) {
		maxheap.insert(arr[i]);
	}
	for (int i = n - 1; i >= 0; i--) {
		arr[i] = maxheap.extractMax();
	}

}

// heapSort2, 借助我们的heapify过程创建堆
// 此时, 创建堆的过程时间复杂度为O(n), 将所有元素依次从堆中取出来, 时间复杂度为O(nlogn)
// 堆排序的总体时间复杂度依然是O(nlogn), 但是比上述heapSort1性能更优, 因为创建堆的性能更优
template<typename T>
void heapSort2(T arr[], int n) {
	MaxHeap<T> maxheap = MaxHeap<T>(arr, n);
	for (int i = n - 1; i >= 0; i--) {
		arr[i] = maxheap.extractMax();
	}
}
```





###  排序算法总结

![image-20220220205712305](https://gitee.com/ChuckieWill/picture/raw/master/img/202202202057660.png)



* 原地排序：就在原来的数组上操作
* 稳定性：如下图



![image-20220220205223400](https://gitee.com/ChuckieWill/picture/raw/master/img/202202202052061.png)



##  搜索算法

###  二分查找法 

> Binary Search
> 对于有序数列，才能使用二分查找法(排序的作用)
>
> 时间复杂度O(logn)
>
> 非递归算法在性能上有微弱优势

* 非递归版

```c++
#include <iostream>

using namespace std;

// 二分查找法 非递归
// 在有序数组arr中,查找target
// 如果找到target,返回相应的索引index
// 如果没有找到target,返回-1
template<typename T>
int binarySearch(T arr[], int n, T target ) {
	//arr已经按升序排好序
	int l = 0;
	int r = n - 1;
	while (l <= r) {
		//int mid = (l + r) / 2;//l+r有可能溢出，所以使用下面的方式求中点
		int mid = l + (r - l) / 2;
		if (target == arr[mid]) {
			return mid;
		}
		else if (target < arr[mid]) {
			r = mid - 1;
		}
		else {
			l = mid + 1;
		}
	}
	return -1;//没有找到则返回-1
}
```

* 递归版

```c++
#include <iostream>
using namespace std;
template<typename T>
int __binarySearch(T arr[], int l, int r, T target) {
	if (l > r)
		return -1;
	//int mid = (l+r)/2;
   // 防止极端情况下的整形溢出，使用下面的逻辑求出mid
	int mid = l + (r - l) / 2;
	if (arr[mid] == target)
		return mid;
	else if (arr[mid] < target)
		return __binarySearch(arr, mid + 1, r, target);
	else
		return __binarySearch(arr, l, mid - 1, target);
}


//二分查找递归版
template<typename T>
int binarySearch2(T arr[], int n, T target) {
	return __binarySearch(arr, 0, n - 1, target);
}
```

### 二分搜索树

>  二分搜索数是一个二叉树，但不一定是完全二叉树
>
>  定义：每个节点的键值大于左孩子;每个节点的键值小于右孩子;以左右孩子为根的子树仍为二分搜索树
>
>  二分搜索树中插入、搜索、删除时间复杂度都是O(logn)

![image-20220222214721719](https://gitee.com/ChuckieWill/picture/raw/master/img/202202222147102.png)

* 二分搜索树的插入和搜索 

* 二分搜索树的遍历
  * 深度优先
    * 前序遍历∶先访问当前节点，再依次递归访问左右子树
    * 中序遍历:先递归访问左子树，再访问自身，再递归访问右子树
    * 后续遍历︰先递归访问左右子树，再访问自身节点
  * 广度优先
* 二分搜索树中删除最大值和最小值
  * 删除最大值思路：一直找右节点，直到没有右节点则这个点是最大值，删除后用这个点的左节点来顶替这个点
  * 删除最小值思路：一直找左节点，直到没有左节点则这个点是最小值，删除后用这个点的右节点来顶替这个点

* 二分搜索树中删除一个节点
  * 用这个节点的右子树中的最小值（右子树中的最小值来顶替这个点才满足二分搜索树的定义）来顶替这个点
    * 找到这个节点的右子树的最小值后，用这个最小值的右节点来顶替这个最小值（原理同上面删除最小值类似），再用这个最小点来顶替这个要删除的点
  * 也可以用这个节点的左子树的最大值来顶替这个点

```c++
#include <iostream>
#include <queue>
#include <cassert>

using namespace std;

// 二分搜索树
template<typename Key, typename Value>
class BST {
private:
	// 树中的节点为私有的结构体, 外界不需要了解二分搜索树节点的具体实现
	struct Node {
		Key key;
		Value value;
		Node* left;
		Node* right;

		Node(Key key, Value value) {
			this->key = key;
			this->value = value;
			this->left = this->right = NULL;
		}

		Node(Node* node) {
			this->key = node->key;
			this->value = node->value;
			this->left = node->left;
			this->right = node->right;
		}
	};

	Node* root;//根节点
	int count;//树中的节点个数

	// 向以node为根的二分搜索树中, 插入节点(key, value), 使用递归算法
	// 返回插入新节点后的二分搜索树的根
	Node* insert(Node* node, Key key, Value value) {
		if (node == NULL) {
			count++;
			return new Node(key, value);
		}
		if (node->key == key)
			node->value = value;
		else if (node->key < key)
			node->right = insert(node->right, key, value);
		else
			node->left = insert(node->left, key, value);

		return node;
	}

	// 查看以node为根的二分搜索树中是否包含键值为key的节点, 使用递归算法
	bool __contain(Node* node, Key key) {
		if (node == NULL)
			return false;
		if (node->key == key)
			return true;
		else if (key < node->key)
			return __contain(node->left, key);
		else
			return __contain(node->right, key);
	}

	// 在以node为根的二分搜索树中查找key所对应的value, 递归算法
    // 若value不存在, 则返回NULL
	Value* __search(Node* node, Key key) {
		if (node == NULL)
			return NULL;
		if (node->key == key)
			return &(node->value);
		else if (key < node->key)
			return __search(node->left, key);
		else
			return __search(node->right, key);
	}

	// 对以node为根的二叉搜索树进行前序遍历, 递归算法
	void __preOrder(Node* node) {
		if (node != NULL) {
			cout << node->key << endl;
			__preOrder(node->left);
			__preOrder(node -> right);
		}
	}

	// 对以node为根的二叉搜索树进行中序遍历, 递归算法
	void __inOrder(Node* node) {
		if (node != NULL) {
			__inOrder(node->left);
			cout << node->key << endl;
			__inOrder(node->right);
		}
	}

	// 对以node为根的二叉搜索树进行后序遍历, 递归算法
	void __postOrder(Node* node) {
		if (node != NULL) {
			__postOrder(node->left);
			__postOrder(node->right);
			cout << node->key << endl;
		}
	}

	// 释放以node为根的二分搜索树的所有节点
    // 采用后续遍历的递归算法
	void destroy(Node* node) {
		if (node != NULL) {
			destroy(node->left);
			destroy(node->right);
			delete node;
			count--;
		}
	}

	// 返回以node为根的二分搜索树的最小键值所在的节点
	Node* mininum(Node* node) {
		if (node->left == NULL)
			return node;

		return mininum(node->left);
	}

	// 返回以node为根的二分搜索树的最大键值所在的节点
	Node* maxinum(Node* node) {
		if (node->right == NULL)
			return node;

		return maxinum(node->right);
	}

	// 删除掉以node为根的二分搜索树中的最小节点
	// 返回删除节点后新的二分搜索树的根
	Node* removeMin(Node* node) {
		if (node->left == NULL) {
			Node* rightNode = node->right;
			delete node;
			count--;
			return rightNode;
		}
		node->left = removeMin(node->left);
		return node;
	}

	// 删除掉以node为根的二分搜索树中的最大节点
	// 返回删除节点后新的二分搜索树的根
	Node* removeMax(Node* node) {
		if (node->right == NULL) {
			Node* leftNode = node->left;
			delete node;
			count--;
			return leftNode;
		}
		node->right = removeMax(node->right);
		return node;
	}

	// 删除掉以node为根的二分搜索树中键值为key的节点, 递归算法
	// 返回删除节点后新的二分搜索树的根
	Node* remove(Node* node, Key key) {
		if (node == NULL) {
			return NULL;
		}
		if (key < node->key) {
			node->left = remove(node->left, key);
			return node;

		}else if(key > node->key){
			node->right = remove(node->right, key);
			return node;
		}else { // key == node->key
			// 待删除节点左子树为空的情况
			if (node->left == NULL) {
				Node* rightNode = node->right;
				delete node;
				count--;
				return rightNode;
			}

			// 待删除节点右子树为空的情况
			if (node->right == NULL) {
				Node* leftNode = node->left;
				delete node;
				count--;
				return leftNode;
			}

			// 待删除节点左右子树均不为空的情况

            // 找到比待删除节点大的最小节点, 即待删除节点右子树的最小节点
            // 用这个节点顶替待删除节点的位置

			Node* tempNode = new Node(mininum(node->right));//找到当前节点右子树的最小节点， 注意这里要new Node 因为后面的removeMin会删掉这个节点
			count++;//new Node相当于加一个点，后面removeMin里count--  抵消
			tempNode->left = node->left;
			tempNode->right = removeMin(node->right);//将node右子树中最小节点删除，并将右子树赋值给，，
			delete node;
			count--;
			return tempNode;
		}

	}


public:
	// 构造函数, 默认构造一棵空二分搜索树
	BST() {
		root = NULL;
		count = 0;
	}

	~BST() {
		destroy(root);
	}

	// 返回二分搜索树的节点个数
	int size() {
		return count;
	}

	// 返回二分搜索树是否为空
	bool isEmpty() {
		return count == 0;
	}

	// 向二分搜索树中插入一个新的(key, value)数据对
	void insert(Key key , Value value) {
		root = insert(root,key, value);
	}

	//判断键值key在二分搜索树中是否存在
	bool contain(Key key) {
		return __contain(root,key);
	}

	//在二分搜索树中查找键值为key的value值
	Value* search(Key key) {
		return __search(root, key);
	}

	//前序遍历
	void preOrder() {
		__preOrder(root);
	}

	//中序遍历
	void inOrder() {
		__inOrder(root);
	}

	//后序遍历
	void postOrder() {
		__postOrder(root);
	}

	//层序遍历 广度优先
	void levelOrder() {
		queue<Node*> q;
		q.push(root);
		while (!q.empty())
		{
			Node* node = q.front();
			q.pop();
			cout << node->key << endl;
			if(node->left)
				q.push(node->left);
			if(node->right)
				q.push(node->right);
		}
	}

	// 寻找二分搜索树的最小的键值
	Key mininum() {
		assert(count != 0);
		Node* minNode = mininum(root);
		return minNode->key;
	}

	// 寻找二分搜索树的最大的键值
	Key maxinum() {
		assert(count != 0);
		Node* maxNode = maxinum(root);
		return maxNode->key;
	}

	// 从二分搜索树中删除最小值所在节点
	void removeMin() {
		if (root)
			root = removeMin(root);
	}

	// 从二分搜索树中删除最大值所在节点
	void removeMax() {
		if (root)
			root = removeMax(root);
	}

	// 从二分搜索树中删除键值为key的节点
	void remove(Key key) {
		root = remove(root, key);
	}
};
```





**二分搜索树的顺序性**

* 二分搜索树的floor和ceil
  * floor是找到最接近key的且小于key的值 ，key可以在二分搜索树中不存在
  * ceil是找到最接近key的且大于key的值，key可以在二分搜索树中不存在
* 二分搜索树的rank(key)   key是排名第几的元素
  * 思路：给每个节点添加一个count，记录以该节点为根的树共有几个节点（包括当前节点）
  * 由于每个节点count的添加，需要在insert和remove方法中维护count

 ![image-20220223231609976](https://gitee.com/ChuckieWill/picture/raw/master/img/202202232316544.png)

* 二分搜索树的select（num）  找排名第num的元素
  * 思路同上面的rank（）方法
  * 由于每个节点count的添加，需要在insert和remove方法中维护count
* 支持重复元素的二分搜索树
  * 思路1：插入时<=则插入到左子树  即左子树包括了等于的情况
  * 思路2：当重复元素特别多时，且同key一定同value时，为了节约空间，也可以给每个节点添加一个属性count用于记录这个阶段被加入了多少次
  * insert和remove等函数需要修改以维护每个节点count的正确

**二分搜索树的局限性**

* 局限性：
  * 当插入的元素是有序的时，插入的结果就不是一个二叉树，而是只有左孩子（降序）或只有右孩子（升序）的情况，树的深度就为n了，时间复杂度就退化为O(n)
  * 以上情况虽然和链表的时间复杂度都是O(n)，但实际情况比链表还要差，因为链表只维护一个指针，而二分搜索树需要维护左节点和右节点两个指针，而且还要一直判断左右节点是否为空
* 解决方案：
  * 平衡二叉树：红黑树、2-3 tree、AVL tree、Splay tree



##  并查集

> **并查集Union Find**
> 主要功能：
>
> * 判断两点是否相连
>
> 主要方法：
>
> * union( p , q ) //将p,q合并到一个组中
> * find( p )          //找到p所在的组
> * isConnected( p , q )  //判断p、 q是否相连

####  **Quick Find**

> 用一个数组存储数据, id的值存储的是组号，组号相同则是相连的
>
> 查的时间复杂度为O(1)
>
> 并的时间复杂度为O(n)

![image-20220224163258018](https://gitee.com/ChuckieWill/picture/raw/master/img/202202241632360.png)

```c++
#include <iostream>
#include <cassert>

using namespace std;

//第一版并查集
namespace UF1 {
	class UnionFind {
	private:
		int *id;   //存储并查集   存储数据结构为数组
		int count; //并查集数据个数
	public:
		UnionFind(int n) {
			count = n;
			id = new int[n];
			for (int i = 0; i < n; i++) {
				id[i] = i; //初始化时每个元素自己一组
			}
		}

		~UnionFind() {
			delete[] id;
		}

		// 查找过程, 查找元素p所对应的集合编号
		//时间复杂度O(1)
		int find(int p) {
			assert(p >= 0 && p < count);
			return id[p];
		}

		// 查看元素p和元素q是否所属一个集合
		// O(1)复杂度
		bool isConnected(int p, int q) {
			return find(p) == find(q);
		}

		// 合并元素p和元素q所属的集合
		// O(n) 复杂度
		void unionElements(int p, int q) {
			int unionP = find(p);
			int unionQ = find(q);
			if (unionP == unionQ)//已经在一个组
				return;
			for (int i = 0; i < count; i++) {
				if (id[i] == unionP) {
					id[i] = unionQ;
				}
			}
		}
	};
	
}
```

####  **Quick Union**

> 用节点存储数据，节点a与节点b相连则节点a的指针指向节点b，返回过来也可以， 即每个节点有一个指针，和谁相连指针就指向谁
>
> **思想用指针但实际用数组**，因为每个节点只有指针这一个属性，则可以用数组的值存储，parent是几就表示和谁相连
>
> 查时间复杂度O(h), h为树的高度
>
> 并时间复杂度O(h), h为树的高度，用到了查的过程，除开查其实时间复杂度为O(1)

* parent初始情况

![image-20220224163553529](https://gitee.com/ChuckieWill/picture/raw/master/img/202202241635774.png)

* 合并操作
  * 先找到要合并的两个点的根节点
  * 再将其中一个根节点指向另一个根节点
  * 为什么不直接让合并的两个点中的一个指向另一个而是让它们的根节点中的一个指向另一个
    * 因为构造的结果是树，查找的时候树的深度越浅，查找越快，所以尽量不要构造成很长的链，所以让它们的根节点中的一个指向另一个，这样树的深度就会尽量的浅

![image-20220224163917648](https://gitee.com/ChuckieWill/picture/raw/master/img/202202241639380.png)



```c++
#include <iostream>
#include <cassert>

using namespace std;

//第二版并查集
namespace UF2 {
	class UnionFind {
	private:
		int* parent;   //存储并查集   存储数据结构为数组
		int count; //并查集数据个数

		
	public:
		UnionFind(int n) {
			count = n;
			parent = new int[n];
			for (int i = 0; i < n; i++) {
				parent[i] = i; //初始化时每个元素自己一组
			}
		}

		~UnionFind() {
			delete[] parent;
		}

		// 查找过程
		// O(h)复杂度, h为树的高度
		int findRoot(int p) {//查找p的根节点
			assert(p >= 0 && p < count);
			while (p != parent[p])
				p = parent[p];
			return p;
		}

		// 查看元素p和元素q是否所属一个集合
		// O(h)复杂度, h为树的高度
		bool isConnected(int p, int q) {
			return findRoot(p) == findRoot(q);
		}

		// 合并元素p和元素q所属的集合
		// O(h)复杂度, h为树的高度
		void unionElements(int p, int q) {
			int rootP = findRoot(p);
			int rootQ = findRoot(q);
			if (rootP == rootQ)
				return;
			parent[rootP] = rootQ;
		}
	};

}
```



####  Quick Union的优化

> Quick Union存在的问题
>
> * 合并的时候是任意选择一个根节点指向另一个根节点
> * 如果是层数高的根节点指向了层数低的根节点，则整体层数增加了
> * 如果是层数低的根节点指向了层数高的根节点，则整体层数没有增加，所以每次合并应该选择这种方式，而不是任意的
> * 例如上面图中的9和4合并，如果是8指向了9则层数变为4， 如果是9指向了8则层数还是3
>
> size优化和rank优化都是使得树的高度降低，即H尽量小，时间复杂仍然都是O(H) H为数的高度

##### **基于size的优化**

* 增加一个属性sz[]  存储每个根节点所在的树包含多少个节点
* *本优化可以提高时间复杂度两个数量级*

```c++
#include <iostream>
#include <cassert>

using namespace std;

//第三版并查集 优化合并的过程
namespace UF3 {   
	class UnionFind {
	private:
		int* parent;   //存储并查集   存储数据结构为数组
		int* sz; // sz[i]表示以i为根的节点个数
		int count; //并查集数据个数


	public:
		UnionFind(int n) {
			count = n;
			parent = new int[n];
			sz = new int[n];
			for (int i = 0; i < n; i++) {
				parent[i] = i; //初始化时每个元素自己一组
				sz[i] = 1;
			}
		}

		~UnionFind() {
			delete[] parent;
			delete[] sz;
		}

		// 查找过程, 查找元素p所对应的集合编号
		// O(h)复杂度, h为树的高度
		int findRoot(int p) {//查找p的根节点
			assert(p >= 0 && p < count);
			while (p != parent[p])
				p = parent[p];
			return p;
		}

		// 查看元素p和元素q是否所属一个集合
		// O(h)复杂度, h为树的高度
		bool isConnected(int p, int q) {
			return findRoot(p) == findRoot(q);
		}

		// 合并元素p和元素q所属的集合
		// O(h)复杂度, h为树的高度
		void unionElements(int p, int q) {
			int rootP = findRoot(p);
			int rootQ = findRoot(q);
			if (rootP == rootQ)
				return;
			if (sz[rootP] >= sz[rootQ]) {
				parent[rootQ] = rootP;
				sz[rootP] += sz[rootQ];
			}
			else
			{
				parent[rootP] = rootQ;
				sz[rootQ] += sz[rootP];
			}
		}
	};

}
```

##### **基于rank的优化**

* 基于size的优化并没有真的比较两个根节点所在的树的深度，而是比较的根节点所在的树的节点个数，这导致结果仍然不理想
  * 如下图，按size方法，size[7] = 6  >  size[8] = 3    ,所以是8指向7，结果仍然使得层数增加了
* 基于rank则是完全按层数来选择
  * 如下图， 按rank方法，rank[7] = 1 < rank[8] =3, 所以应该是层数少的7指向层数高的8
* rank优化的时间复杂度和size优化的时间复杂度差不多，只是可以处理一些特殊情况（避免树的层数太深），应用更广

![image-20220224195418308](https://gitee.com/ChuckieWill/picture/raw/master/img/202202241954265.png)

```c++
#include <iostream>
#include <cassert>

using namespace std;

//第四版并查集 优化合并的过程
namespace UF4 {
	class UnionFind {
	private:
		int* parent;   //存储并查集   存储数据结构为数组
		int* rank;      // rank[i]表示以i为根的集合所表示的树的层数
		int count; //并查集数据个数


	public:
		UnionFind(int n) {
			count = n;
			parent = new int[n];
			rank = new int[n];
			for (int i = 0; i < n; i++) {
				parent[i] = i; //初始化时每个元素自己一组
				rank[i] = 1;
			}
		}

		~UnionFind() {
			delete[] parent;
			delete[] rank;
		}

		// 查找过程, 查找元素p所对应的集合编号
		// O(h)复杂度, h为树的高度
		int findRoot(int p) {//查找p的根节点
			assert(p >= 0 && p < count);
			while (p != parent[p])
				p = parent[p];
			return p;
		}

		// 查看元素p和元素q是否所属一个集合
		// O(h)复杂度, h为树的高度
		bool isConnected(int p, int q) {
			return findRoot(p) == findRoot(q);
		}

		// 合并元素p和元素q所属的集合
		// O(h)复杂度, h为树的高度
		void unionElements(int p, int q) {
			int rootP = findRoot(p);
			int rootQ = findRoot(q);
			if (rootP == rootQ)
				return;
			if (rank[rootP] > rank[rootQ]) {
				parent[rootQ] = rootP;
			}
			else if(rank[rootP] < rank[rootQ])
			{
				parent[rootP] = rootQ;
			}
			else {//rank[rootP] = rank[rootQ] 时，只有此时层数才会加1
				parent[rootQ] = rootP;
				rank[rootP] += 1;
			}
		}
	};

}
```

#####  路径压缩

> 下面两个路径压缩的结果都使得**时间复杂度近乎O(1)**
>
> 理论上递归的方式，树的层数更少，速度应该更快，但实际测试非递归的方式用时更短，应该是递归的过程使得时间更长了

**非递归的方式**

* 优化find函数，在查找一个元素的根元素的过程中同时降低树的层数
* 4不是根节点则让4的父节点指向父节点的父节点即4->2, 此时跳到2，2不是根节点则2的父节点指向父节点的父节点即2->0,结果如下图，此时跳到0，0为根节点，找到返回
* 此过程优化两个方面
  * 查找过程是跳着找的，中间没有访问3和1
  * 查找过程顺便压缩了树的高度，使得下一次查找路径更短

![image-20220224213819105](https://gitee.com/ChuckieWill/picture/raw/master/img/202202242138929.png)

**递归的方式**

* 上面的压缩方式并不是最优结果
* 最优结果应该如下图所示
* 从4开始递归，4->find(4), 3->find(3),2->find(2),1->find(1),递归到底返回则都一条链上的点都指向了根节点

 ![image-20220224220121334](https://gitee.com/ChuckieWill/picture/raw/master/img/202202242201960.png)

```c++
#include <iostream>
#include <cassert>

using namespace std;

//第五版并查集 路径压缩，优化find
namespace UF5 {
	class UnionFind {
	private:
		int* parent;   //存储并查集   存储数据结构为数组
		int* rank;      // rank[i]表示以i为根的集合所表示的树的层数
		int count; //并查集数据个数


	public:
		UnionFind(int n) {
			count = n;
			parent = new int[n];
			rank = new int[n];
			for (int i = 0; i < n; i++) {
				parent[i] = i; //初始化时每个元素自己一组
				rank[i] = 1;
			}
		}

		~UnionFind() {
			delete[] parent;
			delete[] rank;
		}

		// 查找过程, 查找元素p所对应的集合编号
		//非递归版
		int findRoot(int p) {//查找p的根节点
			assert(p >= 0 && p < count);
			while (p != parent[p]) {
				parent[p] = parent[parent[p]];
				p = parent[p];
			}
			return p;
		}
        
        //递归版
        int findRoot(int p) {//查找p的根节点
			assert(p >= 0 && p < count);
			if (p != parent[p]) {
				parent[p] = findRoot(parent[p]);
			}
			return parent[p];//注意这里是parent[p]而不是p,因为需要返回的是根节点，递归的回溯的时候每一层只有parent[p]一直是指向根节点的
		}

		// 查看元素p和元素q是否所属一个集合
		
		bool isConnected(int p, int q) {
			return findRoot(p) == findRoot(q);
		}

		// 合并元素p和元素q所属的集合
		
		void unionElements(int p, int q) {
			int rootP = findRoot(p);
			int rootQ = findRoot(q);
			if (rootP == rootQ)
				return;
			if (rank[rootP] > rank[rootQ]) {
				parent[rootQ] = rootP;
			}
			else if (rank[rootP] < rank[rootQ])
			{
				parent[rootP] = rootQ;
			}
			else {//rank[rootP] = rank[rootQ] 时，只有此时层数才会加1
				parent[rootQ] = rootP;
				rank[rootP] += 1;
			}
		}
	};

}
```

##  图论

###  图论基础

图的分类

* 有向图
* 无向图
* 有权图
* 无权图

图的连通性

* 各点之间是相互连通的

![image-20220225125809972](https://gitee.com/ChuckieWill/picture/raw/master/img/202202251258467.png)

简单图

* 没有自环边和平行边

![image-20220225130229656](https://gitee.com/ChuckieWill/picture/raw/master/img/202202251302297.png)

图的表示

* 邻接矩阵    适合表示稠密的图
* 邻接表        适合表示稀疏的图

###  邻边迭代器

> 遍历一个点的所有邻边
>
> 因为遍历一个点的所有邻边需要拿到存储图的二维数组g，但是g是对象的私有属性，用户不能直接使用
>
> 所以考虑用迭代器，通过迭代器在迭代器内部可以访问g，将访问结果返回，就可以遍历所有邻边了

* 邻接表

```c++
#include <iostream>
#include <vector>
#include <cassert>
using namespace std;


//稀疏图 邻接表 简单无向图
class SparseGraph {
private:
	int vec;//顶点数
	int edge;//边数
	bool directed;//是否是有向图  true是
	vector<vector<int>> g;//邻接表
public:
	SparseGraph(int n, bool directed) {
		assert(n >= 0);
		this->vec = n;
		this->edge = 0;
		this->directed = directed;
		g = vector<vector<int>>(n, vector<int>());
	}

	~SparseGraph() {}

	int getE() {
		return edge;
	}

	int getV() {
		return vec;
	}

	void addEdge(int v, int w) {
		assert(v >= 0 && v < vec);
		assert(w >= 0 && w < vec);
		/*if (hasEdge(v, w))   //时间复杂度太高不使用
			return;*/
		g[v].push_back(w);
		if (v!=w && !directed)
			g[w].push_back(v);
		edge++;
	}


	//时间复杂度O(n)
	bool hasEdge(int v, int w) {
		assert(v >= 0 && v < vec);
		assert(w >= 0 && w < vec);
		for (int i = 0; i < g[v].size(); i++) {
			if (g[v][i] == w)
				return true;
		}
		return false;
	}

	void show() {
		cout << "SparseGraph: " << endl;
		for (int i = 0; i <vec; i++) {
			cout << i << ": ";
			for (int j = 0; j < g[i].size(); j++) {
				cout << g[i][j] << "\t";
			}
			cout << endl;
		}
		cout << endl;
	}


	// 邻边迭代器, 传入一个图和一个顶点,
    // 迭代在这个图中和这个顶点向连的所有顶点
	class adjIterator {
	private:
		SparseGraph& G;//图G的引用
		int v;  //要遍历边的顶点
		int index; //记录遍历的位置

	public:
		adjIterator(SparseGraph& G, int v):G(G) {
			//this->G = G;
			this->v = v;
			this->index = 0;
		}
		~adjIterator() {}
		// 返回图G中与顶点v相连接的第一个顶点
		int begin() {
			index = 0;
			if (G.g[v].size()) {
				return G.g[v][index];
			}
			// 若没有顶点和v相连接, 则返回-1
			return -1;
		}

		// 返回图G中与顶点v相连接的下一个顶点
		int next() {
			index++;
			if (index < G.g[v].size())
				return G.g[v][index];
			// 若没有顶点和v相连接, 则返回-1
			return -1;
		}

		// 查看是否已经迭代完了图G中与顶点v相连接的所有顶点
		bool end() {
			return index >= G.g[v].size();
		}
	};
};
```

* 邻接矩阵

```c++
#include <iostream>
#include <vector>
#include <cassert>

using namespace std;

//稠密图  邻接矩阵   无向简单图
class DenseGraph {
private:
	int vec;//顶点数
	int edge;//边数
	int directed;//是否为有向图
	vector<vector<bool>> g;//图的具体数据
public:
	DenseGraph(int n, bool directed) {
		assert(n >= 0);
		this->vec = n;
		this->edge = 0;
		this->directed = directed;
		g = vector<vector<bool>>(n, vector<bool>(n,false));
	}

	~DenseGraph() {
	
	}

	int getE() {
		return edge;
	}

	int getV() {
		return vec;
	}

	void addEdge(int v, int w) {
		assert(v >= 0 && v < vec);
		assert(w >= 0 && w < vec);
		if (hasEdge(v,w))
			return;
		g[v][w] = true;
		if (!directed)
			g[w][v] = true;
		edge++;

	}

	bool hasEdge(int v, int w) {
		assert(v >= 0 && v < vec);
		assert(w >= 0 && w < vec);
		return g[v][w];
	}

	void show() {
		cout << "DenseGraph: " << endl;
		for (int i = 0; i < vec; i++) {
			cout << i << ": ";
			for (int j = 0; j < vec; j++) {
				cout << g[i][j] <<"\t";
			}
			cout << endl;
		}
		cout << endl;
	}

	// 邻边迭代器, 传入一个图和一个顶点,
    // 迭代在这个图中和这个顶点向连的所有顶点
	class adjIterator {
	private:
		DenseGraph& G;
		int v;
		int index;
		bool isEnd;
	public:
		adjIterator(DenseGraph& G, int v) :G(G) {
			this->v = v;
			this->index = -1; // 索引从-1开始, 因为每次遍历都需要调用一次next()
		}
		~adjIterator() {}

		// 返回图G中与顶点v相连接的第一个顶点
		int begin() {
			index = -1; // 索引从-1开始, 因为每次遍历都需要调用一次next()
			return next();
		}

		// 返回图G中与顶点v相连接的下一个顶点
		int next() {
			// 从当前index开始向后搜索, 直到找到一个g[v][index]为true
			for(index +=  1; index < G.getV(); index++){
				if (G.g[v][index]) {
					return index;
				}
			}
			// 若没有顶点和v相连接, 则返回-1
			return -1;
		}

		// 查看是否已经迭代完了图G中与顶点v相连接的所有顶点
		bool end() {
			return index >= G.getV();
		}
	};
};
```

###  读取图

> c++中文件读写部分  fstream相关

```c++
#include <iostream>
#include <string>
#include <fstream>
#include <sstream>
#include <cassert>

using namespace std;


//读取文件内容构建图  文件格式第一行 v(顶点个数) e(边的条数)   第二行开始i j  (i,j)表示一条边  
//传入图的引用，和文件地址
template<typename Graph>
class ReadGraph {
public:
	ReadGraph(Graph& g, string filename) {
		ifstream file(filename);//打开文件
		string line;//存储读取的一行的内容
		int V, E;
		assert(file.is_open());//判断是否打开成功

		assert(getline(file, line));//将第一行读取到line中
		stringstream ss(line);
		ss >> V >> E;//将ss中的内容读取到V,E中

		assert(V == g.getV());//判断读取的顶点数和初始化的顶点数相同

		//读取每一条边
		for (int i = 0; i < V; i++) {
			assert(getline(file, line));//读取下一行
			stringstream ss(line);
			int a, b;
			ss >> a >> b;//读取边的两个顶点
			assert(a >= 0 && a < V);
			assert(b >= 0 && b < V);
			g.addEdge(a, b);
		}
	}

};
```

* ReadGraph的使用

```c++
string filename = "testG1.txt";
DenseGraph dg = DenseGraph(13, false);
ReadGraph<DenseGraph> rg1(dg, filename);
```

###  深度优先遍历

> 此部分实现了深度优先遍历、计算图的连通分量个数、用并查集记录同一个连通分量
>
> 深度优先遍历时间复杂度
>
> * 邻接表：O(v+E)  访问了每个点和每个边
> * 邻接矩阵：O(v^2)  整个矩阵都访问了一遍

```c++
#include <iostream>

using namespace std;

//传入一个图，按深度优先遍历， 找出一个图中有多少个连通分量
template<typename Graph>
class Component {
private:
	Graph& G;
	bool* visited;//记录已经访问的点
	int ccount;//记录连通分量个数 
	int* id;//记录每个连通分量的标记  并查集 

	//深度优先遍历
	void dfs(int v) {
		visited[v] = true;
		id[v] = ccount;
		typename Graph::adjIterator adj(G, v); //typename声明Graph是typename,不然会报错
		for (int w = adj.begin(); !adj.end(); w = adj.next()) {
			if(!visited[w])
				dfs( w);
		}
	}
public:
	Component(Graph& G) :G(G) {
		visited = new bool[G.getV()];
		ccount = 0;
		id = new int[G.getV()];
		for (int i = 0; i < G.getV(); i++) {
			visited[i] = false;
			id[i] = -1;
		}

		for (int i = 0; i < G.getV(); i++) {
			if (!visited[i]) {
				dfs(i);
				ccount++;
			}
		}
	}

	

	~Component() {
		delete[] visited;
		delete[] id;
	}

	int count() {
		return ccount;
	}

	//判断两点是否相连  用到了并查集
	bool isConnected(int v, int w) {
		assert(v >= 0 && v < G.getV());
		assert(w >= 0 && w < G.getV());

		return id[v] == id[w];
	}
};
```

###  寻路

> 通过深度优先的方式找到两点之间的一条路径（不管是不是最近的一条）

```c++
#include <iostream>
#include <stack>
//#include <vector>

using namespace std;

//传入一个图和一个起始点s, 按深度优先遍历， 构建从s出发是所有路径
template<typename Graph>
class Path {
private:
	Graph& G;
	bool* visited;//记录已经访问的点
	int s;//起始点
	int* from;//记录每个点的前驱节点from[w] = v 表示是从v访问到w的  构建的路径存储在from中

	//深度优先遍历
	void dfs(int v) {
		visited[v] = true;
		typename Graph::adjIterator adj(G, v); //typename声明Graph是typename,不然会报错
		for (int w = adj.begin(); !adj.end(); w = adj.next()) {
			if (!visited[w]) {
				from[w] = v;
				dfs(w);
			}
		}
	}
public:
	Path(Graph& G , int s) :G(G) {
		assert(s >= 0 && s < G.getV());
		visited = new bool[G.getV()];
		from = new int[G.getV()];
		this->s = s;
		for (int i = 0; i < G.getV(); i++) {
			visited[i] = false;
			from[i] = - 1;
		}

		dfs(s);

	}



	~Path() {
		delete[] visited;
		delete[] from;
	}

	//判断s和w之间是否有路
	bool hasPath(int w) {
		assert(w >= 0 && w < G.getV());
		return visited[w];//访问到了w则一定有路径
	}

	//将s到w的路径存入vec
	void path(int w, vector<int> &vec) {
		assert(hasPath(w));

		stack<int>  s;
		int p = w;
		while (p != -1) {
			s.push(p);
			p = from[p];//最后from[s]==-1 所以找到s就会退出
		}
		vec.clear();
		while (!s.empty()) {
			vec.push_back(s.top());
			s.pop();
		}
	}

	//打印s到w的路径
	void showPath(int w) {
		assert(hasPath(w));
		vector<int> vec;
		path(w, vec);
		for (int i = 0; i < vec.size(); i++) {
			cout << vec[i] ;
			if (i == vec.size() - 1)
				cout << endl;
			else
				cout << "--->";
		}

	}
};
```



###  广度优先遍历

> 广度优先遍历时间复杂度
>
> * 邻接表：O(v+E)  访问了每个点和每个边
> * 邻接矩阵：O(v^2)  整个矩阵都访问了一遍
>
> 以下代码实现了广度优先方式的寻路，可对比上面的深度优先方式

```c++
#include <iostream>
#include <stack>
#include <queue>

using namespace std;

//传入一个图，按深度优先遍历， 找出一个图中有多少个连通分量
template<typename Graph>
class ShortestPath {
private:
	Graph& G;
	bool* visited;//记录已经访问的点
	int s;//起始点
	int* from;//记录每个点的前驱节点from[w] = v 表示是从v访问到w的
	int* ord;//记录每一个点的层数，即每个点到起点的距离


public:
	ShortestPath(Graph& G, int s) :G(G) {
		assert(s >= 0 && s < G.getV());
		visited = new bool[G.getV()];
		from = new int[G.getV()];
		ord = new int[G.getV()];
		this->s = s;
		for (int i = 0; i < G.getV(); i++) {
			visited[i] = false;
			from[i] = -1;
			ord[i] = -1;
		}

		//广度优先遍历
		queue<int> q;
		q.push(s);
		visited[s] = true;
		ord[s] = 0;
		while (!q.empty())
		{
			int w = q.front();
			q.pop();
			typename Graph::adjIterator adj(G, w);//拿到s的所有邻接点
			for (int v = adj.begin(); !adj.end(); v = adj.next()) {
				if (!visited[v]) {
					q.push(v);
					visited[v] = true;
					from[v] = w;
					ord[v] = ord[w] + 1;

				}
			}
		}

	}



	~ShortestPath() {
		delete[] visited;
		delete[] from;
		delete[] ord;
	}

	//判断s和w之间是否有路
	bool hasPath(int w) {
		assert(w >= 0 && w < G.getV());
		return visited[w];//访问到了w则一定有路径
	}

	//将s到w的路径存入vec
	void path(int w, vector<int>& vec) {
		assert(hasPath(w));

		stack<int>  s;
		int p = w;
		while (p != -1) {
			s.push(p);
			p = from[p];//最后from[s]==-1 所以找到s就会退出
		}
		vec.clear();
		while (!s.empty()) {
			vec.push_back(s.top());
			s.pop();
		}
	}

	//打印s到w的路径
	void showPath(int w) {
		assert(hasPath(w));
		vector<int> vec;
		path(w, vec);
		for (int i = 0; i < vec.size(); i++) {
			cout << vec[i];
			if (i == vec.size() - 1)
				cout << endl;
			else
				cout << "--->";
		}

	}

	//查询w到s的路径长度
	int length(int w) {
		assert(w >= 0 && w < G.getV());
		return ord[w];
	}
};
```

##  最小生成树

> 生成树：图中有n个节点，通过n-1条边将n个节点连接起来所构成的树就叫生成树
>
> 最小生成树：生成树中n-1条边的权值相加最小的那个生成树
>
> 最小生成树针对：带权无向图、连通图

![image-20220228222623771](https://gitee.com/ChuckieWill/picture/raw/master/img/202202282226104.png)

###  有权图

* 邻接矩阵实现带权图： 矩阵的存储值直接存储权值，为了和下面的邻接表统一，不直接存储权值，而是也存储边的指针，若两点间没有边则存储NULL
* 邻接表实现带权图： 边作为一个单独的类，邻接表存储边的指针

![image-20220228191642778](https://gitee.com/ChuckieWill/picture/raw/master/img/202202281916752.png)

####  **边的定义**

```c++
#include <iostream>

using namespace std;

template<typename Weight>
class Edge {
private:
	int a; //边的顶点
	int b;
	Weight w;//边的权值

public:
	Edge(int a, int b, Weight w) {
		this->a = a;
		this->b = b;
		this->w = w;
	}

	Edge() {}

	~Edge() {}

	int getv() {
		return a; //返回一个顶点
	}

	int getw() {
		return b;//返回另一个顶点
	}

	Weight getWt() {
		return w;//返回权值
	}

	// 给定一个顶点, 返回另一个顶点
	int other(int v) {
		assert(v == a || v == b);
		return v == a ? b : a;
	}

	// 输出边的信息
	friend ostream& operator<<(ostream& os, const Edge& e) {
		os << e.a << "-" << e.b << ": " << e.w;
		return os;
	}

	// 边的大小比较, 是对边的权值的大小比较
	bool operator<(Edge<Weight>& e) {
		return w < e.getWt();
	}
	bool operator<=(Edge<Weight>& e) {
		return w <= e.getWt();
	}
	bool operator>(Edge<Weight>& e) {
		return w > e.getWt();
	}
	bool operator>=(Edge<Weight>& e) {
		return w >= e.getWt();
	}
	bool operator==(Edge<Weight>& e) {
		return w == e.getWt();
	}
};
```



####  **邻接矩阵构建图**

```c++
#include <iostream>
#include <vector>
#include <cassert>
#include "Edge.h"

using namespace std;

//稠密图  邻接矩阵   带权无向简单图 
template<typename Weight>
class DenseGraph {
private:
	int vec;//顶点数
	int edge;//边数
	int directed;//是否为有向图
	vector<vector<Edge<Weight> *>> g;//图的具体数据
public:
	DenseGraph(int n, bool directed) {
		assert(n >= 0);
		this->vec = n;
		this->edge = 0;
		this->directed = directed;
		g = vector<vector<Edge<Weight> *>>(n, vector<Edge<Weight> *>(n, NULL));
	}

	~DenseGraph() {
		for (int i = 0; i < vec; i++) {
			for (int j = 0; j < vec; j++) {
				if (g[i][j] != NULL)
					delete g[i][j];
			}
		}
	}

	int getE() {
		return edge;
	}

	int getV() {
		return vec;
	}

	void addEdge(int v, int w ,Weight wt) {
		assert(v >= 0 && v < vec);
		assert(w >= 0 && w < vec);
		if (hasEdge(v, w)) {
			delete g[v][w];
			if (v!= w && !directed)
				delete g[w][v];
			edge--;
		}
		g[v][w] = new Edge<Weight>(v,w,wt);
		if (v!=w && !directed)
			g[w][v] = new Edge<Weight>(w,v,wt);
		edge++;

	}

	bool hasEdge(int v, int w) {
		assert(v >= 0 && v < vec);
		assert(w >= 0 && w < vec);
		return g[v][w] != NULL;
	}

	void show() {
		cout << "DenseGraph: " << endl;
		for (int i = 0; i < vec; i++) {
			cout << i << ": ";
			for (int j = 0; j < vec; j++) {
				if (g[i][j])
					cout << g[i][j]->getWt() << "\t";
				else
					cout << "NULL\t";
			}
			cout << endl;
		}
		cout << endl;
	}

	// 邻边迭代器, 传入一个图和一个顶点,
	// 迭代在这个图中和这个顶点向连的所有顶点
	class adjIterator {
	private:
		DenseGraph& G;
		int v;
		int index;
		bool isEnd;
	public:
		adjIterator(DenseGraph& G, int v) :G(G) {
			this->v = v;
			this->index = -1; // 索引从-1开始, 因为每次遍历都需要调用一次next()
		}
		~adjIterator() {}

		// 返回图G中与顶点v相连接的第一个顶点
		Edge<Weight>* begin() {
			index = -1; // 索引从-1开始, 因为每次遍历都需要调用一次next()
			return next();
		}

		// 返回图G中与顶点v相连接的下一个顶点
		Edge<Weight>* next() {
			// 从当前index开始向后搜索, 直到找到一个g[v][index]为true
			for (index += 1; index < G.getV(); index++) {
				if (G.g[v][index]) {
					return G.g[v][index];
				}
			}
			// 若没有顶点和v相连接, 则返回-1
			return NULL;
		}

		// 查看是否已经迭代完了图G中与顶点v相连接的所有顶点
		bool end() {
			return index >= G.getV();
		}
	};
};
```

####  **邻接表构建矩阵**

```c++
#include <iostream>
#include <vector>
#include <cassert>
//#include "Edge.h"

using namespace std;


//稀疏图 邻接表 带权简单无向图
template<typename Weight>
class SparseGraph {
private:
	int vec;//顶点数
	int edge;//边数
	bool directed;//是否是有向图  true是
	vector<vector<Edge<Weight> *>> g;//邻接表
public:
	SparseGraph(int n, bool directed) {
		assert(n >= 0);
		this->vec = n;
		this->edge = 0;
		this->directed = directed;
		g = vector<vector<Edge<Weight> *>>(n, vector<Edge<Weight> *>());
	}

	~SparseGraph() {
		for (int i = 0; i < vec; i++) {
			for (int j = 0; j < g[i].size(); j++) {
				delete g[i][j];
			}
		}
	}

	int getE() {
		return edge;
	}

	int getV() {
		return vec;
	}

	void addEdge(int v, int w, Weight wt) {
		assert(v >= 0 && v < vec);
		assert(w >= 0 && w < vec);
		/*if (hasEdge(v, w))   //时间复杂度太高不使用
			return;*/
		g[v].push_back(new Edge<Weight>(v,w,wt));
		if (v != w && !directed)
			g[w].push_back(new Edge<Weight>(w,v,wt));
		edge++;
	}


	//时间复杂度O(n)
	bool hasEdge(int v, int w) {
		assert(v >= 0 && v < vec);
		assert(w >= 0 && w < vec);
		for (int i = 0; i < g[v].size(); i++) {
			if (g[v][i]->other(v) == w)
				return true;
		}
		return false;
	}

	void show() {
		cout << "SparseGraph: " << endl;
		for (int i = 0; i < vec; i++) {
			cout << "vertex " << i << ":\t";
			for (int j = 0; j < g[i].size(); j++) {
				cout << "( to:" << g[i][j]->getw() << ",wt:" << g[i][j]->getWt() << ")\t";
			}
			cout << endl;
		}
		cout << endl;
	}


	// 邻边迭代器, 传入一个图和一个顶点,
	// 迭代在这个图中和这个顶点向连的所有顶点
	class adjIterator {
	private:
		SparseGraph& G;//图G的引用
		int v;  //要遍历边的顶点
		int index; //记录遍历的位置

	public:
		adjIterator(SparseGraph& G, int v) :G(G) {
			//this->G = G;
			this->v = v;
			this->index = 0;
		}
		~adjIterator() {}
		// 返回图G中与顶点v相连接的第一个顶点
		Edge<Weight>* begin() {
			index = 0;
			if (G.g[v].size()) {
				return G.g[v][index];
			}
			// 若没有顶点和v相连接, 则返回-1
			return NULL;
		}

		// 返回图G中与顶点v相连接的下一个顶点
		Edge<Weight>* next() {
			index++;
			if (index < G.g[v].size())
				return G.g[v][index];
			// 若没有顶点和v相连接, 则返回-1
			return NULL;
		}

		// 查看是否已经迭代完了图G中与顶点v相连接的所有顶点
		bool end() {
			return index >= G.g[v].size();
		}
	};
};
```

####  **读取并构建带权图**

```c++
#include <iostream>
#include <string>
#include <fstream>
#include <sstream>
#include <cassert>

using namespace std;


//读取文件内容构建图  文件格式第一行 v(顶点个数) e(边的条数)   第二行开始i j  (i,j)表示一条边  
//传入图的引用，和文件地址
template<typename Graph, typename Weight>
class ReadGraph {
public:
	ReadGraph(Graph& g, string filename) {
		ifstream file(filename);//打开文件
		string line;//存储读取的一行的内容
		int V, E;
		assert(file.is_open());//判断是否打开成功

		assert(getline(file, line));//将第一行读取到line中
		stringstream ss(line);
		ss >> V >> E;//将ss中的内容读取到V,E中

		assert(V == g.getV());//判断读取的顶点数和初始化的顶点数相同

		//读取每一条边
		for (int i = 0; i < E; i++) {
			assert(getline(file, line));//读取下一行
			stringstream ss(line);
			int a, b;
			Weight w;
			ss >> a >> b>>w;//读取边的两个顶点
			assert(a >= 0 && a < V);
			assert(b >= 0 && b < V);
			g.addEdge(a, b, w);
		}
	}

};
```

####  最小堆

```c++
#pragma once
#include <iostream>
#include <cassert>

using namespace std;

//最小堆
template<typename Item>
class MinHeap {

private:
	Item* data;
	int count;
	int capacity;

	void shiftUp(int k) {
		while (k > 0 && data[k / 2] > data[k]) {
			swap(data[k / 2], data[k]);
			k /= 2;
		}
	}

	void shiftDown(int k) {
		while (2 * k <= count - 1) {
			int j = 2 * k;
			if (j + 1 <= count - 1 && data[j + 1] < data[j]) j++;
			if (data[k] <= data[j]) break;
			swap(data[k], data[j]);
			k = j;
		}
	}

public:

	// 构造函数, 构造一个空堆, 可容纳capacity个元素
	MinHeap(int capacity) {
		data = new Item[capacity];
		count = 0;
		this->capacity = capacity;
	}

	// 构造函数, 通过一个给定数组创建一个最大堆
	// 该构造堆的过程, 时间复杂度为O(n)
	MinHeap(Item arr[], int n) {
		data = new Item[n];
		capacity = n;

		for (int i = 0; i < n; i++)
			data[i] = arr[i];
		count = n;

		for (int i = count - 1 / 2; i >= 0; i--)//不是插入，而是就地构建堆，所以时间复杂度比插入小
			shiftDown(i);
	}

	~MinHeap() {
		delete[] data;
	}

	// 返回堆中的元素个数
	int size() {
		return count;
	}

	// 返回一个布尔值, 表示堆中是否为空
	bool isEmpty() {
		return count == 0;
	}

	// 向最小堆中插入一个新的元素 item
	void insert(Item item) {
		assert(count < capacity);
		data[count] = item;
		shiftUp(count);
		count++;
	}

	// 从最小堆中取出堆顶元素, 即堆中所存储的最小数据
	Item extractMin() {
		assert(count > 0);
		Item ret = data[0];
		swap(data[0], data[count - 1]);
		count--;
		shiftDown(0);
		return ret;
	}

	// 获取最小堆中的堆顶元素
	Item getMin() {
		assert(count > 0);
		return data[0];
	}
};
```

####  最小索引堆

```c++
#pragma once
#include <iostream>
#include <cassert>

using namespace std;


template<typename Item>
class IndexMinHeap {

private:
	Item* data;
	int* index;
	int* reverse;
	int count;
	int capacity;

	void shiftUp(int k) {
		while (k > 0 && data[index[k / 2]] > data[index[k]]) {
			swap(index[k / 2], index[k]);
			reverse[index[k / 2]] = k / 2;
			reverse[index[k]] = k;
			k /= 2;
		}
	}

	void shiftDown(int k) {
		while (2 * k <= count - 1) {
			int j = 2 * k;
			if (j + 1 <= count - 1 && data[index[j + 1]] < data[index[j]]) j++;
			if (data[index[k]] <= data[index[j]]) break;
			swap(index[k], index[j]);
			reverse[index[k]] = k;
			reverse[index[j]] = j;
			k = j;
		}
	}

	//查看索引i这个位置的元素是否在堆中
	bool isInHeap(int i) {
		assert(i >= 0 && i < capacity);//首先i这个元素不能超过data中存储元素的个数
		return reverse[i] != -1; // = -1表示这个元素不在堆中

	}

public:

	// 构造函数, 构造一个空堆, 可容纳capacity个元素
	IndexMinHeap(int capacity) {
		data = new Item[capacity];
		index = new int[capacity];
		reverse = new int[capacity];
		for (int i = 0; i < capacity; i++) {
			reverse[i] = -1;
		}
		count = 0;
		this->capacity = capacity;
	}

	// 构造函数, 通过一个给定数组创建一个最小堆
	// 该构造堆的过程, 时间复杂度为O(n)
	IndexMinHeap(Item arr[], int n) {
		data = new Item[n];
		index = new int[n];
		capacity = n;

		for (int i = 0; i < n; i++) {
			data[i] = arr[i];
			index[i] = i;
		}

		count = n;

		for (int i = count - 1 / 2; i >= 0; i--)
			shiftDown(i);
	}

	~IndexMinHeap() {
		delete[] data;
		delete[] index;
		delete[] reverse;
	}

	// 返回堆中的元素个数
	int size() {
		return count;
	}

	// 返回一个布尔值, 表示堆中是否为空
	bool isEmpty() {
		return count == 0;
	}

	// 向最小堆中插入一个新的元素 item  i是用户角度data的位置
	void insert(int i, Item item) {
		assert(count < capacity);
		assert(i >= 0 && i < capacity);
		data[i] = item;
		index[count] = i;
		reverse[i] = count;
		shiftUp(count);
		count++;
	}

	// 从最小堆中取出堆顶元素, 即堆中所存储的最小数据
	Item extractMin() {
		assert(count > 0);
		Item ret = data[index[0]];
		swap(index[0], index[count - 1]);
		reverse[index[0]] = 0;
		reverse[index[count - 1]] = -1;
		count--;
		shiftDown(0);
		return ret;
	}

	//获取最小堆堆顶元素的在data中存储的索引，同时删除堆顶元素
	int extracIndexMin() {
		assert(count > 0);
		Item ret = index[0];
		swap(index[0], index[count - 1]);
		reverse[index[0]] = 0;
		reverse[index[count - 1]] = -1;
		count--;
		shiftDown(0);
		return ret;
	}


	// 获取最小堆中的堆顶元素
	Item getMax() {
		assert(count > 0);
		return data[index[0]];
	}

	//获取最小堆堆顶的索引
	int getMaxIndex() {
		assert(count > 0);
		return index[0];
	}

	// 获取最小索引堆中索引为i的元素
	Item getItem(int i) {
		assert(isInHeap(i));
		return data[i];
	}

	// 将最小索引堆中索引为i的元素修改为newItem
	void change(int i, Item newItem) {
		assert(isInHeap(i));
		data[i] = newItem;
		// 找到indexes[j] = i, j表示data[i]在堆中的位置
		// 之后shiftUp(j), 再shiftDown(j)
		/*for (int j = 0; j < count; j++) {
			if (index[j] = i) {
				shiftUp(j);
				shiftDown(j);
				break;
			}
		}*/

		int j = reverse[i];
		shiftUp(j);
		shiftDown(j);
	}
    
    // 看索引i所在的位置是否存在元素
	bool contain(int index) {

		return reverse[index ] != 0;
	}
};

```



###  Prim算法

####  切分定理

把图中的结点分为两部分,成为一个**切分(Cut)**

如果一个边的两个端点，属于切分(Cut)不同的两边，这个边称为**横切边(CrossingEdge)**

**切分定理:** 给定*任意*切分，横切边中权值最小的边必然属于最小生成树。

![image-20220228223210045](https://gitee.com/ChuckieWill/picture/raw/master/img/202202282232078.png)

####  Lazy Prim

> 时间复杂度O(ElogE)  需要遍历每条边(E)，每条边都需要插入堆和取出堆(logE)

* 用一个最小堆维护权值
* 从0开始，将与0相连的所有边加入堆中，此时堆顶就是横切边中最短的边，即0-7：0.16
* 再将与7相连的所有边加入堆中（加入不重复，判断边的另一个点是否已经访问），拿到堆顶元素1-7：0.19
* 再将与1相连的所有边加入堆中，，，，，
* 当将与2相连的所有边加入堆中后，会发现1-2、2-7这两条边并不是横切边
  * 这就是Lazy的部分，不是横切边，但仍然放在堆中
  * 当堆顶元素就是这个不是横切边的边时，就判断边的两个端点是否在同一个切分中，如果是则在堆中将其删除，再拿到堆顶元素，做同样的判断，直到堆顶元素是横切边

![image-20220301202614156](https://gitee.com/ChuckieWill/picture/raw/master/img/202203012026674.png)

```c++
#pragma once
#include <iostream>
#include <vector>
#include <cassert>
#include "Edge.h"
#include "MinHeap.h"

using namespace std;

//LazyPrim算法实现最小生成树
template<typename Graph , typename Weight>
class LazyPrimMST {
private:
	Graph& g; //带权图
	MinHeap<Edge<Weight>> mp;//最小堆 
	bool* marked;   //记录点是否已访问
	vector<Edge<Weight>> mst;// 记录最小生成树的权值
	Weight minWt;  //最小生成树的权值和

	//访问一个图中的一个顶点，将其边加入堆中
	void visit(int v) {
		assert(!marked[v]);//当前顶点没有访问过
		marked[v] = true; //访问该点

		typename Graph::adjIterator adj(g, v);
		for (Edge<Weight>* i = adj.begin(); !adj.end(); i = adj.next())//拿到与v相连的每一条边
			if (!marked[i->other(v)])//该边没有被加入过则加入堆中
				mp.insert(*i);  //复杂度是O(loge)

	}
public:
	LazyPrimMST(Graph& g) :g(g), mp(MinHeap<Edge<Weight>>(g.getE())) {
		marked = new bool[g.getV()];
		for (int i = 0; i < g.getV(); i++) {
			marked[i] = false;
		}
		mst.clear();

		visit(0);
		while (!mp.isEmpty()) {
			Edge<Weight> e = mp.extractMin();//取出堆顶元素  这一步的时间复杂度是O(loge) e是插入堆中的元素个数
			if (marked[e.getv()] == marked[e.getw()]) //如果边的两个断点在同一个切分中则跳过，该边不是横切边
				continue;
			mst.push_back(e);//找到了最小生成树的一条横切边，并加入最小生成树
			if (!marked[e.getv()])
				visit(e.getv());
			else
				visit(e.getw());
		}

		minWt = mst[0].getWt();
		for (int i = 1; i < mst.size(); i++) {
			minWt += mst[i].getWt();
		}
	}

	~LazyPrimMST() {
		delete[] marked;
	}

	//返回最小生成树
	vector<Edge<Weight>> getMST() {
		return mst;
	}

	//获取最小生成树的权值和
	Weight getMinWt() {
		return minWt;
	}
};
```









####  Prim

> 时间复杂度O(ElogV)  考察了每条边(E), 最小索引堆相较LazyPrim变小了，最多存储顶点个数(logV)

* 利用最小索引堆来维护权值
  * 最小索引堆中data[i]存储的是到达 i 这个点最短的边(已经访问的边中到达该点最短的边)的权值，
* 从0开始，将0的所有边加入最小索引堆，此时堆顶就是横切边中最短的边，即0-7：0.16
* 再考察与7相连的所有边，判断边的另一端的点是否已访问，若已访问则不加入，若未访问则需要判断是否已经有边到达该点，若没有则直接插入，若已经有则判断当前的边的权值是否小于已经有的到达该点的边的权值，若小于则覆盖，否则不修改
  * 对于0-7，因为0已经访问不加入
  * 对于2-7，已经有边0-2：0.26到达2， 2-7：0.34的权值大于0-2：0.26 所以data[2]的值保持不变
  * 对于4-7，已经有边0-4：0.38到达4， 4-7：0.37的权值小于0-4：0.38 所以data[4]的修改为0.37
  * 按以上规则考察与7相连的所有边后，取出堆顶元素：7-1：0.19
* 再考察与1相连的所有边，，，，，

![image-20220301221448922](https://gitee.com/ChuckieWill/picture/raw/master/img/202203012214278.png)

```c++
#pragma once
#include <iostream>
#include <vector>
#include <cassert>
#include "Edge.h"
#include "IndexMinHeap.h"

using namespace std;

//Prim算法实现最小生成树
template<typename Graph, typename Weight>
class PrimMST {
private:
	Graph& g; //带权图
	IndexMinHeap<Weight> imp;//最小堆 存储的是权值
	vector<Edge<Weight>*> edgeTo;//已经加入堆中的边
	bool* marked;   //记录点是否已访问
	vector<Edge<Weight>> mst;// 记录最小生成树的权值
	Weight minWt;  //最小生成树的权值和

	//访问一个图中的一个顶点，将其边加入堆中
	void visit(int v) {
		assert(!marked[v]);//当前顶点没有访问过
		marked[v] = true; //访问该点

		typename Graph::adjIterator adj(g, v);
		for (Edge<Weight>* i = adj.begin(); !adj.end(); i = adj.next()) {//拿到与v相连的每一条边
			int w = i->other(v);
			if (!marked[w]) {//该边的另一个顶点没有被考察过
				if (!edgeTo[w]) { //不存在已经有到达w的边
					imp.insert(w, i->getWt());  //复杂度是O(loge)
					edgeTo[w] = i;
				}
				else if (i->getWt() < edgeTo[w]->getWt())
				{//已经存在达到w的边，此时i的边也到达w，若i边的权值小于原来已经存在的边的权值，则覆盖原来的边
					imp.change(w, i->getWt());
					edgeTo[w] = i;

				}
			}
		}
	}
public:
	PrimMST(Graph& g) :g(g), imp(IndexMinHeap<Weight>(g.getV())) {
		marked = new bool[g.getV()];
		for (int i = 0; i < g.getV(); i++) {
			marked[i] = false;
			edgeTo.push_back(NULL);
		}
		mst.clear();

		visit(0);
		while (!imp.isEmpty()) {
			int v = imp.extracIndexMin();//取出堆顶元素在data中存储的索引 即最小横切边中还未访问的一端的顶点  这一步的时间复杂度是O(loge) e是插入堆中的元素个数
			assert(edgeTo[v]);
			mst.push_back(*edgeTo[v]);//找到了最小生成树的一条横切边，并加入最小生成树
			
			visit(v);
		}

		minWt = mst[0].getWt();
		for (int i = 1; i < mst.size(); i++) {
			minWt += mst[i].getWt();
		}
	}

	~PrimMST() {
		delete[] marked;
	}

	//返回最小生成树
	vector<Edge<Weight>> getMST() {
		return mst;
	}

	//获取最小生成树的权值和
	Weight getMinWt() {
		return minWt;
	}
};
```



### Kruskal算法

> 效率： LazyPrim < kruskal算法 < Prim算法
>
> 时间复杂度O(ElogE)

* 先将所有的边排序  时间复杂度O(ElogE) ，用最小堆排序，便于拿到每个点 
  * 插入和获取堆顶元素时间复杂度都是O(logE), 遍历每条边时间复杂度O(E)
  * 使用基于find优化的路径压缩的并查集，非递归版
* 从最小边开始考察，若这条边加入不使得形成环，则这条边一定是最小生成树的一条边
  * 用并查集来判断边的加入是否形成环，若边的两点是可达的，则说明这条边的加入会形成环
  * 并查集的时间复杂度为O(logV), 每条边都要考察，所以总时间复杂度O(ElogV)



![image-20220303204948079](https://gitee.com/ChuckieWill/picture/raw/master/img/202203032049395.png)

```c++
#pragma once
#include <iostream>
#include <vector>
#include "UF.h"
#include "Edge.h"
#include "MinHeap.h"
using namespace std;

template<typename Graph, typename Weight>
class KruskalMST {
private:
	vector<Edge<Weight>> mst;//存储最小生成树
	Weight minWt;//最小生成树的权值和
public:
	KruskalMST(Graph& g) {
		MinHeap<Edge<Weight>> mp(g.getE());
		for (int i = 0; i < g.getV(); i++) {
			typename Graph::adjIterator adj(g, i);
			for (Edge<Weight>* v = adj.begin(); !adj.end(); v = adj.next()) {
				if(v->getv() < v->getw())//因为是无向图，会重复插入，所以在这里去重
					mp.insert(*v);
			}
		}
		UnionFind uf(g.getV());
		while (!mp.isEmpty() && mst.size() < g.getV()-1) //当mst中边的条数已经等于边数-1时可提前结束循环
		{
			Edge<Weight> e = mp.extractMin();
			if (!uf.isConnected(e.getv(), e.getw())) {//该边不是环则加入最小生成树
				mst.push_back(e);
				uf.unionElements(e.getv(), e.getw());
			}
		}
		minWt = 0;
		for (int i = 0; i < mst.size(); i++) {
			minWt += mst[i].getWt();
		}
	}

	~KruskalMST() {}

	//返回最小生成树的边
	vector<Edge<Weight>> getMST() {
		return mst;
	}

	//返回最小生成树的权值和
	Weight getMinWt() {
		return minWt;
	}
};
```



##  最短路径

> 单源最短路径: 某一个点到图中任意一点的最短路径
>
> 本部分辅助类沿用最小生成树部分的辅助类

###  dijkstra算法

> dijkstra算法: 解决单源最短路径
>
> 前提: 图中不能有负权边
>
> 时间复杂度O(E log(V) )

![image-20220307225117825](https://gitee.com/ChuckieWill/picture/raw/master/img/202203072251192.png)

* 使用最小索引堆选取下一个访问节点，最小索引堆存储已经访问的到达每个点的最短路径，堆顶元素的索引即是下一个要访问的点

```c++
Weight* distTo; //记录源点到每个点的最短路径值
bool* marked;   //记录每个点的访问情况
vector<Edge<Weight>*> from; //记录最短路径中每个点的前驱节点
IndexMinHeap<Weight> imh(g.getV()); // 使用索引堆记录当前找到的到达每个顶点的最短距离
```



* 从0开始，将0标记为已访问，将所有与0相连的边的权值加入堆中，并将1、2、3的前驱标记为0，并记录到达1、2、3的最短路径值分别为5、2、6，取出堆顶元素的索引2，即下一个访问的节点
* 从2开始
  * 将2标记为已访问
  * 考察(2,1)边，因为(0,2)+(2,1)边的权值小于distTo[1]， 所以有如下操作，若没有小于则不操作
    * 因为distTo[1] = 5，值不为零，说明已经插入过堆中，所以使用最小堆的change方法，修改到达1的最短路径为3
    * distTo[1]更新为3
    * 1的前驱节点更新为2
  * 考察（2，4）边，
    * 因为distTo[4] = 0，值为零，说明之前没有插入过堆中，直接插入
    * distTo[4]更新为7
    * 4的前驱节点更新为2
  * ，，，，，
* 关键步骤是**松弛操作**
  * 只有当前点未被访问才进行松弛操作
    * 当前点未被访问，且之前没有赋值，则直接插入堆中
    * 当前点未被访问，且之前有赋值，则比较当前值是否比以前小，如果小则更新堆中的数据
* 为什么松弛操作可以找到更短的路径
  * 为什么每次选堆顶元素，也就是路劲最短的那个点作为下一个扩展点就是可以最终获得最短路劲
  * 因为：如上图0到1的权值为5，而可以到1的不止0，还有2，也就是说存在一个中可能：从0出发到2再到1可能权值更小，同理如果还有多个点也可以到1，则可能存在从0到这些点再到1路劲更短，而应该选择哪个点到1最有可能路劲最短呢？**那一定是与1相连的点中路劲最短的那个点也就是堆顶的那个点**，由此反推，从0出发时，每次选路径最短的那个点扩展

```c++
#pragma once
#include <iostream>
#include <cassert>
#include <vector>
#include <stack>

#include "IndexMinHeap.h"
#include "Edge.h"

using namespace std;

template<typename Graph, typename Weight>
class Dijkstra {
private:
	Graph& g;
	int s;  //源点
	Weight* distTo; //记录源点到每个点的最短路径值
	bool* marked;//记录每个点的访问情况
	vector<Edge<Weight>* > from; //记录最短路径中每个点的前驱节点

public:
	Dijkstra(Graph& g, int s) :g(g) {
		assert(s >= 0 && s < g.getV());
		this->s = s;
		distTo = new Weight[g.getV()];
		marked = new bool[g.getV()];
		for (int i = 0; i < g.getV(); i++) {
			distTo[i] = Weight();//Weight为int的化，则都初始化为0
			marked[i] = false;
			from.push_back(NULL);
		}

		// 使用索引堆记录当前找到的到达每个顶点的最短距离
		IndexMinHeap<Weight> imh(g.getV());
		imh.insert(s, distTo[s]);
		while (!imh.isEmpty()) {
			int cur_id = imh.extracIndexMin();//获取堆顶元素的索引，即下一个要访问的点
			typename Graph::adjIterator adj(g, cur_id);
			marked[cur_id] = true;
			//cout << "marked[cur_id]: "<<marked[cur_id] << endl;
			for (Edge<Weight>* e = adj.begin(); !adj.end(); e = adj.next()) {
				int v = e->other(cur_id);
				Weight wt = e->getWt();

				//松弛操作  
				if (!marked[v]) {//该点还未访问
					if (from[v] == NULL ) {
						distTo[v] = distTo[cur_id] + wt;
						from[v] = e;
						imh.insert(v, distTo[v]);
					}
					if (distTo[cur_id] + wt < distTo[v]) {
						distTo[v] = distTo[cur_id] + wt;
						from[v] = e;
						imh.change(v, distTo[v]);
					}
				}		
			}	
		}
	}

	~Dijkstra() {
		delete[] distTo;
		delete[] marked;
		//delete from[0];
		
	}

	// 返回从s点到w点的最短路径长度
	Weight shortestPathTo(int w) {
		assert(w >= 0 && w < g.getV());
		assert(hasPathTo(w));
		return distTo[w];
	}

	// 判断从s点到w点是否联通
	bool hasPathTo(int w) {
		assert(w >= 0 && w < g.getV());
		return marked[w];

	}

	// 寻找从s到w的最短路径, 将整个路径经过的边存放在vec中
	void shortestPath(int w, vector<Edge<Weight>>& vec) {
		assert(w >= 0 && w < g.getV());
		assert(hasPathTo(w));

		// 通过from数组逆向查找到从s到w的路径, 存放到栈中
		stack<Edge<Weight>*> s;
		while (from[w]) {
			s.push(from[w]);
			w = from[w]->other(w);
		}

		// 从栈中依次取出元素, 获得顺序的从s到w的路径
		while (!s.empty()) {
			vec.push_back(*s.top());
			s.pop();
		}
	}

	// 打印出从s点到w点的路径
	void showPath(int w) {
		assert(w >= 0 && w < g.getV());
		assert(hasPathTo(w));

		vector<Edge<Weight>> vec;
		shortestPath(w, vec);
		for (int i = 0; i < vec.size(); i++) {
			cout << vec[i].getv() << "-->";
			if (i == vec.size() - 1)
				cout << vec[i].getw() << endl;
		}
	}

};
```

###  Bellman-Ford算法

> Bellman-Ford算法：解决有负权边的单源最短路径
>
> 前提：图中不能有负权环
>
> 时间复杂度：O(EV)
>
> 一般用于处理有向图，因为无向图中只要存在一条负权边就一定形成了负权环

**负权环**

* 0--1--2  三点之间就构成了负权环（这三个点构成环，且环的所有边的权值之和为负值），当有负权环时，则不存在最短路径，因为只要一直走这个环，路径会越来越小，直至负无穷
* 1--2 两点之间也够成了负权环
* 0--2 两点之间也够成了负权环

![image-20220309225840814](https://gitee.com/ChuckieWill/picture/raw/master/img/202203092259286.png)



**判断图中是否有负权环**

* 假设一个图没有负权环
* 从一点到另外一点的最短路径，最多经过所有的V个顶点，有V-1条边
* 否则（经过了大于v个顶点），存在顶点经过了两次，既存在负权环，与假设矛盾
* 所以只要经过的点大于v个顶点则判断图中有负权环，不可能存在最短路径，直接结束查找

**松弛操作**

* 核心思想：从一个点出发，通过另外一个点到达本来要到达的点的路径可能比直接到达更短

**前提思考**

* 对一个点的一次松弛操作，就是找到经过这个点的另外一条路径，多一条边，权值更小。
* 如果一个图没有负权环，从一点到另外一点的最短路径,最多经过所有的V个顶线，有V-1条边
* 对所有的点进行V-1次松弛操作

**Bellman-Ford算法思想**

* 对所有的点进行V-1次松弛操作，理论上就找到了从源点到其他所有点的最短路径。
* 如果还可以继续松弛(V次及V次以上)，说明原图中有负权环。
* 算法核心步骤如下:

```c++
    //进行第v次松弛操作，若此次操作仍然可以缩短路径，则一定存在负权环
    bool detectNegativeCycle() {
        for (int i = 0; i < g.getV(); i++) {
            typename Graph::adjIterator adj(g, i);
            for (Edge<Weight>* e = adj.begin(); !adj.end(); e = adj.next()) {
                if (from[e->getw()] == NULL || distTo[e->getv()] + e->getWt() < distTo[e->getw()]) {
                    return true;
                }
            }
        }
        return false;//不存在负权环
    }


    //对每个顶点进行v-1次松弛操作
    for (int pass = 1; pass < g.getV(); pass++) {
        //对每个点进行松弛操作
        for (int i = 0; i < g.getV(); i++) {
            typename Graph::adjIterator adj(g, i);
            //考察i这个点的所有出度边
            for (Edge<Weight>* e = adj.begin(); !adj.end(); e = adj.next()) {
                // 对于每一个边首先判断e->v()可达，也就是e->v()访问过，也确保只能从源点开始，因为一开始只有源点的from[s]初始化了，满足条件
                // 之后看如果e->w()以前没有到达过， 显然我们可以更新distTo[e->w()]
                //若已经记录的源点到达i点的最短距离(distTo[e->getv()])加上出度边的权值(e->getWt())
                //小于源点到这条出度边的终点的距离则松弛有效
                if (from[e->getv()] && (from[e->getw()] == NULL || distTo[e->getv()] + e->getWt() < distTo[e->getw()])） {
                    distTo[e->getw()] = distTo[e->getv()] + e->getWt();//e.getv()是起点，e.getw()是终点
                    from[e->getw()] = e;
                }
            }
        }
    }
                    
    hasNegativeCycle = detectNegativeCycle();
```

* 可以理解为从源点出发的层序遍历，第一次松弛操作只更新了距离源点为1的点，第二次更新了距离源点为2的点，，，，
* 下图是特殊情况，因为源点正好是0, 儿编程中也是选择的从序号最小的点开始扩展，所以等到扩展序号大的点时，一定保证了他的前继节点已经更新了到源点的最短距离，所以一轮松弛操作就可以得到最终结果
* 对每个点都需要进行v-1次松弛操作的原因：源点可能不是正好是0，如果是比较大的点，按编程中的的扩展顺序，当扩展0的时候0的前继节点可能还没更新，所以0在第一次松弛操作时就没有得到扩展，同理，只要比源点小的点都存在这个问题
* 为什么第v次松弛操作时有点没有更新(`from[e->getw()] == NULL`)也可以判定有负权环：因为有负权环的存在，会导致每次松弛操作一直在环中进行，不会继续扩展到下一个节点，所以到v次扩展仍然有点没有访问到，反过来就可以判断有负权环

![image-20220310222210117](https://gitee.com/ChuckieWill/picture/raw/master/img/202203102222665.png)

```
上图的执行结果
bellmanford:
0 -> 0
shortestPathTo:0->0 : 0
0 -> 1
shortestPathTo:0->1 : 5
0 -> 1 -> 2
shortestPathTo:0->2 : 1
0 -> 1 -> 2 -> 4 -> 3
shortestPathTo:0->3 : 3
0 -> 1 -> 2 -> 4
shortestPathTo:0->4 : 6
```

```c++
#pragma once
#include <iostream>
#include <vector>
#include <stack>
#include "Edge.h"

using namespace std;

template<typename Graph, typename Weight>
class BellmanFord {

private:
	Graph& g;                   //图
	int s;                      //源点
	Weight* distTo;             // distTo[i]存储从起始点s到i的最短路径长度
	vector<Edge<Weight>*> from; // from[i]记录最短路径中, 到达i点的边是哪一条
								// 可以用来恢复整个最短路径
	bool hasNegativeCycle;      // 标记图中是否有负权环

    //进行第v次松弛操作，若此次操作仍然可以缩短路径，则一定存在负权环
    bool detectNegativeCycle() {
        for (int i = 0; i < g.getV(); i++) {
            typename Graph::adjIterator adj(g, i);
            for (Edge<Weight>* e = adj.begin(); !adj.end(); e = adj.next()) {
                if (from[e->getw()] == NULL || distTo[e->getv()] + e->getWt() < distTo[e->getw()]) {
                    return true;
                }
            }
        }
        return false;//不存在负权环
    }
public:
	BellmanFord(Graph& g, int s) :g(g) {
		this->s = s;
		distTo = new Weight[g.getV()];
		
		for (int i = 0; i < g.getV(); i++) {
			from.push_back(NULL);
		}

		distTo[s] = Weight();//初始化，如果Weight为int型，则初始化为0
		from[s] = new Edge<Weight>(s, s, Weight());//初始化为一条s指向s的权值为0的边

        //对每个顶点进行v-1次松弛操作
        for (int pass = 1; pass < g.getV(); pass++) {
            //对每个点进行松弛操作
            for (int i = 0; i < g.getV(); i++) {
                typename Graph::adjIterator adj(g, i);
                for (Edge<Weight>* e = adj.begin(); !adj.end(); e = adj.next()) {
                    if (from[e->getv()] && (from[e->getw()] == NULL || distTo[e->getv()] + e->getWt() < distTo[e->getw()])) {
                        distTo[e->getw()] = distTo[e->getv()] + e->getWt();//e.getv()是起点，e.getw()是终点
                        from[e->getw()] = e;
                    }
                }
            }

        }

        hasNegativeCycle = detectNegativeCycle();
       //hasNegativeCycle = false;



	}
	~BellmanFord() {
		delete[] distTo;
		delete from[s];
	}

    // 返回图中是否有负权环
    bool negativeCycle() {
        return hasNegativeCycle;
    }

    // 返回从s点到w点的最短路径长度
    Weight shortestPathTo(int w) {
        assert(w >= 0 && w < g.getV());
        assert(!hasNegativeCycle);
        assert(hasPathTo(w));
        return distTo[w];
    }

    // 判断从s点到w点是否联通
    bool hasPathTo(int w) {
        assert(w >= 0 && w < g.getV());
        return from[w] != NULL;
    }

    // 寻找从s到w的最短路径, 将整个路径经过的边存放在vec中
    void shortestPath(int w, vector<Edge<Weight>>& vec) {

        assert(w >= 0 && w < g.getV());
        assert(!hasNegativeCycle);
        assert(hasPathTo(w));

        // 通过from数组逆向查找到从s到w的路径, 存放到栈中
        stack<Edge<Weight>*> s;
        Edge<Weight>* e = from[w];
        while (e->getv() != this->s) {
            s.push(e);
            e = from[e->getv()];//如果是有向图，e->getv()就恰好是起点
        }
        s.push(e);

        // 从栈中依次取出元素, 获得顺序的从s到w的路径
        while (!s.empty()) {
            e = s.top();
            vec.push_back(*e);
            s.pop();
        }
    }

    // 打印出从s点到w点的路径
    void showPath(int w) {

        assert(w >= 0 && w < g.getV());
        assert(!hasNegativeCycle);
        assert(hasPathTo(w));

        vector<Edge<Weight>> vec;
        shortestPath(w, vec);
        for (int i = 0; i < vec.size(); i++) {
            cout << vec[i].getv() << " -> ";
            if (i == vec.size() - 1)
                cout << vec[i].getw() << endl;
        }
    }
};
```



###  拓展

**单源最短路径算法：**

![image-20220310233643686](https://gitee.com/ChuckieWill/picture/raw/master/img/202203102336420.png)

**所有对最短路径算法：**

> 计算任意两个点之间的最短路径

Floyed算法，处理无负权环的图， 时间复杂度O( V^3 )，动态规划思想

**最长路径算法**

* 最长路径问题不能有正权环。
* 无权图的最长路径问题是指数级难度的。
* 对于有权图，不能使用Dijkstra求最长路径问题。可以使用Bellman-Ford算法（把权值取负就可以直接使用该算法了）。

