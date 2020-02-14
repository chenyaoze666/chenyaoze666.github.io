---
   layout: post
   title:  "C语言 基础 指针和内存"                                                        
   date: 2020-02-11 19:34:18 +0530
   categories: C
---
  C语言基础+指针+内存 个人见解



# C语言

## 什么是C语言

* 一种通用的 过程式的编程语言 广泛用于系统与应用软件的开发
* 人类和计算机交流的一种方式

## ANSIC

* C语言的标准语法

## C语言的特点

* 简单
* 快速
* 高性能
* 兼容性好
* 功能性大
* 易于学习

## 能做什么

* Linux嵌入式 ------- 小工具(cd , ls)
* 和硬件打交道的程序--------(操作系统)
* ARM嵌入式
* 单片机
* Arduino
* 硬件编程-------------(C语言的指针    和内存打交道)
* 对性能要求较高的程序
  * web服务器   nginx     并发量大   比 apache服务器大10倍(C++编写)

## C语言环境

* 基于unix开发   选择linux系统 下编程

## C语言常用指令

* 编辑器

  * emacs
  * vim

* 安装vim

  * sudo apt -get update     更新资源
  * sudo apt -get install vim

* 查看C语言编辑器版本

  * cc -v
  * gcc -v (集成的)

* 常用命令

  * clear                               清除屏幕

  * cd                                    进入目录

    * cd ~                          进入用户家目录

  * pwd                                 查看当前所在目录

  * ls                                      查看当前目录文件和文件夹

    * ls -l                           显示文件类型,创建时间, 用户权限 文件所有者和所属组
      * d开头                表示目录
      * `-`开头               表示普通文件

  * touch                              创建文件

  * rm                                    删除文件

  * mkdir                               创建目录

  * vim/vi                               编辑文件(存在修改,不存在创建)

    * 插入模式                    使用a i o   进入 

    * 命令模式                    Esc

      * a                         当前光标后插入
      * i                           当前光标前插入
      * o                         当前行下一行插入
      * shift + a             当前行尾插入    A
      * shift + i               当前行首插入    I
      * shift + o              当前行上一行插入
      * 
      * x                           删除当前光标位置
      * dd                        删除整行 
      * ​     

      * :w                         保存
      * :q                          退出
      * :wq                       保存并退出

## 第一个C语言程序

* 创建 `a.c` 文件, 使用vim编辑

* 编写文件

  * 导入库     `#include <stdio.h>`

    * `<stdio.h>` 表示标准输入输出流

  * 创建main 

  * ```
    	int main()                 注意不是void
    {
    	printf("hello world!\n");      printf  <stdio.h>提供
    								   \n  表示换行
        return 0;
    }
    ```

* 编译c文件

  * `cc a.c`
  * 输出一个a.out文件
  * 查看文件权限 x  执行文件

* 运行

  * `./a.out`         表示当前目录下的a.out文件 

## C语言实现多个源文件实现一个文件的编程

* 创建`vim hello.c`

  * ```
    #include <stdio.h>
    
    int max (int a, int b)
    {
        if(a>b){
            return a;
        }else{
            return b;
        }
    }
    
    int main()
    {
    	int a1 =33;
        int a2 =21;
        int maxNum=max(a1,a2);
        printf("the max value is %d\n",maxNum);
        return 0;
    }
    ```

* 编译 `gcc hello.c`

* 执行 `./a.out`

* 实现多人编写

  * vim命令模式下 输入 `:sp max.c`
  * 使用`Ctrl + w + 上/下箭头`    选择窗口 

* `:set nu`            打开行号

* 查看max函数所占行数

  * `行数 + dd`   剪切
  * 切换 到 max.c 窗口
  * `p`      粘贴

* 保存所有文件并退出  `:wqa`

* 编译两者  -o 命名 编译后的文件

  * `gcc max.c hello.c -o main.out`

* 执行 `./main.out`
  * 报错    由于max未声明
  * 需在main前声明
    * `#include "max.c"`
      * <>              操作系统预装的库  系统目录 环境变量下查找
      * ""                当前目录查找
    * 这时候编译只需`gcc hello.c`

* 执行`./a.out`

## C语言分开编译文件

* 编译   `gcc -c max.c -o max.o`

* 查看文件权限  `ls -l`

  * max.o    编译了但不可执行

* 删除hello.c  里的max.c的声明在编译

  * //注释

* `gcc max.o hello.c`连着连接

  * 生成 	a.c文件

* 执行 `./a.c`

* 拷贝max.c文件生成 min.c 文件

  * `cp  max.c min.c`

  * 并修改

    ```
    int mix (int a, int b)
    {
        if(a<b){
            return a;
        }else{
            return b;
        }
    }
    ```

* 编译min.c

  *  `gcc -c mix.c -o mix.o`

* 修改hello.c

  * ```
    #include <stdio.h>
    //#include "max.c"
    
    int main()
    {
        int a1 =33;
        int a2 =21;
        int maxNum=max(a1,a2);
        int minNum=min(a1,a2);
        printf("the max value is %d\n,the min value is %d\n",maxNum,minNum);
        return 0;
    }
    ```

* 执行`gcc max.o min.o hello.c`

  * 报错
    * 原因虽然编译了 但是main不清楚 max() 和min() 里的结构
      * 参数名,参数值,函数名,参数类型等

* 声明头文件

  * 创建max.h

  * ```
    int max(int a, int b);
    ```

  * 创建min.h

  * ```
    int min(int a, int b);
    ```

* 修改hello.c

  * ```
    #include <stdio.h>
    #include "max.h"
    #include "min.h"
    
    int main()
    {
        int a1 =33;
        int a2 =21;
        int maxNum=max(a1,a2);
        int minNum=min(a1,a2);
        printf("the max value is %d\n,the min value is %d\n",maxNum,minNum);
        return 0;
    }
    ```

* 执行`gcc max.o min.o hello.c`

## C语言使用makFile的编写

* 方便编译

* `make -v `

  * 安装 `sudo apt-get install make`

* `vim Makefile`       约定相同的文件  (Tab 6个空格)    4个空格不识别  只能以Tab开头

  * ```
    # this is make file
    hello.out:max.o min.o hello.c 
    	gcc max.o min.o hello.c -o hello.out
    max.o:max.c
    	gcc -c max.c
    min.o:min.c
    	gcc -c min.c
    ```

* 执行makefile `make`

  * 默认文件名为 a.out 加 -o 后为 hello.out
  * 好处节约编译时间, 没改动的文件不需要重新编译

## Main函数 && return 0;

* 创建main.c文件

* ```
  #include <stdio.h>
  
  int main(int argv, char* argc[])
  {
      printf("hello word \n");
      return 0;
  }
  ```

* main函数的参数 用于与操作系统交互

* 编译 `gcc main.c -o main.out && ./main.out`

  * &&用于连接多条命令

* 判断上一条命令是否执行成功

  * `echo $?`	
    * 输出为0   则正常     
    * 输出其他 错误码 

* 验证错误码

  * 修改main.c

  * ```
    #include <stdio.h>
    
    int main(int argv, char* argc[])
    {
        printf("hello word \n");
        return 101;
    }
    ```

  * 编译 `gcc main.c -o main2.out && ./main2.out

  * 查看`echo $?`	

    * 输出`101`

  * 输入`./main2.out && ls`

    * 不执行ls

  * 输入`./main.out && ls`

    * 执行ls

## Main参数

* 创建main2.c

* ```
  #include <stdio.h>
  
  int main(int argv, char* argc[])
  {
      printf("argv is %d \n",argv);
      return 0;
  }
  ```

* 编译 `cc main2.c -o m2.out`

* 运行`./m2.out`

  * 输出 argv is 1

* 运行`./m2.out -l`

  * 输出  argv is 2

* 运行`./m2.out -l -a`

  * 输出  argv is 3	

* 创建main3.c

* ```
  #include <stdio.h>
  
  int main(int argv, char* argc[])
  {
      printf("argv is %d \n",argv);
      int i;
      for(i=0; i<argv; i++){
          printf("argc[%d] is %s\n",i,argc[i]);
      }
      return 0;
  }
  ```

* for 里不能初始化i    for(int i=0; i<argv; i++)  报错

* 编译 `cc main3.c -o m3.out`

* 运行`./m3.out -l -a asda dasda`

  * 输出

    ```
    argv is 5
    argc[0] is ./m3.out
    argc[1] is -l
    argc[2] is -a
    argc[3] is asda
    argc[4] is dasda
    ```

## 输入流 输出流 错误流

* 创建文件 cio.c

  * ```
    #include <stdio.h>
    /*
    	指针
    	stdin        读取输入  默认为键盘设备
    	stdout       数据输出  默认为显示器终端
    	stderr       错误流
    */
    int main()
    {
    	printf("hello world! \n");
    	int a;
    	printf("please input :");
    	scanf("%d",&a);
    	printf("input value is : %d\n",a);
    	return 0;
    }
    
    ```

  * &接受输入地址符

* 编译`cc cio.c`

* 执行`./a.out`

* 输入数值回车输出

* 

* stdin        读取输入  默认为键盘设备

* stdout       数据输出  默认为显示器终端

* stderr       错误流

* 可理解为printf("值") 默认封装了一个

  * 输出的函数fprintf(stdout,"值");     f代表文件输出

* scanf("%d",&a);

  * 输入的函数fscanf(stdio,"%d",&a);

* 错误流

  * fprintf(stderr,"错误信息");

* 测试

  * 改造文件cio.c

  * ```
    #include <stdio.h>
    
    int main()
    {
    	// printf("please input the value a\n");
    	fprintf(stdout,"please input the value a :\n");
    	int a;
    	// scanf("%d",&a);
    	fscanf(stdin,"%d",&a);
    	if(a<0){
            fprintf(stderr,"the value must > 0 \n");
            return 1;
    	}
        return 0;
    }
    ```

## Linux通道   管道  重构定向机制

* 创建文件 main.c

  * ```
    #include <stdio.h>
    
    int main()
    {
        printf("input the int value i:\n");
        int i,j;
        scanf("%d",&i);
        printf("input the int value j:\n");
        scanf("%d",&j);
        printf("i+j=%d\n",i+j);
        return 0;
    }
    ```

* 编译`cc main.`c

* 运行 ./a.out

* 改造

  * 重定向   标准的输入流0  输出流1 错误流2   1为默认
  * `./a.out 1>> a.txt`             内容输出到a.txt文件里
  * 生成a.txt

* `cat a.txt`    查看  

* `>>`再次重定向到a.txt   文件`内容不会覆盖`  会记录在下面

* `>`再次重定向到a.txt   文件`内容会覆盖`  

* 创建文件`input.txt`

  * ```
    5
    9
    ```

* 执行`./a.out < input.txt`

* 输出

  * ```
    input the int value i:
    input the int value j:
    i+j=14
    ```

* 改造`main.c`

* ```
  #include <stdio.h>
  
  int main()
  {
      printf("input the int value i:\n");
      int i,j;
      scanf("%d",&i);
      printf("input the int value j:\n");
      scanf("%d",&j);
      if(0!=j){
      	printf("%d/%d=%d\n",i,j,i/j);    
      }else{
       fprintf(stderr,"j != 0\n"); 
       return 1;
      }
      return 0;
  }
  ```

* 编译`cc main.c`

* 运行`./a.out`

* 判断`echo $?`

* 运行`./a.out`

  * 输入j=0的情况

* 导出两者`./a.out 1>t.txt 2>f/txt`

* 查看文件

* 

* 管道符 `|`

* `grep`  查询符合条件的字符串

  * `ls /etc/ | grep ab`   将`|`前的结果给`|`后 通过 `grep` 筛选`符合条件`的字符串输出

* `ps -e`  查看进程

  * `ps -e | grep ssh`    查看是否有ssh进程

## 实用小程序

* 创建`avg.c`

* ```
  #include <stdio.h>
  
  int main()
  {
      int s,n;
      scanf("%d,%d",&s,&n);
      float v=s/n;
      printf("v=%f\n",v);
      return 0;
  }
  ```

* 编译`cc avg.c -o avg.out`

* 测试功能

* 创建`input.c`

* ```
  #include <stdio.h>
  
  int main()
  {
      int flag=1;
      int i;
      int count =0;
      int s=0;
      while(flag){
          scanf("%d",&i);
          if(i==0) break;
          count++;
          s+=i;
      }
      printf("%d,%d\n",s,count);
      return 0;
  }
  ```

* 编译`cc input.c -o input.out`

* 测试功能

* 使用管道 `./input.out | ./avg.out`



## C语言指针和内存

* GDB

  * 查看内存和指针变化

* 初谈指针

* 创建`pointer`目录

* 创建`main.c`

* ```
  #include <stdio.h>
  
  void change(int a,int b)
  {
      int tmp =a;
      a=b;
      b=tmp;
  }
  
  
  int main()
  {
      int a=5;
      int b=3;
      
      change(a,b);
      
      printf("num a=%d\n num b =%d\n",a,b);    
      return 0;
  }
  ```

* 编译`gcc main.c -o main.out`

* 运行`./main.out`发现没有变化

* 改造`mian.c`    引入指针

* ```
  #include <stdio.h>
  
  void change(int *a,int *b)
  {
      int tmp =*a;
      *a=*b;
      *b=tmp;
  }
  
  
  int main()
  {
      int a=5;
      int b=3;
      
      change(&a,&b);
      
      printf("num a=%d\n num b =%d\n",a,b);    
      return 0;
  }
  ```

* 编译`gcc main.c -o main.out`

* 运行`./main.out`发现a和b变化

* 

* 其中`int *a`等价于`int* a`

* `&`取地址符

## GDB工具使用

* `gdb -help`

* 使用调试工具gdb

  * `gcc -g main.c -o main.out`

* `cp main.c main2,c`

* ```
  #include <stdio.h>
  
  void change(int a,int b)
  {
      int tmp =a;
      a=b;
      b=tmp;
  }
  
  
  int main()
  {
      int a=5;
      int b=3;
      
      change(a,b);
      
      printf("num a=%d\n num b =%d\n",a,b);    
      return 0;
  }
  ```

* 编译`gcc -g main2.c -o main2.out`

* `gdb ./main2.out`

  * 输入`l`或`list`
  * `回车`或继续`l`
  * `start`调试  默认断点 main入口
  * `p a` 查看当前a值
  * `n` 下一行
  * `p a`查看当前行a值
  * `p b`查看当前行b值
  * 到change函数行时
  * `s`查看内部
  * 实现交换时查看
    * `p a`
    * `p b`
  * `bt`查看函数堆栈  (main函数 和change函数 编号及执行的函数)
    * 通过编号切换函数栈`f 1`
    * 测试两个函数内的a值(两个a是不同的 虽然值可能相同)  `变量的作用域`
  * `q`退出调试
  * 
  * 调试`main.out`
  * 发现 当进行change函数时 传递a,b的是16进制的内存地址
  * `*`找地址对应的位置保存的a
  * 打印`p a`和`p *a`的区别

##   计算机中数据保存 内存

* 内存最小单位 字节(Byte)   8个二进制位

* 十六进制和十进制

* 学习进制之间的转换

* 内存条和地址总线
  * 32位操作系统做多存`2^10*2^10*2^10*2^2`的字节  内存最大为4G
  * `1024*1024*1024*4`字节
  * `1024*1024*4K`
  * `1024*4M`
  * `4G`
  * 64位操作系统最多 `2^60*16`字节
  * 内存地址编号范围
    * `0000 0000 ... ... 0000 0000` 64个
    * ~
    * `1111 1111 ... ... 1111 1111`64个
  * 编号用16进制表示`0xxxx...fffa7`
  * 其中我们能使用的是在 `0x7fffffffffffffff`以下到`0x0`的内存
    * 以上的给操作系统内核
  * 基本分为
    * 系统内存
    * 栈内存                      第一个执行的程序(main)及main之前的函数 
    * 自有可分配内存
    * 堆内存                       
    * 数据段                        (变量)          
    * 代码段                        低位(0x1)编码段      (C语言程序编译之后的文件)        函数      

  ## 内存及指针

  * 创建`main.c`

  * ```
    #include <stdio.h>
    int global=0;
    
    int rect(int a, int b)
    {
        static int count=0;
        count++;
        global++;
        int s=a*b;
        return s;
    }
    
    int quadrate(int a)
    {
        static int count =0;
        count++;
        global++;
        int s=rect(a,a);
            return s;
    }
    
    int main()
    {
        int a=3;
        int b=4;
        int *pa=&a;
        int *pb=&b;
        int *pglobal=&global;
        int (*pquadrate)(int a)=&quadrate;
        int s=quadrate(a);
        printf("%d\n",s);
    }
    ```

* 编译`gcc -g main.c`

* 调试 `gdb ./a.out`

  * `start`
  * 执行中
  * 栈内存记录每一步操作状态
  * `p &a`  查看存储地址   
  * 其中`pa` 表示 a的地址 即是`&a
  * `int (*pquadrate)(int a)=&quadrate;`
    * ()表示优先匹配指针
    * int 表示返回值类型
    * int a表示参数
    * 整体 pquadrate表示指向这样的函数的 指针

* `变量的本质 是内存`

  * 变量名是代号  

* `指针的本质 内存地址`

* `&a`              a变量 所在栈内存空间的位置   p &a  0xfffffffffdbf    

* 对于`int *pa = &a`

* `&pa`             pa变量所在内存空间位置  

* `pa`               存的是&a   及a变量所在内存空间的位置

* (int *) 表示变量类型

* (int **) 表示指针

* {int (int)} 表示函数

* 代码段

  * `p &rect`                     `0x4005a6`      
  * `p &quadrate`            `0x4005dd`      

* 数据段        全局变量   常量         

  * `p global`                  `0x60104c`

* 函数栈                   函数多次调用

  * `p &a`             `0x7fffffffddfc`                     4个字节
  * `p &b`             `0x7fffffffde00`                      4个字节
  * `p &s`             `0x7fffffffde04`                    4个字节                   int占4个
  * `p &pa`             `0x7fffffffde08`                    8个字节                指针占8个
  * `p &pb`             `0x7fffffffde10`                    8个字节
  * `p &pglobal`             `0x7fffffffde18`                    8个字节
  * `p &pquadrate`             `0x7fffffffde20`                    8个字节
  * 
  * 函数先进后出    内存先进的大,后进的小
  * 静态变量,全局变量,常量都在数据段

## 函数指针
* 修改main.c

  ```
  #include <stdio.h>
  int global=0;
  
  int rect(int a, int b)
  {
      static int count=0;
      count++;
      global++;
      int s=a*b;
      return s;
  }
  
  int quadrate(int a)
  {
      static int count =0;
      count++;
      global++;
      int s=rect(a,a);
          return s;
  }
  
  int main()
  {
      int a=3;
      int b=4;
      int *pa=&a;
      int *pb=&b;
      int *pglobal=&global;
      int (*pquadrate)(int a)=&quadrate;
      //int s=quadrate(a);
      //指针传一个参数
      int s = (*pquadrate)(a);
      printf("%d\n",s);
  }
  ```

* 编译`gcc -g main.c`
* 调试

## 字符串与数组

* 创建`main.c`

* ```
  #include <stdio.h>
  
  int main()
  {
      int a=3;
      int b=2;
      int array[2];
      array[0]=1;
      array[1]=10;
      array[2]=100;
      int *p=&a;
      int i;
      for(i=0;i<8;i++){
          printf("*p=%d\n",*p);
          p++;
      }
      printf("------------------------------\n");
      p=&a;
      for(i=0;i<8;i++){
          printf("p[%d]=%d\n",i,p[i]);
      }
  }
  ```

  

* 数组长度只能为常量 不能写为 int array[b];

* 编译运行

* ```
  *p=3
  *p=1
  *p=2
  *p=1163025464
  *p=32765
  *p=1
  *p=10
  *p=100
  ------------------------------
  p[0]=3
  p[1]=1
  p[2]=2
  p[3]=1163025452
  p[4]=32765
  p[5]=1
  p[6]=10
  p[7]=100
  ```

* 调试

  * 调试循环
  * `p &a`                   `(int *)   0x7fffffffde14`
  * `p p`                   `(int *)   0x7fffffffde14`
  * `p * 0x7fffffffde14`                   3                       表示a的值
  * `p * 0x7fffffffde18`                   0                      表示i的值
  * `p &i`                   `(int *)   0x7fffffffde18`     内存中想同类型的放一起,顺序不一致
  * `p * 0x7fffffffde1c`                   2                        表示b的值
  * 多个显示 `x/6d 0x7fffffffde14`     默认整形
    * 输出`0x7fffffffde14` 到 `de28`         
    * 3                      0                    2                1                     (a i b array[0])
    * 10                 100                                                          (array[1]   array[2])

* 修改

* ```
  #include <stdio.h>
  
  int main()
  {
      int a=3;
      int b=2;
      int array[2];
      array[0]=1;
      array[1]=10;
      array[2]=100;
      int *p=&a;
      p+=3;     //表示移动上次  并非地址值+3    与p[3]; 等效
      *p=101;   //修改array[0]      合并 p[3]=101; 省略p=&a;
      p=&a;     //重新归位
      int i;
      for(i=0;i<8;i++){
          printf("*p=%d\n",*p);
          p++;
      }
      printf("------------------------------\n");
      p=&a;
      for(i=0;i<8;i++){
          printf("p[%d]=%d\n",i,p[i]);
      }
  }
  ```

* p+=3;     //表示移动上次  并非地址值+3    与 p[3]等效

*  `p+=3; *p=101;p=&a; `合并 p[3]=101; 省略p=&a;  因为只是做了偏移

* 编译输出调试

* ```
  *p=3
  *p=1
  *p=2
  *p=-971300728
  *p=32764
  *p=1
  *p=10
  *p=100
  ------------------------------
  p[0]=3
  p[1]=1
  p[2]=2
  p[3]=-971300740
  p[4]=32764
  p[5]=1
  p[6]=10
  p[7]=100
  ```

* 改造

* ```
  #include <stdio.h>
  
  int main()
  {
      int a=3;
      int b=2;
      int array[2];
      //array+=2   报错   
      int *pa=array;//array本身是一个地址
      pa[0]=1;
      pa[1]=10;
      pa[2]=100;
      int *p=&a;
      p[4] =101;
      int i=0;
      for(i=0;i<8;i++){
          printf("*p=%d\n",*p);
          p++;
      }
      printf("------------------------------\n");
      p=&a;
      for(i=0;i<8;i++){
          printf("p[%d]=%d\n",i,p[i]);
      }
  }
  ```

*  //array+=2   报错      因为数组地址  所以array  为指针常量

## 字符数组

* 创建`main.c`

* ```
  #include <stdio.h>
  
  int main()
  {
      char str[]="hello"
      char *str2="world"
      char str3[10];
      printf("input the value \n");
      scanf("%s",str3);
      printf("str is %s\n",str);
      printf("str2 is %s\n",str2);
      printf("str3 is %s\n",str3);
  }
  ```

* 编译调试

  * p str
    * "hello"
  * p str2
    * 0x400784  "world"                  在代码段
  * scanf("%s",str3);    没有加&   `因为str3是数组`

* 改造

* ```
  #include <stdio.h>
  
  int main()
  {
      char str[]="hello"
      char *str2="world"
      char str3[10];
      printf("input the value \n");
      scanf("%s",str);//
      printf("str is %s\n",str);
      printf("str2 is %s\n",str2);
      printf("str3 is %s\n",str3);
  }
  ```

* 编译运行

* 改造

* ```
  #include <stdio.h>
  
  int main()
  {
      char str[]="hello"
      char *str2="world"
      char str3[10];
      printf("input the value \n");
      scanf("%s",str2);//
      printf("str is %s\n",str);
      printf("str2 is %s\n",str2);
      printf("str3 is %s\n",str3);
  }
  ```

* 编译运行 报错

  * 调试 `p &str`  

    * `(char (*) [6]) 0x7fffffffde00`   `可以看出字符串是以字符数组保存的`
    * `x/6cb  0x7fffffffde00 `          `C语言字符数组以 \000 结束`
      * c表示 char类型
      * b表示单字节
    * 104 'h' 101 'e' 108 'l' 108 'l' 111 'o'  0 '\000'

  * `p &str2`

    * `(char **) 0x7fffffffddf8`

  * 修改main.c

  * ```
    #include <stdio.h>
    
    int main()
    {
        char str[]="hello"
        char *str2="world"
        char str3[10];
        printf("input the value \n");
        scanf("%s",str3);//
        str[3]='\0';//
        printf("str is %s\n",str);
        printf("str2 is %s\n",str2);
        printf("str3 is %s\n",str3);
    }
    ```

  * 验证遇到'\0\结束

  * 修改main.c

  * ```
    #include <stdio.h>
    
    int main()
    {
        char str[]="hello"//能存6个字符  '\0'
        char *str2="world"
        char str3[10];
        printf("input the value \n");
        scanf("%s",str);//
        str[3]='\0';//
        printf("str is %s\n",str);
        printf("str2 is %s\n",str2);
        printf("str3 is %s\n",str3);
    }
    ```

  * 验证字符长度越界时  存在相应的之后位置

  * 修改main.c

  * ```
    #include <stdio.h>
    
    int main()
    {
        char str[]="hello"//能存6个字符  '\0'
        char *str2="world"
        char str3[10];
        printf("input the value \n");
        scanf("%s",str3);//
        str[3]='\0';//
        int i=0;
        for(i=0;i<8;i++)
        {
            printf("%c\n",str[i]);
        }
        printf("str is %s\n",str);
        printf("str2 is %s\n",str2);
        printf("str3 is %s\n",str3);
    }
    ```

  * 编译运行

    * 查看运行内存结果


