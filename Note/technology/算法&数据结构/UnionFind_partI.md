# UnionFind（并查集）

### UnionFind

w问题描述：g给定

### Quick Find

> 重点放在判断 is connect 上，设计数据结构让判断两个顶点的连通性

####数据结构

使用一维数组表示所有集合运算，每个元素的值表示该元素属于的集合，初始值设为-1 表示不属于任一集合

- 优势

  在于判断两个点是否属于同一个集合，只用找到数组中的两个值比较大小即可，算法复杂度在O(1)

- 缺陷



### Quick Union

> 重点放在合并两个集合

也是使用一维数组存储信息，每个数组元素表示每个节点，只不过现在存储的是当前元素的父亲节点，例如给定连接情况  a b, 设setlist[b] = a 



### improvment weighting Quick Union

在之前quick union 的基础上优化find 路径过长问题，记录每个root 节点的权重(即每个root 构成的树的元素个数)，优先选择权重小的连接在权重大的root 之后,这样可以较为有效降低树的高度

##### 算法复杂度分析







~~~python

#--coding=utf-8--
import sys



unionset = [-1 for i in range(1000)]

#并查集 
def union(a1,a2,count):
	# a1 ==-1
	if unionset[a1] == -1:
		if unionset[a2]!=-1:
			unionset[a1]=unionset[a2]
		else:
			unionset[a1] = count
			unionset[a2] = count
	# a1 != -1 
	else:
		if unionset[a2]==-1:
			unionset[a2] = unionset[a1]
		else:
			## 两个定点属于两个集合，合并两个集合
			# 保留 a2 的值，将a2 对应set 中的所有元素的值转成a1
			tmp_a2 = unionset[a2]
			for i in range(len(unionset)):
				if unionset[i] == tm:
					unionset[i] = unionset[a1]

def quickfind(a1,a2):
	if unionset[a1]== unionset[a2] and unionset[a1]!=-1:
		return True
	return False

count = 0
while True:
	strinput = raw_input("please input connect row like : 1 0\n")
	tmp = strinput.strip('\n').split()
	if len(tmp)!= 2 :
		continue

	a1 = int(tmp[0])
	a2 = int(tmp[1])

	if quickfind(a1,a2)==False:
		union(a1,a2,count)
		count+=1
		print 'union: '+strinput
	else:
		print strinput +' is connected'



#--coding=utf-8--
import sys



unionset = [-1 for i in range(1000)]

#并查集 
def union(a1,a2,count):
	# a1 ==-1
	if unionset[a1] == -1:
		if unionset[a2]!=-1:
			unionset[a1]=unionset[a2]
		else:
			unionset[a1] = count
			unionset[a2] = count
	# a1 != -1 
	else:
		if unionset[a2]==-1:
			unionset[a2] = unionset[a1]
		else:
			## 两个定点属于两个集合，合并两个集合
			# 保留 a2 的值，将a2 对应set 中的所有元素的值转成a1
			tmp_a2 = unionset[a2]
			for i in range(len(unionset)):
				if unionset[i] == tm:
					unionset[i] = unionset[a1]
def quickfind(a1,a2):
	if unionset[a1]== unionset[a2] and unionset[a1]!=-1:
		return True
	return False

count = 0
while True:
	strinput = raw_input("please input connect row like : 1 0\n")
	tmp = strinput.strip('\n').split()
	if len(tmp)!= 2 :
		continue

	a1 = int(tmp[0])
	a2 = int(tmp[1])

	if quickfind(a1,a2)==False:
		union(a1,a2,count)
		count+=1
		print 'union: '+strinput
	else:
		print strinput +' is connected'

~~~

