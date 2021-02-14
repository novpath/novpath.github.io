---
title: PAT Basic Practice
toc: true
tags:
  - 代码
  - C
  - C++
  - 算法
categories:
  - 代码
  - OJ练习
sticky: false
mermaid: false
math: false
date: 2021-02-14 22:31:01
---

## **1003 我要通过！ (20 分)**

**答案正确**”是自动判题系统给出的最令人欢喜的回复。本题属于 PAT 的“**答案正确**”大派送 —— 只要读入的字符串满足下列条件，系统就输出“**答案正确**”，否则输出“**答案错误**”。

得到“**答案正确**”的条件是：

1. 字符串中必须仅有 `P`、 `A`、 `T`这三种字符，不可以包含其它字符；
2. 任意形如 `xPATx` 的字符串都可以获得“**答案正确**”，其中 `x` 或者是空字符串，或者是仅由字母 `A` 组成的字符串；
3. 如果 `aPbTc` 是正确的，那么 `aPbATca` 也是正确的，其中 `a`、 `b`、 `c` 均或者是空字符串，或者是仅由字母 `A` 组成的字符串。

现在就请你为 PAT 写一个自动裁判程序，判定哪些字符串是可以获得“**答案正确**”的。

<u>**输入格式：**</u>

每个测试输入包含 1 个测试用例。第 1 行给出一个正整数 *n* (<10)，是需要检测的字符串个数。接下来每个字符串占一行，字符串长度不超过 100，且不包含空格。

<u>**输出格式：**</u>

每个字符串的检测结果占一行，如果该字符串可以获得“**答案正确**”，则输出 `YES`，否则输出 `NO`。

<u>**输入样例：**</u>

```in
8
PAT
PAAT
AAPATAA
AAPAATAAAA
xPATx
PT
Whatever
APAAATAA
```

<u>**输出样例：**</u>

```out
YES
YES
YES
YES
NO
NO
NO
NO
```

**解析**：如果P和T字母将待测串分成三个部分a、b、c串(a、b、c为串长)，正确的型其实就两种，一种是前后a、c串长度相等(可以为0)，b=1；另一种是b = 2， c = 2a，以及之后递推得到的关系式；这些情况可以合并为一种等式关系，就是**c = b * a**。此外还要注意P、A、T三个字母都**至少要出现一次**，且**P必须在T之前**出现，所以有如下代码。

```c++
#include<stdio.h>

int main(){
    int num;
    int flagP, flagA, flagT;
    int stra, strb, strc;
    char str[100] = {0, };
    int a[10] = {0, };
    scanf("%d", &num);
    for(int i = 0; i < num; i++){
        scanf("%s", str);
        flagP = 0;
        flagA = 0;
        flagT = 0;
        stra = strb = strc = 0;
        for(int j = 0; j < 100 && str[j] != '\0'; j++){
            if(str[j] == 'P')
                flagP ++;
            if(str[j] == 'A')
                flagA ++;
            if(str[j] == 'T')
                flagT ++;
            if(flagP == 0 && flagT == 0 && str[j] == 'A')
                stra++;
            if(flagP == 1 && flagT == 0 && str[j] == 'A')
                strb++;
            if(flagP == 1 && flagT == 1 && str[j] == 'A')
                strc++;
            if(flagP == 0 && flagT >= 1)
                break;
            if(str[j] != 'P' && str[j] != 'A' && str[j] != 'T'){
                flagP = 0;
                break;
            }
        }
        if(flagP == 1 && flagA >= 1 && flagT == 1 ){
            if(strc == strb * stra)
			a[i] = 1;
        }
    }
    for(int k = 0; k < num; k++){
        if(a[k] == 0)
            printf("NO\n");
        else
            printf("YES\n");
    }
    
    return 0;
}
```



