## 十大排序算法

- [插入排序](#插入排序) 
  - [直接插入排序](#直接插入排序) 
  - [希尔排序](#希尔排序) 
- [选择排序](#选择排序) 
  - [简单选择排序](#简单选择排序) 
  - [堆排序](#堆排序) 
- [交换排序](#交换排序) 
  - [冒泡排序](#冒泡排序) 
  - [快速排序](#快速排序) 
- [归并排序](#归并排序) 
- [基数排序](#基数排序) 
- [计数排序](#计数排序) 
- [桶排序](#桶排序) 

### 排序算法框架

![](./sort/classification.png) 

### 排序算法性质

![](./sort/analysis.png) 

### 插入排序

#### 直接插入排序

1. 从第一个元素开始，认为该元素是已排序的。
2. 取出下一元素，与前面已经排好序的部分进行比较。
3. 若比排好序部分的元素小，则将排好序部分的元素后移到下一位置。
4. 遍历数组，直至结束。

最好的情况是数组有序，时间复杂度为 $O(n)$ ，平均复杂度是 $O(n^2)$ 。

![](./sort/insert.gif)

```java
public class Solution {
	public static void main(String[] args) {
		int[] array = {8, 1, 4, 9, 3, 5, 2, 7, 0, 6};
		insertSort(array);
		System.out.println(Arrays.toString(array));
	}

	private static void insertSort(int[] array) {
		for (int i = 0; i < array.length - 1; i++) {
			int data = array[i + 1];
			int index = i;
			while(index >= 0 && array[index] > data) {
				array[index + 1] = array[index];
				index--;
			}
			array[index + 1] = data;
		}
	}
}

```

#### 希尔排序

![](./sort/shell.png) 

时间复杂度为 $O(nlogn)$ 。

```java
public class Solution {
	public static void main(String[] args) {
		int[] array = {8, 9, 1, 7, 2, 3, 5, 4, 6, 0};
		shellSort(array);
		System.out.println(Arrays.toString(array));
	}

	private static void shellSort(int[] array) {
		int gap = array.length / 2;
		while (gap > 0) {
			for (int i = gap; i < array.length; i++) {
				int index = i - gap;
				int temp = array[i];
				while (index >= 0 && array[index] > temp) {
					swap(array, index, index + gap);
					index -= gap;
				}
//				array[index + gap] = temp;
			}
			gap /= 2;
			System.out.println(Arrays.toString(array));
		}
	}

	private static void swap(int[] array, int i, int index) {
		int temp = array[i];
		array[i] = array[index];
		array[index] = temp;
	}
}
```

### 选择排序

#### 简单选择排序

1. 从未排序的初始数组中寻找最小元素放置首位。
2. 从剩余元素中继续寻找最小元素，放到已排序序列的尾部
3. 遍历数组，直至结束。

时间复杂度为 $O(n^2)$ 。

![](./sort/selection.gif) 

```java
public class Solution {
	public static void main(String[] args) {
		int[] array = {8, 1, 4, 9, 3, 5, 2, 7, 0, 6};
		selectionSort(array);
		System.out.println(Arrays.toString(array));
	}

	private static void selectionSort(int[] array) {
		for (int i = 0; i < array.length; i++) {
			int index = i;
			for (int j = i; j < array.length; j++) {
				if (array[j] < array[index]) {
					index = j;
				}
			}
			swap(array, index, i);
		}
	}

	private static void swap(int[] array, int index, int i) {
		int temp = array[index];
		array[index] = array[i];
		array[i] = temp;
	}
}
```

#### 堆排序

时间复杂度为 $O(nlogn)$ 。

![](./sort/heap.gif) 

```java
public class Solution {
	// 建堆
	public static void creatHeap(int[] arr, int n) {
		// 因为数组是从0开始的
		for (int i = (n - 1) / 2; i >= 0; i--) {
			percolateDown(arr, i, n);
		}
	}
	// 插入
	private static void insertHeap(int[] array, int data, int n) {
		array[n] = data;
		percolatrUp(array, n);
	}
	// 删除栈顶元素
	private static void deleteHeap(int[] arr, int n) {
		arr[0] = arr[n];
		arr[n] = -1;
		percolateDown(arr, 0, n - 1);
	}
	// 上浮
	private static void percolatrUp(int[] array, int n) {
		int data = array[n];
		int father = (n - 1) / 2;
		while (data < array[father] && father >= 0) {
			array[n] = array[father];
			array[father] = data;
			n = father;
			father = (n - 1) / 2;
		}
		array[father] = data;
	}
	// 下滤
	private static void percolateDown(int[] arr, int i, int n) {
		int father = arr[i];
		int child = 2 * i + 1;
		// 遍历整个该根结点的子树
		while (child <= n) {
			// 定位左右结点小的那一个
			if (child + 1 <= n && arr[child + 1] < arr[child]) {
				child += 1;
			}
			// 若根结点比子结点小，说明已经是个小堆
			if (father < arr[child]) {
				break;
			}
			// 互换根结点和子结点
			arr[i] = arr[child];
			arr[child] = father;
			// 重新定位根结点和子结点
			i = child;
			child = i * 2 + 1;
		}
	}
    
	public static void main(String[] args) {
		int[] array = { 15, 13, 12, 5, 20, 1, 8, 9 };
		
		creatHeap(array, array.length - 1);
		System.out.println(Arrays.toString(array));
		
		deleteHeap(array, array.length - 1);
		System.out.println(Arrays.toString(array));
		
		deleteHeap(array, array.length - 2);
		System.out.println(Arrays.toString(array));
		
		insertHeap(array, 3, array.length - 2);
		System.out.println(Arrays.toString(array));
	}
}
```

### 交换排序

#### 冒泡排序

1. 依次比较相邻的两个元素，若前者比后者大则交换，这样数组的最后一位是最大值。
2. 在除了最后一位的未排序数组上继续重复以上步骤，每一步都能找到一个最大值放在后面。
3. 遍历数组，直至结束。

最好的情况是数组已排序，时间复杂为 $O(n)$ ，平均时间复杂度为 $O(n^2)$ 。

![](./sort/bubble.gif) 

```java
import java.util.Arrays;
public class Solution {
	
	private static void bubbleSort(int[] nums) {
		// 循环次数
		for (int i = 0; i < nums.length - 1; i++) {
			// 比较次数
			for (int j = 0; j < nums.length - 1 - i; j++) {
				if (nums[j] > nums[j + 1]) {
					swap(nums, j, j + 1);
				}
			}
		}
	}

	private static void swap(int[] nums, int j, int i) {
		int temp = nums[j];
		nums[j] = nums[i];
		nums[i]= temp; 
	}

	public static void main(String[] args) {
		int[] nums = { 6, 3, 8, 2, 9, 1 };
		bubbleSort(nums);
		System.out.println(Arrays.toString(nums));
	}
}
```

#### 快速排序



时间复杂度为 $O(nlogn)$ 。

![](./sort/quick.gif) 

```java
public class Solution {
	
	// Median-of-Three Partitioning
	public static int selectPivot(int[] array, int left, int right) {
		int middle = (left + right) / 2;
		
		if (array[middle] > array[right])
			swap(array, middle, left);
		if (array[left] > array[right])
			swap(array, left, right);
		if (array[middle] > array[left])
			swap(array, left, middle);
		
		return array[left];
	}
	
	public static void sort(int[] array, int left, int right) {
		if (left >= right)
			return;
		int index = partition(array, left, right);
		sort(array, left, index - 1);
		sort(array, index + 1, right);
    }
	
	public static int partition(int[] array, int left, int right){
        int pivot = selectPivot(array, left, right);
        while(left < right){
            while(left < right && array[right] >= pivot){
                right--;
            }
            if (left < right) {
                array[left++] = array[right];
            }
            while(left < right && array[left] < pivot){
                left++;
            }
            if (left < right) {
                array[right--] = array[left];
            }
        }
    	array[right] = pivot;
        return right;
    }

    public static void swap(int[] array, int left, int right){
    	int value = array[left];
    	array[left] = array[right];
    	array[right] = value;
    }

	public static void main(String[] args) {
		int[] array = {8, 1, 4, 9, 3, 5, 2, 7, 0, 6};
		// System.out.println(Arrays.toString(array));
		sort(array, 0, array.length - 1);
		System.out.println(Arrays.toString(array));
	}
}
```

### 归并排序

1. 将长序列从中间分成两个子序列。
2. 对这两个子序列依次继续执行重复分裂，直至不能再分。
3. 递归返回两两排好序的子序列。

平均时间复杂度为 $O(nlogn)$ 。

![](./sort/merge.gif)  

```java
public class Solution {
	public static void main(String[] args) {
		int[] array = {8, 9, 1, 7, 2, 3, 5, 4, 6, 0};
		int[] arr = MergeSort(array);
		System.out.println(Arrays.toString(arr));
	}

	private static int[] MergeSort(int[] array) {
		if (array.length < 2)
			return array;
		int middle = array.length / 2;
		int[] leftArray = Arrays.copyOfRange(array, 0, middle);
		int[] rightArray = Arrays.copyOfRange(array, middle, array.length);
		return merge(MergeSort(leftArray), MergeSort(rightArray));
	}

	private static int[] merge(int[] leftArray, int[] rightArray) {
		int[] result = new int[leftArray.length + rightArray.length];
		for (int index = 0, i = 0, j = 0; index < result.length; index++) {
			if (i >= leftArray.length) {
				result[index] = rightArray[j++];
			} else if (j >= rightArray.length) {
				result[index] = leftArray[i++];
			} else if (leftArray[i] > rightArray[j]) {
				result[index] = rightArray[j++];
			} else {
				result[index] = leftArray[i++];
			}
		}
		return result;
	}
}

```

### 基数排序

1. 找到数组中最大的数，确定最多一共有几位数。
2. 按照每个数字的最后一位，放入辅助数组中；同时设置一个计数数组，统计以数字 i 结尾的数字个数。
3. 将辅助数组中的元素重新放入原数组中，然后按照下一位继续重复以上动作。

时间复杂度为 $O(n*k)$ 。

![](./sort/radix.gif) 

```java
public class RadixSort {

	public static void main(String[] args) {
		int[] array = {3, 44, 38, 4, 47, 15, 36, 26, 27, 2, 46, 4, 19, 50, 32};
		int[] arr = radixSort(array);
		System.out.println(Arrays.toString(arr));
	}

	private static int[] radixSort(int[] array) {
		if (array == null || array.length < 2) {
			return array;
		}
		// 根据最大值找到最大位数
		int max = 0;
		for (int i = 0; i < array.length; i++) {
			max = Math.max(max, array[i]);
		}
		
		int maxDigit = 0;
		while (max != 0) {
			max /= 10;
			maxDigit++;
		}
		
		// 第一维: 0~9
		int[][] radix = new int[10][array.length];
		// 该位为 i 的元素个数
		int[] count = new int[10];
		
		int m = 1;
		int n = 1;
		
		while (m <= maxDigit) {
			for (int i = 0; i < array.length; i++) {
				int lsd = (array[i] / n) % 10;
				radix[lsd][count[lsd]] = array[i];
				count[lsd]++;
			}
			for (int i = 0, k = 0; i < 10; i++) {
				if (count[i] != 0) {
					for (int j = 0; j < count[i]; j++) {
						array[k++] = radix[i][j];
					}
				}
				count[i] = 0;
			}
			n *= 10;
			m++;
		}
		return array;
	}

}
```

### 计数排序

1. 找到数组中最小值和最大值，辅助数组的大小为两者之差。设最小值为 2，最大值为 9，则辅助数组大小为 7。
2. 统计数组中每个元素出现的次数，减去最小值，存入辅助数组中。比如 2，存放在辅助数组的第 0 位，7 放在辅助数组的第 5 位。
3. 最后反向填充数组。遍历原数组，依次将辅助数组中不为 0 的元素下标加最小值，放回原数组对应位置。

时间复杂度为 $O(n + k)$ 。

![](./sort/count.gif) 

```java
public class Solution {

	public static void main(String[] args) {
		int[] array = {8, 9, 4, 7, 2, 3, 5, 4, 6, 8};
		int[] arr = countSort(array);
		System.out.println(Arrays.toString(arr));
	}

	private static int[] countSort(int[] array) {
		if (array.length == 0)
			return array;
		
		int min = array[0], max = array[0];
		
		for (int i = 0; i < array.length; i++) {
			if (min > array[i]) {
				min = array[i];
			}
			if (max < array[i]) {
				max = array[i];
			}
		}
		
		int[] count = new int[max - min + 1];
		
		for (int i = 0; i < array.length; i++) {
			count[array[i] - min]++;
		}
		
		int i = 0;
		int index = 0;
		while (index < array.length) {
			if (count[i] != 0) {
				array[index] = i + min;
				count[i]--;
				index++;
			} else {
				i++;
			}
		}
		return array;
	}
	
}
```

### 桶排序





### Reference

https://www.cnblogs.com/guoyaohua/p/8600214.html 