---
title: C与C++的若干易遗忘点
toc: true
tags:
  - 代码
  - 语言
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

    (1)%.mf ：让浮点数保留m位小数输出（规则为四舍六入五成双）
    
    (2)%md ：以m位进行对齐输出，高位用空格填充。
    
    (3)%0md ：以m位进行对齐输出，高位用0填充。
    
15. getchar和putchar用来输入和输出键盘上键入的**单个字符**，getchar可以识别空白符（换行符及空格）。

16. 常用math函数

    (1)绝对值函数：fabs(double x)

    (2)向下取整及向上取整函数：floor(double x)和ceil(double x)

    (3)幂函数：pow(double r, double x)

    (4)算术平方根函数：sqrt(double x)

    (5)以**自然对数为底**的对数函数：log(double x)

    (6)三角函数（单位**弧度制**）：sin(double x)、cos(double x)、tan(double x)

    (7)四舍五入函数：round(double x)

17. switch语句：分支条件较多时比较适用

    case间不加break，则会按由上至下的顺序依次执行语句

    ```c
    switch(表达式){
        case 常量表达式1:
            ...
            break;
        case 常量表达式2:
            ...
            break;
         .
         .
         .
        case 常量表达式n:
            ...
            break;
        default:
            ...
    }
    ```

18. break和continue的区别：break跳过当前作用的整个循环，continue跳过当前循环的一轮。

19. C99支持数组大小下标定义时用变量，一般来说数组大小必须是整型常量。

20. memset函数（对数组中的每一个元素赋予相同的值，建议赋0或-1）

    ```c++
    #include<cstring>
    memset(数组名, 0, sizeof(数组名) )
    memset(数组名, -1, sizeof(数组名) )
    ```

21. 字符数组允许直接**赋值字符串**来初始化，但**其他位置不允许**。

    ```c++
    //普通初始化
    char str[15] = {'H', 'e', 'l', 'l', 'o'};
    //直接赋值字符串
    char str[15] = "Hello";
    ```

22. gets输入字符串，puts输出字符串。gets以换行符\n作为输入结束标志，scanf完一个整数后，如果要使用gets需要先用getchar接收整数后的换行符，然后将其存放于一个一维数组中。puts将一维数组的内容在界面上输出，然后输出一个换行符。

23. 每个一维数组的结尾都有一个**空字符\0**表示存放字符串的结尾。

24. 常用的string函数。

    ```c++
    #include<stdio.h>
    #include<string.h>
    int main(){
        char str1[10] = "Hi";
        char str2[10] = "nihao";
        int len = strlen(str1);        // len = 2
        int cmp = strcmp(str1, str2);  // cmp < 0,说明前者字典序更小
        strcat(str1, str2);            // str1 = "Hinihao"
        strcpy(str1, str2);            // str1 = "nihao"
        return 0;
    }
    ```

25. sscanf和sprintf处理字符串。前者作用是将字符数组str中的内容以"%d"的格式写到n中，后者则相反，是将n以"%d"的格式写到str字符数组中。

    ```c++
    #include<stdio.h>
    int main(){
        int n;
        char str[10] = "250";
        sscanf(str, "%d", &n);
        printf("%d\n", n);     //输出250
        int m = 520;
        sprintf(str, "%d", m);
        printf("%s\n", str);   //输出520
        return 0;
    }
    ```

26. 数组可以作函数参数，参数中数组第一维不用填写长度，二维数组需要填写第二维长度，函数调用时仅需填写数组名。**数组传参时，函数内对数组的修改等同与对原数组元素的修改，这与一般值传递不同。**

    ```c++
    #include<stdio.h>
    
    void paraTran(int a[], int b[][3]){
        a[0] = 3;
        a[1] = 2;
        a[2] = 1;
        b[2][2] = 1;
    }
    
    int main(){
        int a[3] = {0, };
        int b[3][3] = {0, };
        paraTran(a, b);
        for(int i = 0; i < 3; i++){
            printf("a[%d] = %d\n", i, a[i]);
            for(int j = 0; j < 3; j++){
                printf("b[%d][%d] = %d\n", i, j, b[i][j]);
            }
        }
        return 0;
    }
    ```

27. 同时定义好几个同类型的指针变量，' * '只会结合于第一个变量名。

    ```c++
    int* p1, p2; //p1为int*型、p2为int型
    ```

28. 对于int*类型的指针变量p来说，p+1是p所指int型变量的**下一个int型变量**的地址，为4个Byte。

    两个指针相减，等价于在求两个指针之间相差了几个**基类型**。

29. 由于指针变量可以进行加减法，a+i等价于&a[i]，*(a+i)等价于a[i]；

30. swap函数观察指针的作用

    ```c++
    void swap(int* a, int* b){
        int temp = *a;
        *a = *b;
        *b = temp;
    }
    ```

31. C++**引用**实现地址交换达到交换两个变量的效果。

    ```c++
    //无法达到效果，因为main函数传入的是地址的副本
    void swap(int* a, int * b){
        int* temp = a;
        a = b;
        b = temp;
    } 
    //可以达到效果,swap内对指针的修改能够返回main()
    void swap(int* &p1, int* &p2){
        int* temp = p1;
        p1 = p2;
        p2 = temp;
    }
    ```

32. **常量不可以引用**

    ```c++
    #include<stdio.h>
    
    void swap(int* &p1, int* &p2){
        int* temp = p1;
        p1 = p2;
        p2 = temp;
    }
    
    int main(){
        int a = 0, b = 1;
        int *p1 = &a, *p2 = &b;
        swap(p1, p2);
        printf("a = %d, b = %d\n", *p1, *p2);
        return 0;
    }
    ```

    

33. 结构体的定义

    ```c++
    struct studentInfo{
        int id;
        char gender; //'F' or  'M'
        char name[20];
        char major[20];
    }ZhangSan, stu[10];
    //等价于
    struct studentInfo{
        int id;
        char gender; //'F' or  'M'
        char name[20];
        char major[20];
    }
    studentInfo ZhangSan;
    studentInfo stu[10];
    ```

    

34. 结构体内部可以定义除自己本身外的任何数据类型，但可以定义自身类型的指针变量。

    ```c++
    struct Node{
        int data;
        struct Node *next;
    }
    //通常也可以把结构体命名为Node，之后就不用加上struct了
    typedef struct Node{
        int data;
        struct Node *next;
    }Node;
    ```

35. 访问结构体内的元素

    ```c++
    struct studentInfo{
        int id;
        char gender; //'F' or  'M'
        char name[20];
        char major[20];
        studentInfo* next;
    }ZhangSan, *p;
    //访问学生张三的信息操作如下
    ZhangSan.id
    ZhangSan.name
    ZhangSan.next
    //访问p中元素的方法
    (*p).id
    (*p).name
    (*p).next
    p->id
    p->name
    p->next
    ```

36. 结构体的初始化与赋值

    ```c++
    #include<stdio.h>
    #include<string.h>
    
    struct studentInfo{
        int id;
        char gender; //'F' or  'M'
        char name[20];
        char major[20];
        studentInfo* next;
    }ZhangSan, *p;
    
    int main(){
        /*可以单独赋值如ZhangSan.id = 1，但字符数组除了初始化
        外不能直接赋字符串，如ZhangSan.major = "math"*/
    	struct studentInfo ZhangSan = {20211289, 'M', 
    	"ZhangSan", "Material Science"};  
    	char str[20] = {'0'};  //不可以直接赋右值ZhangSan.major
    	strcpy(str, ZhangSan.major);
    	printf("%s", str);
        scanf("%d %s", &ZhangSan.id, &ZhangSan.name);//支持读入时赋值
        printf("id = %d name = %s", ZhangSan.id, ZhangSan.name);
    	return 0;
    }
    ```

37. 利用生成的**构造函数**进行初始化与赋值。

```c++
struct studentInfo{
    int id;
    char gender;
    //默认生成的构造函数
    studentInfo(){}
};
/*-----------------------*/
//如果要手动提供id和gender的初始化参数
struct studentInfo{
    int id;
    char gender;
    //填写如下的参数用以对结构体的内部变量赋值
    studentInfo(int _id, char _gender){
        //赋值
        id = _id;
        gender = _gender;
    }
}
/*----------------------*/
//自定义构造函数简化
struct studentInfo{
    int id;
    char gender;
    studentInfo(int _id, char _gender): id(_id), gender(_gender){}
}
/*----------------------*/
//构造函数定义完毕后对结构体变量的赋值
studentInfo stu = studentInfo(20211289, 'M');
```

**若自己重新定义了构造函数，则不能不经过初始化就定义结构体变量。**此时studentInfo(){}被自己定义的构造函数覆盖，可以把其重新加上，多个构造函数可以并存，这样更能适应不同的场合。

```c++
struct studentInfo{
    int id;
    char gender;
    //用以不用初始化就定义结构体变量；
    studentInfo(){}
    //只初始化gender参数
    studentInfo(char _gender){
        gender = _gender;
    }
    //同时初始化所有参数
    studentInfo(int _id, char _gender){
        id = _id;
        gender = _gender;
    }
};
```

```c++
#include<stdio.h>

struct Point{
    int x, y;
    Point(){}   //这样可以不经过初始化就定义pt[10]
    Point(int _x, int _y): x(_x), y(_y) {} //用以提供x和y的初始化
}pt[10];

int main(){
    int num = 0;
    for(int i = 0; i < 3; i++){
        for(int j = 0; j < 3; j++){
            pt[num++] = Point(i, j); //直接使用构造函数
        }
    }
    for(int k = 0; k < num; k++){
        printf("(%d, %d)", pt[k].x, pt[k].y);
        if((k+1) % 3 == 0)
        printf("\n");
    }
    
    return 0;
}
```

