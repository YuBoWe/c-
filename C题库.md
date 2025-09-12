## 1. 将一个正整数分解质因数

**题目：**将一个正整数分解质因数。例如：输入 90,打印出 **90=2\*3\*3\*5**。

**程序分析：**对n进行分解质因数，应先找到一个最小的质数k，然后按下述步骤完成：

- (1)如果这个质数恰等于（小于的时候，继续执行循环）n，则说明分解质因数的过程已经结束，另外 打印出即可。
- (2)但n能被k整除，则应打印出k的值，并用n除以k的商,作为新的正整数n.重复执行第二步。
- (3)如果n不能被k整除，则用k+1作为k的值,重复执行第一步。

```c
#include<stdio.h>
int main()
{
    int n,i;
    printf("请输入整数：");
    scanf("%d",&n);
    printf("%d=",n);
    for(i=2;i<=n;i++)
    {
        while(n%i==0)
        {
            printf("%d",i);
            n/=i;
            if(n!=1) printf("*");
        }
    }
    
    printf("\n");
    return 0;
}
```

## 2. 猴子吃桃问题

**题目：**猴子吃桃问题：猴子第一天摘下若干个桃子，当即吃了一半，还不瘾，又多吃了一个 第二天早上又将剩下的桃子吃掉一半，又多吃了一个。以后每天早上都吃了前一天剩下 的一半零一个。到第10天早上想再吃时，见只剩下一个桃子了。求第一天共摘了多少。

**程序分析：**采取逆向思维的方法，从后往前推断。

1) 设x1为前一天桃子数，设x2为第二天桃子数， 则：

x2=x1/2-1, x1=(x2+1)*2

x3=x2/2-1, x2=(x3+1)*2

以此类推： x前=(x后+1)*2

2) 从第10天可以类推到第1天，是一个循环过程。

程序源代码：

```c
#include <stdio.h>
#include <stdlib.h>
int main(){
    int day, x1 = 0, x2;
    day=9;
    x2=1;
    while(day>0) {
        x1=(x2+1)*2;  // 第一天的桃子数是第2天桃子数加1后的2倍
        x2=x1;
        day--;
    }
    printf("总数为 %d\n",x1);
    
    return 0;
}
```

## 3. 两个乒乓球队进行比赛

**题目：**两个乒乓球队进行比赛，各出三人，甲队为 **a、b、c** 三人，乙队为 **x、y、z** 三人。

抽签决定比赛名单，有人向队员打听比赛的名单：a 说他不和 x 比，c 说他不和 x、z 比，请编写代码找出三队赛手的名单。

**思路：**

1. 使用 **3 个循环** 分别枚举甲队的每个队员与乙队的队员匹配的可能性。

2. 添加

    

   约束条件

   ，过滤掉不满足题目要求的组合：

   - a 不和 x 比赛。
   - c 不和 x 和 z 比赛。

3. 确保每个乙队的队员只能和一个甲队的队员比赛。

```c
//  Created by www.runoob.com on 15/11/9.
//  Copyright © 2015年 菜鸟教程. All rights reserved.
//
 
#include <stdio.h>
 
int main() {
    // 定义甲队和乙队的成员
    char teamA[] = {'a', 'b', 'c'}; // 甲队：a, b, c
    char teamB[] = {'x', 'y', 'z'}; // 乙队：x, y, z
 
    // 变量 i, j, k 分别表示 a, b, c 的对手
    char i, j, k;
 
    // 枚举 a 的对手
    for (i = 'x'; i <= 'z'; i++) {
        // 枚举 b 的对手
        for (j = 'x'; j <= 'z'; j++) {
            // 确保 a 和 b 的对手不同
            if (i != j) {
                // 枚举 c 的对手
                for (k = 'x'; k <= 'z'; k++) {
                    // 确保 c 的对手与 a 和 b 的对手不同
                    if (i != k && j != k) {
                        // 满足题目条件：a 不和 x 比，c 不和 x 和 z 比
                        if (i != 'x' && k != 'x' && k != 'z') {
                            // 输出匹配结果
                            printf("比赛顺序：a--%c\tb--%c\tc--%c\n", i, j, k);
                        }
                    }
                }
            }
        }
    }
 
    return 0;
}
```

## 4. 打印菱形

```
   *
  ***
 *****
*******
 *****
  ***
   *

```

```c
#include <stdio.h>
int main()
{
    int i,j,k;
    for(i=0;i<=3;i++) {
        for(j=0;j<=2-i;j++) {
            printf(" ");
        }
        for(k=0;k<=2*i;k++) {
            printf("*");
        }
        printf("\n");
    }
    for(i=0;i<=2;i++) {
        for(j=0;j<=i;j++) {
            printf(" ");
        }
        for(k=0;k<=4-2*i;k++) {
            printf("*");
        }
        printf("\n");
    }
  
}
```

## 5. 年龄递归

**题目：**有5个人坐在一起，问第五个人多少岁？他说比第4个人大2岁。问第4个人岁数，他说比第3个人大2岁。问第三个人，又说比第2人大两岁。问第2个人，说比第一个人大两岁。最后问第一个人，他说是10岁。请问第五个人多大？

**程序分析：**利用递归的方法，递归分为回推和递推两个阶段。要想知道第五个人岁数，需知道第四人的岁数，依次类推，推到第一人（10岁），再往回推。。

```c
#include<stdio.h>
int age(int n)
{
    if(n == 1)
        return 10;
    else
        return age(n - 1) + 2;
}

int main(void)
{
    int n;
    n = age(5);
    printf("%d\n",n);
    return 0;
}

```

## 6. 杨辉三角形

```c
1
1    1
1    2    1
1    3    3    1
1    4    6    4    1
```

```c
#include <stdio.h>

#define SIZE 50   // 最大行数

void printYangHui(int n) {
    int a[SIZE][SIZE] = {0};
    for (int i = 0; i < n; i++) {
        a[i][0] = a[i][i] = 1;  // 每行开头和结尾为 1
        for (int j = 1; j < i; j++) {
            a[i][j] = a[i-1][j-1] + a[i-1][j];
        }
    }
    // 打印
    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= i; j++) {
            printf("%d ", a[i][j]);
        }
        printf("\n");
    }
}

int main(void) {
    int n;
    printf("请输入杨辉三角的行数: ");
    scanf("%d", &n);
    printYangHui(n);
    return 0;
}

```

## 7. 约瑟夫环

约瑟夫环问题描述：

- n 个人围成一圈，从第 1 个人开始报数。
- 每次数到第 m 个人时，该人出列。
- 然后从下一个人重新开始数，直到所有人出列。



```c
#include <stdio.h>

#define SIZE 100

void josephus(int n, int m) {
    int arr[SIZE];
    for (int i = 0; i < n; i++) arr[i] = 1;  // 1 表示存活

    int cnt = 0;   // 报数计数器
    int left = n;  // 剩余人数
    int idx = 0;   // 当前索引

    while (left > 0) {
        if (arr[idx]) {      // 如果此人还在圈里
            cnt++;
            if (cnt == m) {  // 数到 m，该人出列
                printf("%d ", idx + 1);
                arr[idx] = 0;
                cnt = 0;
                left--;
            }
        }
        idx = (idx + 1) % n; // 循环到下一个人
    }
    printf("\n");
}

int main(void) {
    int n, m;
    printf("请输入人数 n 和报数 m: ");
    scanf("%d %d", &n, &m);
    josephus(n, m);
    return 0;
}

```

## 8. 猴子分桃

题目：海滩上有一堆桃子，五只猴子来分。第一只猴子把这堆桃子平均分为五份，多了一个，这只 猴子把多的一个扔入海中，拿走了一份。第二只猴子把剩下的桃子又平均分成五份，又多了 一个，它同样把多的一个扔入海中，拿走了一份，第三、第四、第五只猴子都是这样做的， 问海滩上原来最少有多少个桃子

🔹 算法思路

1. 假设原始桃子数为 `n`。

2. 每只猴子操作规则：

   - 如果 `(当前桃子数 - 1) % 5 == 0`，那么可以分。

   - 否则不合法。

   - 分完后，猴子拿走 `(当前桃子数 - 1)/5`，剩下的为：
     $$
     当前桃子数 = (当前桃子数 - 1) \times \frac{4}{5}
     $$

3. 我们从最小的可能数 `n=1` 开始，不断递增，直到找到第一个满足 5 次操作都成立的数，即答案。



```c
#include <stdio.h>

// 判断给定的桃子数是否满足条件
int check(int n) {
    for (int i = 0; i < 5; i++) {
        if ((n - 1) % 5 != 0) return 0; // 不能整分
        n = (n - 1) / 5 * 4;            // 剩余桃子
    }
    return 1;
}

// 找到最小的桃子数
int findPeach() {
    int n = 1;
    while (1) {
        if (check(n)) return n;
        n++;
    }
}

int main(void) {
    int result = findPeach();
    printf("原来最少有 %d 个桃子\n", result);
    return 0;
}
```

🔹 时空复杂度

- **时间复杂度**：O(k·m)，其中 k 为找到答案前的枚举次数，m=5 为猴子数量（常数）。
- **空间复杂度**：O(1)，只使用了常量存储。

📌 运行结果：最少桃子数是 **3121**。



## 9. 求0—7所能组成的奇数个数

思路

奇数的末位必须是 {1,3,5,7} → 4 种。

最高位（如果有多位数）不能为 0 → 7 种。

中间位可以是 0–7 → 8 种。

对于位数 `n`：

- 如果 `n=1`：4 种。
- 如果 `n≥2`：共有 `7 × 8^(n-2) × 4` 种。

遍历 1~k 位，累加结果

```c
#include <stdio.h>
#include <math.h>

// 计算0-7组成的k位以内奇数个数
long long func(int k) {
    long long sum = 0;
    if (k >= 1) sum += 4; // 1位数
    for (int n = 2; n <= k; n++) {
        sum += 7 * (long long)pow(8, n - 2) * 4;
    }
    return sum;
}

int main(void) {
    int k;
    printf("请输入最大位数k: ");
    scanf("%d", &k);

    long long result = func(k);
    printf("0-7能组成的所有奇数个数为: %lld\n", result);

    return 0;
}

```

