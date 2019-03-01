---
title: LeetCode Algorithm Binary Search
date: 2018-12-13 10:15:48
tags: LeetCode BinarySearch
---

## 二分查找

### 正常实现

```java
public int binarySearch(int[] nums, int key) {
    int low = 0, high = nums.length - 1;
    while (low <= high) {
      // notice the <=
        int mid = low + (high - low) / 2;
        if (nums[mid] == key) {
            return mid;
        } else if (nums[mid] > key) {
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return -1;
}
```

### 时间复杂度

二分查找也称为折半查找，每次都能将查找区间减半，这种折半特性的算法时间复杂度为 O(logN)。

### 中值mid的计算

有两种计算中值 m 的方式：
* mid = (low + high) / 2
* mid = low + (high - low) / 2
low + high 可能出现加法溢出，最好使用第二种方式。

### 变种

二分查找可以有很多变种，变种实现要注意边界值的判断。例如在一个有重复元素的数组中查找 key 的最左位置的实现如下：
```java
public int binarySearch(int[] nums, int key) {
    int l = 0, h = nums.length - 1;
    while (l < h) {
        int m = l + (h - l) / 2;
        if (nums[m] >= key) {
            h = m;
        } else {
            l = m + 1;
        }
    }
    return l;
}
```
该实现和正常实现有以下不同：
* 循环条件为 l < h
* h 的赋值表达式为 h = m
* 最后返回 l 而不是 -1

在 nums[m] >= key 的情况下，可以推导出最左 key 位于 [l, m] 区间中，这是一个闭区间。h 的赋值表达式为 h = m，因为 m 位置也可能是解。

在 h 的赋值表达式为 h = mid 的情况下，如果循环条件为 l <= h，那么会出现循环无法退出的情况，因此循环条件只能是 l < h。以下演示了循环条件为 l <= h 时循环无法退出的情况：
```java
nums = {0, 1, 2}, key = 1
l   m   h
0   1   2  nums[m] >= key
0   0   1  nums[m] < key
1   1   1  nums[m] >= key
1   1   1  nums[m] >= key
...
```
当循环体退出时，不表示没有查找到 key，因此最后返回的结果不应该为 -1。为了验证有没有查找到，需要在调用端判断一下返回位置上的值和 key 是否相等。

### x 的平方根

[69. x 的平方根(Easy)](https://leetcode-cn.com/problems/sqrtx/description/)

思路：二分查找
非负整数，只保留整数部分。
可在[0,x]之间查找，且结果应满足 sqrt = x/sqrt
low = 0; high = x;
1. 当x<=1时，return x
2. 当low<=high循环查找
   mid = low + (high - low)/2
   sqrt = x/mid;
   if(mid == sqrt),return mid
   else if(mid>sqrt), high = mid - 1
   else low = mid + 1;
3. return h
> 对于 x = 8，它的开方是 2.82842...，最后应该返回 2 而不是 3。在循环条件为 l <= h 并且循环退出时，h 总是比 l 小 1，也就是说 h = 2，l = 3，因此最后的返回值应该为 h 而不是 l。

```java
public int mySqrt(int x) {
    if(x <= 1){
        return x;
    }
    int low = 0, high = x;
    while(low <= high){
        int mid = low + (high - low)/2;
        int sqrt = x/mid;
        if(mid == sqrt){
            return mid;
        }else if(mid > sqrt){
            high = mid - 1;
        }else{
            low = mid + 1;
        }
    }
    return high;
}
```

### 寻找比目标字母大的最小字母

[744. 寻找比目标字母大的最小字母(Easy)](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/description/)

思路：二分查找
寻找有序数组里面比目标字母大的最小字母
low = 0; high = length;
1. 二分查找数组，low<high时循环,mid = low + (high - low)/2;
    * letters[mid] > target, high =mid;
    * letters[mid] <= target, low = mid + 1;
2. 正常return letters[high]（high与low皆可）;考虑边界情况：
* 若是target 小于数组中的所有字符，返回第一个字符，正常return letters[high];
* 若是target 大于数组中的所有字符，亦返回第一个字符（要求：数组里字母的顺序是循环的）；
notice:
* low<high ，由于high=mid有不变的情况，若想等则可能死循环

```java
public char nextGreatestLetter(char[] letters, char target) {
    int low = 0, high = letters.length - 1;
    while(low < high){
        int mid = low + (high - low)/2;
        if(letters[mid] > target){
            // notice >=, high = mid
            high = mid;
        }else{
            // letters[mid] < target
            low = mid + 1;
        }
    }
    if(high == letters.length - 1 && target >= letters[high]){

        return letters[0];
    }
    return letters[high];
}
```

### 寻找有序数组中的单一元素

[540. 有序数组中的单一元素(Medium)](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/description/)


思路：
low = 0, high = nums.length - 1
while(low <= high){
    mid = low + (high - low)/2
    如何二分？ 有序数组！
    令目标为target

    mid偶数(对应数组位置eg 0.2)，if (nums[mid] == nums[mid+1]){
        此时target应在[mid+2,high],即low = mid + 2;
    }else{
        此时考虑target是否就是mid位置，或在[low mid-2]
        即if(nums[mid] != nums[mid-1]) return nums[mid];
        else high = mid -2
    }

    mid奇数(对应数组位置eg 1.3),if(nums[mid] == nums[mid-1]){
        此时target应在[mid+1,high],即low = mid + 1;
    }else{
        此时考虑target是否就是mid位置，或在[low mid-1]
        即if(nums[mid] != nums[mid+1]) return nums[mid];
        else high = mid - 1
    }
}
return nums[low]

```Java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int low = 0, high = nums.length - 1;
        while(low < high){
            int mid = low + (high - low)/2;
            if(mid%2 == 0){
                if(nums[mid] == nums[mid+1]){
                    low = mid + 2;
                }else{
                    if(nums[mid] != nums[mid-1]){
                        return nums[mid];
                    }else{
                        high = mid - 2;
                    }
                }    
            }else{
                if(nums[mid] == nums[mid-1]){
                    low = mid + 1;
                }else{
                    if(nums[mid] != nums[mid+1]){
                        return nums[mid];
                    }else{
                        high = mid - 1;
                    }
                }
            }

        }
        return nums[low];
    }
}

```

other思路：二分查找
low = 0; high = length-1;
eg: 012345678
    003324488
假设targetIndex为单一元素在数组中的位置，
若mid为偶数！且m+1<targetIndex, 则有nums[m] == nums[m+1];即targetIndex在[m+2,high],即 low = mid + 2
若mid为偶数！且m+1>=targetIndex, 则有nums[m] != nums[m+1];即targetIndex在[low,m],即 high = mid
return high(low)
notice:
* low<high ，由于high=mid有不变的情况，若相等则可能死循环

```java
public int singleNonDuplicate(int[] nums) {
    int low = 0, high = nums.length - 1;
    while(low < high){
        int mid = low + (high - low)/2;
        if(mid % 2 == 1){
            // make sure mid is even
            mid--;
        }
        if(nums[mid] == nums[mid+1]){
            low = mid + 2;
        }else{
            // nums[mid] != nums[mid+1]
            high = mid;
        }
    }
    return nums[high];
}
```

### 第一个错误的版本

[278. 第一个错误的版本(Easy)](https://leetcode-cn.com/problems/first-bad-version/description/)

思路：二分查找
第一个错误的版本target，以错误的版本之后的所有版本都是错的
low = 0; high = length-1;mid = low + (high - low)/2
* isBadVersion(mid)==true, target在[low,mid],high = mid;
* isBadVersion(mid)==false, target在[mid+1,high],low = mid + 1;
return high
notice:
* low<high ，由于high=mid有不变的情况，若相等则可能死循环

```java
public int firstBadVersion(int n) {
    int low = 1, high = n;
    while(low < high){
        int mid = low + (high - low)/2;
        if(isBadVersion(mid)){
            high = mid;
        }else{
            //isBadVersion(mid) == false
            low = mid + 1;
        }
    }
    return high;
}
```

### 寻找旋转排序数组中的最小值

[153. 寻找旋转排序数组中的最小值(Medium)](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/description/)

思路：二分查找
low = 0; high = length-1;mid = low + (high - low)/2
* nums[mid] < nums[high], min在[low,mid], high = mid;
* nums[mid] > nums[high], min在[mid+1,high],low = mid+1
return nums[high]
notice:
* low<high ，由于high=mid有不变的情况，若相等则可能死循环
* compare nums[mid]~nums[high]

```java
public int findMin(int[] nums) {
    int low = 0, high = nums.length - 1;
    while(low < high){
        int mid = low + (high - low)/2;
        if(nums[mid] < nums[high]){
            high = mid;
        }else{
            // nums[mid] > nums[high]
            low = mid + 1;
        }
    }
    return nums[high];
}
```

### 在排序数组中查找元素的第一个和最后一个位置

[34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

思路：要求时间复杂度O(logN),二分查找

```Java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        int anslow = -1, anshigh = -1;
        while(low <= high){
            int mid = low + (high - low)/2;
            if(nums[mid] == target){
                // judge []
                anslow = anshigh = mid;
                while((anslow >= 1) && (nums[anslow-1] == target)){
                    anslow--;
                }
                while((anshigh <= nums.length - 2) && (nums[anshigh+1] == target)){
                    anshigh++;
                }
                break;
            }else if(nums[mid] < target){
                low = mid + 1;
            }else{
                high = mid - 1;
            }
        }
        return new int[]{anslow,anshigh};
    }
}
```

### 分治 为运算表达式设计优先级

[241 为运算表达式设计优先级](https://leetcode-cn.com/problems/different-ways-to-add-parentheses/)

```java
class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> ans = new ArrayList<Integer>();
        for(int i = 0; i < input.length(); i++){
            char c = input.charAt(i);
            if(c == '+' || c == '-' || c == '*'){
                List<Integer> left = new ArrayList<Integer>();
                List<Integer> right = new ArrayList<Integer>();
                left = diffWaysToCompute(input.substring(0,i));
                right = diffWaysToCompute(input.substring(i+1));
                for(int l : left){
                    for(int r : right){
                       switch(c){
                           case '+':
                               ans.add(l+r);
                               break;
                           case '-':
                               ans.add(l-r);
                               break;
                           case '*':
                               ans.add(l*r);
                               break;

                       }
                    }
                }


            }

        }
        if(ans.size() == 0){
            ans.add(Integer.valueOf(input));
        }
        return ans;
    }
}

/*
分治算法，考虑将每个表达式分解成左右两个表达式，递归求解剩下的值。
当递归到只剩一个数字，或者只剩数字+‘运算符’+数字时直接进行笛卡尔计算。
考虑用ArrayList，注意返回值
ps: string.length(); string.substring(); ArrayList.size();
*/
```
