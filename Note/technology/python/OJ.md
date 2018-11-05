# OJ

###20171220_07_integerReverse

这是开始暴露出各种编程问题

~~~python
class Solution(object):
    def reverse(self,x):
        '''
        :type x: int
        :return int
        '''
        result = []
        num =1;
        flag = 1
        reversenum = 0;
        if x >0x7ffffffff:
            return 0
        if x < 0 :
            x *= -1
            flag = -1
        elif x ==0:
            result.append(0)
        while x>0:
            c = x%10
            #result.append(c)
            result.insert(0,c)
            x = (x-c)/10
            num = num+1
        for i in range(len(result)):
            reversenum = reversenum + result[i]*(10**i)
            #reversenum = reversenum + result[i]*(10^i)
        #if reversenum > 2147483647:
        if reversenum > 0x7ffffffff:
            reversenum = 0
        return reversenum*flag
~~~

1. 代码风格问题，看人家的代码每个函数之前就会写好输入参数，输出参数，以及类型
2. 基本的python 语法，if ，for ，这都能忘
3. 学习到的当你想用for 从大到小迭代，例如打印一个字符串的逆序，例如对list a 这时有两种
   - 用reversed 函数，`list(reversed(range(len(a))))` 这里reversed 函数是将序列数据翻转放回一个object ，所以要注意强转一下
   - 用range 的第三个参数，range(len(a)-1,-1,-1),注意到这里的第三个参数为-1 ，第一参数注意这里就要-1，因为打印是从头递增,直到第二个参数，也就是[a,b)
4. 题目提到输入要判断是不是`32-bit signed integer`,32位整型数，一般integer 都是32位，也就是说用32位二进制数表示，可以看到0x7ffffffff，为什么用这个表示呢，我们需要1位表示正负，因此也是有31位表示具体数值，因此31位表示的16进制位0x7ffffffff
5. list 添加字符有两种方式，一种是insert 一种是append

> 总结一下，真的是要多些程序，无论再忙，希望能坚持

