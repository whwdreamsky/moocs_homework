### python code learning

> 这个笔记是对他人python 代码的学习,更多是python 的一些高级用法,为了写代码的更加专业 : )

1. python 脚本命令行输入参数规范,让用户更加方便使用

   ~~~python
   ###before informal
    if len(sys.argv)<3:
           print 'testfile,modle'
           exit(0)
       loadmodel(sys.argv[2])
       f = open(sys.argv[1])
   #now formal
   from argparse import ArgumentParser
   parser = ArgumentParser()
   parser.add_argument('testfile',type='str',help='test file path')
   parser.add_argument('modlefile',type='str',help='modle file path from training eg: word \t weight')
   args =ap.parse_args()
   loadmodel(args.modlefile)
   f = open(args.testfile,'r')

   ~~~


   ~~~python

   #输入参数的一种情况，用户选择train or infer ，group 是一组参数
   group = parser.add_mutually_exclusive_group()
       group.add_argument('--train', action='store_true', default=False,
                           dest='train', help='Input --train to Train')
       group.add_argument('--infer', action='store_true',default=False,
                           dest='infer', help='Input --infer to Infer')
       parser.add_argument('--train_data_path', type=str,
                           default='feature/X_train', dest='train_data_path',
                           help='Path to training data')
   ~~~

   ​

2. **==注释==** 起码在每个函数之前加一行注释

> fuck 为啥不给个例子呢，差评

2. 神器 **defaultdict** 使用

    defaultdict 来自*collections* 包,是一个强大的字典,可以 定义函数作为词典中的值的赋值过程

   - defaultdict + lamda

   详细~~见  https://docs.python.org/2/library/collections.html#collections.defaultdict

> 同志什么时候能总结一下呢，之后又遇到同样的问题无法解决

   ​