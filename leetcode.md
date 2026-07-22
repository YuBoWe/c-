## 2.



```c
struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2) {
    struct ListNode* p = l1, *q = l2;
    struct ListNode* head = NULL;
    struct ListNode* tail = NULL;
    int addnum = 0;

    while(p && q){
        struct ListNode* r = (struct ListNode*)malloc(sizeof(struct ListNode));
        int sum = p->val + q->val + addnum;
        r->val = sum % 10;
        addnum = sum / 10;
        r->next = NULL;

        if (head == NULL){
            head = r;
            tail = r;
        } else{
            tail->next = r;
            tail = tail->next;
        }
        p = p->next;
        q = q->next;
    }

    while(p) {
        struct ListNode* r = (struct ListNode*)malloc(sizeof(struct ListNode));
        if(addnum){
            int temp = p->val + addnum;
            addnum = temp / 10;
            r->val = temp % 10;
        } else {
            r->val = p->val;
        }
        r->next = NULL;
        tail->next = r;
        tail = tail->next;
        p = p->next;
    }

    while(q) {
        struct ListNode* r = (struct ListNode*)malloc(sizeof(struct ListNode));
        if(addnum){
            int temp = q->val + addnum;
            addnum = temp / 10;
            r->val = temp % 10;
        } else {
            r->val = q->val;
        }
        r->next = NULL;
        tail->next = r;
        tail = tail->next;
        q = q->next;
    }

    if (addnum){
        struct ListNode* r = (struct ListNode*)malloc(sizeof(struct ListNode));
        r->val = addnum;
        r->next = NULL;
        tail->next = r;
    }

    return head;
}
```



## 3. Longest Substring Without Repeating Characters

Given a string `s`, find the length of the **longest**  without duplicate characters.



**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3. Note that "bca" and "cab" are also correct answers.
```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```



```c
int find(char *str, char b, int tail, int end){ // 找到字符重复位置
    for(int i = tail; i < end; i++){
        if(str[i] == b) return i;
    }
    return -1;
}

int lengthOfLongestSubstring(char* s) {
    if (!s || !*s) return 0;

    char str[40000] = {0};
    int tail = 0, index = 0;
    int n = 0, max = 0;

    for(int i = 0; s[i]; i++){
        int pos = find(str, s[i], tail, index);
        str[index++] = s[i];

        if(pos != -1){
            tail = pos + 1;
            n = index - tail;
        } else {
            n++;
        }

        if(max < n) max = n;
    }
    return max;
}

```

标准解

```c
#include <string.h>

int lengthOfLongestSubstring(char* s) {
    if (!s || !*s) return 0;

    int last[256];
    memset(last, -1, sizeof(last));

    int left = 0;
    int maxLen = 0;

    for (int i = 0; s[i]; i++) {
        unsigned char c = (unsigned char)s[i];

        if (last[c] >= left) {
            left = last[c] + 1;
        }

        last[c] = i;

        int curLen = i - left + 1;
        if (curLen > maxLen)
            maxLen = curLen;
    }
    return maxLen;
}

```



## 4. Median of Two Arrays

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

 

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

**Example 2:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```



easy

```c
double findMedianSortedArrays(int* nums1, int nums1Size, int* nums2, int nums2Size) {
    int resSize = nums1Size + nums2Size;
    int res[resSize+1];
    int i = 0, j = 0, k = 0;
    while(i< nums1Size && j < nums2Size){
        if(nums1[i] < nums2[j]){
            res[k++] = nums1[i++];
        } else res[k++] = nums2[j++];
    }
    while(i < nums1Size)res[k++] = nums1[i++];
    while(j < nums2Size)res[k++] = nums2[j++];
    
    double median;
    if(resSize % 2 == 0){
        int n = resSize/2;
        median = (res[n-1] + res[n])/2.0;
    }else{
        int n = resSize/2;
        median = res[n];
    }

    return median;
}
```



## 5. Longest Palindromic Substring

Given a string `s`, return *the longest*  pallindromic substring (A **substring** is a contiguous **non-empty** sequence of characters within a string) in `s`.

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"
```

hard

```c
#include <stdlib.h>
#include <string.h>

int foo(char *s, int l, int r) {
    while (l < r) {
        if (s[l] != s[r]) return 0;
        l++;
        r--;
    }
    return 1;
}


char* longestPalindrome(char* s) {
    int len = strlen(s);
    if (len == 0) return strdup("");

    int bestL = 0;
    int bestLen = 1;

    for (int i = 0; i < len; i++) {
        for (int j = i; j < len; j++) {
            int curLen = j - i + 1;
            if (curLen <= bestLen) continue;

            if (foo(s, i, j)) {
                bestL = i;
                bestLen = curLen;
            }
        }
    }

    char *res = malloc(bestLen + 1);
    strncpy(res, s + bestL, bestLen);
    res[bestLen] = '\0';
    return res;
}

```



```
CHANGE MASTER TO 
  MASTER_HOST='mysql-0.mysql-headless.ops.svc.cluster.local',
  MASTER_USER='repl',
  MASTER_PASSWORD='123456',
  MASTER_LOG_FILE='mysql-0-bin.000003', -- 填刚才查到的File
  MASTER_LOG_POS=827; -- 填刚才查到的Position
START SLAVE;
```
