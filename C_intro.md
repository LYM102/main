# C语言入门
> written by Yiming Li

> 本文章的内容参考至Stephen Prata的《C Primer Plus》
## `int`类型认识
- c语言中存在三个附属关键字修饰的基本整数类型：
    1. `short int`（或者简写`short`）类型：占用的储存地方比较小用于较小数值的情况
    2. `long int`(或者简写`long`)类型：占有的储存地方比较多
    
    `long`和`short`都是有符号类型
    
    3. `unsingned int`或者`unsigned`只用于表示非负的情况
    
    现在再计算机中规定`long long`占64位；`long`占32位；`short`占16位;`int`占16位或者是32位
  ```c
  //整数溢出的情况
  #include <stdio.h>
  int main()
  {
      int i = 2147483647;
      unsigned int j = 4294967295,q = 0;

      printf("%d,%d,%d",i,i+1,i+2);
      printf("%u,%u,%u,%u,%u",j,j+1,j+2,q,q-1);
      return 0;
  }
  ```
  ```
  输出结果：
  2147483647,-2147483648,-2147483647
  4294967295,0,1,0,4294967295
  ```
- 打印`short`,`long`,`long long`,`unsigned`的类型：
  ```c
  /*print2.c--更多printf()的特性*/
  #include <stdio.h>
  int main()
  {
      unsigned int un = 3000000000;
      short end = 200;
      long big = 65537;
      long long verybig = 12345678908642;

      printf("un = %u and not %d\n", un, un);
      printf("end = %hd and %d\n", end, end);
      printf("bog = %ld and not %d\n", big, big);
      printf("verybig = %lld and not %d\n", verybig, verybig);
      return 0;
  }
  ```
  ```
  输出结果（可能不同）
  un = 3000000000 and not -1294967296
  end = 200 and 200
  bog = 65537 and not 65537
  verybig = 12345678908642 and not 1942899938
  ```
  - 程序解释：
    1. 使用错误的转换说明会得到意想不到的结果。第一行输出，对于`un`(无符号变量)，使用`%d`会生成负值！其原因是无符号值`3000000000`和有符号值`-1294967296`在系统内存中的内部表示完全相同。因此如果告诉`printf()`函数是一个无符号数，他打印一个值，如果告诉他是有符号数，他打印另外一个值。但是在较小的数字如96就不会出现这样的情况。
    2. 第二行输出，对于`short`类型的变量`end`,在`printf()`中无论是使用(`%hd`)还是`%d`都是一样的结果，这是因为在计算机读取`short`类型的时候都会先将`short`类型转换成`int`类型。那为什么要这样多此一举呢？因为在计算机中使用`int`类型传输数据要更快。
    3. 但是`%hd`中的`h`又有什么作用呢？