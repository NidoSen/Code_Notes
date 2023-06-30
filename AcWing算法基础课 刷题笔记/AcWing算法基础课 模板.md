# 第一讲 基础算法


## 快速排序

```C++
void quickSort(int q[], int left, int right) {
    if (left >= right) {
        return;
    }
    int i = left - 1, j = right + 1, x = q[left + right >> 1];
    while (i < j) {
        while (q[++i] < x);
        while (q[--j] > x);
        if (i < j) {
            swap(q[i], q[j]);
        }
    }
    quickSort(q, left, j);
    quickSort(q, j + 1, right);
}
```

## 归并排序

```C++
void mergeSort(int q[], int left, int right) {
    if (left >= right) {
        return;
    }

    int mid = left + right >> 1;
    mergeSort(q, left, mid);
    mergeSort(q, mid + 1, right);

    int i = left, j = mid + 1, k = left;
    while (i <= mid && j <= right) {
        if (q[i] <= q[j]) {
            tmp[k++] = q[i++];
        }
        else {
            tmp[k++] = q[j++];
        }
    }

    while (i <= mid) {
        tmp[k++] = q[i++];
    }
    while (j <= right) {
        tmp[k++] = q[j++];
    }

    for (i = left; i <= right; i++) {
        q[i] = tmp[i];
    }
}
```

## 二分查找

```C++
/* 整数二分模板 */
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[left, right]被划分成[left, mid]和[mid + 1, right]时使用：
int bsearch_1(int left, int right) {
    while (left < right) {
        int mid = left + right >> 1;
        if (check(mid)) {
            right = mid;    // check()判断mid是否满足性质
        }
        else {
            left = mid + 1;
        }
    }
    return left;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int left, int right) {
    while (left < right) {
        int mid = left + right + 1 >> 1;
        if (check(mid)) {
            left = mid;
        }
        else {
            right = mid - 1;
        }
    }
    return left;
}
```

```C++
/* 浮点数二分模板 */
double bsearch_3(double left, double right) {
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (right - left > eps) {
        double mid = (left + right) / 2;
        if (check(mid)) {
            right = mid;
        }
        else {
            left = mid;
        }
    }
    return left;
}
```

## 高精度

```c++
string a, b;
cin >> a >> b;
vector<int> A, B;
for (int i = a.length() - 1; i >= 0; i--) {
    A.push_back(a[i] - '0');
}
for (int i = b.length() - 1; i >= 0; i--) {
    B.push_back(b[i] - '0');
}
```

```C++
/* 高精度加法 */
// C = A + B, A >= 0, B >= 0
vector<int> add(const vector<int>& A, cosnt vector<int>& B) {
    if (A.size() < B.size()) {
        return add(B, A);
    }

    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i++) {
        t += A[i];
        if (i < B.size()) {
            t += B[i];
        }
        C.push_back(t % 10);
        t /= 10;
    }
    if (t) {
        C.push_back(t);
    }
    
    return C;
}
```

```C++
/* 高精度减法 */
bool judge(const vector<int>& A, const vector<int>& B) {
    if (A.size() != B.size()) {
        return A.size() > B.size();
    }
    else {
        for (int i = A.size() - 1; i >= 0; i--) {
            if (A[i] == B[i]) {
                continue;
            }
            else if (A[i] > B[i]) {
                return true;
            }
            else if (A[i] < B[i]) {
                return false;
            }
        }
        return true;
    }
}

// C = A - B, 满足A >= B, A >= 0, B >= 0
vector<int> sub(const vector<int>& A, const vector<int>& B) {
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i++) {
        t = A[i] - t;
        if (i < B.size()) {
            t -= B[i];
        }
        C.push_back((t + 10) % 10);
        if (t < 0) {
            t = 1;
        }
        else {
            t = 0;
        }
    }

    while (C.size() > 1 && C.back() == 0) {
        C.pop_back();
    }
    
    return C;
}
```

```C++
/* 高精度乘低精度 */
// C = A * b, A >= 0, b >= 0
vector<int> mul(const vector<int>& A, int b) {
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size() || t; i ++ ) {
        if (i < A.size()) {
            t += A[i] * b;
        }
        C.push_back(t % 10);
        t /= 10;
    }

    while (C.size() > 1 && C.back() == 0) {
        C.pop_back();
    }

    return C;
}
```

```C++
/* 高精度除以低精度 */
// A / b = C ... r, A >= 0, b > 0
vector<int> div(const vector<int>& A, int b, int& r) {
    vector<int> C;
    
    r = 0;
    for (int i = A.size() - 1; i >= 0; i--) {
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    
    reverse(C.begin(), C.end());
    while (C.size() > 1 && C.back() == 0) {
        C.pop_back();
    }
    
    return C;
}
```

## 前缀与差分

```C++
/* 一维前缀和 */
s[i] = a[1] + a[2] + ... a[i] = s[i - 1] + a[i]
a[left] + ... + a[right] = s[right] - s[left - 1]
```

```C++
/* 二维前缀和 */
s[i][j] = 第i行j列格子左上部分所有元素的和 = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + a[i][j]
以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵的和为：
s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1]
```

```C++
/* 一维差分 */
b[i] = a[i] - a[i - 1]
给区间[left, right]中的每个数加上c：
b[left] += c, b[right + 1] -= c
a[i] = b[i - 1] + a[i - 1]
```

```C++
/* 二维差分 */
b[i][j] = a[i][j] + a[i - 1][j - 1] - a[i][j - 1] - a[i - 1][j];
给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：
b[x1][y1] += c, b[x2 + 1][y1] -= c, b[x1][y2 + 1] -= c, b[x2 + 1][y2 + 1] += c
a[i][j] = a[i - 1][j] + a[i][j - 1] - a[i - 1][j - 1] + b[i][j]
```

## 位运算

```C++
求n的第k位数字: n >> k & 1
返回n的最后一位1：lowbit(n) = n & -n
```

## 离散化

```C++
vector<int> alls; // 存储所有待离散化的值
sort(alls.begin(), alls.end()); // 将所有值排序
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素

// 二分求出x对应的离散化的值
int find(int x) {// 找到第一个大于等于x的位置
    int left = 0, right = alls.size() - 1;
    while (left < right) {
        int mid = left + right >> 1;
        if (alls[mid] >= x) {
            right = mid;
        }
        else {
            left = mid + 1;
        }
    }
    return right + 1; // 映射到1, 2, ...n
}
```

## 区间合并

```C++
// 将所有存在交集的区间合并
void merge(vector<PII> &segs) {
    vector<PII> res;

    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs) {
        if (ed < seg.first) {
            if (st != -2e9) {
                res.push_back({st, ed});
            }
            st = seg.first;
            ed = seg.second;
        }
        else {
            ed = max(ed, seg.second);
        }
    }

    if (st != -2e9) {
        res.push_back({st, ed});
    }

    segs = res;
}
```

# 第二讲 数据结构


## 单链表

```C++
// head存储链表头，e[]存储节点的值，ne[]存储节点的next指针，index表示当前用到了哪个节点
int head, e[N], ne[N], index;

// 初始化
void init() {
    head = -1;
    index = 0;
}

// 在链表头插入一个数a
void insert(int a) {
    e[idx] = a, ne[idx] = head, head = index++;
}

// 在第k个插入的数后面插入一个数
void insert_k(int k, int x) {
    e[idex] = x, ne[idex] = ne[k - 1], ne[k - 1] = idex++;
}

// 将头结点删除，需要保证头结点存在
void remove() {
    head = ne[head];
}

// 删除第k个插入的数后面的数
void remove_k(int k) {
    if (k == 0) {
        head = ne[head];
    }
    else {
        ne[k - 1] = ne[ne[k - 1]];
    }
}
```

## 双链表

```C++
// e[]表示节点的值，l[]表示节点的左指针，r[]表示节点的右指针，index表示当前用到了哪个节点
int e[N], l[N], r[N], index;

// 初始化
void init() {
    //0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    index = 2;
}

// 在节点a的右边插入一个数x
void insert(int a, int x) {
    e[index] = x;
    l[index] = a, r[index] = r[a]; //先更新插入节点
    l[r[a]] = idx, r[a] = index++; //再更新左右节点
}

// 在最左边插入一个数x
void insert_left(int x) {
    insert(0, x);
}

// 在最右边插入一个数x
void insert_right(int x) {
    insert(l[1], x);
}

// 删除节点a
void remove(int a) {
    l[r[a]] = l[a];
    r[l[a]] = r[a];
```

## 栈

```C++
// tt表示栈顶
int stk[N], tt = 0;

// 向栈顶插入一个数
stk[++tt] = x;

// 从栈顶弹出一个数
tt -- ;

// 栈顶的值
stk[tt];

// 判断栈是否为空，如果 tt > 0，则表示不为空
if (tt > 0) {

}
```

## 队列

```C++
/* 普通队列 */
// hh 表示队头，tt表示队尾
int q[N], hh = 0, tt = -1;

// 向队尾插入一个数
q[++tt] = x;

// 从队头弹出一个数
hh++;

// 队头的值
q[hh];

// 判断队列是否为空，如果 hh <= tt，则表示不为空
if (hh <= tt) {

}
```

```C++
/* 循环队列 */
// hh 表示队头，tt表示队尾的后一个位置
int q[N], hh = 0, tt = 0;

// 向队尾插入一个数
q[tt++] = x;
if (tt == N) {
    tt = 0;
}

// 从队头弹出一个数
hh++;
if (hh == N) {
    hh = 0;
}

// 队头的值
q[hh];

// 判断队列是否为空，如果hh != tt，则表示不为空
if (hh != tt) {

}
```

## 单调栈

```C++
常见模型：找出每个数左边离它最近的比它大/小的数
int tt = 0;
for (int i = 1; i <= n; i++) {
    while (tt && check(stk[tt], i)) {
        tt--;
    }
    stk[++tt] = i;
}
```

```C++
// 使用STL
#include<vector>
#include<stack>

vector<int> vec;
stack<int> stk;

for (int i = 0; i < vec.size(); i++) {
    while (!st.empty() && check(stk.top(), i)) {
        stk.pop();
    }
    stk.push(i);
}
```

## 单调队列

```C++
常见模型：找出滑动窗口中的最大值/最小值
int hh = 0, tt = -1;
for (int i = 0; i < n; i++) {
    while (hh <= tt && check_out(q[hh])) { // 判断队头是否滑出窗口
        hh++;
    }
    while (hh <= tt && check(q[tt], i)) {
        tt--;
    }
    q[++tt] = i;
}
```

```C++
// 使用STL
#include<vector>
#include<deque>

vector<int> vec;
deque<int> dque;
for (int i = 0; i < vec.size(); i++) {
    while (!dque.empty() && check_out(dque.front())) { // 判断队头是否滑出窗口
        dque.pop_front();
    }
    while (!dque.empty() && check(dque.back(), i)) {
        dque.pop_back();
    }
   dque.push_back();
}
```

## KMP

```C++
// s[]是长文本，p[]是模式串，n是s的长度，m是p的长度
求模式串的Next数组：
for (int i = 2, j = 0; i <= m; i ++ ) {
    while (j && p[i] != p[j + 1]) {
        j = ne[j];
    }
    if (p[i] == p[j + 1]) {
        j++;
    }
    ne[i] = j;
}

// 匹配
for (int i = 1, j = 0; i <= n; i ++ ) {
    while (j && s[i] != p[j + 1]) {
        j = ne[j];
    }
    if (s[i] == p[j + 1]) {
        j++;
    }
    if (j == m) {
        j = ne[j];
        // 匹配成功后的逻辑
    }
}
```

```C++
// 使用STL
// s[]是长文本，p[]是模式串，n是s的长度，m是p的长度
// 求模式串的next数组：
#include<vector>

string s, p;
vector<int> next(p.length(), -1);
for (int i = 1, j = -1; i < p.length(); i++) {
	while (j >= 0 && p[i] != p[j + 1]) {
		j = next[j];
    }
	if (p[i] == p[j + 1]) {
        j++;
    }
    next[i] = j;
}
// 匹配
for (int i = 0, j = -1; i < s.length(); i++) {
    while (j >= 0 && s[i] != p[j + 1]) {
        j = next[j];
    }
    if (s[i] == p[j + 1]) {
        j++;
    }
    if (j == p.length() - 1) {
        // 匹配成功后的逻辑
    }
}
```

## Tire树

```C++
int son[N][26], cnt[N], idx;
// 0号点既是根节点，又是空节点
// son[][]存储树中每个节点的子节点
// cnt[]存储以每个节点结尾的单词数量

// 插入一个字符串
void insert(char *str) {
    int p = 0;
    for (int i = 0; str[i]; i++) {
        int u = str[i] - 'a';
        if (!son[p][u]) {
            son[p][u] = ++idx;
        }
        p = son[p][u];
    }
    cnt[p]++;
}

// 查询字符串出现的次数
int query(char *str) {
    int p = 0;
    for (int i = 0; str[i]; i++) {
        int u = str[i] - 'a';
        if (!son[p][u]) {
            return 0;
        }
        p = son[p][u];
    }
    return cnt[p];
}
```

```C++
class Trie {
private:
    bool isEnd;
    Trie* next[26];
public:
    Trie() {
        isEnd = false;
        memset(next, NULL, sizeof(next));
    }
    
    void insert(string word) {
        Trie* node = this;
        for (char c : word) {
            if (node->next[c - 'a'] == NULL) {
                node->next[c - 'a'] = new Trie();
            }
            node = node->next[c - 'a'];
        }
        node->isEnd = true;
    }
    
    bool search(string word) {
        Trie* node = this;
        for (char c : word) {
            node = node->next[c - 'a'];
            if (node == NULL) {
                return false;
            }
        }
        return node->isEnd;
    }
    
    bool startsWith(string prefix) {
        Trie* node = this;
        for (char c : prefix) {
            node = node->next[c - 'a'];
            if (node == NULL) {
                return false;
            }
        }
        return true;
    }
};
```

## 并查集

```C++
(1)朴素并查集：

    int p[N]; //存储每个点的祖宗节点

    // 返回x的祖宗节点
    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i++) {
        p[i] = i;
    }

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
```

```C++
(2)维护size的并查集：

    int p[N], size[N];
    //p[]存储每个点的祖宗节点, size[]只有祖宗节点的有意义，表示祖宗节点所在集合中的点的数量

    // 返回x的祖宗节点
    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ ) {
        p[i] = i;
        size[i] = 1;
    }

    // 合并a和b所在的两个集合：
    size[find(b)] += size[find(a)];
    p[find(a)] = find(b);
```

```C++
(3)维护到祖宗节点距离的并查集：

    int p[N], d[N];
    //p[]存储每个点的祖宗节点, d[x]存储x到p[x]的距离

    // 返回x的祖宗节点
    int find(int x) {
        if (p[x] != x) {
            int u = find(p[x]);
            d[x] += d[p[x]];
            p[x] = u;
        }
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i++) {
        p[i] = i;
        d[i] = 0;
    }

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
    d[find(a)] = distance; // 根据具体问题，初始化find(a)的偏移量
```

## 堆

```C++
// h[N]存储堆中的值, h[1]是堆顶，x的左儿子是2x, 右儿子是2x + 1
// ph[k]存储第k个插入的点在堆中的位置
// hp[k]存储堆中下标是k的点是第几个插入的
int h[N], ph[N], hp[N], size;

// 交换两个点，及其映射关系
void heap_swap(int a, int b) {
    swap(ph[hp[a]],ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(h[a], h[b]);
}

void down(int u) {
    int t = u;
    if (u * 2 <= size && h[u * 2] < h[t]) {
        t = u * 2;
    }
    if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) {
        t = u * 2 + 1;
    }
    if (u != t) {
        heap_swap(u, t);
        down(t);
    }
}

void up(int u) {
    while (u / 2 && h[u] < h[u / 2]) {
        heap_swap(u, u / 2);
        u >>= 1;
    }
}

// O(n)建堆
for (int i = n / 2; i; i--) {
    down(i);
}
```

## 哈希

```C++
/* 一般哈希 */
(1) 拉链法
    int h[N], e[N], ne[N], idx;

    // 向哈希表中插入一个数
    void insert(int x) {
        int k = (x % N + N) % N;
        e[idx] = x;
        ne[idx] = h[k];
        h[k] = idx++;
    }

    // 在哈希表中查询某个数是否存在
    bool find(int x) {
        int k = (x % N + N) % N;
        for (int i = h[k]; i != -1; i = ne[i]) {
            if (e[i] == x) {
                return true;
            }
        }
        return false;
    }
```

```C++
(2) 开放寻址法
    int h[N];

    // 如果x在哈希表中，返回x的下标；如果x不在哈希表中，返回x应该插入的位置
    int find(int x) {
        int t = (x % N + N) % N;
        while (h[t] != null && h[t] != x) {
            t++;
            if (t == N) {
                t = 0;
            }
        }
        return t;
    }
```

```C++
/* 字符串哈希 */
核心思想：将字符串看成P进制数，P的经验值是131或13331，取这两个值的冲突概率低
小技巧：取模的数用2^64，这样直接用unsigned long long存储，溢出的结果就是取模的结果

typedef unsigned long long ULL;
ULL h[N], p[N]; // h[k]存储字符串前k个字母的哈希值, p[k]存储 P^k mod 2^64

// 初始化
p[0] = 1;
for (int i = 1; i <= n; i++) {
    h[i] = h[i - 1] * P + str[i];
    p[i] = p[i - 1] * P;
}

// 计算子串 str[l ~ r] 的哈希值
ULL get(int l, int r) {
    return h[r] - h[l - 1] * p[r - l + 1];
}
```

## C++ STL

```C++
vector, 变长数组，倍增的思想
    size()  返回元素个数
    empty()  返回是否为空
    clear()  清空
    front()/back()
    push_back()/pop_back()
    begin()/end()
    []
    支持比较运算，按字典序

pair<int, int>
    first, 第一个元素
    second, 第二个元素
    支持比较运算，以first为第一关键字，以second为第二关键字（字典序）

string，字符串
    size()/length()  返回字符串长度
    empty()
    clear()
    substr(起始下标，(子串长度))  返回子串
    c_str()  返回字符串所在字符数组的起始地址

queue, 队列
    size()
    empty()
    push()  向队尾插入一个元素
    front()  返回队头元素
    back()  返回队尾元素
    pop()  弹出队头元素

priority_queue, 优先队列，默认是大根堆
    size()
    empty()
    push()  插入一个元素
    top()  返回堆顶元素
    pop()  弹出堆顶元素
    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;

stack, 栈
    size()
    empty()
    push()  向栈顶插入一个元素
    top()  返回栈顶元素
    pop()  弹出栈顶元素

deque, 双端队列
    size()
    empty()
    clear()
    front()/back()
    push_back()/pop_back()
    push_front()/pop_front()
    begin()/end()
    []

set, map, multiset, multimap, 基于平衡二叉树（红黑树），动态维护有序序列
    size()
    empty()
    clear()
    begin()/end()
    ++, -- 返回前驱和后继，时间复杂度 O(logn)

    set/multiset
        insert()  插入一个数
        find()  查找一个数
        count()  返回某一个数的个数
        erase()
            (1) 输入是一个数x，删除所有x   O(k + logn)
            (2) 输入一个迭代器，删除这个迭代器
        lower_bound()/upper_bound()
            lower_bound(x)  返回大于等于x的最小的数的迭代器
            upper_bound(x)  返回大于x的最小的数的迭代器
    map/multimap
        insert()  插入的数是一个pair
        erase()  输入的参数是pair或者迭代器
        find()
        []  注意multimap不支持此操作。 时间复杂度是 O(logn)
        lower_bound()/upper_bound()

unordered_set, unordered_map, unordered_multiset, unordered_multimap, 哈希表
    和上面类似，增删改查的时间复杂度是 O(1)
    不支持 lower_bound()/upper_bound()， 迭代器的++，--

bitset, 圧位
    bitset<10000> s;
    ~, &, |, ^
    >>, <<
    ==, !=
    []

    count()  返回有多少个1

    any()  判断是否至少有一个1
    none()  判断是否全为0

    set()  把所有位置成1
    set(k, v)  将第k位变成v
    reset()  把所有位变成0
    flip()  等价于~
    flip(k) 把第k位取反
```

## 
