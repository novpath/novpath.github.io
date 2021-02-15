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

## **1001 害死人不偿命的(3n+1)猜想 (15 分)**

卡拉兹(Callatz)猜想：

对任何一个正整数 *n*，如果它是偶数，那么把它砍掉一半；如果它是奇数，那么把 (3*n*+1) 砍掉一半。这样一直反复砍下去，最后一定在某一步得到 *n*=1。卡拉兹在 1950 年的世界数学家大会上公布了这个猜想，传说当时耶鲁大学师生齐动员，拼命想证明这个貌似很傻很天真的命题，结果闹得学生们无心学业，一心只证 (3*n*+1)，以至于有人说这是一个阴谋，卡拉兹是在蓄意延缓美国数学界教学与科研的进展……

我们今天的题目不是证明卡拉兹猜想，而是对给定的任一不超过 1000 的正整数 *n*，简单地数一下，需要多少步（砍几下）才能得到 *n*=1？

**输入格式：**

每个测试输入包含 1 个测试用例，即给出正整数 *n* 的值。

**输出格式：**

输出从 *n* 计算到 1 需要的步数。

**输入样例：**

```in
3
```

**输出样例：**

```out
5
```

**解析**：快乐模拟就行，直接按题给算法写。

```c++
#include<stdio.h>

int main(){
    int count = 0;
    int num;
    scanf("%d", &num);
    while(num != 1){
        if(num % 2 == 0){
            count++;
            num /= 2;
        }else{
            count++;
            num = (3 * num + 1) / 2;
        }
    }
    printf("%d", count);
    
    return 0;
}
```

## 1002 写出这个数 (20 分)

读入一个正整数 *n*，计算其各位数字之和，用汉语拼音写出和的每一位数字。

**输入格式：**

每个测试输入包含 1 个测试用例，即给出自然数 *n* 的值。这里保证 *n* 小于 10100。

**输出格式：**

在一行内输出 *n* 的各位数字之和的每一位，拼音数字间有 1 空格，但一行中最后一个拼音数字后没有空格。

**输入样例：**

```in
1234567890987654321123456789
```

**输出样例：**

```out
yi san wu
```

**解析：**输入样例过大，应该**当做字符串处理**。先求出串每个数位总和total，然后用数组记录每个数位的值并逆序输出每位读音即可，最后一位后面不输出空格。

```c++
#include<stdio.h>
#include<string.h>

int main(){
	char str[101] = {0, };
	char numbit[4] = {0, };
	int total = 0;
	scanf("%s", str);
	int len = strlen(str);
    for(int i = 0; i < len; i++){
    	total += str[i] - 48;
	}
	int j = 0;
	while(total != 0){
		numbit[j] = total % 10;
		total /= 10;
		j++; 
	}
    for(int k; k < j; k++){
    	switch(numbit[j - k - 1]){
    		case 1:
    			printf("yi");
    			break;
    		case 2:
    			printf("er");
    			break;
    		case 3:
    			printf("san");
    			break;
    		case 4:
    			printf("si");
    			break;
    		case 5:
    			printf("wu");
    			break;
            case 6:
    			printf("liu");
    			break;
    		case 7:
    			printf("qi");
    			break;
    		case 8:
    			printf("ba");
    			break;
    		case 9:
    			printf("jiu");
    			break;
    		case 0:
    			printf("ling");
    			break;
		}
		if(k != j - 1){
			printf(" ");
		}
	}
	
	return 0;
}
```



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



