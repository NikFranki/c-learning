# 第一章课后习题

## 问题

1. C 是一种自由形式的语言，也就是说并没有规则规定它的外观究竟应该怎么样。但是本章的例子程序遵循了一定的空白使用规则。对此你有何想法?
答：为了提高程序的可阅读性和可维护性。

2. 把声明（如函数原型的声明）放在头文件中，并在需要时用 #include 指令把它们包含与源文件中，这种做法有什么好处？
答：可以从头文件中直接获取有什么函数，方便直接使用。

3. 使用 #define 指令给字面量常量取名有什么好处？
答：如果命名正确，更容易看到命名常量的含义，而字面量只显示其值。

4. 依次打印一个十进制整数、字符串、浮点值，你应该在 printf 函数中分别使用说明格式代码？试编一例，让这些打印值以空格分隔，并在输出行的末尾添加一个换行符。
答："%d %s %g\n"。

5. 编写一条 scanf 语句，它需要读取两个整数，分别保存与 quantity 和 price 变量，然后在读取一个字符串，保存在一个名叫 department 的字符数组中。
答：

    ```c
    scanf("%d %d %s", &quantity, &price, department);
    ```

6. C 语言并不执行数组下标的有效性检查。你觉得为什么这个明显的安全手段会从语言中省略？
答：程序员可以在有需要的地方输入下标检查，在已经知道子脚本是正确的地方（例如：之前已经检查过），再次检查它不会产生额外的开销。但它们被省略的真正原因是，子脚本是作为指针表达式实现的。

7. 本章描述的 rearrange 程序包含下面的语句

    ```c
    strncpy(output + output_col, input + columns[col], nchars);
    ```

    strcpy 函数只接受两个参数，所以它实际上所复制的字符数由第2个参数指定。在本程序中，如果用 strcpy 函数取代 strncpy 函数会出现什么结果？
    答：复制的字符对多余实际需要的字符，但是 output_col 将被正确地更新，因此下一段字符被复制到输出数组的适当位置，替换前面操作中的任何额外字符。唯一的潜在问题是，无界限的 strcpy 可能会将更多的字符复制到输出函数中，超过它所能容纳的控件，从而破坏一些其他变量。

8. rearrange 程序包含下面的语句

```c
while (gets(input) != NULL) {
```

你认为这段代码会出现什么问题？
答：当一个数组作为函数参数被通过时，这个函数不知道数组的长度，因此，gets 无法预防一个非常长的输入行的溢出，而 fgets 这个函数，它的参数要求有数组的长度，因此不会出现这个情况。

## 编程练习

1. "Hello world" 程序常常是 C 编程新手所编写的第一个程序。它在标准输出中打印 Hello world!，并在后面加一个换行符。当你希望摸索出如何在自己的系统中运行 C 编译器时，这个小程序往往是一个很好的测试例。
答：

    ```c
    #include <stdio.h>

    int main(void)
    {
        printf("Hello world!");
        putchar('\n');
        return 0;
    }
    ```

2. 编写一个程序，从标准输入读取几行输入。每行输入都打印到标准输出上，前面要加上行号。在编写这个程序时要试图让程序能够处理的输入行的长度没有限制。
答：

    ```c
    #include <stdio.h>
    #include <stdlib.h>

    int main(void)
    {
        int num = 0;
        char c;

        while (1)
        {
            printf("line %d: ", ++num);

            while ((c = getchar()) != EOF)
            {
                if (c == '\n')
                {
                    printf("\n\n");
                    break;
                }
                else
                {
                    putchar(c);
                }
            }
        }

        return EXIT_SUCCESS;
    }
    ```

3. 编写一个程序，从标准输入读取一些字符，并把他们写到标准输出上。它同时应该计算 checksum 值，并写在字符的后面。
答：

    ```c
    #include <stdio.h>
    #include <stdlib.h>

    int main(void)
    {
        int c;
        char sum = -1;

        /**
        * Read the chapters one by one, and add them to the sum.
        * 
        */
        while ((c = getchar()) != EOF)
        {
            putchar(c);
            sum += c;
        }

        printf("%d\n", sum);
        return EXIT_SUCCESS;
    }
    ```

4. 编写一个程序，一行行地读取输入行，直至到达文件尾。算出每行输入行的长度，然后把最长的那行打印出来。为了简单起见，你可以假定所有的输入行均不超过 1000 个字符。
答：

    ```c
    /**
    * Reads lines of input from the standard input and prints the longest line that
    * was found to the standard output. It is assumed that no line will exceed
    * 1000 characters
    */

   #include <stdio.h>
   #include <stdlib.h>
   #include <string.h>

   #define MAX_LEN 1001 /* Buffer size for longest line */

   int main(void)
   {
       char input[MAX_LEN];
       int len;
       char longest[MAX_LEN];
       int longest_len;

       /**
        * Initiallize length of the longest line found so far.
        */
       longest_len = -1;

       /**
        * Reads input lines, one by one.
        */
       while (gets(input) != NULL)
       {
           /**
            * Get length of this line. if it is longer than the previous
            * longest line, save this line
            */
           len = strlen(input);
           if (len > longest_len)
           {
               longest_len = len;
               strcpy(longest, input);
           }
       }

       /**
        * if we saved any line at all from the input, print it now.
        */
       if (longest_len >= 0)
       {
           puts(longest);
       }

       return EXIT_SUCCESS;
   }
    ```

5. rearrange 程序中的下列语句

    ```c
    if (columns[col] >= len ...)
        break;
    ```

    当字符的列范围超出输入行的末尾就停止复制。这条语句只有当列范围以递增顺序出现才是正确的，但事实上并非如此。请修改这条数据，即使列范围不是按顺序读取也能正确完成任务。
答：略

6. 修改 rearrange 程序，去除输入列标号中的个数必须是偶数的限制。如果读取的列标号为奇数个，函数就会把最后一个列范围设置为最后一个列标号所指定的列到行尾之间的范围。从最后一个列标号直至行尾的所有字符都将被复制到字符串。
答: 略
