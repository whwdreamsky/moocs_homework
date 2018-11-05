# python 基础

1. 文件输入输出

   ```python
   line1 = 'qbs'
   line2 = 'fwef'
   nl = '\n' 
   line3 = line1,n1,line2,n2
   fout.write(line1)
   #writelines 是可以接受一个line 的list,而不是加'\n'哈
   fout.writelines(line3)
   ```

2. xrang 和rang

   ```python
   ## range 返回一个list 对象,
   for i in range(1,100)
   ## xrange 适合
   for i in xrange(1,100000000):
   ```

   ​

3. zip 函数

   ~~~python
   # zip 可以同时遍历多个list 返回一个list ,list 的值是遍历的list中index相同的
   >>> x = [1, 2, 3]
   >>> y = [4, 5, 6]
   >>> zipped = zip(x, y)
   >>> zipped
   [(1, 4), (2, 5), (3, 6)]
   >>> x2, y2 = zip(*zipped)
   >>> x == list(x2) and y == list(y2)
   True
   ~~~

4. **global** 变量 当你希望修改全局的==静态变量==的时候使用,当你使用可变的变量例如list,dict 是不需要使用global声明的

   ~~~python
   #全局初始化一个dict
   a = dict(a=1,b=2,c=3)
   alist = [1,23,12,43]
   num=globalnum=123
   def changedict():
       a['e']=5
       alist.append(2344)
       num = 23444
   changedict()
   print 'dict:',a
   print 'list:',alist
   print 'num:',num
   print 'globalnum:',globalnum
   '''
   dict: {'a': 1, 'c': 3, 'b': 2, 'e': 5}
   list: [1, 23, 12, 43, 2344]
   num: 123
   globalnum: 23444
   '''
   ~~~

5. String 

   ~~~python
   a = 'this is the dog which eat the bread'
   print a.replace('the','',1)
   print a 
   ~~~

   > 这里主要是replace 方法返回一个新串而a 本身不改变,这个应该怎么做呢,什么时候知道是对变量操作,什么时候是对本身的字符串操作呢

6. Python math

   ~~~python
   import math
   math.fabs(-1)
   pow(10,4)
   ~~~

   ​

7. Python中的变量，引用，拷贝和作用域问题

   与 c++ 不同，python 中每个变量在没有赋值时，没有类型，例如直接打印下是'undefine'，

   python 中的对象：

   在Python中，对象分为两种：可变对象和不可变对象，不可变对象包括int，float，long，str，tuple等，可变对象包括list，set，dict等

   > Python的所有变量其实都是指向内存中的对象的一个指针，都是值的引用，而其类型是跟着对象走的。总结来说：在Python中，**类型是属于对象的，而不是变量, 变量和对象是分离的，对象是内存中储存数据的实体，变量则是指向对象的指针**

   http://xianglong.me/article/python-variable-quote-copy-and-scope/

   作用域

   ~~~python
   def scope_test():
   	def do_local():
           spam = "local spam"
       def do_nonlocal():
           nonlocal spam
           spam = "nonlocal spam"
   	def do_global():
           global spam
           spam = "global spam"
   	spam = "test spam"
   	do_local()
   	print("After local assignment:", spam)
   	do_nonlocal()
   	print("After nonlocal assignment:", spam)
   	do_global()
   	print("After global assignment:", spam)
   scope_test()
   print("In global scope:", spam)def scope_test():
   ~~~

   > Python-doc

   output:

   ~~~
   After local assignment: test spam
   After nonlocal assignment: nonlocal spam
   After global assignment: nonlocal spams.
   In global scope: global spam
   ~~~

8. python list 

   值拷贝问题，因为python 中了都是用引用，所以往往在赋值给一个临时变量时就会同时改变两个名称的值，

   这时候就要用到list 的拷贝

   ~~~python
   #1
   newlist = list(oldlist)
   #2
   import copy
   newlist = copy.copy(oldlist)
   #3
   newlist = oldlist[:]
   #4 同时拷贝 list中的嵌套元素
   newlist = copy.deepcopy(oldlist)
   ~~~

   list 判断相等

   ~~~python
   b = a[:]
   a is b
   a ==b
   ~~~

   创建制定 内容的list

   ~~~python
   listset = [-1 for i in range(100)]
   ~~~

   ​

9. https://stackoverflow.com/questions/2612802/how-to-clone-or-copy-a-list

10. 输出运行时间

  ~~~python
  import time
  start_time = time.time()
  main()
  print("--- %s seconds ---" % (time.time() - start_time))
  ~~~

11. dict

    ~~~python
    # 从dict 中弹出一个键
    my_dict.pop('key', None)
    ~~~

    ​


###学习资源

python 编程规范:https://github.com/qiwsir/StarterLearningPython/blob/master/n001.md

python cookbook https://github.com/dabeaz/python-cookbook



