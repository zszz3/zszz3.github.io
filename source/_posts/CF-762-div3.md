---
title: CF-762-div3
date: 2022-01-13 22:56:57
tags: Codeforces
---



### A. Spy Detected!

**题意**：给你n个数，找到其中与其他n-1个数不同的那个数的位置，（保证其他n-1个数位置相同）

**题解**：可以先排序，不同的那个数就在头或尾，找到后输出这个数的位置。

 <!-- more -->

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=1e7+100;
const int mod=1e9+7;
const ll INF=1e18;
int n,m,k,t;
pair<int,int>pi[SIZE];
void init(){}
void solve(){
    cin>>n;
    for(int i=1;i<=n;i++){
        int x;
        cin>>pi[i].first;
        pi[i].second=i;
    }
    sort(pi+1,pi+1+n);
    if(pi[1].first!=pi[2].first){
        cout<<pi[1].second<<endl;
    }
    else cout<<pi[n].second<<endl;
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    init();
    while(t--)
        solve();
    return 0;
}
```

### B. Almost Rectangle

**题意：**给定一个n*n正方体方格，在这个方格里有两个点是星星，其余都是点，再将两个点变换成星星，使四个星星构成一个方形

**题解**：判断在不在同行或者同列即可，是的话就特殊处理，不是的话将（ax，by）和（bx，ay）都变成星星即可

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=2e5+100;
const int mod=1e9+7;
const int INF=0x3f3f3f3f;
int n,m,k,t;
string s[500];
pair<int,int>p[2];
void solve(){
    cin>>n;
    int flag=0;
    for(int i=0;i<n;i++){
        cin>>s[i];
        for(int j=0;j<n;j++){
            if(s[i][j]=='*'){
                p[flag].first=i;
                p[flag++].second=j;
            }
        }
    }
    if(p[0].first==p[1].first){
        if(p[0].first==0){
            s[p[0].first+1][p[0].second]='*';
            s[p[1].first+1][p[1].second]='*';
        }
        else {
            s[p[0].first-1][p[0].second]='*';
            s[p[1].first-1][p[1].second]='*';
        }
    }
    else if(p[0].second==p[1].second){
            if(p[0].second==0){
            s[p[0].first][p[0].second+1]='*';
            s[p[1].first][p[1].second+1]='*';
        }
        else {
            s[p[0].first][p[0].second-1]='*';
            s[p[1].first][p[1].second-1]='*';
        }
    }
    else {
        s[p[0].first][p[1].second]='*';
        s[p[1].first][p[0].second]='*';
    }
    for(int i=0;i<n;i++){
        cout<<s[i]<<endl;
    }
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    while(t--)
        solve();
    return 0;
}
```

### C. A-B Palindrome

**题意**：给定一个由0，1，？组的字符串，？可以由0或1代替，构造出一个有a个1和b个0的回文串，不能则输出-1，否则输出这个回文串

**题解**：这题半年前写的记不起来了，贴个类似题解

首先判断这个字符串中有没有’?’，如果没有，就是最简单的了，只要判断这个字符串是否满足上面两点，满足则输出原字符串，不满足则输出-1.
如果这个字符串中有’?’，则得进一步考虑，把对称的两个字符一致，比如01??0，1对应?，说明对应的也应该是1.全部更新以下，然后再判断已有的a,b有没有超，超就输出-1结束判断。更新期间也查询对应两个字符如果都不是’?’，但不一致时输出-1结束判断。更新结束后，根据还差的ans0,ans1的数量往上补，这时候的字符串肯定是这种的：0??0,01?10,01010类似这样的。补齐完后再次对字符串判断上面两个原则，最终可以判断出来，并输出答案。

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=2e5+100;
const int mod=1e9+7;
const int INF=0x3f3f3f3f;
int n,m,k,t;
int a[SIZE];
void solve(){
    cin>>n;
    for(int i=1;i<=n+2;i++){
        cin>>a[i];
    }
    sort(a+1,a+3+n);
    ll res=0;
    for(int i=1;i<=n;i++){
        res+=a[i];
    }
    if(res==a[n+1]){
        for(int i=1;i<=n;i++){
            cout<<a[i]<<" ";
        }
        cout<<endl;
        return ;
    }
    res+=a[n+1];
    for(int i=1;i<=n+1;i++){
        res-=a[i];
        if(res==a[n+2]){
            for(int j=1;j<=n+1;j++){
                if(j!=i){
                    cout<<a[j]<<" ";
                }
            }
            cout<<endl;
            return ;
        }
        res+=a[i];
    }
    cout<<-1<<endl;

}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    while(t--)
        solve();
    return 0;
}
```



### D. Corrupted Array

**题意**：给定n+2个数，从中找到n个数，使这n个数的和等于另外两个数的其中之一即可，按任意顺序输出这n个数。

**题解**：先排序，找到的n个数的和一定是$a_{n+1}$或者是$a_{n+2}$

* 和等于$a_{n+1}$的情况：判断前n个数的和是不是等于$a_{n+1}$
* 和等于$a_{n+2}$的情况：先求出1到n+1个数的和，然后遍历1~n+1，每次丢掉第i个数，判断其他n个数的和是否等于$a_{i+2}$即可

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=2e5+100;
const int mod=1e9+7;
const int INF=0x3f3f3f3f;
int n,m,k,t;
int a[SIZE];
void solve(){
    cin>>n;
    for(int i=1;i<=n+2;i++){
        cin>>a[i];
    }
    sort(a+1,a+3+n);
    ll res=0;
    for(int i=1;i<=n;i++){
        res+=a[i];
    }
    if(res==a[n+1]){
        for(int i=1;i<=n;i++){
            cout<<a[i]<<" ";
        }
        cout<<endl;
        return ;
    }
    res+=a[n+1];
    for(int i=1;i<=n+1;i++){
        res-=a[i];
        if(res==a[n+2]){
            for(int j=1;j<=n+1;j++){
                if(j!=i){
                    cout<<a[j]<<" ";
                }
            }
            cout<<endl;
            return ;
        }
        res+=a[i];
    }
    cout<<-1<<endl;

}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    while(t--)
        solve();
    return 0;
}
```

### E. Permutation by Sum

**题意**：给定permutations的定义：长度为n的permutations就是[1,2,3....n]，其中序列顺序任意，给定n，l，r，s，试求出长度为n，第L个数到第R个数的和为s的序列，如果无则输出-1

**题解**：判定有没有这样的序列：已知第L个数到第R个数一共有R-L+1个数，长度记为len，求出此长度序列之和可取范围（求和公式）如果在这个范围内，则有解，否则无解。

判断出有解后，就是简单构造了（？感觉很简单，具体怎么构造看代码注释吧）

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=2e5+100;
const int mod=1e9+7;
const int INF=0x3f3f3f3f;
int n,m,t;
int l,r,s;
int a[SIZE];
bool flag[5000];
void solve(){
    cin>>n>>l>>r>>s;
    memset(flag,0,sizeof flag);
    int len=r-l+1;
    int maxn=(2*n-len+1)*len/2;  //最大值
    int minn=(1+len)*len/2;        //最小值
    if(s<minn||s>maxn){
        cout<<-1<<endl;
        return ;
    }
    int cha=s-minn;  //先构造出 1，2，3。。len 设为最小值，求出还差多少
    int st=cha/len;  //每个数都加上st
    int ao=cha%len;  //另外多少个数需要都+1
    for(int i=1;i<=len;i++){
        a[i]=i+st;   //a序列存放你构造出的len个数
      if(i>=len-ao+1) a[i]+=1;
        flag[a[i]]=1;   //这些数需要在特定位置输出，所以需要提前标记
    }
    int pos=1;
    for(int i=1;i<=n;i++){
        if(i>=l&&i<=r){
            cout<<a[i-l+1]<<" ";  //如果在这个范围，就输出这个序列
        }
        else {
            while(flag[pos]){
                ++pos;  //否则一直找随机的数字未出现过的数字输出
            }
            cout<<pos<<" ";
            flag[pos]=1; //找到过后要标记，不能再输出了
            
        }
    }
    cout<<endl;
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    while(t--)
        solve();
    return 0;
}
```

### F. Education

**题意**：给定n个数$a_1$~$a_n$ 和 n-1个数$b_1$~$b_{n-1}$, 设定初始位置在1，在对于的第i个位置停留一回合可以获得$a_i$块钱，同意的你也可以花费$b_i$元钱和一回合时间使位置+1，你的目标是在最少的回合攒够s元钱，输出花费的最少回合为多少

* 输入规定$a_1$≤$a_2$≤…≤$a_n$

**题解**：贪心的思维来想，你最后一定是在只靠所在的最后一个位置来攒钱的，在其他位置停留的目标都是为了升级，那么从1到n跑一遍，假定每个位置都是最后一个位置，判断花费的最少的钱

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=2e5+100;
const int mod=1e9+7;
const int INF=0x3f3f3f3f;
int n,m,k,t;
int l,r,s;
int a[SIZE];
bool flag[5000];
void solve(){
    cin>>n>>l>>r>>s;
    memset(flag,0,sizeof flag);
    k=r-l+1;
    int maxn=(2*n-k+1)*k/2;
    int minn=(1+k)*k/2;
    if(s<minn||s>maxn){
        cout<<-1<<endl;
        return ;
    }
    int cha=s-minn;
    int st=cha/k;
    int ao=cha%k;
    for(int i=1;i<=k;i++){
        a[i]=i+st;
        if(i>=k-ao+1) a[i]+=1;
        flag[a[i]]=1;
    }
    int pos=1;
    for(int i=1;i<=n;i++){
        if(i>=l&&i<=r){
            cout<<a[i-l+1]<<" ";
        }
        else {
            while(flag[pos]){
                ++pos;
            }
            cout<<pos<<" ";
            flag[pos]=1;
        }
    }
    cout<<endl;
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    while(t--)
        solve();
    return 0;
}
```

### G. Short Task

**题意**：d(n)=n的约数和，给定c，求使d(n)=c的n

**题解**：埃氏筛，[(26条消息) 《算法竞赛中的初等数论》（一）正文 0x00整除、0x10 整除相关（ACM / OI / MO）（十五万字符数论书）_繁凡さん的博客-CSDN博客](https://fanfansann.blog.csdn.net/article/details/113797542#0x131__1083)，数论相关，约数和看看繁凡博客吧QAQ，我就不讲了

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=1e7+100;
const int mod=1e9+7;
const ll INF=1e18;
int n,m,k,t;
ll s[SIZE];
ll v[SIZE];
void init(){
    for(int i=1;i<=1e7;i++){
        for(int j=1;j<=1e7/i;j++){
            s[i*j]+=i;
        }
    }
for(int i=1;i<=1e7;i++){
    if(s[i]<=1e7){
        if(!v[s[i]]) v[s[i]]=i;
    }
}
}
void solve(){
    cin>>n;
    if(v[n]) cout<<v[n]<<endl;
    else cout<<-1<<endl;
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    init();
    while(t--)
        solve();
    return 0;
}
```
