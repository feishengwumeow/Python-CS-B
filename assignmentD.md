# Assignment #D: 十全十美 

Updated 1254 GMT+8 Dec 17, 2024

2024 fall, Complied by <mark>邱泽霖 化学与分子工程学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02692: 假币问题

brute force, http://cs101.openjudge.cn/practice/02692

思路：

好早之前写的，当时乱起变量名，看了半天才看懂自己写了啥

大概就是用集合，让轻的一端与重的一端分别不断取交集，最后哪个集合有元素哪个集合里就是假币。

代码：

```python
vocaloid=['A','B','C','D','E','F','G','H','I','J','K','L']

tn=int(input())
for i in range(0,tn):
    kagamine_ren = []
    kagamine_rin = []
    hatsune_miku = []
    t = []
    testout = False
    for luo_tianyi in range(3):
        trial=input().split()
        for c in range(4):
            t.append(trial[0][c])
            t.append(trial[1][c])
        if trial[-1]=="even":
            for j in range(4):
                hatsune_miku.append(trial[0][j])
                hatsune_miku.append(trial[1][j])
        if trial[-1]=="up":
            if len(kagamine_ren) == 0:
                for j in range(4):
                    kagamine_ren.append(trial[0][j])
                    kagamine_rin.append(trial[1][j])
            else:
                kagamine_ren=set(kagamine_ren)&(set([trial[0][0],trial[0][1],trial[0][2],trial[0][3]]))
                kagamine_rin=set(kagamine_rin)&(set([trial[1][0],trial[1][1],trial[1][2],trial[1][3]]))
            testout=True
        if trial[-1]=="down":
            if len(kagamine_ren) == 0:
                for j in range(4):
                    kagamine_rin.append(trial[0][j])
                    kagamine_ren.append(trial[1][j])

            else:
                kagamine_rin = set(kagamine_rin) & (set([trial[0][0], trial[0][1], trial[0][2], trial[0][3]]))
                kagamine_ren = set(kagamine_ren) & (set([trial[1][0], trial[1][1], trial[1][2], trial[1][3]]))
            testout=True
    if testout:
        vocaloid=set(t)
        fakeh=set(kagamine_ren).difference(set(hatsune_miku))
        fakel=set(kagamine_rin).difference(set(hatsune_miku))
        heavy=False
        if len(fakeh)>0:
            heavy=True
        if heavy:
            print(list(fakeh)[0]+' is the counterfeit coin and it is heavy.')
        else:
            print(list(fakel)[0]+' is the counterfeit coin and it is light.')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241218043735708](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241218043735708.png)



### 01088: 滑雪

dp, dfs similar, http://cs101.openjudge.cn/practice/01088

思路：

个人使用bfs与dp结合，如果下一步的点低于现在的点，则判断：如果已经算过下一步出发的最长坡，则不再搜索，直接使用该值与当前长度相加，若还没算过，则bfs。用时1h。

代码：

```python
import queue
import functools
@functools.lru_cache(maxsize=None)
def main():
    R,C=map(int,input().split())
    mount=[]
    #jk=0
    dx=[1,0,-1,0]
    dy=[0,1,0,-1]
    for i in range(R):
        mountp=list(map(int,input().split()))
        mount.append([])
        for j in range(C):
            mount[-1].append([mountp[j],1,1])
    q=queue.Queue()
    #print(mount)
    longestWay=set()
    for i in range(R):
        for j in range(C):
            #print(i,j)
            if mount[i][j][1]==1:
                q.put([i,j])
                while not q.empty():
                    g=q.get()
                    a=g[0]
                    b=g[1]
                    lw=1
                    for k in range(4):
                        ap=a+dx[k]
                        bp=b+dy[k]
                        if 0<=ap<R and 0<=bp<C:
                            if mount[ap][bp][0]<mount[a][b][0]:
                                if mount[ap][bp][2]!=1:
                                    mount[i][j][2]=max(mount[ap][bp][2]+mount[a][b][1],mount[i][j][2])
                                elif mount[ap][bp][1]<mount[a][b][1]+1:
                                    mount[ap][bp][1]=mount[a][b][1]+1
                                    mount[i][j][2]=max(mount[i][j][2],mount[ap][bp][1])
                                    #jk+=1
                                    q.put([ap,bp])
                                longestWay.add(mount[i][j][2])
    try:
        print(max(longestWay))
    except:
        print(1)
    #print(jk)
main()
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241218043931177](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241218043931177.png)



### 25572: 螃蟹采蘑菇

bfs, dfs, http://cs101.openjudge.cn/practice/25572/

思路：

将dfs的判定从一个点拓展为两个点，不难写，用时20min。

代码：

```python
import queue
dx=[1,0,-1,0]
dy=[0,1,0,-1]
n=int(input())
beach=[]
for i in range(n):
    beachp=list(map(int,input().split()))
    beach.append([])
    for j in range(n):
        beach[-1].append([beachp[j],True])
q=queue.Queue()
found=False
for i in range(n):
    for j in range(n):
        if beach[i][j][0]==5:
            q.put([i,j])
            try:
                if beach[i][j+1][0]==5:
                    crab=[0,1]
                else:
                    crab=[1,0]
            except:
                crab=[1,0]
            found=True
            break
    if found:
        break
reach=False
while not q.empty():
    t=q.get()
    x=t[0]
    y=t[1]
    beach[x][y][1]=False
    for k in range(4):
        xp=x+dx[k]
        yp=y+dy[k]
        xpc=xp+crab[0]
        ypc=yp+crab[1]
        if 0<=xp<n and 0<=yp<n and 0<=xpc<n and 0<=ypc<n and beach[xp][yp][1]:
            if beach[xp][yp][0]!=1 and beach[xpc][ypc][0]!=1:
                #print(xp,yp)
                #print(beach)
                if beach[xpc][ypc][0]==9 or beach[xp][yp][0]==9:
                    reach=True
                    break
                q.put([xp,yp])
    if reach:
        break
if reach:
    print('yes')
else:
    print('no')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241218044030446](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241218044030446.png)



### 27373: 最大整数

dp, http://cs101.openjudge.cn/practice/27373/

思路：

使用冒泡排序，即可找出最好的排列方式，好像讲过，但还是debug了一段时间，用时40min

代码：

```python
m=int(input())
n=int(input())
u=input().split()
nl=list(map(int,u))
k=len(str(max(nl)))
for i in range(n):
    for j in range(i):
        kmin=min(len(u[i]),len(u[j]))
        if u[i][:kmin]==u[j][:kmin]:
            if u[i]+u[j]>u[j]+u[i]:
                u[i],u[j]=u[j],u[i]
        else:
            if u[i]>u[j]:
                u[i],u[j]=u[j],u[i]
al=[0]*(m+1)
haveNumber=set()
haveNumber.add(0)
for i in range(n):
    haveNumberp=[]
    alp=[0]*(m+1)
    for j in haveNumber:
        if j+len(u[i])<=m:
            #print(j+len(u[i]))
            alp[j+len(u[i])]=max(int(str(al[j])+u[i]),al[j+len(u[i])])
            haveNumberp.append(j+len(u[i]))
    #print(haveNumberp)
    #print(al,haveNumber)
    for k in haveNumberp:
        haveNumber.add(k)
    #print(haveNumber)
    for k in range(m+1):
        al[k]=max(al[k],alp[k])
print(max(al))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241218044159607](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241218044159607.png)



### 02811: 熄灯问题

brute force, http://cs101.openjudge.cn/practice/02811

思路：

第一行确定后所有行随之确定，思路挺难想的，查找了思路后一遍就AC了，用时1.5h

代码：

```python
import copy
lights=[0]*5
dx=[0,0,1,0,-1]
dy=[0,1,0,-1,0]
def turn(x,y,lights):
    for i in range(5):
        a=x+dx[i]
        b=y+dy[i]
        if 0<=a<5 and 0<=b<6:
            lights[a][b]=(lights[a][b]+1)%2
    return lights
for i in range(5):
    lights[i]=list(map(int,input().split()))
for c in range(64):
    turns=[0]*30
    lightsp=copy.deepcopy(lights)
    for i in range(6):
        if c // (2**(i)) %2==1:
            lightsp=turn(0,i,lightsp)
            turns[i]+=1
    for i in range(4):
        for j in range(6):
            if lightsp[i][j]==1:
                lightsp=turn(i+1,j,lightsp)
                #print(lightsp)
                turns[6*i+6+j]+=1
    if 1 not in lightsp[-1]:
        for i in range(5):
            print(' '.join(map(str,turns[6*i:6*i+6])))
        break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241218044341769](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241218044341769.png)



### 08210: 河中跳房子

binary search, greedy, http://cs101.openjudge.cn/practice/08210/

思路：

没注意到binary search的标签，死也想不到是二分查找，看了题解后才有思路。用时1.5h

代码：

```python
L,N,M=map(int,input().split())
stones=[]
for i in range(N):
    stones.append(int(input()))
dist=[stones[0]]
for i in range(1,N):
    dist.append(stones[i]-stones[i-1])
dist.append(L-stones[-1])
distre=dist[::-1]
ltarget=L/2
lth=L
ltl=0
t=0
ans=0
f=False
if M==N:
    f=True
    ans=L
def Lt(ltarget):
    remove=0
    i=0
    after=dist[i]
    i+=1
    while i<=N:
        if after<ltarget:
            after+=dist[i]
            remove+=1
            i+=1
        else:
            after=dist[i]
            i+=1
        if remove>M or (remove==M and after<ltarget):
            #print(ltarget,False)
            return False
    #print(ltarget,True)
    return True
ltarget=L//2
lmax=L
lmin=0
while True:
    if M==N:
        ans=L
        break
    if Lt(ltarget):
        if lmax>ltarget+1:
            lmin=ltarget
            ltarget=(ltarget+lmax)//2
        else:
            ans=ltarget
            break
    else:
        lmax=ltarget
        ltarget=(ltarget+lmin)//2
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241218044531150](C:\Users\Feishengwu\AppData\Roaming\Typora\typora-user-images\image-20241218044531150.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>



经过一段时间的训练，代码实现都基本没有问题，思路正确的题目大多都能一遍AC，就怕一些思路非常难想的题目，比如这次作业的后两题，思路并不特别明显，非常依赖于标签的提示，考试的时候会有很大的隐患。这次作业明显暴露出我二分查找的写法不太熟悉，明后天针对性地加以练习。

