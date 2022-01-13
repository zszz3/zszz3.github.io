## 0X7E6 Week1

### A-Divan and a Store

**题意**:第一行输入n,l,r,k,代表你一共有n块巧克力,你要买价格在l和r之间的巧克力,一共有k元钱,询问你能买的最多巧克力的个数,

 <!-- more -->

**题解**:找到第一个大于l的位置开始买巧克力即可

```c++
#include<iostream>
#include<string>
#include<vector>
#include<algorithm>
#include<set>
#include<queue>
#include<deque>
using namespace std;
typedef long long ll;
#define yes cout<<"YES"<<endl
#define no cout<<"NO"<<endl
#define mp make_pair
#define pb push_back
const int SIZE=2e6+10;
const int mod=1e9+7;
const int INF=1e9+7;
int t,n,m,k;
int l,r;
int a[SIZE];
void solve(){
    cin>>n>>l>>r>>k;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    sort(a+1,a+1+n);  //排序
    int ans=0;
    int pos=lower_bound(a+1,a+1+n,l)-a;   //lower_bound函数找第一个大于等于l的元素的位置
    for(int i=pos;a[i]<=r&&i<=n;i++){
        if(k>=a[i]) {ans++,k-=a[i];}
    }
    cout<<ans<<endl;
    }
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    while(t--) solve();
    }
```

### B-Divan and a New Project

**题意**:一共有n+1一个建筑在一条直线上,每个建筑都在整数点且不重复,小明站在第0号建筑,他将访问其他n个建筑每个都$a_i$次,每次花费的时间为2*abs($a_i$-$a_0$),试求出小明访问其他n个建筑花费的时间和,并通过摆放位置使这个值最小

**题解**:我觉着思路还是蛮简单的其实,实际上就是一个贪心,使$a_i$ 大的离$a_0$最近即可,其他就是代码实现的问题了

<img src="https://s2.loli.net/2022/01/12/fJUicD5vypCgXPW.png" alt="无标题.png" style="zoom:33%;" />

我觉着看这张图还挺清晰明了的,我们对于$a_i$排序,让$a_0$在第0个位置,让次数最大的变成$a_1$放在1这个位置,紧接着就一次类推了,麻烦一点的可能是这个过程,具体看代码

```c++
#include<iostream>
#include<string>
#include<vector>
#include<algorithm>
#include<set>
#include<queue>
#include<deque>
using namespace std;
typedef long long ll;
#define yes cout<<"YES"<<endl
#define no cout<<"NO"<<endl
#define mp make_pair
#define pb push_back
const int SIZE=2e6+10;
const int mod=1e9+7;
const int INF=1e9+7;
int t,n,m,k;
int l,r;
struct node{
    int first;
    int second;
}a[SIZE];
bool cmp(node a,node b){  //按照x的值排序
    if(a.first==b.first) return a.second<b.second;
    else return a.first<b.first;
}
bool cmp2(node a,node b){
    return a.second<b.second;   //按照他的序号排序,还原位置
}
void solve(){
    cin>>n;
    for(int i=1;i<=n;i++){
        ll x;
        cin>>x;
        a[i].first=-x;
        a[i].second=i;
    }
    sort(a+1,a+1+n,cmp);   //按照x的值从大到小排序,本来逻辑上应该是从小到大,但输入时是按照-x输入的
    ll now=1;
    ll ans=0;
    for(int i=1;i<=n;i++){
        ans+=-a[i].first*2*abs(now);  //计算答案
        a[i].first=now;    //c
        if(i%2) now=-1*now;
        else now=-now+1;
    }
    cout<<ans<<endl;
    sort(a+1,a+1+n,cmp2);
    cout<<0<<" ";
    for(int i=1;i<=n;i++) cout<<a[i].first<<" ";
    cout<<endl;
    }
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    while(t--) solve();
    }
```



### C-Divan and bitwise operations

**题意**:对于某个长为n的序列，给你该序列若干个子段及其元素的或（这些子段必然完全覆盖整个序列），告诉你一定存在一个序列满足要求，现在让你构造出满足要求的任一个序列，求它的所有子序列的异或和之和。（只需输出异或和之和）

**题解**:

* 由于位运算对每个位都是独立的，考虑分别对每一位求解
* 首先看输入的给我们带来什么，对于[l,r]得到的同或和x，考虑它的某一位，如果这一位是0，说明[l,r]每个元素这位都是0，如果这一位是1，至少有1个1，后者没什么用
* 考虑某一位bit，一共有n个元素，假设有a个1，b个0，显然有a + b = n a+b=na+b=n
* 我们如果要对结果（所有子序列异或和的总和），就要选出若干个元素使它们异或和为1，根据异或和的性质，需要选择`奇数个1和若干个（0）0`
* 选奇数个1的方案为$C^1_a$+$C^3_a$+... =$2^{a-1}$,选任意个0的方案为$2^b$
* $2^{a-1}$ * $2^b$ = $2^{a+b-1}$=$2^{n-1}$ 当前bit位上贡献为1<<bit
* 所以对于每个位，只要有1，那么这个位对结果的贡献就是$2^{n-1}$*(1<<bit) ,否则就是0
* 于是乎我们把每一个拆开,然后求答案和即可
* 参考链接 [(11条消息) Divan and bitwise operations 异或，同或，组合数学（1500）_m0_51448653的博客-CSDN博客](https://blog.csdn.net/m0_51448653/article/details/121592218?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-1.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-1.no_search_link&utm_relevant_index=2)

```c++
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <unordered_set>
#include <math.h>
#define endl '\n'
#define fi first
#define se second
#define pb push_back

using namespace std;
using ll = long long;

typedef pair<int, int> PII;

const ll mod = 1e9 + 7;

void solve()
{
    int n, m, v = 0; cin >> n >> m;
    for (int i = 1, l, r, x; i <= m && cin >> l >> r >> x; i ++ ) v |= x;
    
    ll fac = 1;
    for (int i = 1; i <= n - 1; i ++ ) fac = fac * 2ll % mod;
    cout << fac * v % mod << endl;
}

int main()
{
    cin.tie(nullptr) -> sync_with_stdio(false);
    
    int _;
    cin >> _;
    while (_ -- )
        solve();
    
    return 0;
}
```



### D Linear Keyboard

 **题意**:输入有两行,第一行给定你26个字母a-z的顺序,第二行给定你一个字符串s,询问你这个字符串每两个字符之间的距离和为多少\

**题解** 定义一个数组记录每个字符的位置,紧接着遍历一遍s字符求和即可

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=2e5+100;
const int mod=1e9+7;
const int INF=1e9+7;
int n,m,k,t;
int pos[SIZE];
void init(){};
void solve(){
    string s1,s2;
    cin>>s1>>s2;
    for(int i=0;i<s1.size();i++){
        pos[s1[i]]=i;
    }
    ll ans=0;
    for(int i=1;i<s2.size();i++){
        ans+=abs(pos[s2[i]]-pos[s2[i-1]]);
    }
    cout<<ans<<endl;
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



### E-Odd Grasshopper

 **题意**:小明在$x_0$的位置,他要进行n次跳跃,第一次跳跃距离为1,第二次为2....如果他所处的位置为奇数 就往右跳,偶数往左跳,询问n次跳跃后的位置

**题解**:找规律题多试几组例子就可以找到规律,假设在一个偶数位置,第一次跳-1,第二次跳+2,第三次+3,第四次-4,以此类推,每四个一循环,紧接着在模拟剩余的要跳的就行了  (注意啊要开longlong 不然爆了)

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=2e5+100;
const int mod=1e9+7;
const int INF=1e9+7;
int n,m,k,t;
int pos[SIZE];
void init(){};
void solve(){
    ll x;
    ll n;
    cin>>x>>n;
    for(ll i=n/4*4+1;i<=n;i++){
        if(x%2==0){
            x-=i;
        }
        else x+=i;
    }
    cout<<x<<endl;
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

### F-Minimum Extraction

**题意**:给一数组，每次可以将最小数从数列去除且其他所有数减去这个最小数，求过程中最大的最小数

**题解**:每次减去的数字是固定的,所以每次最小的数字已经确定了,就是不知道是谁罢了,所以模拟这个过程就好,但是如果你每次都对每个数减去这个最小的数,这个复杂的是o($n^2$),会时间超限,于是说需要开一个pre数组记录每次减去的数字

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=2e5+100;
const int mod=1e9+7;
const int INF=1e9+7;
int n,m,k,t;
ll a[SIZE];
void init(){};
void solve(){
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    sort(a+1,a+1+n);
    ll pre=a[1];
    ll maxn=pre;
    for(int i=2;i<=n;i++){
        a[i]-=pre;   //减去最小的数
        maxn=max(a[i],maxn);
        pre+=a[i];
    }
    cout<<maxn<<endl;
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

### G-Blue-Red Permutation

**题意**:给定一个数组a，他们每个元素具有red和blue中的其中一种，red属性的元素可以变成大于等于这个元素的任意数，blue属性的元素可以变成小于等于这个元素的任意数。现有一个数组a，问他们能否组成一个新数列包含1到n的所有数。

**题解**:把蓝色都往左移,红色都往又移动,定义两个堆,一个大顶堆,一个小顶堆,尽量补全即可

``` c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=2e5+100;
const int mod=1e9+7;
const int INF=1e9+7;
int n,m,k,t;
int a[SIZE];
priority_queue<int,vector<int>,less<int>>r;   //大顶堆   栈顶是最大的元素
priority_queue<int,vector<int>,greater<int>>b;  //小顶堆 栈顶是最小的元素
void init(){};
void solve(){
    int cntr=0;
    int cntb=0;
    while(!r.empty()) r.pop();  //清空
    while(!b.empty()) b.pop();  //清空
    cin>>n;
    for(int i=0;i<n;i++) cin>>a[i];
    string s;
    cin>>s;
    for(int i=0;i<s.size();i++){
        if(s[i]=='B'){
            b.push(a[i]),cntb++;
        }
        else r.push(a[i]), cntr++;
    }
    int flag=0;
    for(int i=n;i>n-cntr;i--){
        int top=r.top();
        r.pop();
        if(top>i){
            cout<<"NO"<<endl;
            return ;
        }
    }
    for(int i=1;i<=cntb;i++){
        int top=b.top();
        b.pop();
        if(top<i){
            cout<<"NO"<<endl;
            return ;
        }
    }
    cout<<"YES"<<endl;
    return ;
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

### H-Robot on the Board 1

**题意**: 有一个机器人，可以在n*m的方格内行走，现在给出一个指令，让你在方格内找到一个坐标，使机器人不走出方格且尽可能执行完全部的指令

**题解**:这题可以纵向和横向分开来看啊,先看横向,我们记录一下这些指令执行往右能移动的最大位置maxn和往左能移动的最大位置minn,如果$maxn-minn+1=n$ 就说明超出最大能移动范围了,紧接着你的出发位置就是,n-maxn, 纵向同理

``` c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int SIZE=1e6;
const int mod=1e9+7;
int n,t,m;
string s;
void solve(){
    cin>>n>>m;
    cin>>s;
    int maxx=0,minn=0;
    int pos=0;
    for(int i=0;i<s.size();i++){
        if(maxx-minn+1==n) break;
        if(s[i]=='U') pos--;
        if(s[i]=='D') pos++;
        minn=min(minn,pos);
        maxx=max(maxx,pos);
    }
    cout<<n-maxx<<" ";
    maxx=0; minn=0; pos=0;
    for(auto it:s){
        if(maxx-minn+1==m) break;
        if(it=='L') pos--;
        if(it=='R') pos++;
        minn=min(minn,pos);
        maxx=max(maxx,pos);
    }
    cout<<m-maxx<<endl;
    return ;
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    cin>>t;
    while(t--){
        solve();
    }
    return 0;
    }
```

