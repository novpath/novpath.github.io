---
title: C与C++的若干易遗忘点
toc: true
tags:
  - 代码
  - C
  - C++
categories:
  - 代码
  - C语言
sticky: false
mermaid: false
math: false
date: 2021-02-08 18:51:49
---

1. 看码一遍不如手敲一遍，手敲一遍不如理解一遍。

2. 不要在程序中混用cin和scanf、cout与printf有时会出问题。

3. 变量名第一个字符必须是**字母**或者**下划线**，除了第一个字符以外的其他字符必须是**字母、数字**或**下划线**。

4. 如果long long型赋大于2^31-1的初值，需要在初值后面加上LL否则会编译错误。

5. ASCII码的范围是0~127，**小写字母比大写字母的ASCII码值大32**。0~9、A~Z、a~z的ASCII码值为48~57、65~90、97~122.

6. 字符常量一定是**单个字符**，需要用**单引号**标出来。

7. 转义字符\n代表换行；\0代表空字符NULL，其ASCII码值为0；空格(Space)的ASCII码值为32。

8. 字符串常量可以作为初值赋值给字符数组，并用%s的格式输出；不能将字符串常量赋值给字符变量。

   ```c
   #include<stdio.h>
   int main(){
       char str[15] = "I Love You.";
       printf("%s", str);
       return 0;
   }
   ```

9. 定义常量的两种方法

   ```c
   #define e 2.718         //符号常量
   const double e = 2.718; //const常量
   ```

10. **宏定义**加法与两个数的MAX结构。由于宏定义是**整体替换**，为了保证运算顺序与结果符合我们的期望，每个变量外面都要加一个括号。

    ```c
    #define ADD(x, y) ((x) + (y))
    #define MAX(x, y) ((x) > (y) ? (a) : (b))
    ```

11. **数组名本身代表这个数组第一个元素的地址**，调用scanf函数时不用加&。

    ```c
    char str[15];
    scanf("%s", str);
    ```

12. 除了%c外，scanf对其他格式符（如%d、%s）的输入是以空白符(空格或换行等)为结束判断标志的，除非使用%c记录读入的空格，否则其他情况都会自动跳过空格。

    ```c
    #include<stdio.h>
    int main(){
        char str[10];
        int a, b;
        char c;
        scanf("%d%d%c%s", &a, &b, &c, str);    //输入：2 5 Zai(p.s.:2、5虽然隔开但两个%d之间可不加空格)
        printf("a=%d,b=%d,c=%c,str=%s", a, b, c, str); //结果：a=2,b=5,c= ,str=zai
        return 0;
    }
    ```

13. printf和scanf的格式符基本一样，除了数据类型double，前者为%f后者为%lf。

14. 常见输出格式

    