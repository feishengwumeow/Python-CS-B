# Assignment #C: 五味杂陈 

Updated 1148 GMT+8 Dec 10, 2024

2024 fall, Complied by <mark>邱泽霖 化学与分子工程学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 1115. 取石子游戏

dfs, https://www.acwing.com/problem/content/description/1117/

思路：

若a>2b,则先手有主动权，必胜，若a%b==0，先手直接拿完，胜，若2b>a>b,则先手只有一种选择，相当于拿一次后对方先手。用时8min

代码：

```python
def game(a,b):
    if b>a:
        a,b=b,a
    if a%b==0 or a//b>=2:
        return True
    else:
        return not game(a-b,b)
while True:
    a,b=map(int,input().split())
    if a==b==0:
        break
    if game(a,b):
        print("win")
    else:
        print("lose")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210162716662](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241210162716662.png)



### 25570: 洋葱

Matrices, http://cs101.openjudge.cn/practice/25570

思路：

根据行数与列数判断数字属于第几层，题目很简单，用时5min。（好喜欢这首歌）

代码：

```python
import math
n=int(input())
k=math.ceil(n/2)
al=[0]*k
for i in range(n):
    matrixirow=list(map(int,input().split()))
    for j in range(n):
        oc=min(i,n-i-1,j,n-j-1)
        al[oc]+=matrixirow[j]
print(max(al))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241210163352203](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241210163352203.png)



### 1526C1. Potions(Easy Version)

greedy, dp, data structures, brute force, *1500, https://codeforces.com/problemset/problem/1526/C1

思路：

都喝，要死了再把最毒的吐出来，用时30min

代码：

```python
#import functools
#@functools.lru_cache(maxsize=None)
def main():
    n=int(input())
    pl=list(map(int,input().split()))
    kl=[]
    ul=[]
    cl=0
    if pl[0]>=0:
        kl.append(pl[0])
        cl+=1
        ul.append(pl[0])
    else:
        kl.append(0)
    for i in range(1,n):
        if pl[i]+kl[-1]>=0:
            kl.append(pl[i]+kl[-1])
            ul.append(pl[i])
            cl+=1
        else:
            if len(ul)>0:
                minul=min(ul)
                if pl[i]>minul:
                    kl.append(kl[-1]+pl[i]-minul)
                    ul.remove(minul)
                    ul.append(pl[i])
        #print(kl)
        #print(ul)
    print(cl)
main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241217105533637](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241217105533637.png)



### 22067: 快速堆猪

辅助栈，http://cs101.openjudge.cn/practice/22067/

思路：

记录堆猪中每一只猪下的最轻的猪，直接输出即可。

代码：

```python
#pigs=[]
pigsmin=[20001]
while True:
    try:
        k=input()
        if k=='pop':
            if len(pigsmin)>1:
                pigsmin.pop()
        if k[:4]=='push':
            #pigs.append(int(k[5:]))
            pigsmin.append(min(pigsmin[-1],int(k[5:])))
        if k=='min':
            if len(pigsmin)>1:
                print(pigsmin[-1])
    except EOFError:
        break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210175716559](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241210175716559.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/

思路：

bfs,如果走到重复的格子，则比较其与之前到达该格子所用的最小体力，若小于该值，则刷新此格子的数据，好像可以不用Dijkstra(,用时20min

代码：

```python
import queue
import copy
m,n,p=map(int,input().split())
dx=[1,0,-1,0]
dy=[0,1,0,-1]
ditu=[]
for i in range(m):
    ditup=input().split()
    ditu.append([])
    for j in range(n):
        try:
            ditu[-1].append([int(ditup[j]),-1])
        except:
            ditu[-1].append(['#',-1])
for i in range(p):
    ditui=copy.deepcopy(ditu)
    q=queue.Queue()
    a,b,c,d=map(int,input().split())
    q.put([a,b])
    ansl=[]
    #print(ditui)
    ditui[a][b][1]=0
    if not (ditui[a][b][0]=='#' or ditui[c][d][0]=='#'):
        while not q.empty():
            t=q.get()
            x0,y0=t[0],t[1]
            for j in range(4):
                xp=x0+dx[j]
                yp=y0+dy[j]
                if 0<=xp<m and 0<=yp<n and ditui[xp][yp][0]!='#':
                    #print(xp,yp)
                    if ditui[xp][yp][1]==-1:
                        #print('A')
                        ditui[xp][yp][1]=ditui[x0][y0][1]+abs(ditui[xp][yp][0]-ditui[x0][y0][0])
                        q.put([xp,yp])
                    else:
                        #print('B')
                        if ditui[xp][yp][1]>ditui[x0][y0][1]+abs(ditui[xp][yp][0]-ditui[x0][y0][0]):
                            ditui[xp][yp][1]=ditui[x0][y0][1]+abs(ditui[xp][yp][0]-ditui[x0][y0][0])
                            q.put([xp,yp])
                    if xp==c and yp==d:
                        ansl.append(ditui[xp][yp][1])
    if len(ansl)==0:
        print('NO')
    else:
        print(min(ansl))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241217110040731](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241217110040731.png)



### 04129: 变换的迷宫

bfs, http://cs101.openjudge.cn/practice/04129/

思路：

一开始思路不好实现，花了4h，后面看了题解的思路，用了求余的方法判断，最后自己15min就写出来了

代码：

```python
import queue
T=int(input())
DX=[1,0,-1,0]
DY=[0,1,0,-1]
for i in range(T):
    R,C,K=map(int,input().split())
    ditu=[]
    q=queue.Queue()
    for i in range(R):
        ditup=input()
        ditu.append([])
        for j in range(C):
            if ditup[j]=='.':
                ditu[-1].append([0,0])
            elif ditup[j]=='S':
                ditu[-1].append([0,0])
                q.put([i,j])
            elif ditup[j]=='#':
                ditu[-1].append([1,0])
            elif ditup[j]=='E':
                ditu[-1].append([0,0])
                endpoint=[i,j]
    #print(ditu)
    tmin=[]
    while not q.empty():
        l=q.get()
        xn=l[0]
        yn=l[1]
        for d in range(4):
            xp=xn+DX[d]
            yp=yn+DY[d]
            t=ditu[xn][yn][1]+1
            if 0<=xp<R and 0<=yp<C:
                #print(xp,yp)
                if (ditu[xp][yp][0]==0 or (ditu[xp][yp][0]==1 and t%K==0)) and (-(t%K)-1 not in ditu[xp][yp]):
                    #print('A')
                    ditu[xp][yp][1]=t
                    ditu[xp][yp].append(-(t%K)-1)
                    q.put([xp,yp])
                    #print(ditu)
                if xp==endpoint[0] and yp==endpoint[1]:
                    tmin.append(t)
    if len(tmin)>0:
        print(min(tmin))
    else:
        print("Oop!")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241217110103497](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241217110103497.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

这周花在计概上的时间多了不少，但在变换的迷宫卡了太久，选做的题没有完全跟上，下周主要是把选做的题补上，在多做一些其他的题目，不会的题不能死磕，超过一定时间没写出来就该看题解了。可喜的是bfs的写法明显熟练了，下周把dp和greedy的题再多练练。



