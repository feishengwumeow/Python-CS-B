# Assignment #B: Dec Mock Exam大雪前一天

Updated 1649 GMT+8 Dec 5, 2024

2024 fall, Complied by <mark>邱泽霖 化学与分子工程学院</mark>



**说明：**

1）⽉考： AC1<mark>（请改为同学的通过数）</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E22548: 机智的股民老张

http://cs101.openjudge.cn/practice/22548/

思路：

用时10min，股民尽量在高点买入股票，并在低点卖出。

代码：

```python
k=list(map(int,input().split()))
s=k[0]
m=k[0]
ans=0
c=False
for i in range(1,len(k)):
    if c:
        m=s
        c=False
    elif k[i]>m:
        m=k[i]
    if m-s>ans:
        ans=m-s
    if k[i]<s:
        s=k[i]
        c=True
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210155724273](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241210155724273.png)



### M28701: 炸鸡排

greedy, http://cs101.openjudge.cn/practice/28701/

思路：

先炸最长时间的，再安排时间段的鸡块，尽量让所有鸡块剩余的炸时间相近

代码：

```python
def aver(a:list,b:int):
    for i in range(0,len(a)-1):
        if a[i]>a[i+1]:
            if (a[i]-a[i+1])*(i+1)<=b and b>0:
                b-=(a[i]-a[i+1])*(i+1)
                for j in range(i+1):
                    a[j]=a[i+1]
                return aver(a,b)
            else:
                for j in range(i+1):
                    a[j]-=b/(i+1)
                return a
    for i in range(len(a)):
        a[i]-=b/len(a)
    return a
def boil(chick:list,m:int,chicksum:int):
    if max(chick)==min(chick):
        return chick[0]*len(chick)/m
    if len(chick)>=2 and m>=2:
        chicksum-=chick[0]
        #print(chick)
        if chicksum>(m-1)*chick[0]:
            #print("A",end='')
            #print(chicksum,end='')
            return chick[0]+boil(aver(chick[1:],chick[0]*(m-1)),m,chicksum-(m-1)*chick[0])
        else:
            #print("B",end='')
            return boil(chick[1:],m-1,chicksum)
    elif m==1:
        #print("C",end='')
        return chicksum
    else:
        #print("D",end='')
        return chick[0]
n,k=map(int,input().split())
chickist=list(map(int,input().split()))
chickist.sort(reverse=True)
chicksum=sum(chickist)
print('%.3f'%boil(chickist,k,chicksum))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241210155851456](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241210155851456.png)



### M20744: 土豪购物

dp, http://cs101.openjudge.cn/practice/20744/

思路：

难题，考试花了一个多小时到最后都没写出来，考完试又花了一个小时还是没写出来，最后看了题解的双dp列表，感觉非常巧妙，但太考验思维，对思维能力比较差的学生不太友好。

代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### T25561: 2022决战双十一

brute force, dfs, http://cs101.openjudge.cn/practice/25561/

思路：

暴力枚举即可，难度不高，写的比较累，用时30min

代码：

```python
def main():
    n,m=map(int,input().split())
    prgd=[]
    sast=[]
    sest=[]
    semtp=[0]*(n+2)
    tries=1
    ansl=[]
    for i in range(n):
        prgd.append(input().split())
        sest.append(len(prgd[-1]))
        tries*=sest[-1]
        for j in range(len(prgd[-1])):
            prgd[-1][j]=list(map(int,prgd[-1][j].split(':')))
    #print(prgd)
    #print(tries)
    #print(sest)
    for i in range(m):
        sast.append(input().split())
        for j in range(len(sast[-1])):
            sast[-1][j]=list(map(int,sast[-1][j].split('-')))
    for i in range(m):
        sast[i].sort(key=lambda x:x[1],reverse=True)
        #print(sast)
    try:
        semtp[0]=sest[0]
        for i in range(1,n):
            semtp[i]=sest[i]*semtp[i-1]
            #print(sest)
    except:
        return 0
    for Try in range(tries):
        stex=[0]*(m+2)
        stex[prgd[0][Try%sest[0]][0]]+=prgd[0][Try%sest[0]][1]
        for i in range(1,n):
            stex[prgd[i][Try//semtp[i-1]%sest[i]][0]]+=prgd[i][Try//semtp[i-1]%sest[i]][1]
        #print(stex)
        sump=sum(stex)
        for i in range(1,m+1):
            for j in range(len(sast[i-1])):
                if sast[i-1][j][0]<=stex[i]:
                    stex[i]-=sast[i-1][j][1]
                    break
        #print(stex)
        ansl.append(sum(stex)-sump//300*50)
        #print(ansl)
    print(min(ansl))
main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210160044177](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241210160044177.png)



### T20741: 两座孤岛最短距离

dfs, bfs, http://cs101.openjudge.cn/practice/20741/

思路：

基本的bfs，难度不高，用时30min

代码：

```python
import queue
import copy
dir=[[1,0],[0,1],[-1,0],[0,-1]]
n=int(input())
chizu=[0]*n
for i in range(n):
    chizu[i]=[]
    inp=input()
    l=len(inp)
    for j in range(l):
        chizu[i].append([int(inp[j]),False,0])
q=queue.Queue()
k=0
for i in range(n):
    for j in range(l):
        if chizu[i][j][0]==1:
            chizu[i][j][1]=True
            k=1
            break
    if k==1:
        break
q.put([i,j])
while not q.empty():
    loc=q.get()
    for d in range(4):
        if 0<=loc[0]+dir[d][0]<n and 0<=loc[1]+dir[d][1]<l:
            if chizu[loc[0]+dir[d][0]][loc[1]+dir[d][1]][0]==1 and chizu[loc[0]+dir[d][0]][loc[1]+dir[d][1]][1]==False:
                chizu[loc[0]+dir[d][0]][loc[1]+dir[d][1]][1]=True
                q.put([loc[0]+dir[d][0],loc[1]+dir[d][1]])
chizuready=copy.deepcopy(chizu)
ansl=[]
q1=queue.Queue()
q1.put([i,j])
for i in range(n):
    for j in range(l):
        if chizu[i][j]==[1,True,0]:
            q1.put([i,j])
            #print(chizu)
while not q1.empty():
    loc1=q1.get()
    for d in range(4):
        if 0<=loc1[0]+dir[d][0]<n and 0<=loc1[1]+dir[d][1]<l:
            if chizu[loc1[0]+dir[d][0]][loc1[1]+dir[d][1]]==[0,False,0]:
                chizu[loc1[0]+dir[d][0]][loc1[1]+dir[d][1]][1]=True
                chizu[loc1[0]+dir[d][0]][loc1[1]+dir[d][1]][2]=chizu[loc1[0]][loc1[1]][2]+1
                q1.put([loc1[0]+dir[d][0],loc1[1]+dir[d][1]])
                #print(loc1[0]+dir[d][0],loc1[1]+dir[d][1])
                #print(chizu)
            if chizu[loc1[0]+dir[d][0]][loc1[1]+dir[d][1]][:2]==[0,True]:
                if chizu[loc1[0]+dir[d][0]][loc1[1]+dir[d][1]][2]>chizu[loc1[0]][loc1[1]][2]+1:
                    chizu[loc1[0]+dir[d][0]][loc1[1]+dir[d][1]][2]=chizu[loc1[0]][loc1[1]][2]+1
                    q1.put([loc1[0]+dir[d][0],loc1[1]+dir[d][1]])
            if chizu[loc1[0]+dir[d][0]][loc1[1]+dir[d][1]]==[1,False,0]:
                ansl.append(chizu[loc1[0]][loc1[1]][2])
                break
print(min(ansl))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210160100133](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241210160100133.png)



### T28776: 国王游戏

greedy, http://cs101.openjudge.cn/practice/28776

思路：

用时10min，让左右手数字乘积小的人排前面，好像难以严格证明，但确实AC了。

代码：

```python
n=int(input())
a,b=map(int,input().split())
daijin=[]
for i in range(n):
    daijin.append(list(map(int,input().split())))
daijin.sort(key=lambda x:x[0]*x[1])
ansl=[]
mul=a
for i in range(n):
    ansl.append(mul//daijin[i][1])
    mul*=daijin[i][0]
print(max(ansl))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241210160151819](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241210160151819.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

感觉greedy与dp的题目会显著地难于其他类型。对搜索已经熟悉了非常多。像这次考试，由于在两题M难度上花了太长时间，后面三题反而相对简单的T难度题没时间完成。期末考试的时候在完成E难度的题目后如果看不出greedy和dp的思路，可以优先考虑完成bfs,dfs等题目。这周在赶政治课的论文，练习量偏少。但是其他学科期末任务已经基本完成，下周就能花大量的时间在计概上，计划每天写2h。



