# 排序算法总结
```
稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面；
不稳定：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；
内排序：所有排序操作都在内存中完成；
外排序：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；
时间复杂度： 一个算法执行所耗费的时间。
空间复杂度：运行完一个程序所需内存的大小。
```



排序的分类

- 比较排序
    - 快速排序
    - 归并排序
    - 堆排序
    - 冒泡排序

在排序的最终结果里，元素之间的次序依赖于它们之间的比较。每个数都必须和其他数进行比较，才能确定自己的位置。  
在冒泡排序之类的排序中，问题规模为n，又因为需要比较n次，所以平均时间复杂度为O(n²)。在归并排序、快速排序之类的排序中，问题规模通过分治法消减为logN次，所以时间复杂度平均O(nlogn)。
比较排序的优势是，适用于各种规模的数据，也不在乎数据的分布，都能进行排序。可以说，比较排序适用于一切需要排序的情况。
- 非比较排序
    - 计数排序
    - 基数排序
    - 桶排序

非比较排序是通过确定每个元素之前，应该有多少个元素来排序。针对数组arr，计算arr[i]之前有多少个元素，则唯一确定了arr[i]在排序后数组中的位置。    

非比较排序只要确定每个元素之前的已有的元素个数即可，所有一次遍历即可解决。算法时间复杂度O(n)。
非比较排序时间复杂度底，但由于非比较排序需要占用空间来确定唯一位置。所以对数据规模和数据分布有一定的要求。

## 冒泡排序
最佳情况：T(n) = O(n) 最差情况：T(n) = O(n2) 平均情况：T(n) = O(n2)

算法描述
- 1.比较相邻的元素。如果第一个比第二个大，就交换它们两个；
- 2.对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
- 3.针对所有的元素重复以上的步骤，除了最后一个；
- 4.重复步骤1~3，直到排序完成。

`说明：按从小到大顺序排序，第一次遍历就将最大的数字放到了最后；进行第二次遍历时，只需比较到倒数第二个数字即可。`
```java
public static void bubbleSort(int[] array) {
        if (array == null || array.length <= 1) {
            return;
        }

        int length = array.length;//先写成临时变量

        // 外层循环控制比较轮数i，肯定是n次遍历比较
        for (int i = 0; i < length; i++) {
            // 内层循环控制每一轮比较次数，每进行一轮排序都会找出一个较大值，丢到最后
            // (array.length - 1)防止索引越界，(array.length - 1 - i)减少比较次数
            for (int j = 0; j < length - 1 - i; j++) {
                // 前面的数大于后面的数就进行交换
                if (array[j] > array[j + 1]) {
                    int temp = array[j + 1];
                    array[j + 1] = array[j];
                    array[j] = temp;
                }
            }
        }

    }
```

## 选择排序
最佳情况：T(n) = O(n2) 最差情况：T(n) = O(n2) 平均情况：T(n) = O(n2)

算法描述
n个记录的直接选择排序可经过n-1趟直接选择排序得到有序结果。具体算法描述如下：

- 初始状态：无序区为R[1…n]，有序区为空；
- 第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1…i-1]和R(i…n）。该趟排序从当前无序区中-选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1…i]和R[i+1…n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；
- n-1趟结束，数组有序化了。


表现最稳定的排序算法之一，无论什么数据进去都是O(n2)的时间复杂度，所以用到它的时候，数据规模越小越好。
唯一的好处可能就是不占用额外的内存空间了吧。

`说明：从小到大排序，有序区在前面，无序区在后面；初始化无序区第一个为最小，每一次在后面中寻找是否存在一个最小数字与无序区第一个元素交换`
```java

public static void selectionSort(int[] array) {
	if (array == null || array.length <= 1) {
		return;
	}

	int length = array.length;

	for (int i = 0; i < length - 1; i++) {//有序区在前面，i++这是在逐步缩小无序区长度
		// 保存最小数的索引
		int minIndex = i;//无序区第一个数字为最小索引初始化

		for (int j = i + 1; j < length; j++) {
			// 找到最小的数
			if (array[j] < array[minIndex]) {
				minIndex = j;
			}
		}

		// 交换元素位置,不等于才交换避免了两元素相等时的交换
		if (i != minIndex) {
			swap(array, minIndex, i);
		}
	}

}

private static void swap(int[] array, int a, int b) {
	int temp = array[a];
	array[a] = array[b];
	array[b] = temp;
}

```
## 插入排序
最佳情况：T(n) = O(n) 最坏情况：T(n) = O(n2) 平均情况：T(n) = O(n2)

算法描述
一般来说，插入排序都采用in-place在数组上实现。具体算法描述如下：

- 1.从第一个元素开始，该元素可以认为已经被排序；
- 2.取出下一个元素，在已经排序的元素序列中从后向前扫描；
- 3.如果该元素（已排序）大于新元素，将该元素移到下一位置；
- 4.重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；
- 5.将新元素插入到该位置后；
- 6.重复步骤2~5。

说明：`默认索引0有序，然后无序区从索引1开始。针对无序区，无序区的每一个元素依次赋值临时变量insertnum，依次倒叙的与有序区最后一元素开始相比，期间有序区元素后移，insertnum寻找合适位置插入。`
```java

public static void insertionSort(int[] array) {
	if (array == null || array.length <= 1) {
		return;
	}

	int length = array.length;

	// 要插入的数
	int insertNum;

	for (int i = 1; i < length; i++) {
		insertNum = array[i];//索引0默认可认为有序，所以从索引1开始
		// 已经排序好的元素个数
		int j = i - 1;
		while (j >= 0 && array[j] > insertNum) {
			// 针对前面有序区，从后到前循环，将大于insertNum的数向后移动一格
			array[j + 1] = array[j];
			j--;//索引前移，准备与前一个数比较
		}

		// 跳出while，索引j位置元素比insertnum小，所以在索引j+1处插入
        //将需要插入的数放在要插入的位置
		array[j + 1] = insertNum;
	}
}

```


## 希尔排序
`最佳情况：T(n) = O(nlog2 n) 最坏情况：T(n) = O(nlog2 n) 平均情况：T(n) =O(nlog2n)`

>简单插入排序经过改进之后的一个更高效的版本，也称为缩小增量排序，同时该算法是冲破O(n2）的第一批算法之一。它与插入排序的不同之处在于，它会优先比较距离较远的元素。希尔排序又叫缩小`增量`排序。
[希尔排序](https://blog.csdn.net/ThinkWon/article/details/101538166)

说明：先定义gap序列，一般{n/2,(n/2)/2…1}。
算法描述

我们来看下希尔排序的基本步骤，在此我们选择增量gap=length/2，缩小增量继续以gap = gap/2的方式，这种增量选择我们可以用一个序列来表示，{n/2,(n/2)/2…1}，称为增量序列。希尔排序的增量序列的选择与证明是个数学难题，我们选择的这个增量序列是比较常用的，也是希尔建议的增量，称为希尔增量，但其实这个增量序列不是最优的。此处我们做示例使用希尔增量。

先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，具体算法描述：

- 选择一个增量序列t1，t2，…，tk，其中ti>tj，tk=1；
- 按增量序列个数k，对序列进行k 趟排序；
- 每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m 的子序列，分别对各子表进行直接插入排序。仅增量因子为1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

```java

public static void shellSort(int[] array) {
	if (array == null || array.length <= 1) {
		return;
	}

	int length = array.length;

	// temp为临时变量，gap增量默认是长度的一半，每次变为之前的一半，直到最终数组有序
	int temp, gap = length / 2;

	while (gap > 0) {
		for (int i = gap; i < length; i++) {
			// 将当前的数与减去增量之后位置的数进行比较，如果大于当前数，将它后移
			temp = array[i];//起始点由gap决定
			int preIndex = i - gap;
			//以下为操作语句，对子序列进行插值
			while (preIndex >= 0 && array[preIndex] > temp) {
				array[preIndex + gap] = array[preIndex];
				preIndex -= gap;//比较更前一个索引对应的数值，如果更前没有数值，此次操作使得preIndex负数跳出while
				//这地方不错，挺巧妙dave
				//赞
			}
			// 将当前数放到空出来的位置
			array[preIndex + gap] = temp;


		}
		gap /= 2;
	}
}


```

## 归并排序

`最佳情况：T(n) = O(n) 最差情况：T(n) = O(nlogn) 平均情况：T(n) = O(nlogn)`

和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是O(n log n）的时间复杂度。代价是需要额外的内存空间。


算法描述
- 把长度为n的输入序列分成两个长度为n/2的子序列；
- 对这两个子序列分别采用归并排序；
- 将两个排序好的子序列合并成一个最终的排序序列。

```java
/**
 * Description: 归并排序
 *
 * @param array
 * @return void
 * @author JourWon
 * @date 2019/7/11 23:37
 */
public static void mergeSort(int[] array) {
	if (array == null || array.length <= 1) {
		return;
	}

	sort(array, 0, array.length - 1);
}

private static void sort(int[] array, int left, int right) {
	if (left == right) {
		return;
	}
	int mid = left + ((right - left) >> 1);
	// 对左侧子序列进行递归排序
	sort(array, left, mid);
	// 对右侧子序列进行递归排序
	sort(array, mid + 1, right);
	// 合并
	merge(array, left, mid, right);
}

private static void merge(int[] array, int left, int mid, int right) {
	int[] temp = new int[right - left + 1];
	int i = 0;
	int p1 = left;
	int p2 = mid + 1;
	// 比较左右两部分的元素，哪个小，把那个元素填入temp中
	while (p1 <= mid && p2 <= right) {
		temp[i++] = array[p1] < array[p2] ? array[p1++] : array[p2++];
	}
	// 上面的循环退出后，把剩余的元素依次填入到temp中
	// 以下两个while只有一个会执行，因为跳出循环的时候有一个不满足跳出，所以下方只有一个执行
	while (p1 <= mid) {
		temp[i++] = array[p1++];
	}
	while (p2 <= right) {
		temp[i++] = array[p2++];
	}
	// 把最终的排序的结果复制给原数组
	for (i = 0; i < temp.length; i++) {
		array[left + i] = temp[i];
	}
}


```

## 快速排序

快速排序的基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。

算法描述

快速排序使用分治法来把一个串（list）分为两个子串（sub-lists）。具体算法描述如下：

- 从数列中挑出一个元素，称为 “基准”（pivot）；
- 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
- 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

```java

/**
 * Description: 快速排序
 *
 * @param array
 * @return void
 * @author JourWon
 * @date 2019/7/11 23:39
 */
public static void quickSort(int[] array) {//重载
	quickSort(array, 0, array.length - 1);
}


private static void quickSort(int[] array, int left, int right) {
	if (array == null || left >= right || array.length <= 1) {
		return;
	}
	int mid = partition(array, left, right);
	quickSort(array, left, mid);
	quickSort(array, mid + 1, right);
}


private static int partition(int[] array, int left, int right) {
	int temp = array[left];//定义pivot
	while (right > left) {
		// 先判断基准数和后面的数依次比较
		while (temp <= array[right] && left < right) {
			--right;
		}
		// 当基准数大于了 arr[left]，则填坑
		if (left < right) {
			array[left] = array[right];
			++left;
		}
		// 现在是 arr[right] 需要填坑了
		while (temp >= array[left] && left < right) {
			++left;
		}
		if (left < right) {
			array[right] = array[left];
			--right;
		}
	}
	array[left] = temp;
	return left;
}

```