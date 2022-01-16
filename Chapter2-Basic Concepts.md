# 第二章课后习题

## 问题

1. 在 C 语言中，注释不允许嵌套。在下面的代码段中，用注释来“注释掉”一段语句会导致什么结果？
答：外面的注释在第一个封闭的注释的结尾处结束。这使得变量 i 在函数的其余部分未被定义；短语 End of commented-out code 将是一个 syntax 错误，而最后的结束语*/将是非法的。

2. 把一个大型程序放入一个单一的源文件中有什么优点？有什么缺点？
答：
    优点：
    a.当你想修改一个函数时，很容易确定它在哪个文件中
    b.你可以安全地使用较长的函数名称。根据你的特定系统的限制，内部名称必须在前31个字符或更多的地方相互区别，而外部名称必须在前六个字符中相互区别。
    缺点：
    a.根据你的编辑器的强大程度，在一个大文件中找到某段代码可能比小文件更难。
    b.根据你的操作系统、你所使用的编辑器类型以及文件的大小，编辑一个大文件可能比编辑一个小文件更耗时。
    c.如果你在编辑器中犯了一个错误，很容易就会失去整个程序。
    d.即使你只改变了一个函数，整个程序也必须重新编译，这比只重新编译被修改的函数花费的时间要长。
    e.如果程序中的通用函数被埋在所有针对该问题的代码中，就很难再利用。

3. 你需要把 printf 函数打印出下面这段文本（包括两边的双引号）。你应该使用什么样的字符串常量参数？“Blunder??!??”
答："\"Blunder\?\?!??\""

4. \40 的值是多少？\100 \x40 \x100 \0123 \x0123 的值又分别是多少？
答：

    ```c
    #include<stdio.h>
    int main()
    {
        char c1 = '\40';
        char c2 = '\100';
        char c3 = '\x40';
        char c4 = '\0123';
        char c5 = '\x0123';
        printf( "%c%c%c%c%c", c1, c2, c3, c4, c5 );

        return 0;
    }
    输出： （空格）@@3#
    ```

5. 下面这条语句的结果是什么？

    ```c
    int x/*blah blah*/y;
    ```

    答：预处理器将注释替换为一个空格，这使得产生的语句是非法的。

6. 下面的声明存在什么错误（如果有的话）？

    ```c
    int case, If, While, Stop, stop;
    ```

    答：没有什么。与C关键字或最后两个标识符之间没有冲突，因为它们的字符情况都是不同的

7. 是非题：因为 C （除了预处理指令之外）是一种自由形式的语言，唯一规定程序应如何编写的规则是语法规则，所以程序实际看上去的样子无关紧要。
答：略

8. 下面程序中的循环是否正确？

    ```c
    int main(void)
    {
        int x, y;

        x = 0;
        while (x < 10)
        {
            y = x * x;
            printf("%d\t%d\n", x, y);
            x += 1;
        }
    }

    #include <stdio.h>

    int main(void)
    {
        int x, y;
        x = 0;
        while(x < 10)
        {
            y = x * x;
            printf("%d\t%d");
            x += 1;
        }
    }
    哪个程序更便于检查其正确性？
    ```

    答：略

9. 假定你有一个 C 程序，它的 main 函数位于文件 main.c，它还有一些函数位于文件 list.c 和 report.c。在编译和链接这个程序时，你应该使用说明命令？
答：答案会因系统而异，但在UNIX系统上 ``cc main.c list.c report.c``

10. 接上题，如果你想使程序链接到 parse 函数库，你应该对命令作何修改？
答：答案将因系统而异，但在UNIX系统上，你应该添加``–lparse``

11. 假定你有一个 C 程序，它由几个单独的文件组成，而这几个文件又分别包含其他文件。如果你对 list.c 作了修改，你应该是用什么命令进行重新编译？如果是 list.h 或者 table.h 作了修改，又分别使用什么命令？
答：略

## 编程练习

1. 编写一个程序，它由 3 个函数组成，每个函数分别保存在一个单独的源文件中。函数 increment 接受一个整形参数，它的返回值是该函数的值加 1。increment 函数应该位于文件 increment.c 中。第 2 个函数称为 negate，它也接受一个整形参数，它的返回值是该参数的负值。最后一个函数是 main，保存于文件 mian.c 中，它分别用参数 10，0和 - 10 调用另外两个函数，并打印结果。
答：

    ```c
    #include <stdio.h>

    int increment(int value);
    int negate(int value);

    int increment(int value)
    {
        return value + 1;
    }

    int negate(int value)
    {
        return -value;
    }

    int main(void)
    {
        printf("%d %d\n", increment(10), negate(10));
        printf("%d %d\n", increment(0), negate(0));
        printf("%d %d\n", increment(-10), negate(-10));

        return 0;
    }
    ```

2. 编写一个程序，它从标准输入读取 C 源代码，并验证所有的花括号都正确地成对出现。注意：你不必担心注释内部、字符串常量内部和字符常量形式的花括号。
答：

    ```c
    #include <stdio.h>
    
    int main(void)
    {
        int count = 0;
        char c;

        while ((c = getchar()) != EOF)
        {
            if (c == '{')
            {
                count++;
            }
            if (c == '}')
            {
                count--;
            }

            if (count < 0)
            {
                break;
            }
        }

        if (count < 0 || count > 0)
        {
            printf("error\n");
        }
        else
        {
            printf("pass\n");
        }

        return 0;
    }
    ```
