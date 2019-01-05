---
title: Leetcode Algorithm Sort
date: 2018-11-05 17:21:58
tags: Leetcode Sort
---

## 排序

### 快速选择

用于求解 Kth Element 问题，使用快速排序的 partition() 进行实现。需要先打乱数组，否则最坏情况下时间复杂度为 O(N<sup>2</sup>)。

### 堆排序

用于求解 TopK Elements 问题，通过维护一个大小为 K 的堆，堆中的元素就是 TopK Elements。
堆排序也可以用于求解 Kth Element 问题，堆顶元素就是 Kth Element。
快速选择也可以求解 TopK Elements 问题，因为找到 Kth Element 之后，再遍历一次数组，所有小于等于 Kth Element 的元素都是 TopK Elements。
可以看到，快速选择和堆排序都可以求解 Kth Element 和 TopK Elements 问题。

#### Kth Element

[215. Kth Largest Element in an Array (Medium)](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)
题目描述：找到第 k 大的元素。

**排序**：时间复杂度 O(NlogN)，空间复杂度 O(1)
> Arrays:
> static void	sort​(int[] a) Sorts the specified array into ascending numerical order.
> The sorting algorithm is a Dual-Pivot Quicksort by Vladimir Yaroslavskiy, Jon Bentley, and Joshua Bloch. This algorithm offers O(n log(n)) performance on many data sets that cause other quicksorts to degrade to quadratic performance, and is typically faster than traditional (one-pivot) Quicksort implementations.

``` Java
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

``` java
public int findKthLargest(int[] nums, int k) {
    Arrays.sort(nums);
    return nums[nums.length - k];
}
```

**堆排序**：时间复杂度 O(NlogK)，空间复杂度 O(K)。
> PriorityQueue:
> * boolean add​(E e)	Inserts the specified element into this priority queue.
> * E peek​()	Retrieves, but does not remove, the head of this queue, or returns null if this queue is empty.
> * E	poll​()	Retrieves and removes the head of this queue, or returns null if this queue is empty.

``` Java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> pq = new PriorityQueue<>();
    // small heap
    for (int ele : nums){
        pq.add(ele);
        if(pq.size() > k){
          // keep the heap size is K
            pq.poll();
        }
    }
    return pq.peek();
}
```

**快速排序**：时间复杂度 O(N)，空间复杂度 O(1)。最坏情况下时间复杂度为 O(N<sup>2</sup>)

``` java
public int findKthLargest(int[] nums, int k) {
    int resultPosition = nums.length - k;
    int left = 0, right = nums.length - 1;
    while(left < right){
        int eachPosition = partition(nums,left,right);
        if(eachPosition == resultPosition){
            break;
        }else if(eachPosition < resultPosition){
            left = eachPosition + 1;
        }else{
            right = eachPosition - 1;
        }
    }
    return nums[resultPosition];
}
private int partition(int[] nums, int left, int right){
    int leftPoint = left, rightPoint = right + 1;
    // be careful about the leftPoint is no 0, rightPoint is no length + 1;
    while(true){
        // the sign is nums[left]
        while(nums[++leftPoint] < nums[left] && leftPoint < right);
        while(nums[--rightPoint] > nums[left] && rightPoint > left);
        if(leftPoint >= rightPoint){
            break;
        }
        swap(nums,leftPoint,rightPoint);
    }
    // the nums[rightPoint] must small than nums[left]
    swap(nums,left,rightPoint);
    return rightPoint;
}
private void swap(int[] a, int i, int j){
    int temp = a[i];
    a[i] = a[j];
    a[j] = temp;
}
```

### 桶排序

#### 出现频率最多的K个数

[347. Top K Frequent Elements (Medium)](https://leetcode.com/problems/top-k-frequent-elements/description/)

``` Java
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

设置若干个桶，每个桶存储出现频率相同的数，并且桶的下标代表桶中数出现的频率，即第 i 个桶中存储的数出现的频率为 i。
把数都放到桶之后，从后向前遍历桶，最先得到的 k 个数就是出现频率最多的的 k 个数。
> Map：Map< , > frequencyForNum = new HashMap<>();
> * V put​(K key, V value)	Associates the specified value with the specified key in this map (optional operation).
> * default V	getOrDefault​(Object key, V defaultValue)	Returns the value to which the specified key is mapped, or defaultValue if this map contains no mapping for the key.
> * Set<K>	keySet​()	Returns a Set view of the keys contained in this map.
> * V	get​(Object key)	Returns the value to which the specified key is mapped, or null if this map contains no mapping for the key.

> List: List<>[]buckets = new ArrayList[]; buckets[] = new ArrayList<>();
> * boolean	add​(E e)	Appends the specified element to the end of this list (optional operation).
> * boolean	addAll​(Collection<? extends E> c)	Appends all of the elements in the specified collection to the end of this list, in the order that they are returned by the specified collection's iterator (optional operation).

``` java
public List<Integer> topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> frequencyForNum = new HashMap<>();
    // key : num, value : frequency;
    for (int num : nums) {
        frequencyForNum.put(num, frequencyForNum.getOrDefault(num, 0) + 1);
        // be careful about the + 1 position;
    }
    List<Integer>[] buckets = new ArrayList[nums.length + 1];
    // assume frequency is sub so need nums.length+1;
    for (int key : frequencyForNum.keySet()) {
        int frequency = frequencyForNum.get(key);
        if (buckets[frequency] == null) {
            buckets[frequency] = new ArrayList<>();
            // new a element to put, notice the constructor;
        }
        buckets[frequency].add(key);
    }
    List<Integer> topK = new ArrayList<>();
    for (int i = buckets.length - 1; i >= 0 && topK.size() < k; i--) {
        if (buckets[i] != null) {
            topK.addAll(buckets[i]);
        }
    }
    return topK;
}
```

#### 按照字符出现次数对字符串排序

[451. Sort Characters By Frequency (Medium)](https://leetcode.com/problems/sort-characters-by-frequency/description/)

``` Java
Input:
"tree"
Output:
"eert"
Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.

Input:
"cccaaa"
Output:
"cccaaa"
Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.
```

> StringBuilder：StringBuilder resultString = new StringBuilder();
> * StringBuilder	append​(char c)	Appends the string representation of the char argument to this sequence.
> * String	toString​()	Returns a string representing the data in this sequence.

``` java
public String frequencySort(String s) {
    Map<Character,Integer> frequencyChar = new HashMap<>();
    for(char c : s.toCharArray()){
        frequencyChar.put(c,frequencyChar.getOrDefault(c,0) + 1);
    }

    List<Character>[] buckets = new ArrayList[s.length() + 1];
    for(char c : frequencyChar.keySet()){
        int value = frequencyChar.get(c);
        if(buckets[value] == null){
            buckets[value] = new ArrayList<>();
        }
        buckets[value].add(c);
    }

    StringBuilder resultString = new StringBuilder();
    for(int i = buckets.length - 1; i >= 0; i--){
        // notice the loop times -  bucket[0~buckets.length-1]
        if(buckets[i] == null){
            continue;
        }
        for(char c : buckets[i]){
            // every char need output frequency times;
            for(int j = 0; j < i; j++){
                resultString.append(c);
            }
        }
    }
    return resultString.toString();
}
```

#### 荷兰国旗问题

荷兰国旗包含三种颜色：红、白、蓝。
有三种颜色的球，算法的目标是将这三种球按颜色顺序正确地排列。
它其实是三向切分快速排序的一种变种，在三向切分快速排序中，每次切分都将数组分成三个区间：小于切分元素、等于切分元素、大于切分元素，而该算法是将数组分成三个区间：等于红色、等于白色、等于蓝色。

[75. Sort Colors (Medium)](https://leetcode.com/problems/sort-colors/description/)

``` Java
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

``` java
zero=-1;one=0;two=nums.length;6   202110
num[one]==2 --two swap(num,two,one); swap and don't know divide,so one stay  now two is 5       002112
false eg:
num[one]==0 ++zero swap(num,zero,one); swap and don't know divide, so one stay now zero is 0    002112
num[one]==0 ++zero swap(num,zero,one); swap and don't know divide, so one stay now zero is 1    002112
num[one]==0 ++zero swap(num,zero,one); swap and don't know divide, so one stay now zero is 2    200112
num[one]==2 --two swap(num,two,one); swap and don't know divide,so one stay  now two is 4       100122
num[one]==1 ++one; now one is 1                                                                 100122
num[one]==0 ++zero swap(num,zero,one); swap and don't know divide, so one stay now zero is 3    110022

make sure position is zero,one,two(zero -1, need efficient so two is nums.length); (one < two, keep loop)
zero=-1;one=0;two=nums.length;6   202110
num[one]==2 --two swap(num,two,one); swap a don't know value mean efficient one is a new element,so one stay now two is 5              002112
true eg:
num[one]==0 ++zero swap(num,zero,one) one++; zero move means one also move, now zero is 0, one is 1    002112
num[one]==0 ++zero swap(num,zero,one) one++; zero move means one also move, now zero is 1, one is 2    002112
num[one]==2 --two swap(num,two,one); swap and don't know divide,so one stay  now two is 4              001122
num[one]==1 ++one; now one is 3                                                                        001122
num[one]==1 ++one; now one is 4                                                                        001122
stop .. one = two = 4
```

``` java
public void sortColors(int[] nums) {
    int zero = -1, one = 0, two = nums.length;
    while(one < two){
        if(nums[one] == 0){
            ++zero;
            swap(nums,zero,one);
            one++;
        }else if(nums[one] == 2){
            --two;
            swap(nums,two,one);

        }else{
            ++one;
        }
    }
    return;
}

public void swap(int[] nums, int i, int j){
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
    return;
}
```
