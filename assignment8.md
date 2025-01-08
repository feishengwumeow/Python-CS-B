# Assignment #8: 田忌赛马来了

Updated 1021 GMT+8 Nov 12, 2024

2024 fall, Complied by <mark>邱泽霖 化学与分子工程学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 12558: 岛屿周⻓

matices, http://cs101.openjudge.cn/practice/12558/ 

思路：

对每一行检验，若有0/1交界处，则岛屿长度+1，在开头与结尾各放一行0，每行左右加一个0。每行只需与上一行对比，没必要用二维数组，以节省空间。感觉不难。

代码：

```python
def ccal(list1,list2,m,c):
    for i in range(m+1):
        if list1[i]!=list2[i]:
            c+=1
        if list2[i]!=list2[i+1]:
            c+=1
    return c,list2
n,m=map(int,input().split())
list0=[0]*(m+2)
c=0
for i in range(n):
    list1=[0]+list(map(int,input().split()))+[0]
    c,list0=ccal(list0,list1,m,c)
c,list0=ccal(list0,[0]*(m+2),m,c)
print(c)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241118182244590](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241118182244590.png)



### LeetCode54.螺旋矩阵

matrice, https://leetcode.cn/problems/spiral-matrix/

与OJ这个题目一样的 18106: 螺旋矩阵，http://cs101.openjudge.cn/practice/18106

思路：

先向右，再向下……已输出的数后标记为False，碰到边界或False时转向。

代码：

```python
class Solution(object):
    def spiralOrder(self, matrix):
        m=len(matrix)
        n=len(matrix[0])
        output=[]
        op=0
        for i in range(m):
            for j in range(n):
                matrix[i][j]=[matrix[i][j],True]
        i=0
        l=-1
        j=0
        k=0
        while op<m*n:
            for j in range(l+1,n):
                if matrix[i][j][1]:
                    output.append(matrix[i][j][0])
                    matrix[i][j][1]=False
                    op+=1
                else:
                    j-=1
                    break
            for k in range(i+1,m):
                if matrix[k][j][1]:
                    output.append(matrix[k][j][0])
                    matrix[k][j][1]=False
                    op+=1
                else:
                    k-=1
                    break
            for l in range(j-1,-1,-1):
                if matrix[k][l][1]:
                    output.append(matrix[k][l][0])
                    matrix[k][l][1]=False
                    op+=1
                else:
                    l+=1
                    break
            for i in range(k-1,-1,-1):
                if matrix[i][l][1]:
                    output.append(matrix[i][l][0])
                    matrix[i][l][1]=False
                    op+=1
                else:
                    i+=1
                    break
        return output    
        
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241118184512158](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241118184512158.png)



### 04133:垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/

思路：

对每个点看能清理多少垃圾

代码：

```python
d=int(input())
n=int(input())
trashs=[]
maxn=1
max=0
for i in range(n):
    trashs.append(list(map(int,input().split())))
for i in range(0,1025):
    for j in range(0,1025):
        sum=0
        for k in trashs:
            if abs(k[0]-i)<=d and abs(k[1]-j)<=d:
                sum+=k[2]
        if sum>max:
            max=sum
            maxn=1
        elif sum==max:
            maxn+=1
print(maxn,max)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241118182732984](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241118182732984.png)



### LeetCode376.摆动序列

greedy, dp, https://leetcode.cn/problems/wiggle-subsequence/

与OJ这个题目一样的，26976:摆动序列, http://cs101.openjudge.cn/routine/26976/

思路：

本来还想用dp，后来发现最长摆动序列必须从头到尾，只循环一次即可，复杂度为O(n)

代码：

```python
class Solution(object):
    def wiggleMaxLength(self, nums):
        last=nums[0]
        for i in range(1,len(nums)):
            if nums[i]!=last:
                last=nums[i]
                length=2
                lastdif=nums[i]-nums[0]
                break
        if last!=nums[0]:
            for j in range(i+1,len(nums)):
                if (nums[j]-last)*lastdif<0:
                    lastdif=nums[j]-last
                    last=nums[j]
                    length+=1
                elif (nums[j]-last)*lastdif>0:
                    lastdif=nums[j]-last
                    last=nums[j]
            return length
        else:
            return 1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241118182903982](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241118182903982.png)



### CF455A: Boredom

dp, 1500, https://codeforces.com/contest/455/problem/A

思路：

模板型dp题

代码：

```python
n=int(input())
nums=list(map(int,input().split()))
nlist=[0]*(max(nums)+1)
anslist=[0]*(max(nums)+1)
for i in range(n):
    nlist[nums[i]]+=1
anslist[0]=0
anslist[1]=nlist[1]
for i in range(2,max(nums)+1):
    anslist[i]=max(anslist[i-1],anslist[i-2]+nlist[i]*i)
print(anslist[-1])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241118183019653](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241118183019653.png)



### 02287: Tian Ji -- The Horse Racing

greedy, dfs http://cs101.openjudge.cn/practice/02287

思路：

个人认为，先将两组马进行排序，然后逐个交换使田忌赢尽可能多，即可得出正确答案。感觉思路可能有瑕疵，AC得不明不白的，在反复debug中就莫名其妙地过了。

代码：

```python
def sgn(x):
	if x>0:
		return 1
	if x==0:
		return 0
	if x<0:
		return -1
while True:
	n=int(input())
	if n==0:
		break
	t=list(map(int,input().split()))
	k=list(map(int,input().split()))
	t.sort()
	k.sort()
	while True:
		kk=k
		for miku in range(2):
			for i in range(n):
				for j in range(i):
					if sgn(t[i]-k[i])+sgn(t[j]-k[j])<=sgn(t[i]-k[j])+sgn(t[j]-k[i]):
						k[i],k[j]=k[j],k[i]
		if kk==k:
			break
	ans=0
	for i in range(n):
		ans+=sgn(t[i]-k[i])
	print(ans*200) 
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241118183836807](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241118183836807.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>



进一步熟悉了dp的写法，并尽量使用greedy解决问题，在简单题中有了简化自己代码的能力，可以达到比较快的速度。但较复杂的题思路不太清晰，田忌赛马题解的思路难以想到，导致写题用时极长。下次应该在精神清醒的时候写计概，说不定思路会比较清晰。

