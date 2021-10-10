---
layout: post
title:  "LeetCode第【262】场周赛题解"
date:   2021-10-11 00:08:25 +0800
categories: jekyll update
---
​	本文主要解析LeetCode在2021年10月11日早上10：30组织的第【262】场周赛题目，[竞赛链接](https://leetcode-cn.com/contest/weekly-contest-262)。有兴趣的同学可以关注一下我的LeetCode[个人账号](https://leetcode-cn.com/u/chen-xiao-bin-5/)，以及我的[个人主页](https://hzchenxiaobin.github.io/)。

------

### 题目一、5894. 至少在两个数组中出现的值【简单】 [题目链接](https://leetcode-cn.com/problems/two-out-of-three/)

#### 题目描述

> 给你三个整数数组 nums1、nums2 和 nums3 ，请你构造并返回一个 不同 数组，且由 至少 在 两个 数组中出现的所有值组成。数组中的元素可以按 任意 顺序排列。



#### 示例 1：

>输入：nums1 = [1,1,3,2], nums2 = [2,3], nums3 = [3]
>输出：[3,2]
>
>解释：至少在两个数组中出现的所有值为：
>	3 ，在全部三个数组中都出现过。
>	2 ，在数组 nums1 和 nums2 中出现过。



#### 示例 2：

> 输入：nums1 = [3,1], nums2 = [2,3], nums3 = [1,2]
> 输出：[2,3,1]
>
> 解释：至少在两个数组中出现的所有值为：
>
> 解释：至少在两个数组中出现的所有值为：
> 	2 ，在数组 nums2 和 nums3 中出现过。
> 	3 ，在数组 nums1 和 nums2 中出现过。
> 	1 ，在数组 nums1 和 nums3 中出现过。



#### 示例 3：

> 输入：nums1 = [1,2,2], nums2 = [4,3,3], nums3 = [5]
> 输出：[]
> 解释：不存在至少在两个数组中出现的值。



#### 提示：

> 1 <= nums1.length, nums2.length, nums3.length <= 100
> 1 <= nums1[i], nums2[j], nums3[k] <= 100



#### 分析

本题是一道简单题，计算在三个数组中至少重复两次的元素，直接用set模拟即可。



#### 代码

```java
class Solution {
    public List<Integer> twoOutOfThree(int[] nums1, int[] nums2, int[] nums3) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums1) {
            set.add(num);
        }

        Set<Integer> result = new HashSet<>();
        for (int num : nums2) {
            if (set.contains(num)) result.add(num);
            set.add(num);
        }

        //这里的set添加nums2不能与上一个循环合并的原因是nums2中的元素可能是重复的
        // 比如nums2数组为[1,2,2]时，会错误的把2加入到结果中
        for (int num : nums2) {
            set.add(num);
        }

        for (int num : nums3) {
            if (set.contains(num)) result.add(num);
        }

        return new ArrayList<>(result);
    }
}
```

------

### 第二题、5895.获取单值网格的最小操作数【中等】 [题目链接](https://leetcode-cn.com/problems/minimum-operations-to-make-a-uni-value-grid/)

#### 题目描述

> 给你一个大小为 m x n 的二维整数网格 grid 和一个整数 **x** 。每一次操作，你可以对 grid 中的任一元素 **加 x** 或 **减 x** 。
>
> **单值网格** 是全部元素都相等的网格。
>
> 返回使网格化为单值网格所需的 **最小** 操作数。如果不能，返回 -1 。

#### 示例 1：

> 输入：grid = [[2,4],[6,8]], x = 2
> 输出：4
> 解释：可以执行下述操作使所有元素都等于 4 ：	
>
> 2 加 x 一次。
> 6 减 x 一次。
> 8 减 x 两次。
> 共计 4 次操作。

####  示例 2：

> 输入：grid = [[1,5],[2,3]], x = 1
> 输出：5
> 解释：可以使所有元素都等于 3 。

####  示例 3：

> 输入：grid = [[1,2],[3,4]], x = 2
> 输出：-1
> 解释：无法使所有元素相等。

#### 提示：

> m == grid.length
> n == grid[i].length
> 1 <= m, n <= 10^5
> 1 <= m * n <= 10^5
> 1 <= x, grid[i][j] <= 10^4



#### 分析

本题是一道中等题
第一步、判断所有的元素对于x的余数是不是相同，如果不同则无法通过加/减x到达同一个数。
第二步、将所有的数通过加减x到达同一个数，这个数是这些数的中位数，所以求出中位数，然后统计每个元素到达这个数的操作次数即可。

#### 代码

```java
class Solution {
     public int minOperations(int[][] grid, int x) {
        int p = grid[0][0] % x;
        int m = grid.length, n = grid[0].length;
        if (m * n == 1) return 0;

        int[] arr = new int[10001];
        for (int[] row : grid) {
            for (int val : row) {
                //判断是不是能够通过加/减x 到达同一个数
                if ((val % x) != p) return -1;
                arr[val]++;
            }
        }

        //求中位数
        int target = 0;
        int count = 0;
        int size = (m * n) / 2 + 1;
        for (int i = 0; i < 10001; i++) {
            count += arr[i];
            if (count >= size) {
                target = i;
                break;
            }
        }

        //求操作次数
        int res = 0;
        for (int[] row : grid) {
           for(int val : row) {
                res += Math.abs(target -val) / x;
            }
        }

        return res;
    }
}
```



------

### 题目三、5896.股票价格波动【中等】 [题目链接](https://leetcode-cn.com/problems/stock-price-fluctuation/)

#### 题目描述

> 给你一支股票价格的数据流。数据流中每一条记录包含一个 **时间戳** 和该时间点股票对应的 **价格** 。
>
> 
>
> 不巧的是，由于股票市场内在的波动性，股票价格记录可能不是按时间顺序到来的。某些情况下，有的记录可能是错的。如果两个有相同时间戳的记录出现在数据流中，前一条记录视为错误记录，后出现的记录 **更正** 前一条错误的记录。
>
> 请你设计一个算法，实现：
>
> **更新** 股票在某一时间戳的股票价格，如果有之前同一时间戳的价格，这一操作将 **更正** 之前的错误价格。
>
> 找到当前记录里 **最新股票价格** 。最新股票价格 定义为时间戳最晚的股票价格。
>
> 找到当前记录里股票的 **最高价格** 。
>
> 找到当前记录里股票的 **最低价格** 。
>
> 请你实现 **StockPrice** 类：
>
> StockPrice() 初始化对象，当前无股票价格记录。
>
> void update(int timestamp, int price) 在时间点 timestamp 更新股票价格为 price 。
>
> int current() 返回股票 最新价格 。
>
> int maximum() 返回股票 最高价格 。
>
> int minimum() 返回股票 最低价格 。

 

#### 示例 1：

> 输入：
> ["StockPrice", "update", "update", "current", "maximum", "update", "maximum", "update", "minimum"]
> [[[], [1, 10], [2, 5], [], [], [1, 3], [], [4, 2], []]
>
> 输出：
> [null, null, null, 5, 10, null, 5, null, 2]
>
> 解释：
> StockPrice stockPrice = new StockPrice();
> stockPrice.update(1, 10); // 时间戳为 [1] ，对应的股票价格为 [10] 。
> stockPrice.update(2, 5);  // 时间戳为 [1,2] ，对应的股票价格为 [10,5] 。
> stockPrice.current();     // 返回 5 ，最新时间戳为 2 ，对应价格为 5 。
> stockPrice.maximum();     // 返回 10 ，最高价格的时间戳为 1 ，价格为 10 。
> stockPrice.update(1, 3);  // 之前时间戳为 1 的价格错误，价格更新为 3 .
>  // 时间戳为 [1,2] ，对应股票价格为 [3,5] 。
> stockPrice.maximum();     // 返回 5 ，更正后最高价格为 5 。
> stockPrice.update(4, 2);  // 时间戳为 [1,2,4] ，对应价格为 [3,5,2] 。
> stockPrice.minimum();     // 返回 2 ，最低价格时间戳为 4 ，价格为 2 。

 

#### 提示：

> 1 <= timestamp, price <= 10^9
> update，current，maximum 和 minimum 总 调用次数不超过 10^5 。
> current，maximum 和 minimum 被调用时，update 操作 **至少** 已经被调用过 一次 。



#### 分析

本题是一道中等题，对于Java的选手有比较大的优势。

本题可以抽象成：

1.相同的时间戳需要替换股票价格

2.求最大时间戳对应的值

3.求股票价格的最大值和最小值

1.根据第一条很容易想到通过map来存储时间戳和股票价格，由于需要查询最大时间戳对应的值，所以可以使用TreeMap。

2.根据第三条可以使用TreeSet或者TreeMap进行存储股票价格，由于股票价格可能会重复（在更换的时候需要剔除对应的股票价格），需要记录股票价格出现的次数，所以使用TreeMap存储股票价格和出现的次数比较合适。

比如:

1.update(1,1)

2.update(2,1)

3.update(3,2)

第3步需要更新时间戳为1的股价，所以会删除股价1，如果用TreeSet则不会记录1出现的次数，只能直接remove(1),导致时间戳为2的股价1也删除了，使用TreeMap时将1出现的次数2减1，还可以保留1。

#### 代码

```
class StockPrice {

    TreeMap<Integer, Integer> map = new TreeMap<>();
    TreeMap<Integer, Integer> priceMap = new TreeMap<>();

    public StockPrice() {

    }

    public void update(int timestamp, int price) {
        if (map.containsKey(timestamp)) {
            int prePrice = map.get(timestamp);
            int count = priceMap.get(prePrice);
            if (count == 1) {
                priceMap.remove(prePrice);
            } else {
                priceMap.put(prePrice, count - 1);
            }
        }
        map.put(timestamp, price);
        priceMap.put(price, priceMap.getOrDefault(price, 0) + 1);
    }

    public int current() {
        return map.lastEntry().getValue();
    }

    public int maximum() {
        return priceMap.lastKey();
    }

    public int minimum() {
        return priceMap.firstKey();
    }
}

/**
 * Your StockPrice object will be instantiated and called as such:
 * StockPrice obj = new StockPrice();
 * obj.update(timestamp,price);
 * int param_2 = obj.current();
 * int param_3 = obj.maximum();
 * int param_4 = obj.minimum();
 */
```

------

### 第四题、5897.将数组分成两个数组并最小化数组和的差【困难】 [题目链接](https://leetcode-cn.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference/)

#### 题目描述

> 给你一个长度为 2 * n 的整数数组。你需要将 nums 分成 两个 长度为 n 的数组，分别求出两个数组的和，并 **最小化** 两个数组和之 差的绝对值 。nums 中每个元素都需要放入两个数组之一。
>
> 请你返回 **最小** 的数组和之差。



#### 示例 1：

> 输入：nums = [3,9,7,3]
> 输出：2
>
> 解释：最优分组方案是分成 [3,9] 和 [7,3] 。
> 数组和之差的绝对值为 abs((3 + 9) - (7 + 3)) = 2 。



#### 示例 2：

>输入：nums = [-36,36]
>输出：72
>解释：最优分组方案是分成 [-36] 和 [36] 。
>数组和之差的绝对值为 abs((-36) - (36)) = 72 。



#### 示例 3：

>输入：nums = [2,-1,0,4,-2,-9]
>输出：0
>解释：最优分组方案是分成 [2,4,-9] 和 [-1,0,-2] 。
>数组和之差的绝对值为 abs((2 + 4 + -9) - (-1 + 0 + -2)) = 0 。

 

#### 提示：

>1 <= n <= 15
>nums.length == 2 * n
>-10^7 <= nums[i] <= 10^7

#### 分析

本题是一道困难题，稍微有点复杂。

1.将2n个数分成两个长度为n的数组，假设第一个长度为n的数组的和是sum1，第二个长度为n的数组的和是sum2，整个数组的和为sum，那么则有
    1. sum = sum1 + sum2
    2. 两数之差target = Math.abs(sum1 - sum2)
通过第1点和第2点,将sum1替换为sum - sum2，则可以推出
    3.. 两数之差target = Math.abs(sum - 2 * sum2)

可得若需要target越小，则2 * sum2 与sum的距离越小越好，可得结论：
【结论1】将2n个元素分成两个长度为n的数组，其中一个数组的和与总和的一半越接近越好。

本题也可以描述为：从长度为2n的数组中选出n个数，使得选出的数的和与数组的一半最接近，求该最接近的值。

2.对数据量最大的case进行分析
从30个数中选出15个数，那么有1.5*10^8中选法，在力扣上提交妥妥会超时。
考虑将数组分成两个长度为n的数组，从第一个数组中选出p(0<=p<=15)个数，从第二个数组中选出n-p个数，使得这两个数组与sum/2的值最接近。那么第一个数组有15个元素，从15个元素中选出0~15个数总共1*10^5(估算)种选法，不会超时。

#### 代码

```java
class Solution {
    public int minimumDifference(int[] nums) {
        int n = nums.length / 2;
        
        //将原数组分成两个长度为n的数组
        int[] leftArr = new int[n], rightArr = new int[n];
        int sum = 0;

        for (int i = 0; i < n; i++) {
            leftArr[i] = nums[i];
            sum += nums[i];
        }

        for (int i = n; i < nums.length; i++) {
            rightArr[i - n] = nums[i];
            sum += nums[i];
        }

        Map<Integer, TreeSet<Integer>> leftMap = new HashMap<>();
        Map<Integer, TreeSet<Integer>> rightMap = new HashMap<>();
        //求出每个数组所有子数组的个数以及对应的和
        dfs(leftArr, 0, 0, 0, leftMap);
        dfs(rightArr, 0, 0, 0, rightMap);

        int target = sum / 2;
        int ans = Integer.MAX_VALUE;
        for (Map.Entry<Integer, TreeSet<Integer>> entry : rightMap.entrySet()) {
            int len = entry.getKey();
            TreeSet<Integer> rightSet = entry.getValue();
            TreeSet<Integer> leftSet = leftMap.get(n - len);
            if (leftSet == null) {
                continue;
            }

            for (Integer num : rightSet) {
                if (leftSet.ceiling(target - num) != null) {
                    //求出最小值
                    ans = Math.min(Math.abs(sum - 2 * (leftSet.ceiling(target - num) + num)), ans);
                }
                if (leftSet.floor(target - num) != null) {
                    //求出最小值
                    ans = Math.min(Math.abs(sum - 2 * (leftSet.floor(target - num) + num)), ans);
                }
            }
        }

        return ans;
    }

    private void dfs(int[] arr, int index, int length, int sum, Map<Integer, TreeSet<Integer>> map) {
        TreeSet<Integer> set = map.get(length);
        if (set == null) {
            set = new TreeSet<>();
        }
        set.add(sum);
        map.put(length, set);

        for (int i = index; i < arr.length; i++) {
            dfs(arr, i + 1, length + 1, sum + arr[i], map);
        }
    }
}
```
