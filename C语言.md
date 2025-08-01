# 基础

## 进制转换

**题目：** 小明最近学习了数据的各种表示方法，明白了计算机的存储方式。例如十进制、二进制、八进制、十六进制、指数形式。老师给小明并布置了作业把十进制转为其他进制。但是小明不想手算，于是偷懒的小明想通过计算机快速完成老师布置的作业。

**输入：** 一个十进制整数 x

**输出：** x 的十进制，八进制，十六进制，指数形式，各个不同输出用空格隔开

**输入样例：** 1000

**输出样例：** 1000 1750 3e8 1.000000e+003

```c
#include <stdio.h>
// 思路：直接设置占位符的格式后即可输出结果
int main() {
    int x;
    scanf("%d", &x);
    printf("%d %o %x %e", x, x, x, (double)x);
    return 0;
}
```

## 平闰年判定

**题目：** 设计程序，输入一个年份，判断是平年还是闰年

**输入：** 一个整数

**输出：** 判断平年还是闰年，平年则输出 1，闰年则输出 0

**输入样例：** 1949

**输出样例：** 1

```c
#include <stdio.h>
// 常考点：满足条件(是 4 的倍数但是不是 100 的倍数)或者(是 400 的倍数)的年份是闰年
int isLeapYear(int year) {
    return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
}
int main() {
    int year;
    scanf("%d", &year);
    printf("%d", !isLeapYear(year)); // 取反是为了和题目输出配对
    return 0;
}
```

## 字符串反向保存

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 使用递归方法编写函数，实现将输入的字符串反向保存 (6分)
 * 
 * 分析
 * 本题如果写一般循环的话非常容易。
 * 主要考察通过C的基础操作以及尾递归和循环的转化。
 * 
 * 要点
 * - C类型字符串的长度如何获取？
 * - 循环转尾递归
 * - 输入输出字符串(gets puts) (在代码中未体现)
*/

# include <stdio.h>

void __reverse(char* str, int left, int right) {
    if (left >= right) return;
    char temp_char;
    temp_char = str[left]; // 交换字符
    str[left++] = str[right];
    str[right--] = temp_char;
    __reverse(str, left, right); // 递归传递调用
}

void reverse(char* str) { // 驱动函数
    int length = 0; 
    for (length = 0; str[length]; length++); // 获取字符串长度
    __reverse(str, 0, length - 1);
    for (int i=0; i < length; i++){
        printf("%c", str[i]);
    }
}

int main(){
    char str[1024];
    gets(str);
    reverse(str);
    return 0;
}
```

## 链表排序

**输入：** 第一行输入一个整数 n 代表链表原本的节点个数，第二行输入 n 个整数。

**输出：** 从小到大排序后的链表。

**输入样例：**

```
5
5 2 9 1 7 
```

**输出样例：**

```
1 2 5 7 9 
```

e

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct _node {
    int value;
    struct _node *next;
} Node;

typedef struct _list {
    Node *head;
} List;

void sortList(List *list) {
    if (!list || !list->head || !list->head->next) return;

    Node *tail = list->head;

    while (tail->next) {
        Node *minPrev = tail;
        Node *prev = tail;
        Node *curr = tail->next;

        while (curr) {
            if (curr->value < minPrev->next->value) {
                minPrev = prev;
            }
            prev = curr;
            curr = curr->next;
        }

        Node *minNode = minPrev->next;
        minPrev->next = minNode->next;
        minNode->next = tail->next;
        tail->next = minNode;

        tail = tail->next;
    }
}

```

```c
List *createList() {
    List *list = (List *)malloc(sizeof(List));
    list->head = (Node *)malloc(sizeof(Node));  // dummy head node
    list->head->next = NULL;
    return list;
}

void insertNode(List *list, int val) {
    Node *p = list->head;
    while (p->next) p = p->next;

    Node *newNode = (Node *)malloc(sizeof(Node));
    newNode->value = val;
    newNode->next = NULL;
    p->next = newNode;
}

void printList(List *list) {
    Node *p = list->head->next;
    while (p) {
        printf("%d ", p->value);
        p = p->next;
    }
    printf("\n");
}

int main() {
    List *list = createList();
    insertNode(list, 5);
    insertNode(list, 2);
    insertNode(list, 9);
    insertNode(list, 1);
    insertNode(list, 7);

    printf("Before sort: ");
    printList(list);

    sortList(list);

    printf("After sort : ");
    printList(list);

    return 0;
}

```

## 排序并删除字符串中的空格

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 编写程序，从键盘上输入一个字符串，然后将输入的字符串按照字符的ASCII值排序，删除字符串中的空格，
 * 且相同的字符在串中仅出现一次，将排序后的字符串在屏幕上显示
 * 
 * 分析
 * 主要考察计数排序的退化情况和ASCII码
 * 
 * 要点
 * - 计数排序的退化情况
 * - 字符串输入和输出
 * - 数组、数组的初始化
 * - 基础ASCII码有128个，扩展打印字符集有256个
 * 
 * 附加知识：几个重要的ASCII
 *  ' '      32
 *  '0'      48
 *  'A'      65
 *  'a'      97 
*/

# include <stdio.h>

int main() {
    int index[256] = { 0 };
    char str[1024];  // 虽然这种写法很不漂亮，但考试所迫，必须如此(否则代码的篇幅会变得很长)
    gets(str);
    for (int i = 0; str[i]; i++) index[str[i]]++;
    for (int i = 0; i < 256; i++) {
        if (i != 32 && index[i]) putchar(i); // 空格的ASCII是32
    }
    return 0;
}
```

## 文件合并

**题目：** 给定两个文件“test1.txt”和“test2.txt”，请把两个文件中的内容合并而后写到文件“test3.txt”之中。

**输入样例 & 输出样例：** 不便展示，请查看题目的视频讲解的展示样例。

```c
#include <stdio.h>
// 考察文件操作基本函数: fgetc 和 fputc
// 形参声明类型为 const char *，因为 fopen 函数的要求也是 const char * 类型
// 写 char *file1 也行的，只是有警告信息，当然还是规范写好些，把 const 加上
void mergeFiles(const char *file1, const char *file2, const char *outputFile) {
    FILE *f1, *f2, *f3;
    // 读模式打开两个文件，记得配套写上 fclose，不写必然扣分
    f1 = fopen(file1, "r");
    f2 = fopen(file2, "r");
    // 写模式打开输出文件，记得配套写上 fclose
    f3 = fopen(outputFile, "w");
    char ch;
    // 读取第一个文件并写入输出文件
    while ((ch = fgetc(f1)) != EOF)
        fputc(ch, f3);
    // 读取第二个文件并写入输出文件
    while ((ch = fgetc(f2)) != EOF)
        fputc(ch, f3);
    // 配套的 fclose
    fclose(f1); fclose(f2); fclose(f3);
}
int main() {
    mergeFiles("D:/xxx/xxx/test1.txt", "D:/xxx/xxx/test2.txt", "D:/xxx/xxx/test3.txt");
    return 0;
}
```

## 二次方程的根-

**题目：** 编写程序，求方程的根，用三个函数分别求当 b2−4ac*b*2−4*a**c* 大于 0、等于 0、和小于 0 时的根，并输出结果。

**输入：** 三个数，分别代表二次方程中的系数 a, b, c

**输出：** 方程的根，包括实根和虚根，格式为 x1=? x2=?，中间用空格分隔，保留三位小数。

**输入样例：** 4 1 1

**输出样例：** x1=-0.125+0.484i x2=-0.125-0.484i

```c
#include <stdio.h>
#include <math.h>

// 这题重点考察多个 if 的简单应用，同时需要重点注意调用 math.h 的函数时清楚形参类型
int main() {
    double a, b, c; // 注意一定定义成浮点数类型，不然会丢失小数部分
    scanf("%lf %lf %lf", &a, &b, &c);
    double delta = b * b - 4 * a * c;
    if (delta > 0)
        // 注意不要括号弄错了
        printf("x1=%.3f x2=%.3f", (-b + sqrt(delta)) / (2 * a), (-b - sqrt(delta)) / (2 * a));
    else if (delta == 0) // 这里为了尽可能减少版面占用就省略了花括号，大家要写上
        printf("x1=%.3f x2=%.3f", (-b) / (2 * a), (-b) / (2 * a));
    else
        printf("x1=%.3f+%.3fi x2=%.3f-%.3fi", -b / (2 * a), (sqrt(-delta)) / (2 * a), -b / (2 * a), (sqrt(-delta)) / (2 * a));
    return 0;
}
```



















```c
#include <stdio.h>
#define maxsize 101

typedef struct{
    char ch;
    int times;
}S;

int foo(S list[], char v, int *len){
    int flag = 0;
    for(int i=0; i<*len; i++){
        if (list[i].ch == v){
            list[i].times++;
            flag = 1;
        }
    }
    if(flag == 0){
        list[*len].ch = v;
        list[*len].times = 1;
        (*len)++;
    }
    return *len;
}

int compare(S list[], int slen){
    
}

int main(){
    S list[maxsize];
    printf("输入字符串长度:\n");
    int len, slen=0;
    scanf("%d", &len);
    char str1[len];
    for(int i=0; i<len; i++)
        scanf(" %c", &str1[i]);

    str1[len] = '\0';
    for(int i = 0; i<len; i++)
        printf("%c", str1[i]);
    printf("\n");
    
    for(int i=0; i<len; i++){
        foo(list, str1[i], &slen);
    }

    for(int i=0; i<slen; i++){
        printf("%c: %d\n", list[i].ch, list[i].times);
    }
    return 0;
}
```

## 求五个各不相同的正整数，它们的和是135

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 有五个各不相同的正整数，它们的和是135，且按照从小到大的顺序，后面一个数是前面一个数的整倍数。
 * 编写程序求这几个数。
 * 
 * 分析
 * 翻译题。将题意用编程语言重新写一遍即可。鉴于数字很小，选用五重循环。
*/
#include <stdio.h>

int main() {
    for (unsigned int i = 1; i <= 135; i++)
        for (unsigned int j = 2; i * j <= 135; j++) 
            for (unsigned int k = 2; i * j * k <= 135; k++)
                for (unsigned int l = 2; i * j * k * l <= 135; l++) 
                    for (unsigned int m = 2; i * j * k * l * m <= 135; m++)
                        if (i + i * j + i * j * k + i * j * k * l + i * j * k * l * m == 135)
                            printf("%ud, %ud, %ud, %ud, %ud\n", i, i * j, i * j * k, i * j * k * l, i * j * k * l * m);
    return 0;
}


1d, 2d, 4d, 8d, 120d
-1d, 2d, 4d, 16d, 112d
1d, 2d, 4d, 32d, 96d
-1d, 2d, 6d, 18d, 108d
-1d, 2d, 6d, 42d, 84d
-1d, 2d, 12d, 24d, 96d
-3d, 6d, 18d, 36d, 72d
```

## 判断素数

**题目：** 输入正数 n，判断 n 是否为素数。若为素数则输出 yes，否则输出 no。

**输入：** 一个正整数

**输出：** 判断是否为素数，是则输出 yes，否则输出 no。

**输入样例：** 17

**输出样例：** yes

```c
#include <stdio.h>
// 考察基本的素数判断，这个函数请务必做到非常熟练，脱口而出
int isPrime(int n) {
    if (n == 1) return 0; // 1 既不是素数也不是合数
    if (n == 2) return 1; // 2 是特殊的质数
    for (int i = 2; i * i <= n; i++) // 一定注意 i 从 2 开始
        if (n % i == 0)
            return 0; // 不是素数
    return 1;
}
int main() {
    int n;
    scanf("%d", &n);
    printf("%s", isPrime(n) ? "yes" : "no");
    return 0;
}
```

## 清洗注释(文件)

**题目：** 给定一个源代码文件“code1.c”，文件中有相当数量的注释，只包括//一种形式，且保证注释单独占用一行，不会和代码糅杂，请你把文件里的纯代码内容清洗出来并存到“code2.c”文件中。

```c
#include <stdio.h>
#define maxLine 100

// 考察基本的 fgets 和 fputs 函数直接按照行为单位进行处理
// 可以一个个字符去读，然后遇到/就开始读取后面一个字符
// 如果字符为//则这一整行都不用输出，直到遇到换行符

int main() {
    FILE *fp = fopen("xxx/xxx/code1.c", "r");
    FILE *fout = fopen("xxx/xxx/code2.c", "w");
    char str[maxLine]; // 接受从文件读取的一行数据，假设一行最大为100个字符
    while (fgets(str, maxLine, fp) != NULL) { // 读取一行数据
        // 注意 fgets 是回车作为标志符的，因为一行到下一行就是回车
        for (int i = 0; str[i] != '\n'; i++) {
            if (str[i] == '/' && str[i + 1] == '/') {
                str[i] = '\n'; str[i + 1] = '\0'; // 遇到注释符号则直接截断
                break;
            }
        }
        fputs(str, fout);
    }
    fclose(fp);
    fclose(fout); // 记得配套写上
    return 0;
}
```

## 巧算自然数(easy)

**题目内容：**

编程实现输入一个自然数，若为偶数，则把它除以2；若为奇数，则把它乘以3加1。经过如此有限次运算后，总可以得到自然数值1。输出经过多少次可以得到自然数1和每次得到的值。

**输入格式：**

输入一个自然数

**输出格式：**

输出经过多少次可以得到自然数1和每次得到的值

**输入样例：**

22[回车]

**输出样例：**

22,11,34,17,52,26,13,40,20,10,5,16,8,4,2,1[回车] step=16[回车]

```c
#include <stdio.h>

int main() {
    int n, count = 0;
    scanf("%d", &n);  // 输入一个自然数

    while (1) {
        printf("%d", n);  // 打印当前值
        if (n == 1) break;  // 到达1时终止
        printf(",");  // 打印逗号分隔符

        // Collatz规则：奇数×3+1，偶数除以2
        if (n % 2 == 0)
            n = n / 2;
        else
            n = n * 3 + 1;

        count++;  // 步数增加
    }

    printf("\nstep=%d\n", count);  // 输出步数
    return 0;
}

```



## 筛选信息(文件)

**题目：** 给定文件“stu.txt”，该文件中的每一行存着格式为“clsID:A name:B score:C”的学生数据，其中 A 是整数代表班级编号，B 是字符串代表名字，C 是整数代表分数，冒号是英文冒号。

例如一行数据样例：“clsID:1 name:ck score:20”，注意空格。

请你把所有班级为 1 且分数不及格的同学的信息提取出来并按照原格式存入到另一个文件“stu2.txt”。

```c
#include <stdio.h>
// 考察 fscanf 和 fprintf 的基本使用
int main() {
    FILE *fp = fopen("D:\\xxx\\stu.txt", "r");
    FILE *fout = fopen("D:\\xxx\\stu2.txt", "w");
    int classNum, score; // 班级编号和分数
    char name[100]; // 姓名，假设名字长度不超过 100 字符
    // 使用 fscanf 按格式逐行读取数据，注意每一行末尾的回车也要处理
    while (fscanf(fp, "clsID:%d name:%s score:%d\n", &classNum, name, &score) != EOF) {
        if (classNum == 1 && score < 60)
            fprintf(fout, "clsID:%d name:%s score:%d\n", classNum, name, score);
    }
    fclose(fp);
    fclose(fout); // 一定要记得关闭文件
    return 0;
}
```



## 整数逆序

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 编写函数，实现将存放在变量n中的整数，逆序存放在变量m中。
 * 例如原来变量n保存的是483，程序运行后变量m中存放整数384
 * 
 * 分析
 * 用简单的逐位提取即可，难度较低。
 * 可以使用数组存放各位，但没必要，这么做既低效，又会暴露出编程者较低的算法水平。
 * 最简洁明快而优雅的写法呈现在下方：
 * 
 * 要点
 * - 循环
*/

int reverse_int(int n) {
    int res = 0;
    while (n) {
        res *= 10;
        res += n % 10;  // 如果想要强化效率，此处仍可做修改，但没必要
        n /= 10;
    }
    return res;
}
```

## 字符串按ASCII码值从大到小排序

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 编写程序，实现从键盘上输入两个字符串，将它们合并之后按ASCII码值从大到小排序在屏幕输出，相同的字符仅输出一次。
 * 
 * 分析
 * 算法与2004-1-2完全一致，使用计数排序的退化情况。
 * 
 * 要点
 * - 计数排序的退化情况
*/
# include <stdio.h>

int main() {
    int index[256] = { 0 };
    char str1[1024], str2[1024];  // 虽然这种写法很不漂亮，但考试所迫，必须如此(否则代码的篇幅会变得很长) 
    gets(str1);
    gets(str2);
    for (unsigned int i = 0; str1[i]; i++) index[str1[i]]++;
    for (unsigned int i = 0; str2[i]; i++) index[str2[i]]++;
    for (unsigned int i = 255; i >=0; i--) {
        if (index[i]) putchar(i);
    }
    return 0;
}
```

## 从文件存取变量

**题目：** 请自行编程实现把一个数组的数据存入文件，然后读取该文件并解析到变量之中

```c
#include <stdio.h>
// 了解 fread 和 fwrite，大家自己学习的时候做区分
int main() {
    int n = 5;
    int arr[5] = {10, 20, 30, 40, 50};
    // 写入数组到文件
    FILE *fp = fopen("data.bin", "wb"); // 打开文件以二进制写模式
    // 使用 fwrite 将数组写入文件
    fwrite(arr, sizeof(int), n, fp);
    fclose(fp); // 关闭文件
    // 从文件读取数组
    int readArr[5]; // 用于存储从文件读取的数据
    // 使用 fread 从文件读取数据
    fp = fopen("data.bin", "rb");
    fread(readArr, sizeof(int), n, fp);
    // 输出从文件读取的字节数据
    for (int i = 0; i < n; i++)
        printf("%d ", readArr[i]);
    fclose(fp); // 关闭文件
    return 0;
}
```

## 最大公约数（必须熟练）

**题目：** 编写程序，实现求两个正整数 m, n 的最大公约数。

**输入：** 两个整数，用空格隔开

**输出：** 这两个整数的最大公约数

**输入样例：** 20 15

**输出样例：** 5

```c
#include <stdio.h>
// 必须熟练其中一个方法，脱口而出，推荐熟练第二个
// 常规方法: 暴力枚举每个数，只要满足条件就代表是
int gcd1(int a, int b) { // gcd 的全称是:greatest common divisor
    // 初始化认为最大公约数就是 a,b 之中较小的那个
    int result = a < b ? a : b;
    // 暴力尝试每个数
    while (result > 0) {
        if (a % result == 0 && b % result == 0)
            return result; // 如果满足则说明是最大公约数
        result--; // 尝试下一个数
    }
}
// 辗转相除法: 数学方法，可以查看视频讲解
int gcd2(int a, int b) {
    int remainder = a % b;
    while (remainder != 0) {
        a = b;
        b = remainder;
        remainder = a % b;
    }
    return b;
}
int main() {
    int a, b;
    scanf("%d %d", &a, &b);
    printf("%d", gcd1(a, b));
    return 0;
}
```

## 计算时钟的夹角

**题目内容：** 钟面上的时针和分针之间的夹角总是在 0～180之间（包括 0 和 180）。举例来说，在十二点的时候两针之间的夹角为 0，而在六点的时候夹角为 180，在三点的时候为 90。本题要解决的是计算 12:00 到 11:59 之间任意一个时间的夹角。

**输入格式：** 每组测试数据包含两个数字：第一个数字代表小时（大于 0 小于等于 12），第二个数字代表分（在区间 [0, 59] 上）。

**输出格式：** 对应每组测试数据，用常用格式显示时间以及这个时候时针和分针间的最小夹角，精确到小数点后一位。输出格式如下所示。 再看一看，想一想：是否可以不用 if 语句，只使用 printf 函数来简化你的程序？

**输入样例：**

```
12 0
```

**输出样例：**

```
At 12:00 the angle is 0.0 degrees.
```

```c
#include <stdio.h>
#include <math.h>

int main() {
    int h, m;
    while (scanf("%d %d", &h, &m) == 2) {
        double minute_angle = 6.0 * m;
        double hour_angle = 30.0 * h + 0.5 * m;
        double diff = fabs(hour_angle - minute_angle);
        double angle = fmin(diff, 360.0 - diff);
        printf("At %02d:%02d the angle is %.1f degrees.\n", h, m, angle);
    }
    return 0;
}

```

> ###  **算法思路**
>
> - 分针每分钟走 6 度（360/60）。
> - 时针每小时走 30 度（360/12），同时每分钟走 0.5 度（30/60）。
> - 输入小时 `h` 和分钟 `m` 后：
>   - 分针角度 = `6 * m`
>   - 时针角度 = `30 * h + 0.5 * m`
>   - 两者差的绝对值即夹角，最终取 `min(角度, 360 - 角度)` 保证是最小夹角。

## 保存学生的考号和课程成绩

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 有一个保存学生课程成绩的结构数组，保存学生的考号和课程成绩。
 * 请编写一个函数，将保存在结构数组中的数据按照课程成绩从高到低的顺序存放在一个单向链表中。
 * 结构体定义如下 
 *  struct student {
 *      int no;
 *      float score;
 * };
 * 
 * 分析
 * 是要求较高的一道题。
 * 朴素地想，可以使用链表的插入排序来解决这一问题。但必须想到插入排序的效率偏低，而链表不支持快速排序等折半排序法。
 * 故考虑先对数组进行从小到大的排序，再对链表执行头插法构建得到从大到小的链表。
 * 本实现要求较高，如无能力则可以使用链表插入排序。
 * 
 * 要点
 * - 链表的头插法构建
 * - malloc函数
 * - 快速排序qsort的库包含及调用
 * - 函数指针、void指针及指针类型转换
 * 
 * 背景知识
 * qsort方法中比较函数的返回值
 *  int compare(const void* a, const void* b)中：
 *  若返回值为负整数，则a会被排在b之前
 *  若返回值为正整数，则a会被排在b之后
 *  若返回值是0，则a和b不区分前后
*/
# include <stdlib.h> // 为了使用qsort和malloc

typedef struct student {
    int no;
    float score;
}student;

typedef struct node_stu {
    student value;
    struct node_stu* next;
}node_stu;

int compare_stu(const void* a, const void* b) { // 考虑到本题目score是float，故不可轻易返回差值。
    return (student*a)->score < (student*b)->score ? 1 : -1;
}

node_stu* sort_student(student* array, unsigned int length) {
    if (!length) return 0;
    qsort(array, length, sizeof(student), compare_stu);//排序
    node_stu* res = 0;
    for (unsigned int i = 0; i < length; ++i) {//尾插
        node_stu* temp = (node_stu*)malloc(sizeof(node_stu));
        temp->next = res;
        temp->value.no = array[i].no;
        temp->value.score = array[i].score;
        res = temp;
    }
    return res;
}
```

## 确定母亲节

**题目内容：** 母亲节是每年的五月第二个星期天，针对输入的年份，计算该年的哪一天是母亲节。

**输入格式:** 年份

**输出格式：** 日期（5月的哪一天）

**输入样例：**

```
2014
```

**输出样例：**

```
11
```

```c
#include <stdio.h>
#include <time.h>

int main() {
    int year;
    scanf("%d", &year);

    struct tm t = {0}; // 初始化时间结构体
    t.tm_year = year - 1900; // 年份需要减去1900
    t.tm_mon = 4;            // 5月，月份从0开始算，所以5月是4
    t.tm_mday = 1;           // 从5月1号开始

    mktime(&t);              // 填充tm_wday（星期几）

    // 计算第一个星期天距离5月1日的偏移天数
    int offset = (7 - t.tm_wday) % 7;

    int mothers_day = 1 + offset + 7; // 第二个星期天 = 第一个星期天 + 7

    printf("%d\n", mothers_day);
    return 0;
}

```

## 最小公倍数(必须熟练）

**题目：** 编写程序，实现求两个正整数 m, n 的最小公倍数。

**输入：** 两个整数，用空格隔开

**输出：** 这两个整数的最小公倍数

**输入样例：** 4 5

**输出样例：** 20

```c
#include <stdio.h>
// 同样必须熟练其中一个方法，脱口而出，推荐熟练第一个
// 利用最大公约数求解最小公倍数，m,n 的最小公倍数为: m * n / gcd(m, n)
int gcd(int a, int b) {
    int remainder = a % b;
    while (remainder != 0) {
        a = b;
        b = remainder;
        remainder = a % b;
    }
    return b;
}
int lcm1(int m, int n) { // lcm 全称是 least common multiple
    return (m * n) / gcd(m, n);
}
// 暴力破解求解最小公倍数
int lcm2(int m, int n) {
    int result = m > n ? m : n; // 初始化为二者之中更大的值
    while (result % m != 0 || result % n != 0)
        result++; // 只要不满足条件就尝试下一个
    return result;
}
int main() {
    int m, n;
    scanf("%d %d", &m, &n);
    printf("%d", lcm2(m, n));
    return 0;
}
```

## 文件指针偏移

**题目：** 请创建文件“test1.txt”接收用户的输入，输入只有数字和小写字母，如果用户输入的是数字，则存到文件内容的开头，如果用户输入的是字母，则存到文件内容的结尾。

```c
#include <stdio.h>
// 考察 fseek 和 rewind 的基本使用
int main() {
    FILE *fp = fopen("out.txt", "r+"); // 使用读写模式打开文件
    char ch;
    while ((ch = getchar()) != '#') {
        if (ch >= 'a' && ch <= 'z') { // 字母追加到文件末尾
            fseek(fp, 0, SEEK_END); // 移动到文件末尾
            fputc(ch, fp);
        } else if (ch >= '0' && ch <= '9') { // 数字插入到文件开头
            // 注意一定要先保留原有的内容，然后按照新内容+原内容的方式写入
            // 否则会出现覆盖问题
            char buffer[1024]; // 读取文件原本的内容的缓冲区
            // 将文件指针移动到文件开头，等价于 fseek(fp, 0, SEEK_SET);
            rewind(fp);
            fread(buffer, sizeof(char), sizeof(buffer), fp);
            rewind(fp); // 再次将文件指针移动到文件开头
            fprintf(fp, "%c%s", ch, buffer); // 插入新内容并写回原内容
        }
    }
    fclose(fp); // 记得关闭文件
    return 0;
}
```

## 使用链栈转换8进制

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 已知一个无符号整数n，写一算法，将其转化为8进制数。要求用链栈来实现。
 * 
 * 分析
 * 没啥意思的题，为了用栈而用栈。
 * 考研可以将链栈当作ADT来用，但是如果纸面有富裕，建议简单地实现一下。
 * 
 * 要点
 * - 链栈ADT
*/

# include "../Common/Lists.hpp"
# include <malloc.h>
# include <stdio.h>

typedef node_int* stack;

stack initstack() {
    stack res = (stack)malloc(sizeof(node_int));
    res->next = 0;
    res->value = 0;
    return res;
}

int empty(stack sta) {
    return sta->next == 0;
}

void push(stack sta, int value) {
    node_int* temp = (node_int*)malloc(sizeof(node_int));
    temp->next = sta->next;
    temp->value = value;
    sta->next = temp;
}

int pop(stack sta) {
    node_int* temp = sta->next;
    int res = temp->value;
    sta->next = sta->next->next;
    free(temp);
    return res;
}

// 以上内容考试时如无要求可以不写

void trans(int n) {
    stack numStack = initstack();
    while (n) {
        push(numStack, n % 8);
        n /= 8;
    }
    while (!empty(numStack)) {
        int temp = pop(numStack);
        printf("%d", temp);
    }
}
```

## 求最后3位数值

**题目内容：** 小明在中学会求一个数的 n 次方了，但计算机还不会，请你来编写一个程序吧。由于计算机比较“笨”，所以我们编写程序的功能不用太强，只要能算出 a 的 n 次方的最后 3 位数就可以了。

**输入格式:** a 和 n 的值。假设 a <= 150。

**输出格式：** 求 a 的 n 次方的最后 3 位数。 提示：再想一想，你的方法对吗？你用的是什么数据类型？

**输入样例：**

```
10 0
```

**输出样例：**

```
The last 3 numbers is 1
```

```c
#include <stdio.h>

// 快速幂：计算 a^n % 1000
int qpow(int a, int n) {
    int res = 1;
    a %= 1000; // 保证a本身小于1000，提高效率
    while (n > 0) {
        if (n % 2 == 1) {
            res = (res * a) % 1000;
        }
        a = (a * a) % 1000;
        n /= 2;
    }
    return res;
}

int main() {
    int a, n;
    scanf("%d %d", &a, &n);
    int ans = last3(a, n);
    printf("The last 3 numbers is %d.\n", ans);
    return 0;
}

```

## 数组的批量增（必须熟练）

**题目：** 设计程序，实现把一个数组的所有元素插入到另一个数组的指定索引位置。

**输入：** 第一行是第一个数组的元素个数 len1，第二行是空格分开的 len1 个元素，第三行是第二个数组的元素个数 len2，第四行是空格分开的 len2 个元素，第五行是插入索引位置。

**输出：** 输出第二个数组插入到第一个数组后，第一个数组的所有元素。

**输入样例：**

```
3
10 20 30
2
40 50
0
```

**输出样例：**

```
40 50 10 20 30
```

```c
#include <stdio.h>
#define maxsize 100

// 考察数组的基本操作，需要非常熟练，脱口而出
int batchInsertElem(int arr1[], int len1, int arr2[], int len2, int index) {
    if (len1 + len2 > maxsize || index > len1 || index < 0)
        return len1; // 空间不够或者插入位置不合法则返回原本的长度
    
    // 先为arr2腾出len2个位置，把arr1的[len-1, index]的元素从后往前逐一挪动
    for (int i = len1 - 1; i >= index; i--)
        arr1[i + len2] = arr1[i];
    
    // 把arr2批量放入
    for (int i = 0; i < len2; i++)
        arr1[index + i] = arr2[i]; // 把arr2一个个放入
    
    len1 += len2; // 注意数组的长度增加！
    return len1; // 插入成功返回新的数组长度
}

int main() {
    int len1;
    scanf("%d", &len1);
    int arr1[maxsize] = {0};
    for (int i = 0; i < len1; i++)
        scanf("%d", &arr1[i]);
    
    int len2;
    scanf("%d", &len2);
    int arr2[maxsize] = {0};
    for (int i = 0; i < len2; i++)
        scanf("%d", &arr2[i]);
    
    int index;
    scanf("%d", &index);
    
    len1 = batchInsertElem(arr1, len1, arr2, len2, index);
    
    for (int i = 0; i < len1; i++)
        printf("%d ", arr1[i]);
    
    return 0;
}
```



# 强化

## 判断完数（必须熟练）

**题目：** 输入一个正整数 n，判断 n 是否是完数，是的话输出 yes，否则输出 no。完数是指除自身外的因子之和等于它本身。例如 6=1+2+3，因此 6 是完数。

**输入：** 一个正整数 n

**输出：** 是完数则输出 yes，否则输出 no

**输入样例：** 496

**输出样例：** yes

```c
#include <stdio.h>
// 判断 n 是否是完数, 本质上就是暴力的方式找出所有因数
int func(int n) {
    int sum = 0;
    for (int i = 1; i < n; i++)
        if (n % i == 0)
            sum += i; // 把所有因数求和
    if (sum == n) // 判断因数和是否等于本身
        return 1;
    return 0;
}

int main() {
    int n;
    scanf("%d", &n);
    if (func(n) == 1)
        printf("yes");
    else
        printf("no");
    return 0;
}
```

## 递归方法实现将输入的字符串反向输出

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 编写函数，采用递归方法实现将输入的字符串反向输出
 * 
 * 分析
 * 基础递归，为了考递归而考递归，没什么意思。
*/

# include <stdio.h>
# include <string.h>

void __reverse_output(char* _str, unsigned int loca, unsigned int length) {
    if (loca >= length) return;
    __reverse_output(_str, loca + 1, length);
    printf("%c", _str[loca]);
}

void reverse_output() {
    char _str[1024];
    gets(_str);
    __reverse_output(_str, 0, strlen(_str));
}
```

## 锻炼身体吧

**题目内容：**

沫沫，灰灰和渣渣去锻炼身体，如果对他们跑步的距离分别只计整数 a、b、c，由于他们身高的差距，造成了 a<=b<=c，并且渣渣跑了N米之后就再也跑不动了。但是，按照他们事先的约定：只有当 c\*c==a\*a+b*b 时，他们的锻炼才有效果。

现在他们三个人想知道，如果渣渣的极限N已知，那么有多少种有锻炼效果的跑步方案（即a, b, c有多少种可能的取值）。

**输入格式:**

一个整数N，表示渣渣的体力极限， N <= 5000。

**输出格式:**

一个整数，代表他们跑步方案的个数。

Hint: 对于输入N=5，有方案 a=3, b=4, c=5, 共一种。

**输入样例：**

```
5
```

**输出样例：**

```
1
```

```c
#include <stdio.h>

int main() {
    int N;
    scanf("%d", &N);

    int count = 0;

    for (int c = 1; c <= N; c++) {
        for (int a = 1; a <= c; a++) {
            for (int b = a; b <= c; b++) {
                if (c * c == a * a + b * b) {
                    count++;
                }
            }
        }
    }

    printf("%d\n", count);
    return 0;
}

```

## 左小右大（必须熟练）

**题目：** 输入一串整型正整数，设计程序，以第一个元素为基准，把比第一个元素小的都放在其左边，比第一个元素大的放其右边，和它相等的元素都放到中间，也就是“左小右大”的情况。

**输入：** 第一行是元素个数 n，第二行是空格分开的 n 个元素，总长度不超过 100

**输出：** 按照规则“左小右大”划分后且相对次序保持不变后的数组

**输入样例：**

```
8
30 10 0 50 60 20 80 30
```

**输出样例：**

```
10 0 20 30 30 50 60 80
```

```c
#include <stdio.h>
#include <stdlib.h>

// 首先说明利用数组进行排序的双指针的划分方法是不可行的，不会具有稳定性，而且更新邻近
// 相等的元素都放到一起的情况
// 所以我们考虑插入排序来实现稳定划分，三次遍历：第一次遍历先顺序排列在大于的元素
// 右边，第二次遍历放到前面的元素，最后遍历把等于元素放回原位，此处不再书写代码
// 目的是，模拟先进车的插队操作，本质上依旧是数组的插入，需要左边偏一次即可，还是有
// 一定的复杂度，需要额外数组

// 把下标为index的元素往前插队 step 位（代码和数组批量操作以及这里是插入单个元素）

void jumpQueue(int arr[], int index, int step){
    int temp = arr[index];
    int destIndex = index - step; // 计算应该插入的位置
    // 把[index, len-1]的元素都后退一位
    for(int i = index - 1; i >= destIndex; i--)
        arr[i + 1] = arr[i];
    arr[destIndex] = temp; // 插入
}

void Partition(int arr[], int length){
    int pivot = arr[0]; // 基准
    int counter = 0; // 记录大于pivot的元素个数，方便知道往前插队多少个元素
    for(int i = 1; i < length; i++){ // 从第二个元素开始遍历
        if(arr[i] < pivot) // 如果arr[i]小于基准则应该往前插队
            jumpQueue(arr, i, counter + 1); // 左移 counter+1 步
        else if(arr[i] > pivot) // 如果arr[i]大于基准则只需记录counter+1
            counter++;
        else{ // 如果arr[i]等于基准则counter+1，同时把arr[i]往前移 counter 步
            jumpQueue(arr, i, counter);
            counter++;
            // 这两行代码顺序是可调换的，参考视频讲解
        }
    }
}

int main(){
    int n;
    scanf("%d", &n);
    int arr[n];
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);
    Partition(arr, n);
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}

```

## 质因数分解

**题目：从键盘输入整数 n(n>1)，将 n 分解为若干质数(素数)之积，例如，当 n=10 时，输出结果为：2, 5，当 n=40 时，输出结果为：2, 2, 2, 5**

**输入：** 一个整数 n

**输出：** n 的质因数，从小到大排列

**输入样例：**

```
30
```

**输出样例：**

```
2 3 5
```

```c
#include <stdio.h>
// 思路 1: 可以寻找一个数的因数并判断是否是质数，是的话则不断尝试累除直到不行就下一个，这个思路不再撰写，因为思路二更方便
// 思路 2: 不需要判断是否是质数，因为他的因数会一直被分解成 2, 3, 5, 7 这四个质因数
void func(int n){
    int factorList[4] = {2, 3, 5, 7};
    for(int i = 0; i < 4; i++){ // 尝试每一个质因数
        int factor = factorList[i];
        if(n % factor != 0)
            continue;
        while(n % factor == 0){ // 不断根据该因数分解 n
            printf("%d ", factor);
            n = n / factor;
        }
    }
    if(n != 1) // n 剩下的部分已经无法被分解（比如 22）或者 n 是质数的情况，比如 17
        printf("%d", n);
}
int main(){
    int num;
    scanf("%d", &num);
    func(num);
    return 0;
}
```

## 结构体数据排序plus

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 有一个保存学生课程成绩的结构数组，保存学生的学号、课程编号和课程成绩。
 * 请编写一个函数，将保存在结构数组中的数据先按照课程编号从小到大，再按照课程成绩从高到低的顺序存放在一个单向链表中。
 * 结构体定义如下 
 *  struct student {
 *      int sno; // 学生编号
 *      int cno; // 课程编号
 *      float score;
 *  };
 * 
 * 分析
 * 与2005-1-4大致相似，只是需要重新编写一下
 * 
 * 要点
 * - 链表的头插法构建
 * - malloc函数
 * - 快速排序qsort的库包含及调用
 * - 函数指针、void指针及指针类型转换
 * 
 * 背景知识
 * qsort方法中比较函数的返回值
 *  int compare(const void* a, const void* b)中：
 *  若返回值为负整数，则a会被排在b之前
 *  若返回值为正整数，则a会被排在b之后
 *  若返回值是0，则a和b不区分前后
*/

# include <stdlib.h> // 为了使用qsort和malloc

struct student {
    int sno; // 学生编号
    int cno; // 课程编号
    float score;
};

struct node_stu {
    student value;
    node_stu* next;
};

int compare_stu(const void* a, const void* b) { // 考虑到本题目score是float，故不可轻易返回差值。 // 反向排序
    if (((student*)a)->cno == ((student*)b)->cno) // 注意观察括号
        return ((student*)a)->score > ((student*)b)->score ? 1 : -1;
    else return -((student*)a)->cno + ((student*)b)->cno;
}

node_stu* sort_student(student* array, unsigned int length) {
    if (!length) return 0;
    qsort(array, length, sizeof(student), compare_stu);
    node_stu* res = 0;
    for (unsigned int i = 0; i < length; ++i) {
        node_stu* temp = (node_stu*)malloc(sizeof(node_stu));
        temp->next = res;
        temp->value.sno = array[i].sno;
        temp->value.cno = array[i].cno;
        temp->value.score = array[i].score;
        res = temp;
    }
    return res;
}
```

## 输入 5 个单词，按**从大到小的字典序**排序并输出

```c
#include <stdio.h>
#include <string.h>

#define SIZE 5
#define LEN 100

int main() {
    char words[SIZE][LEN];
    char temp[LEN];

    // 输入 5 个单词
    for (int i = 0; i < SIZE; i++) {
        scanf("%s", words[i]);
    }
	
    // 选择排序：按字典序从大到小排序
    for (int i = 0; i < SIZE - 1; i++) {
        int max_idx = i;
        for (int j = i + 1; j < SIZE; j++) {
            if (strcmp(words[j], words[max_idx]) > 0) {
                max_idx = j;
            }
        }
        if (max_idx != i) {
            strcpy(temp, words[i]);
            strcpy(words[i], words[max_idx]);
            strcpy(words[max_idx], temp);
        }
    }

    // 输出排序后的单词
    for (int i = 0; i < SIZE; i++) {
        printf("%s\n", words[i]);
    }

    return 0;
}

```

## 判断数根（必须熟练）

**题目：**
 输入一个正整数，输出该数的数根。数根可以通过把一个数的各个位上的数字加起来得到，如果得到的数是一位数，那么这个数就是数根；如果结果是两位数或者包括更多位的数字，那么再把这些数字加起来，如此进行下去，直到得到是一位数为止。

比如，对于 24 来说，把 2 和 4 相加得到 6，由于 6 是一位数，因此 6 是 24 的数根。

再比如 39，把 3 和 9 加起来得到 12，由于 12 不是 1 位数，因此还得把 1 和 2 加起来，最后得到 3，这是一个 1 位数，因此 3 是 39 的数根。

------

**输入：**
 一个正整数 n

**输出：**
 输出 n 的数根

**输入样例：**

```
39
```

**输出样例：**

```
3
```

```c
#include <stdio.h>
int func(int n){
    int res =0;
    while(n){
        res += n%10;
        n /= 10;
    }
    if(res < 10) return res;
    else return func(res);
}

int main(){
    int num;
    scanf("%d", &num);
    printf("%d", func(num));
    return 0;
}
```

## 字符串比较（必须熟练）

**题目：**
 编写程序，将两个不包含换行符的字符串 s1 和 s2 比较，若 s1 > s2，输出 1；若 s1 = s2，输出 0；否则输出 -1，要求不准调用 strcmp 函数。

**输入：**
 输入总共两行，第一行是第一个字符串，第二行是第二个字符串，字符串的长度均小于 100。

**输出：**
 第一个字符串和第二个字符串比较后的结果

------

**输入样例：**

```nginx
bc  
abcd
```

**输出样例：**

```
1
```

```c
#include <stdio.h>

// 字符串的比较规则参考视频讲解，并非只是根据长度比较，而是按位比较字符大小
int cmpStr(char s1[], char s2[]) {
    int result = 0; // 记录比较的结果，初始化假设相等
    int i = 0;      // 从索引 0 开始按位逐个比较
    while (s1[i] == s2[i] && s1[i] != '\0' && s2[i] != '\0')
        i++;

    if (s1[i] == '\0' && s2[i] == '\0')
        result = 0;  // 两个字符串完全相等的情况，说明他们都到了空字符
    else  // s1 和 s2 的第 i 个字符出现了不同，则只要看哪个更大即可
        result = (s1[i] - s2[i]) > 0 ? 1 : -1;

    return result;
}

int main() {
    char s1[101] = {'\0'}, s2[101] = {'\0'};
    gets(s1);  // 输入第一个字符串
    gets(s2);  // 输入第二个字符串
    printf("%d", cmpStr(s1, s2));
    return 0;
}

```

## 质因数分解(递归)

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 从键盘上输入整数n，将n分解为若干质数之积，并输出之。
 * 
 * 分析
 * 算是一道比较有水平的题目。个人建议使用递归来做会比较快乐。
 *  
 * 代码解释
 * 边找因数，边检验输入数是否是素数。若是素数，直接输出，若不是素数，一分为二，分别进行。
 * 对素数输入的效率可能在O(n)。
 */

# include <stdio.h>

void divid(unsigned int target) {
    for (unsigned int i = 2; i <= target; i++) {
        if (i == target) {
            printf("%d", target);
            return;
        } else if (target % i == 0) {
            divid(i);
            return divid(target / i);
        }
    }
}
```

## 寻找特殊偶数

**题目内容**

有一种特殊偶数，它每一位上的数字都两两不相同。我们现在需要找出四位数中某一区间内的这类偶数。

**输入格式：** 所要寻找的四位偶数的范围。每组输入有两个数字：第一个数字是最小范围；第二个数字是最大范围。如果输入遇到0，输入结束。

**输出格式：** 列出此范围内的所有特殊偶数，并且列出此范围内特殊偶数的个数。

**输入样例：**

```
3000 3020
6040 6090
0 0
```

**输出样例：**

```
3012 3014 3016 3018
counter=4
6042 6048 6052 6054 6058 6072 6074 6078 6082 6084
counter=10
```

**输入样例：**

```
9 101
87 -1
0 0
```

**输出样例：**

```
Error
Error
Error
```

```c
#include <stdio.h>

// 判断一个数字的每一位是否都不同
int isSpecialEven(int num) {
    if (num % 2 != 0) return 0; // 必须是偶数

    int digits[10] = {0};
    int temp = num;

    while (temp > 0) {
        int d = temp % 10;
        if (digits[d]) return 0; // 出现重复
        digits[d] = 1;
        temp /= 10;
    }

    return 1;
}

int main() {
    int a, b;
    while (1) {
        scanf("%d %d", &a, &b);
        if (a == 0 && b == 0) break;
        if (a > b || a < 1000 || b > 9999) {
            printf("Error\n");
            continue;
        }
        int count = 0;
        for (int i = a; i <= b; i++) {
            if (isSpecialEven(i)) {
                printf("%d ", i);
                count++;
            }
        }
        printf("\ncounter=%d\n", count);
    }
    return 0;
}

```

## 字符串排序（必须熟练）

**题目**

编写程序，实现字符串之间的升序排序并输出结果。

**输入**：
 第一行是一个整数 n，代表 n 个字符串（n < 10 且每个字符串长度都小于 80），接下来 n 行都是字符串。

**输出**：
 递增排序后的结果。

**输入样例**

```
3
abc
b
aaaaa
```

**输出样例**

```
aaaaa
abc
b
```

```c
#include <stdio.h>
#include <string.h>

#define maxsize 81

// 考虑能否对复合结构排序，字符串的比较规则参考上一个题目的视频讲解
int cmpStr(char s1[], char s2[]) {
    int result = 0; // 记录比较的结果，初始化假设相等
    int i = 0;      // 从索引0开始挨位比较
    while(s1[i] == s2[i] && s1[i] != '\0' && s2[i] != '\0')
        i++;
    if(s1[i] == '\0' && s2[i] == '\0')
        result = 0;  // 两个字符串完全相等的情况，说明他们都到了空字符串
    else // s1 和 s2 的第 i 个字符出现不同，则只要看哪一个更小即可
        result = (s1[i] - s2[i]) > 0 ? 1 : -1;
    return result;
}

void selectSort(char str[][maxsize], int n){
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++)
            if (cmpStr(str[j], str[minIndex]) == -1) // 排序基准
                minIndex = j;

        // 交换最小值和第 i 个元素的位置
        // 注意不能直接写 str[i] = str[minIndex]，可以调用 strcpy 或者自己写
        char temp[maxsize] = {'\0'};
        strcpy(temp, str[i]);
        strcpy(str[i], str[minIndex]);
        strcpy(str[minIndex], temp);
    }
}

int main(){
    int n;
    scanf("%d", &n);
    char str[n][maxsize];
    for(int i = 0; i < n; i++)
        scanf("%s", str[i]);
    selectSort(str, n);
    for(int i = 0; i < n; i++)
        printf("%s\n", str[i]);
    return 0;
}
```

## 同构数

**题目：**
正整数 n 如果是它的平方的尾部，则称为同构数，例如 5 的平方是 25，而 25 的尾部是 5，所以 5 是一个同构数。请编写程序实现判断一个数是否是同构数，是的话输出 yes，否则输出 no

**输入：** 一个整数 n
**输出：** 判断是否是同构数，是的话输出 yes，否则输出 no

**输入样例：** `25`
**输出样例：** `yes`

```c
#include <stdio.h.h>  
// 本质上也是提取数位，根据题目定义做即可  
int func(int n){  
    int nSquare = n * n;  
    // 不断取两个数的最低位的数位比较，直到某个数的所有位全部比完为止  
    while( n != 0 && nSquare != 0){  
        if(n % 10 != nSquare % 10) // 一旦出现数位不同就说明  
            return 0;  
        else{ // 舍弃最低位准备比较下一位  
            n /= 10; nSquare /= 10;  
        }  
    }  
    return 1;  
}  
  
int main(){  
    int n; scanf("%d", &n);  
    printf("%s", func(n) == 1 ? "yes" : "no"); return 0;  
    // 为了排版一起就把 main 函数部分语句挤到一起啦  
}

```

## 奇数求和

**题目内容：** 用递归算法实现，输入整数 n (n>0)，求 1+3+5+7....+(2*n-1) 的和

**输入格式:** 输入整数 n

**输出格式:** 输出和

**输入样例:**

```
5[回车]
```

**输出样例:**

```
25[回车]
```

```c
#include <stdio.h>

// 递归函数实现奇数求和
int odd_sum(int n) {
    if (n == 1)
        return 1;
    return odd_sum(n - 1) + (2 * n - 1);
}

int main() {
    int n;
    scanf("%d", &n);      // 输入整数 n
    printf("%d\n", odd_sum(n));  // 输出奇数和
    return 0;
}
```

## 进制求值

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 一个自然数的七进制表示是一个三位数，而这个自然数的九进制表示也是一个三位数，
 * 且这两个三位数的数码顺序刚好相反，求这个三位数
 * 
 * 分析
 * 基本进制转换题。然后还有传统艺能"反着写"，基本白送。
 */

int find_number() {
    for (int i = 7 * 7 * 7; i < 7 * 7 * 7 * 7; i++) {
        int temp_num = i;
        int num_in_7 = 0, num_in_9 = 0;
        while (temp_num) {
            num_in_7 *= 10;
            num_in_7 += temp_num % 7;
            temp_num /= 7;
        }
        temp_num = i;
        while (temp_num) {
            num_in_9 *= 10;
            num_in_9 += temp_num % 9;
            temp_num /= 9;
        }
        int reversed_num_9 = 0;
        while (num_in_9) {
            reversed_num_9 *= 10;
            reversed_num_9 += num_in_9 % 10;
            num_in_9 /= 10;
        }
        if (reversed_num_9 == num_in_7) return i;
    }
    return -1;
}
```

## 十进制转K进制（必须熟练）

**题目：** 把十进制正整数转换为K进制字符串，题目设定K不大于16 且长度不会超过32位
**输入：** 一个正整数n 和转换的进制K
**输出：** 正整数的K进制字符串

**输入样例：** 60 16
**输出样例：** 3C（如果存在字母则统一大写

```c
#include <stdio.h>
#define maxsize 100

// 十进制转K进制的底层原理， 取除k 取余法实现十进制转任意进制必须熟练
int decimalToKBase(int decimal, int k, char result[], int resLen){
    while(decimal != 0){
        int remainder = decimal % k;
        char bit = (remainder >= 10) ? ('A' + remainder - 10) : ('0' + remainder);
        result[resLen++] = bit;  // 注意从左往右存的是低位, 所以需要逆序
        decimal = decimal / k;
    }

    for(int i = 0, j = resLen - 1; i < j; i++, j--){  // 通过逆序让高位在左
        char temp = result[i]; result[i] = result[j]; result[j] = temp;
    }
    return resLen;
}

int main(){
    int decimal, k;
    scanf("%d %d", &decimal, &k);
    char result[maxsize] = {};  // 用于存储结果, 用char 类型更方便
    int resLen = 0;
    resLen = decimalToKBase(decimal, k, result, resLen);
    for(int i = 0; i < resLen; i++)
        printf("%c", result[i]);
    return 0;
}

```

## 奖牌排名（必须熟练）

**题目：** 编程实现一个国家奖牌系统，排名规则：金牌最多者在前，金牌相同的国家则银牌较多者在前，银牌相同的国家则比较铜牌，铜牌相同的国家则根据英文名字按照字符比较规则从小到大排列。

**输入：** 第一行输入 n 代表 n 个国家，剩下的 n 行分别输入四个信息：国家名称、金牌数量、银牌数量、铜牌数量，用空格隔开，国家名称不会超过 20 个字符。

**输出：** 按照奖牌系统排序规则后的国家名称，用空格隔开。

**输入样例：**

```
4
AC 10 20 30
BDE 10 30 20
C 15 40 10
DF 10 30 20
```

**输出样例：**

```
C BDE DF AC
```

```c
#include <stdio.h>
#include <string.h>

// 考察把多层次排序的元素封装成一个结构体，然后转换成对结构体的排序

// 定义一个国家的奖牌数量的结构体
typedef struct {
    int medalNums[3]; // 从左往右依次代表金、银、铜
    char name[21];    // 国家名称
} medalList;

// 比较基准函数，如果 a 应该排在 b 的前面，则返回 1，否则返回 -1
int compare(medalList a, medalList b) {
    for (int i = 0; i < 3; i++) {
        if (a.medalNums[i] != b.medalNums[i])
            return a.medalNums[i] > b.medalNums[i]; // 比较金、银、铜奖牌数量
    }
    // 走到这里则说明奖牌数量完全相同，只要比较名字 a 是否小于名字 b 即可
    return strcmp(a.name, b.name) == -1;
}

// 一页装不下这么多代码，排序函数和 main 函数都在下一页，尽可能让大家看得完整些
// 从大到小排序，有了基准就改一下数据的类型就行了
void selectionSort(medalList arr[], int n) {
    for (int i = 0; i < n; i++) {
        int maxIndex = i;
        for (int j = i + 1; j < n; j++)
            if (compare(arr[j], arr[maxIndex]) == 1) // 调用基准函数
                maxIndex = j; // 找到按照规则更优的结构体的索引
        medalList temp = arr[i];
        arr[i] = arr[maxIndex];
        arr[maxIndex] = temp; // 交换三部曲
    }
}

int main() {
    int n;
    scanf("%d", &n);
    medalList arr[n];
    for (int i = 0; i < n; i++)
        scanf("%s %d %d %d", arr[i].name, &arr[i].medalNums[0],
              &arr[i].medalNums[1], &arr[i].medalNums[2]);
    selectionSort(arr, n);
    for (int i = 0; i < n; i++)
        printf("%s ", arr[i].name);
    return 0;
}
```

## 求字符串长度的递归函数

```c
/* 
 * Chinese Comment by UTF-8
 * 
 * 题目内容
 * 请编写一个求字符串长度的递归函数，函数原型为：int strlong(char *string)
 * 
 * 分析
 * 传统艺能：循环转尾递归
 */

int strlong(char* string) {
    if (string[0] == '\0') return 0;
    else return strlong(string + 1) + 1;
}
```

## 卖鸭子

**题目内容：**

编程调用递归函数。一个人赶着鸭子去每个村庄卖，每经过一个村子卖去所赶鸭子的一半又一只。这样他经过了7个村子后还剩2只鸭子，问他出发时共赶多少只鸭子？经过每个村子时依次卖出多少只鸭子？

**输入格式：**

无

**输出格式：**

出发时总鸭子数

每个村子卖出鸭子数

**输出样例：** (不是结果，仅表示格式)

```c
sum=25[回车]
sell=8,sell=4,[回车]
```

**✅ 算法思路（逆推递归）**

设：

- 第 $n$ 个村子进入前有 $x$ 只鸭子
- 经过第 $n$ 个村子后卖出 $x/2 + 1$ 只，剩下 $x - (x/2 + 1) = x/2 - 1$

反过来：

- 如果第 $n$ 个村子**后剩下 $y$ 只**
- 那么进入前应该是 $x = 2 \times (y + 1)$

从最后一个村子往前递归回去即可。

```c
#include <stdio.h>

#define VILLAGE_NUM 7

// 递归计算出发前总鸭子数
int calc(int n, int left[], int sell[]) {
    if (n == VILLAGE_NUM) {
        left[n] = 2;  // 最后一个村子卖完之后剩2只
    } else {
        calc(n + 1, left, sell);
        left[n] = (left[n + 1] + 1) * 2;
    }
    // 每个村子卖掉多少鸭子
    sell[n] = left[n] / 2 + 1;
    return left[0];
}

int main() {
    int left[VILLAGE_NUM + 1];  // 每个村子前剩余鸭子数（从出发到最后）
    int sell[VILLAGE_NUM];      // 每个村子卖出的鸭子数

    int total = calc(0, left, sell);

    printf("sum=%d\n", total);
    for (int i = 0; i < VILLAGE_NUM; i++) {
        printf("sell=%d", sell[i]);
        if (i < VILLAGE_NUM - 1) printf(",");
    }
    printf("\n");

    return 0;
}

```

