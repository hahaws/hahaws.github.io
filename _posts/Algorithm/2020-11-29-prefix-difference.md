---
layout: default
title: 前缀和和差分
tags: Algorithm
---

### 前缀和

前缀和就是用一个数组保存原数组前n项的和, 前缀和的用处就是计算在一个数组中给定范围的和

### 差分

差分和前缀和是一对逆运算

假设原数组是A, 它的差分数组是B, 那么A就是B的前缀和数组

> ![](http://latex.codecogs.com/svg.latex?A=a_1,a_2,a_3,a_4,...,a_n)

> ![](http://latex.codecogs.com/svg.latex?B=b_1,b_2,b_3,b_4,...,b_n)

> ![](http://latex.codecogs.com/svg.latex?a_1=b_1)

> ![](http://latex.codecogs.com/svg.latex?a_2=b_1+b_2)

> ![](http://latex.codecogs.com/svg.latex?a_3=b_1+b_2+b_3)

> ![](http://latex.codecogs.com/svg.latex?a_n=b_1+b_2+b_3+...+b_n)

如果对`b[l]`进行操作使`b[l] = b[l] + C`, 那么

> ![](http://latex.codecogs.com/svg.latex?b_l=b_l+C)

> ![](http://latex.codecogs.com/svg.latex?a_l=b_1+b_2+...+b_l+c)

> ![](http://latex.codecogs.com/svg.latex?=>a_l=a_l+c)

> ![](http://latex.codecogs.com/svg.latex?=>a_n=a_n+c)

可以看出, 如果对差分数组的第`l`项`+C`, 那么原数组从`l`项到数组结束的元素都会`+C`

所以我们要对原数组的`[l, r]`项的所有元素都`+C`, 只需要对差分数组`B[l] += C`, `B[r+1] -= C`, 再通过求前缀和得到原数组, 那么就会对区间`[l, r]`之间的数全部`+C`

在构造差分数组时, 我们先假定原数组`A`的所有元素都是`0`, 那么差分数组就是`0`, 对差分数组进行插入操作`[1,1]+a[1]`, `[2,2]+a[2]`, ..., `[n, n]+a[n]` 然后求前缀和就是给定的原数组

#### 例题

输入一个长度为n的整数序列。

接下来输入m个操作，每个操作包含三个整数l, r, c，表示将序列中[l, r]之间的每个数加上c

```cpp

#include <bits/stdc++.h>

using namespace std;

const int N = 100010;
int n, m;
int a[N], b[N];

void insert(int l, int r, int c)
{
    b[l] += c;
    b[r+1] -= c;
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) cin >> a[i];
    
    for (int i = 1; i <= n; ++i) insert(i, i, a[i]);
    
    while (m--)
    {
        int l, r, c;
        cin >> l >> r >> c;
        insert(l, r, c);
    }
    
    for (int i = 1; i <= n; ++i) a[i] = a[i-1] + b[i];
    
    for (int i = 1; i <= n; ++i) cout << a[i] << " ";
    
    return 0;
}
```

**差分矩阵**

有个原矩阵`A[N][N]`, 对矩阵内的子矩阵`(x1, y2, x2, y2)`范围内的值`+C`

需要做以下操作

```
b[x1][y1] += C
b[x2+1][y1] -= C
b[x1][y2+1] -= C
b[x2+1][y2+1] += C
```

如下图所示

![avatar]({{ site.url }}/assets/img/diff.png)

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1010;

int n, m, q;
int a[N][N], b[N][N];

void insert(int x1, int y1, int x2, int y2, int c)
{
    b[x1][y1] += c;
    b[x2+1][y1] -= c;
    b[x1][y2+1] -= c;
    b[x2+1][y2+1] += c;
}

int main()
{
    cin >> n >> m >> q;
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j)
            cin >> a[i][j];
            
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j)
            insert(i, j, i, j, a[i][j]);
            
    while (q--)
    {
        int x1, y1, x2, y2, c;
        cin >> x1 >> y1 >> x2 >> y2 >> c;
        insert(x1, y1, x2, y2, c);
    }
    
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j)
            a[i][j] = a[i-1][j] + a[i][j-1] - a[i-1][j-1] + b[i][j];
    
    for (int i = 1; i <= n; ++i)
    {
        for (int j = 1; j <= m; ++j)
            cout << a[i][j] << " ";
        cout << endl;
    }
}
```