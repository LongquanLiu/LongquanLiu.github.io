---
title: Leetcode Algorithm Double Pointer
date: 2018-11-02 15:04:54
tags: Leetcode
---

## 双指针

双指针主要用于遍历数组，两个指针指向不同的元素，从而协同完成任务。
### 有序数组的 Two Sum

[167. Two Sum II - Input array is sorted (Easy)](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/)
``` Java
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

``` java
public int[] twoSum(int[] numbers, int target) {
    int i = 0 , j = numbers.length - 1;
    while(i < j){
        int sum = numbers[i] + numbers[j];
        if(sum == target){
        return new int[] {i+1, j+1};
        }else if (sum < target){
            i++;
        }else{
            j--;
        }
    }
    return null;
}
```

### 两数平方和

[633. Sum of Square Numbers (Easy)](https://leetcode.com/problems/sum-of-square-numbers/description/)
``` Java
Input: 5
Output: True
Explanation: 1 * 1 + 2 * 2 = 5
```

``` java
public boolean judgeSquareSum(int c) {
    int i = 0, j = (int) Math.sqrt(c);
    while (i <= j) {
        int powSum = i * i + j * j;
        if (powSum == c) {
            return true;
        } else if (powSum > c) {
            j--;
        } else {
            i++;
        }
    }
    return false;
}
```

### 反转字符串中的元音字符

[345. Reverse Vowels of a String (Easy)](https://leetcode.com/problems/reverse-vowels-of-a-string/description/)
``` Java
Input: "hello"
Output: "holle"
Input: "leetcode"
Output: "leotcede"
```
> mind：使用双指针指向待反转的两个元音字符，一个指针从头向尾遍历，一个指针从尾到头遍历。
> Java detail:
> * 用一个静态final的HashSet<Character> vowels 保存所有元音字符
> * 用char[] result 保存最后返回结果 new String(result);
> * s.charAt(int i);vowels.contains(char c);

``` java
private final static HashSet<Character> vowels = new HashSet<>(Arrays.asList('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'));

public String reverseVowels(String s) {
    int i = 0, j = s.length() - 1;
    char[] result = new char[s.length()];
    while (i <= j) {
        char ci = s.charAt(i);
        char cj = s.charAt(j);
        if (!vowels.contains(ci)) {
            result[i++] = ci;
        } else if (!vowels.contains(cj)) {
            result[j--] = cj;
        } else {
            result[i++] = cj;
            result[j--] = ci;
        }
    }
    return new String(result);
}
```

### 回文字符串

[680. Valid Palindrome II (Easy)](https://leetcode.com/problems/valid-palindrome-ii/description/)
``` Java
Input: "aba"
Output: True
```

``` java
public boolean validPalindrome(String s) {
    int i = 0, j = s.length() - 1;
    while(i < j){
        if(s.charAt(i) == s.charAt(j)){
            i++;
            j--;
        }else{
            return isPalindrome(s, i, j-1) || isPalindrome(s, i+1, j);
        }
    }
    return true;
}
public boolean isPalindrome(String s, int i, int j){
    while(i < j){
        if(s.charAt(i) == s.charAt(j)){
            i++;
            j--;
        }else{
            return false;
        }
    }
    return true;
}
```

### 归并两个有序数组

[88. Merge Sorted Array (Easy)](https://leetcode.com/problems/merge-sorted-array/description/)
``` Java
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3
Output: [1,2,2,3,5,6]
```

``` java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int index1 = m - 1, index2 = n - 1;
    int indexMerge = m + n - 1;
    while (index1 >= 0 || index2 >= 0) {
        if (index1 < 0) {
            nums1[indexMerge--] = nums2[index2--];
        } else if (index2 < 0) {
            nums1[indexMerge--] = nums1[index1--];
        } else if (nums1[index1] > nums2[index2]) {
            nums1[indexMerge--] = nums1[index1--];
        } else {
            nums1[indexMerge--] = nums2[index2--];
        }
    }
}
```

### 判断链表是否存在环

[141. Linked List Cycle (Easy)](https://leetcode.com/problems/linked-list-cycle/description/)

``` java
public boolean hasCycle(ListNode head) {
    if (head == null) {
        return false;
    }
    ListNode l1 = head, l2 = head.next;
    while (l1 != null && l2 != null && l2.next != null) {
        if (l1 == l2) {
            return true;
        }
        l1 = l1.next;
        l2 = l2.next.next;
    }
    return false;
}
```

### 最长子序列

[524. Longest Word in Dictionary through Deleting (Medium)](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/description/)
``` Java
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]
Output:
"apple"
```

``` java
public String findLongestWord(String s, List<String> d) {
    String longestWord = "";
    for(String targeWord : d){
        int l1 = longestWord.length(), l2 = targeWord.length();
        if(l1 > l2 || (l1 == l2 && longestWord.compareTo(targeWord) < 0)){
            // with the smallest lexicographical order.
            continue;
        }else{
            if(isValid(s,targeWord)){
                longestWord = targeWord;
            }
        }
    }
    return longestWord;
}

public boolean isValid(String s, String targeWord){
    int i = 0, j = 0;
    while(i < s.length() && j < targeWord.length()){
        if(s.charAt(i) == targeWord.charAt(j)){
            j++;
        }
        i++;
    }
    return j == targeWord.length();
}
```
