---
title: LeetCode Algorithm Greedy
date: 2018-11-14 09:57:48
tags: LeetCode Greedy
---

## 贪心思想

保证每次操作都是局部最优的，并且最后得到的结果是全局最优的。

### 分配饼干

[455. Assign Cookies (Easy)](https://leetcode.com/problems/assign-cookies/description/)

``` Java
Input: [1,2,3], [1,1]
Output: 1
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3.
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
Input: [1,2], [1,2,3]
Output: 2
Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2.
You have 3 cookies and their sizes are big enough to gratify all of the children,
You need to output 2.
```

题目描述：每个孩子都有一个满足度，每个饼干都有一个大小，只有饼干的大小大于等于一个孩子的满足度，该孩子才会获得满足。求解最多可以获得满足的孩子数量。
给一个孩子的饼干应当尽量小又能满足该孩子，这样大饼干就能拿来给满足度比较大的孩子。因为最小的孩子最容易得到满足，所以先满足最小的孩子。
> 证明：假设在某次选择中，贪心策略选择给当前满足度最小的孩子分配第 m 个饼干，第 m 个饼干为可以满足该孩子的最小饼干。假设存在一种最优策略，给该孩子分配第 n 个饼干，并且 m < n。我们可以发现，经过这一轮分配，贪心策略分配后剩下的饼干一定有一个比最优策略来得大。因此在后续的分配中，贪心策略一定能满足更多的孩子。也就是说不存在比贪心策略更优的策略，即贪心策略就是最优策略。

```java
public int findContentChildren(int[] g, int[] s) {
    Arrays.sort(g);
    Arrays.sort(s);
    int gN = 0, sN = 0;
    while(gN < g.length && sN < s.length){
        if(g[gN] <= s[sN]){
            gN++;
        }
        sN++;
    }
    return gN;
}
```

### 计算让一组区间不重叠所需要移除的区间个数

[435. Non-overlapping Intervals (Medium)](https://leetcode.com/problems/non-overlapping-intervals/description/)

```Java
Input: [ [1,2], [2,3], [3,4], [1,3] ]
Output: 1
Explanation: [1,3] can be removed and the rest of intervals are non-overlapping.
```

题目描述：计算让一组区间不重叠所需要移除的区间个数。
先计算最多能组成的不重叠区间个数，然后用区间总个数减去不重叠区间的个数。
在每次选择中，区间的结尾最为重要，选择的区间结尾越小，留给后面的区间的空间越大，那么后面能够选择的区间个数也就越大。
按区间的结尾进行排序，每次选择结尾最小，并且和前一个区间不重叠的区间。

> [Java中几种常见的比较器的实现方法](https://blog.csdn.net/danmoqianhua/article/details/50557809)
> [Java 自定义比较器](https://blog.csdn.net/whing123/article/details/77851737)
> [Java中Lambda表达式的使用](https://www.cnblogs.com/franson-2016/p/5593080.html)
> Arrays:
> static <T> void	sort​(T[] a, Comparator<? super T> c)	Sorts the specified array of objects according to the order induced by the specified comparator. 根据指定对象的顺序对指定的对象数组进行排序
> static <T> Comparator<T>	comparingInt​(ToIntFunction<? super T> keyExtractor)	Accepts a function that extracts an int sort key from a type T, and returns a Comparator<T> that compares by that sort key.从类型T中提取int排序键的函数，并返回一个比较器<T>，通过该排序键进行比较。

```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
 public int eraseOverlapIntervals(Interval[] intervals) {
     if(intervals.length == 0){
         return 0;
     }
     Arrays.sort(intervals, new IntervalComparator());
     int noOverlapCount = 1;
     // begin with one
     int nowEnd = intervals[0].end;
     for(int i = 1; i < intervals.length; i++){
         if(intervals[i].start < nowEnd){
             // overlapping
             continue;
         }
         nowEnd = intervals[i].end;
         noOverlapCount++;
     }
     return intervals.length - noOverlapCount;
 }

class IntervalComparator implements Comparator<Interval> {
	@Override
	public int compare(Interval o1, Interval o2) {
		if(o1.end > o2.end){
			return 1;
		}else if(o1.end < o2.end){
			return -1;
		}else{
			return 0;
		}
	}
}

```

```java
lambda 表示式：Arrays.sort(intervals, Comparator.comparingInt(o -> o.end));
使用 lambda 表示式创建 Comparator 会导致算法运行时间过长，如果注重运行时间，可以修改为内嵌Comparator 语句：
Arrays.sort(intervals, new Comparator<Interval>() {
    @Override
    public int compare(Interval o1, Interval o2) {
        return o1.end - o2.end;
    }
});
```

### 投飞镖刺破气球

[452. Minimum Number of Arrows to Burst Balloons (Medium)](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)

```Java
Input:
[[10,16], [2,8], [1,6], [7,12]]
Output:
2
Explanation:
One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).
```

题目描述：气球在一个水平数轴上摆放，可以重叠，飞镖垂直投向坐标轴，使得路径上的气球都会刺破。求解最小的投飞镖次数使所有气球都被刺破。
也是计算不重叠的区间个数，不过和 Non-overlapping Intervals 的区别在于，[1, 2] 和 [2, 3] 在本题中算是重叠区间。

> [二维数组的Comparator用法：同时将二维数组的两列作为条件](https://blog.csdn.net/youngogo/article/details/82155449)

```java
public int findMinArrowShots(int[][] points) {
    if(points.length == 0){
        return 0;
    }
    Arrays.sort(points, new PointsComparator());
    // sort by end
    int noOverlapCount = 1;
    int nowEnd = points[0][1];
    for(int i = 1; i < points.length; i++){
        if(points[i][0] <= nowEnd){
            continue;
        }
        noOverlapCount++;
        nowEnd = points[i][1];
    }
    return noOverlapCount;
}

class PointsComparator implements Comparator<int []> {
	@Override
	public int compare(int[] o1, int[] o2) {
		if(o1[1] > o2[1]){
			return 1;
		}else if(o1[1] < o2[1]){
			return -1;
		}else{
			return 0;
		}
	}
}
```

### 根据身高和序号重组队列

[406. Queue Reconstruction by Height(Medium)](https://leetcode.com/problems/queue-reconstruction-by-height/description/)

```Java
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]
Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

题目描述：一个学生用两个分量 (h, k) 描述，h 表示身高，k 表示排在前面的有 k 个学生的身高比他高或者和他一样高。
> 为了使插入操作不影响后续的操作，身高较高的学生应该先做插入操作，否则身高较小的学生原先正确插入的第 k 个位置可能会变成第 k+1 个位置。So 身高降序
> 身高降序、k 值升序，然后按排好序的顺序插入队列的第 k 个位置中。
> eg: people： [[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]] 排序后： [[7,0], [7,1], [6,1], [5,0], [5,2], [4,4]]
> 然后从数组people第一个元素开始，放入到数组result中，放入的位置就是离result开始位置偏移了元素第二个数字后的位置。如下：
> 1. people: [7,0] 插入到离开始位置偏移了0个距离的位置。 result: [[7,0]]
> 2. people: [7,1] 插入到离开始位置偏移了1个距离的位置，即插入到[7,0]的后面。 result: [[7,0], [7,1]]
> 3. people: [6,1] 插入到离开始位置偏移了1个距离的位置，即插入到[7,0]的后面。 result: [[7,0], [6,1], [7,1]] 此时体现身高降序排序的意义
> 4. people: [5,0] 插入到离开始位置偏移了0个距离的位置，即插入到[7,0]的前面。 result: [[5,0], [7,0], [6,1], [7,1]]
> 5. people: [5,2] 插入到离开始位置偏移了2个距离的位置，即插入到[7,0]的后面。 result: [[5,0], [7,0], [5,2], [6,1], [7,1]]
> 6. people: [4,4] 插入到离开始位置偏移了4个距离的位置，即插入到[6,1]的后面。 result: [[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
> 这种算法体现了元素第二个数字与其插入位置的关系

> ArrayList:
> void	add​(int index, E element)	Inserts the specified element at the specified position in this list.
> <T> T[]	toArray​(T[] a)	Returns an array containing all of the elements in this list in proper sequence (from first to last element); the runtime type of the returned array is that of the specified array.

```java
public int[][] reconstructQueue(int[][] people) {
    if(people.length == 0){
        return new int[0][0];
        // notice the return;
    }
    Arrays.sort(people,new PeopleComparator());
    // Sort in descending order of height; ascending order of k
    List<int []> queue = new ArrayList<>();
    for(int[] ele : people){
        queue.add(ele[1],ele);
        // need ascending order of k
    }
    return queue.toArray(new int[queue.size()][]);
}

class PeopleComparator implements Comparator<int []>{
    @Override
	public int compare(int[] o1, int[] o2) {
		if(o1[0] > o2[0]){
			return -1;
		}else if(o1[0] < o2[0]){
			return 1;
		}else{
			if(o1[1] > o2[1]){
                return 1;
            }else if(o1[1] < o2[1]){
                return -1;
            }else{
                return 0;
            }
		}
	}
}
```

### 划分字母区间

[763. 划分字母区间(Medium)](https://leetcode-cn.com/problems/partition-labels/description/)

题目描述：字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一个字母只会出现在其中的一个片段。返回一个表示每个字符串片段的长度的列表。
思路：
1. 统计每个字符'a~z'在String中最后出现的位置，可由一个int[26]保存
2. 遍历整个字符串，查看字符串第一个字符在字符串出现的最后位置p1，则由起始位置p0~该位置p1，为第一个子片段，然后继续判断该子片段内包含的下一个字符在字符串出现的最后位置p2，若p2 > p1，则第一个子片段更新为p0~p2，否则不变；继续判断子片段内剩余字符在字符串出现的最后位置，until该子片段所有包含的字符没有相同字符出现在子片段之外。则该片段为第一个最优子片段。记录该子片段个数，继续遍历字符串！可由一个List<integer> 保存

```logic
a b a b c b a c a d  e f  e  g  d  e  h  i  j  h  k  l  i  j
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
firstIndex=0
while(firstIndex(0) < S.length()(24)),           --------------while1
lastIndex(0)=firstIndex(0),

 for (int i(0) = firstIndex(0); i < S.length() && i <= lastIndex(0); i++)  ------------for1
index = 'a'~8,
// 判断子片段是否扩大，若扩大则需检查到最新片段结尾;
 if (index(8) > lastIndex(0)),lastindex(8)=index(8), 子片段需扩大，扩大则需检查到最新片段结尾;

 for (i=1; i < S.length() && i <= lastIndex(8); i++)  ------------for2
index = 'b'~5,
// 判断子片段是否扩大，若扩大则需检查到最新片段结尾;
 if (index(5) > lastIndex(8)),子片段不变，继续检查子片段剩余元素 ;

for (i=2; i < S.length() && i <= lastIndex(8); i++)  ------------for3
index = 'a'~8,
// 判断子片段是否扩大，若扩大则需检查到最新片段结尾;
 if (index(8) > lastIndex(8)),子片段不变，继续检查子片段剩余元素 ;
.....

// 子片段内的每个字符的同种元素都在改划分里，即可将该划分的个数加入结果partion中
partitions.add(lastIndex - firstIndex + 1);
// 从划分片段后的第一个元素继续寻找新的子片段
firstIndex = lastIndex + 1;
```

```java
public List<Integer> partitionLabels(String S) {
    int lastIndexInString[] = new int[26];
    for(int i = 0; i < S.length(); i++){
        lastIndexInString[char2Index(S.charAt(i))] = i;
    }
    List<Integer> partion = new ArrayList<>();
    int firstIndex = 0;
    while(firstIndex < S.length()){
        int lastIndex = firstIndex;
        for(int i = firstIndex; i < S.length() && i <= lastIndex; i++){
            int index = lastIndexInString[char2Index(S.charAt(i))];
            if(index > lastIndex){
                lastIndex = index;
            }
        }
        partion.add(lastIndex - firstIndex + 1);
        firstIndex = lastIndex + 1;
    }
    return partion;
}
public int char2Index(char c){
    return c - 'a';
}
```

### 种花问题

[605. 种花问题(Easy)](https://leetcode-cn.com/problems/can-place-flowers/description/)
思路：
1. 遍历花坛整个数组，若当前位置和pre，next都为0，则可种下一朵花，否则继续遍历，由此统计可种的花朵数count
2. 注意起始位置与结尾位置，只需要当前位置与next, or 当前位置与pre都为0即可。遍历此处时，可分别将pre和next置为0，即与普通判断相同无需多加判断

```java
public boolean canPlaceFlowers(int[] flowerbed, int n) {
    int length = flowerbed.length;
    int count = 0;
    for(int i = 0; i < length && count < n; i++){
        if(flowerbed[i] == 1)
            continue;
        int pre = i == 0 ? 0 : flowerbed[i - 1];
        int next = i == length - 1 ? 0 : flowerbed[i + 1];
        if(pre == 0 && next == 0){
            count++;
            flowerbed[i] = 1;
        }
    }
    return count >= n;
}
```

### 判断子序列

[392. 判断子序列(Easy)](https://leetcode-cn.com/problems/is-subsequence/description/)
思路：s，t
1. 遍历字符串s中的每个字符，查看在t中的位置
2. 可用String.indexOf​(Char c, int fromIndex),从某个位置后查询

```java
public boolean isSubsequence(String s, String t) {
    int index = -1;
    for(char c : s.toCharArray()){
        index = t.indexOf(c, index + 1);
        if(index == -1)
            return false;
    }
    return true;
}
```

### 非递减数列

[665. 非递减数列(Medium)](https://leetcode-cn.com/problems/non-decreasing-array/description/)
思路：
change = 0;决定是否需要改变
1. 遍历数组，若num[i-1] <= num[i],continue
2. 考虑num[i-1] > num[i]时需要如何修改num[i-1] or num[i]才能使之前的子序列为非递减子序列(可修改一次考虑Change最多为1)：
* 正常是修改num[i - 1] = num[i]，往小的改，取小的这样不影响后续操作 423-223
* 特殊需要修改num[i] = num[i-1]，整体扩大，对于 3423-3223（no），修改时若num[i-2]>num[i]，则需要往大了改，即num[i] = num[i-1]，且能保证num[i-1]为之前子序列最大值

```java
public boolean checkPossibility(int[] nums) {
    int change = 0;
    for(int i = 1; i < nums.length; i++){
        if(nums[i-1] <= nums[i]){
            continue;
        }else{
            // need change
            if(change == 1){
                return false;
            }
            change++;
            if((i-2)>=0 &&nums[i-2] > nums[i]){
                // special case 3423 change big
                nums[i] = nums[i-1];
            }else{
                // change small no influence after
                nums[i-1] = nums[i];
            }
        }
    }
    return true;
}
```

### 买卖股票的最佳时机 II

[122. 买卖股票的最佳时机 II(easy)](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/description/)
思路：
alreadyIn=0,判断是否已买入
maxProfit=0
1. 遍历数组
* price[i]<price[i+1],
    * alreadyIn=0, buy=price[i],alreadyIn=1;
    * alreadyIn=1, continue;
* price[i]>=price[i+1],
    * alreadyIn=0, continue;
    * alreadyIn=1, sale = price[i], maxProfit = sale - buy; alreadyIn=0;

```java
public int maxProfit(int[] prices) {
    int alreadyIn = 0;
    int profit = 0;
    int sale = 0, buy = 0;
    for(int i = 0; i < prices.length - 1; i++){
        if(prices[i] < prices[i+1]){
            if(alreadyIn == 0){
                buy = prices[i];
                alreadyIn = 1;
            }else{
                // alreadyIn = 1
                continue;
            }
        }else{
            // prices[i] >= prices[i+1]
            if(alreadyIn == 0){
                continue;
            }else{
                // alreadyIn = 1
                sale = prices[i];
                profit += sale - buy;
                alreadyIn = 0;
            }
        }
    }
    if(alreadyIn == 1){
        // if loop out and judge have in or not
        profit += prices[prices.length - 1] - buy;
    }
    return profit;
}
```

### 最大子序和

[53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/description/)
思路：
preSum = nums[0], 连续子数组，preSum保存着遍历至i时当前连续子数组的最大值
maxSum = preSum, maxSum 保存着遍历i时之前子序列(0~i-1)的最大值，添加一个元素不会影响之前子序列的最大值，可用贪心 注意初始值
1. 遍历数组
preSum为负值可跳过,preSum=num[i],否则为preSum=preSum+nums[i]
令maxSum为max(maxSum,preSum)

```java
public int maxSubArray(int[] nums) {

    int preConSum = nums[0];
     int maxSum = preConSum;
    for (int i = 1; i < nums.length; i++){
        if(preConSum > 0){
            preConSum = preConSum + nums[i];
        }else{
            preConSum = nums[i];
        }

        if(maxSum < preConSum){
            maxSum = preConSum;
        }
    }
    return maxSum;
}
```

### 买卖股票的最佳时机

[121. 买卖股票的最佳时机(Easy)](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/description/)
思路：
buy = nums[0]
sale = buy; //不能在买入股票前卖出股票。
nowProfit = maxProfit = 0;
1. 遍历数组
  1) num[i]<buy,
       nowProfit=sale - buy;
       maxProfit = max(maxProfit,nowProfit);   //判断是否更新profit
       buy = num[i]; sale = buy //重新买入由于不能在买入股票前卖出股票,无需考虑sale
  2) num[i]>buy,
     * sale < num[i], sale = num[i]
     * sale > num[i],continue;

2.遍历全局结束后还需判断是否更新profit
  nowProfit=sale - buy;
  maxProfit = max(maxProfit,nowProfit);   //判断是否更新profit

```java
public int maxProfit(int[] prices) {
    if(prices.length == 0){
        return 0;
    }
    int buy = prices[0];
    int sale = buy;
    int maxProfit = 0;
    int nowProfit = 0;
    for(int i = 1; i < prices.length; i++){
        if(prices[i] < buy){
            //判断是否更新profit
            nowProfit = sale - buy;
            if(nowProfit > maxProfit){
                maxProfit = nowProfit;
            }
            //重新买入由于不能在买入股票前卖出股票,无需考虑sale
            buy = prices[i];
            sale = buy;
        }else{
            //prices[i]>buy
            if(sale < prices[i]){
                sale = prices[i];
            }else{
                //sale >= prices[i]
                continue;
            }
        }
    }
    //判断是否更新profit
    nowProfit = sale - buy;
    if(nowProfit > maxProfit){
        maxProfit = nowProfit;
    }
    return maxProfit;
}
```
