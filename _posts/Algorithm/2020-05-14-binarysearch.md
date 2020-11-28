---
layout: default
title: 二分搜索(binary search)
tags: Algorithm
---

# 二分搜索(binary search)

二分搜索（binary search），也称折半搜索（half-interval search）、对数搜索（logarithmic search），是一种在有序数组中查找某一特定元素的搜索算法

## 详解

虽然二分搜索的题目或其他情况大部分都是在有序的序列上搜索。但是这并不是二分搜索的必要条件。

二分搜索的本质是能够在给出的序列中找到一个性质可以将序列一分为二，这样的情况就能够使用二分搜索来解决问题

假设目标值在闭区间[l, r]中， 每次将区间长度缩小一半，当l = r时，我们就找到了目标值

以下图为例

![avatar]({{ site.url }}/assets/img/b_search.png)

要在范围`[l, r]`之间查找某个值`x`, 对于`x`有这样的性质`check(k<=x)`和`check(k>x)`的返沪值恰好是相反的`True`和`False`, 这种情况就适用二分查找

二分查找将`mid`放在左半部分或者右半部分是有区别的

**版本一**

当我们将区间`[l, r]`划分成`[l, mid]`和`[mid + 1, r]`时，其更新操作是`r = mid`或者`l = mid + 1`;，**计算mid时不需要加1**

```cpp
int bsearch(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }
    return l;
}
```

**版本二**

当我们将区间`[l, r]`划分成`[l, mid - 1]`和`[mid, r]`时，其更新操作是`r = mid - 1`或者`l = mid`;，**此时为了防止死循环，计算mid时需要加1**

```cpp
int bsearch(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

发明KMP算法的大佬是这么说的

> Although the basic idea of binary search is comparatively straightforward, the details can be surprisingly tricky

> 尽管二分查找的基本思路想读简单，但是其中的一些细节却非常棘手

### 代码示例

既可以用递归的方法写，也可以用循环的方法写

注意这里取中间值的时候, `l+r`有可能溢出, 所以写`l + (r - l) / 2`

```cpp
// 递归
int binarySearch(vector<int>& nums, int start, int end, int target)
{
    if(end > start) return -1;
    int mid = start + (end - start) / 2;
    if (nums[mid] == target)
        return mid;
    else if (nums[mid] < target)
        return binarySearch(nums, mid + 1, end, target);
    else
        return binarySearch(nums, start, mid - 1);
}


// 循环
int binarySearch(vector<int>& nums, int target)
{
    int start = 0, end = nums.size() - 1, ans = -1;
    while (start <= end)
    {
        int mid = start + (end - start) / 2;
        if (nums[mid] < target)
            start = mid + 1;
        else if (nums[mid] > target)
            end = mid - 1;
        else
            ans = mid;
            break;
    }
    return ans;         // 程序唯一出口
}
```


### 例题分析

给定一组有序的序列, 如`[1, 2, 2, 3, 3, 5]`, 另外指定一个数字`x`, 输出`x`在序列中的起始位置, 如果不存在输出`-1 -1`

```cpp
include <iostream>

using namespace std;

const int N = 100010;

int n, q, k;
int ar[N];

int main()
{
    cin >> n >> q;
    for (int i = 0; i < n; ++i) cin >> ar[i];

    while (q--)
    {
        cin >> k;
        int l = 0, r = n - 1;
        while (l < r)
        {
            int mid = (l + r) >> 1;
            // 这里找x的左边界
            if (ar[mid] >= k) r = mid;  
            else l = mid + 1;
        }

        if (ar[l] != k) cout << "-1 -1" << endl;
        else
        {
            cout << l << " ";
            int l = 0, r = n - 1;
            while (l < r)
            {
                int mid = (l + r + 1) >> 1;
                // 找x的右边界, 这里当条件True的时候更新的是l=mid, 那么就需要在二分的时候mid为(l+r+1)/2
                if (ar[mid] <= k) l = mid;
                else r = mid - 1;
            }
            cout << l << endl;
        }
    }
    return 0;
}
```

### 浮点数二分

用浮点数二分的方法计算数的三次方根

```cpp
#include <iostream>

using namespace std;

int main()
{
    double x, l, r;
    cin >> x;
    if (x >= 0) l = 0, r = x;
    else l = x, r = 0;
    while (r - l > 1e-8)
    {
        double mid = (l + r) / 2;
        if (mid * mid * mid >= x) r = mid;
        else l = mid;
    }
    printf("%lf\n", l);
    return 0;
}
```

### 复杂度分析

每次都把搜索范围缩小一半，时间复杂度O(logn)

空间复杂度O(1), 用递归的时候尾递归可以改成循环

