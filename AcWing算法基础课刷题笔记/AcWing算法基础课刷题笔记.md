# AcWing算法基础课

## 贪心

### 区间问题

#### 905.区间选点

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N=1e5+10;

struct range{
    int left,right;
}ranges[N];

static bool cmp(const range& u,const range& v){
    return u.right<v.right;
}

int main(){
    int n;
    cin>>n;
    for(int i=0;i<n;i++) cin>>ranges[i].left>>ranges[i].right;
    
    sort(ranges,ranges+n,cmp);//注意是怎么调用sort函数的
    int x=ranges[0].right,count=1;
    for(int i=1;i<n;i++){
        if(ranges[i].left>x){
            x=ranges[i].right;
            count++;
        }
    }
    cout<<count;
    
    return 0;
}
```

#### 908. 最大不相交区间数量

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N=1e5+10;

struct range{
    int left,right;
}ranges[N];

static bool cmp(const range& u,const range& v){
    return u.right<v.right;
}

int main(){
    int n;
    cin>>n;
    for(int i=0;i<n;i++) cin>>ranges[i].left>>ranges[i].right;
    
    sort(ranges,ranges+n,cmp);
    int x=ranges[0].right,count=1;
    for(int i=1;i<n;i++){
        if(ranges[i].left>x){
            x=ranges[i].right;
            count++;
        }
    }
    cout<<count;
    
    return 0;
}
```

#### 906.区间分组

1. 将所有区间按左端点从小到大排序
2. 从前往后处理每个区间，判断能否将其放到某个现有的组中
   1. 如果不存在这样的组**（所有组的最小右端点仍比该区间左端点大）**，则开新组
   2. 如果存在这样的组，将其放进去，并更新当前组的max_right

```C++
#include<iostream>
#include<algorithm>
#include<queue>

using namespace std;

const int N=1e5+10;

struct range{
    int l,r;
}ranges[N];

static bool cmp(const range& u,const range& v){
    if(u.l!=v.l) return u.l<v.l;
    else return u.r<v.r;
}

int main(){
    int n;
    cin>>n;
    for(int i=0;i<n;i++) cin>>ranges[i].l>>ranges[i].r;
    
    sort(ranges,ranges+n,cmp);
    
    priority_queue<int,vector<int>,greater<int>> heap;//注意这是小根堆的写法，priority_queue<int> heap默认为大根堆
    for(int i=0;i<n;i++){
        if(heap.empty()||heap.top()>=ranges[i].l) heap.push(ranges[i].r);
        else{
            heap.pop();
            heap.push(ranges[i].r);
        }
    }
    
    cout<<heap.size();
    
    return 0;
}
```

#### 907.区间覆盖

题意：

​		给出N个闭区间以及一个线段区间[start，end]，选择尽可能少的区间，将线段区间完全覆盖。若存在，输出所需最少区间数，如果无解，输出-1。

思路：

1. 将左端点从小到大排序；
2. 从前往后依次枚举每个区间，在所有能覆盖start的区间中，选择右端点最大的区间，然后将start更新成右端点的最大值。

```C++
#include<iostream>
#include<algorithm>
#include<queue>

using namespace std;

const int N=1e5+10;

struct range{
    int l,r;
}ranges[N];

static bool cmp(const range& u,const range& v){
    if(u.l!=v.l) return u.l<v.l;
    else return u.r<v.r;
}

int main(){
    int s,t;
    cin>>s>>t;
    int n;
    cin>>n;
    for(int i=0;i<n;i++) cin>>ranges[i].l>>ranges[i].r;
    
    sort(ranges,ranges+n,cmp);
    
    int i=0,count=0;
    while(i<n){
        int j=i,r=-2e9;
        while(j<n&&ranges[j].l<=s){
            r=max(r,ranges[j].r);
            j++;
        }
        count++;
        if(r<s){
            cout<<-1;
            return 0;
        }
        else if(r>=t){
            cout<<count;
            return 0;
        }
        else{
            s=r;
            i=j;
        }
    }
    cout<<-1;
    
    return 0;
}
```

### Huffman树

#### 148.合并果子

```C++
#include<iostream>
#include<queue>

using namespace std;

int main(){
    int n;
    cin>>n;
    priority_queue<int,vector<int>,greater<int>> heap;
    for(int i=0;i<n;i++){
        int a;
        cin>>a;
        heap.push(a);
    }
    
    int result=0;
    for(int i=0;i<n-1;i++){
        int a,b;
        a=heap.top();heap.pop();
        b=heap.top();heap.pop();
        result+=a+b;
        heap.push(a+b);
    }
    
    cout<<result;
    
    return 0;
}
```

### 排序不等式

#### 913.排队打水

题意：

​		有 n 个人排队到 11 个水龙头处打水，第 i 个人装满水桶所需的时间是 t~i~，请问如何安排他们的打水顺序才能使所有人的等待时间之和最小？

思路：

​		按照从小到大的顺序排队，时间最小。

```C++
#include<iostream>
#include<algorithm>

using namespace std;
typedef long long LL;

const int N=1e5+10;

int t[N];

int main(){
    int n;
    cin>>n;
    for(int i=0;i<n;i++) cin>>t[i];
    
    sort(t,t+n);
    LL res=0;
    for(int i=0;i<n;i++) res+=t[i]*(n-i-1);
    
    cout<<res;
    
    return 0;
}
```

### 绝对值不等式

#### 104.货仓选址

题意：

​		在一条数轴上有 N家商店，它们的坐标分别为 A~1~-A~N~。现在需要在数轴上建立一家货仓，每天清晨，从货仓到每家商店都要运送一车商品。为了提高效率，求把货仓建在何处，可以使得货仓到每家商店的距离之和最小。

思路：

​		尽可能往中间放。

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N=1e5+10;

int a[N];

int main(){
    int n;
    cin>>n;
    for(int i=0;i<n;i++) cin>>a[i];
    
    sort(a,a+n);
    int res=0;
    for(int i=0;i<n;i++) res+=abs(a[i]-a[n/2]);
    
    cout<<res;
    
    return 0;
}
```

### 推公式

#### 125.耍杂技的牛

题意：

​		农民约翰的 N头奶牛（编号为1...N）计划逃跑并加入马戏团，为此它们决定练习表演杂技。

​		奶牛们不是非常有创意，只提出了一个杂技表演：叠罗汉，表演时，奶牛们站在彼此的身上，形成一个高高的垂直堆叠。奶牛们正在试图找到自己在这个堆叠中应该所处的位置顺序。这 N 头奶牛中的每一头都有着自己的重量 W~i~ 以及自己的强壮程度 S~i~。

​		一头牛支撑不住的可能性取决于它头上所有牛的总重量（不包括它自己）减去它的身体强壮程度的值，现在称该数值为风险值，风险值越大，这只牛撑不住的可能性越高。

​		您的任务是确定奶牛的排序，使得所有奶牛的风险值中的最大值尽可能的小。

思路：

​		按照 W~i~+ S~i~ 从小到大的顺序排，最大的危险系数一定是最小的。

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N=5e4+10;

struct cow{
    int w,s;
}cows[N];

static bool cmp(const cow& a,const cow& b){
    return a.w+a.s<b.w+b.s;
}

int main(){
    int n;
    cin>>n;
    for(int i=0;i<n;i++) cin>>cows[i].w>>cows[i].s;
    
    sort(cows,cows+n,cmp);
    int res=-2e9,sum=0;
    for(int i=0;i<n;i++){
        res=max(res,sum-cows[i].s);
        sum+=cows[i].w;
    }
    
    cout<<res;
    
    return 0;
}
```
