# Debug Tutorial

> 在codeblocks 和visual C++ 两个软件下 调试教程

###CodeBlocks

1. 新建一个命令行应用程序(Console application),之后选择C 语言

   ![3-3](/home/oliver/Documents/moocs_homework/photo/cdebug_1.png)

2. 点击菜单栏中的`project`-> `Bulid Options`,确认勾选了如下两条选项

   ![3-3](/home/oliver/Documents/moocs_homework/photo/cdebug_2.png)

3. 打开watch 窗口:点击 `Debug`-> `Debugging Windows`-> `watches`,watches 窗口是可以很方便的看到调试过程中变量的值

   ![3-3](/home/oliver/Documents/moocs_homework/photo/cdebug_3.png)

4. 在编辑代码区,也就是行号之后左键设置断点

5. 调试方式: 在菜单栏有多种调试方式,分别为next cursor,next line,step into ,step out,next instruction,step into instruction

   - next cursor:下一个断点
   - nextline : 下一行
   - step into: 进入子函数
   - step out : 跳出子函数


![3-3](/home/oliver/Documents/moocs_homework/photo/cdebug_4.png) 

###调试实践"杨辉三角"

正确答案与错误答案

![3-3](/home/oliver/Documents/moocs_homework/photo/cdebug_5.png)