# 第一讲 基础算法

## 快速排序

### 模板

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

### [AcWing 785. 快速排序](https://www.acwing.com/problem/content/787/)

```C++
#include<iostream>
using namespace std;

const int N = 1e5 + 10;
int q[N];

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

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &q[i]);
    }
    
    quickSort(q, 0, n - 1);
    
    for (int i = 0; i < n; i++) {
        printf("%d ", q[i]);
    }
    
    return 0;
}
```

### [AcWing 786. 第k个数](https://www.acwing.com/problem/content/description/788/)

```C++
#include<iostream>
using namespace std;

const int N = 1e5 + 10;
int q[N];

int quickSelect(int q[], int left, int right, int k) {
    if (left >= right) {
        return q[left];
    }
    
    int i = left - 1, j = right + 1, x = q[left + right >> 1];
    while (i < j) {
        while (q[++i] < x);
        while (q[--j] > x);
        if (i < j) {
            swap(q[i], q[j]);
        }
    }
    
    if (k <= j + 1) {
        return quickSelect(q, left, j, k);
    }
    else {
        return quickSelect(q, j + 1, right, k);
    }
}

int main() {
    int n, k;
    scanf("%d%d", &n, &k);
    for (int i = 0; i < n; i++) {
        scanf("%d", &q[i]);
    }
    
    printf("%d", quickSelect(q, 0, n - 1, k));
    
    return 0;
}
```

## 归并排序

### 模板

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

### [AcWing 787. 归并排序](https://www.acwing.com/problem/content/789/)

```C++
#include<iostream>
using namespace std;

const int N = 1e5 + 10;
int q[N], temp[N];

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
            temp[k++] = q[i++];
        }
        else {
            temp[k++] = q[j++];
        }
    }
    while (i <= mid) {
        temp[k++] = q[i++];
    }
    while (j <= right) {
        temp[k++] = q[j++];
    }
    
    for (int i = left; i <= right; i++) {
        q[i] = temp[i];
    }
}

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &q[i]);
    }
    
    mergeSort(q, 0, n - 1);
    
    for (int i = 0; i < n; i++) {
        printf("%d ", q[i]);
    }
    
    return 0;
}
```

### [AcWing 788. 逆序对的数量](https://www.acwing.com/problem/content/790/)

```C++
#include<iostream>
using namespace std;

const int N = 1e5 + 10;
int q[N], temp[N];
long long count = 0;

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
            temp[k++] = q[i++];
        }
        else {
            count += mid - i + 1;
            temp[k++] = q[j++];
        }
    }
    while (i <= mid) {
        temp[k++] = q[i++];
    }
    while (j <= right) {
        temp[k++] = q[j++];
    }
    
    for (int i = left; i <= right; i++) {
        q[i] = temp[i];
    }
}

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &q[i]);
    }
    
    mergeSort(q, 0, n - 1);
    
    printf("%ld", count);
    
    return 0;
}
```

## 二分

### 模板

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

### [AcWing 789. 数的范围](https://www.acwing.com/problem/content/791/)

```C++
#include<iostream>
using namespace std;

const int N = 1e5 + 10;
int a[N];

int main() {
    int n, q;
    scanf("%d%d", &n, &q);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    for (int i = 0; i < q; i++) {
        int k;
        scanf("%d", &k);
        
        int left = 0, right = n - 1;
        while (left < right) {
            int mid = left + right >> 1;
            if (a[mid] >= k) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }
        if (a[left] != k) {
            printf("-1 -1\n");
            continue;
        }
        else {
            printf("%d ", left);
        }
        
        left = 0, right = n - 1;
        while (left < right) {
            int mid = left + right + 1 >> 1;
            if (a[mid] <= k) {
                left = mid;
            }
            else {
                right = mid - 1;
            }
        }
        printf("%d\n", left);
    }
    
    
    return 0;
}
```

### [AcWing 790. 数的三次方根](https://www.acwing.com/problem/content/792/)

```C++
#include<iostream>
using namespace std;

const double eps = 1e-7;

int main() {
    double n;
    scanf("%lf", &n);
    
    double left = -100, right = 100;
    while (right - left > eps) {
        double mid = (left + right) / 2;
        if (mid * mid * mid >= n) {
            right = mid;
        }
        else {
            left = mid;
        }
    }
    
    printf("%.6lf", left);
    
    return 0;
}
```

## 高精度

### 模板

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

### [AcWing 791. 高精度加法](https://www.acwing.com/problem/content/793/)

```C++
#include<iostream>
#include<vector>

using namespace std;

vector<int> add(const vector<int>& A, const vector<int>& B) {
    if (A.size() < B.size()) {
        return add(B, A);
    }
    
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i++) {
        t += A[i];
        if (i < B.size()) {
            t+= B[i];
        }
        C.push_back(t % 10);
        t /= 10;
    }
    if (t) {
        C.push_back(t);
    }
    
    return C;
}

int main() {
    string a, b;
    cin >> a >> b;
    
    vector<int> A, B;
    for (int i = a.length() - 1; i >= 0; i--) {
        A.push_back(a[i] - '0');
    }
    for (int i = b.length() - 1; i >= 0; i--) {
        B.push_back(b[i] - '0');
    }
    
    vector<int> C = add(A, B);
    
    for (int i = C.size() - 1; i >= 0; i--) {
        printf("%d", C[i]);
    }
    
    return 0;
}
```

### [AcWing 792. 高精度减法](https://www.acwing.com/problem/content/description/794/)

```C++
#include<iostream>
#include<vector>

using namespace std;

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


int main() {
    string a, b;
    cin >> a >> b;
    
    vector<int> A, B;
    for (int i = a.length() - 1; i >= 0; i--) {
        A.push_back(a[i] - '0');
    }
    for (int i = b.length() - 1; i >= 0; i--) {
        B.push_back(b[i] - '0');
    }
    vector<int> C;
    if (judge(A, B)) {
        C = sub(A, B);
    }
    else {
        printf("-");
        C = sub(B, A);
    }
    
    for (int i = C.size() - 1; i >= 0; i--) {
        printf("%d", C[i]);
    }
    
    return 0;
}
```

### [AcWing 793. 高精度乘法](https://www.acwing.com/problem/content/795/)

```C++
#include<iostream>
#include<vector>

using namespace std;

vector<int> mul(const vector<int>& A, int b) {
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size() || t > 0; i++) {
        if (i < A.size()) {
            t = A[i] * b + t;
        }
        C.push_back(t % 10);
        t /= 10;
    }
    
    while (C.size() > 1 && C.back() == 0) {
        C.pop_back();
    }
    
    return C;
}

int main() {
    string a;
    int b;
    cin >> a >> b;
    
    vector<int> A;
    for (int i = a.length() - 1; i >= 0; i--) {
        A.push_back(a[i] - '0');
    }
    vector<int> C = mul(A, b);
    
    for (int i = C.size() - 1; i >= 0; i--) {
        printf("%d", C[i]);
    }
    
    return 0;
}
```

### [AcWing 794. 高精度除法](https://www.acwing.com/problem/content/796/)

```C++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

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

int main() {
    string a;
    int b;
    cin >> a >> b;
    
    vector<int> A;
    for (int i = a.length() - 1; i >= 0; i--) {
        A.push_back(a[i] - '0');
    }
    int r;
    vector<int> C = div(A, b, r);
    
    for (int i = C.size() - 1; i >= 0; i--) {
        printf("%d", C[i]);
    }
    printf("\n%d", r);
    
    return 0;
}
```

## 前缀与差分

### 模板

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

### [AcWing 795. 前缀和](https://www.acwing.com/problem/content/797/)

```c++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;
int a[N], s[N];

int main() {
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
        s[i] = s[i - 1] + a[i];
    }
    
    for (int i = 0; i < m; i++) {
        int left, right;
        scanf("%d%d", &left, &right);
        printf("%d\n", s[right] - s[left - 1]);
    }
    
    return 0;
}
```

### [AcWing 796. 子矩阵的和](https://www.acwing.com/problem/content/798/)

```C++
#include<iostream>

using namespace std;

const int N = 1e3 + 10;
int a[N][N], s[N][N];

int main() {
    int n, m, q;
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            scanf("%d", &a[i][j]);
            s[i][j] = a[i][j] + s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];
        }
    }
    
    for (int i = 0; i < q; i++) {
        int x1, y1, x2, y2;
        scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
        printf("%d\n", s[x2][y2] - s[x2][y1 - 1] - s[x1 - 1][y2] + s[x1 - 1][y1 - 1]);
    }
    
    return 0;
}
```

### [AcWing 797. 差分](https://www.acwing.com/problem/content/799/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int a[N], b[N];

int main() {
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &a[i]);
        b[i] = a[i] - a[i - 1];
    }
    
    for (int i = 0; i < m; i++) {
        int l, r, c;
        scanf("%d%d%d", &l, &r, &c);
        b[l] += c;
        b[r + 1] -= c;
    }
    
    for (int i = 1; i <= n; i++) {
        a[i] = a[i - 1] + b[i];
        printf("%d ", a[i]);
    }
    
    return 0;
}
```

### [AcWing 798. 差分矩阵](https://www.acwing.com/problem/content/800/)

```C++
#include<iostream>

using namespace std;

const int N = 1e3 + 10;

int a[N][N], b[N][N];

int main() {
    int n, m, q;
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            scanf("%d", &a[i][j]);
            b[i][j] = a[i][j] - a[i - 1][j] - a[i][j - 1] + a[i - 1][j - 1];
        }
    }
    for (int i = 0; i < q; i++) {
        int x1, y1, x2, y2, c;
        scanf("%d%d%d%d%d", &x1, &y1, &x2, &y2, &c);
        b[x1][y1] += c;
        b[x2 + 1][y1] -= c;
        b[x1][y2 + 1] -= c;
        b[x2 + 1][y2 + 1] += c;
    }
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            a[i][j] = b[i][j] + a[i - 1][j] + a[i][j - 1] - a[i - 1][j - 1];
            printf("%d ", a[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```

## 双指针算法

### [AcWing 799. 最长连续不重复子序列](https://www.acwing.com/problem/content/801/)

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 1e5 + 10;

int a[N], Count[N];

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    
    int i, j, result = 0;
    for (i = 0, j = 0; i < n; i++) {
        while (j < n && Count[a[j]] == 0) {
            Count[a[j]]++;
            j++;
            result = max(result, j - i);
        }
        Count[a[i]]--;
    }
    
    printf("%d", result);
    
    return 0;
}
```

### AcWing 800. 数组元素的目标和

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;
int a[N], b[N];

int main() {
    int n, m, x;
    scanf("%d%d%d",&n, &m, &x);
    
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    for (int j = 0; j < m; j++) {
        scanf("%d", &b[j]);
    }
    
    int i, j;
    for (i = 0, j = m - 1; i < n; i++) {
        while (a[i] + b[j] > x) {
            j--;
        }
        if (a[i] + b[j] == x) {
            break;
        }
    }
    
    printf("%d %d", i, j);
    
    return 0;
}
```

### [AcWing 2816. 判断子序列](https://www.acwing.com/problem/content/2818/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;
int a[N], b[N];

int main() {
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    for (int j = 0; j < m; j++) {
        scanf("%d", &b[j]);
    }
    
    int i, j;
    for (i = 0, j = 0; i < n && j < m; j++) {
        if (a[i] == b[j]) {
            i++;
        }
    }
    
    if (i == n) {
        printf("Yes");
    }
    else {
        printf("No");
    }
    
    return 0;
}
```

## 位运算

### 模板

```C++
求n的第k位数字: n >> k & 1
返回n的最后一位1：lowbit(n) = n & -n
```

### [AcWing 801. 二进制中1的个数](https://www.acwing.com/problem/content/description/803/)

```C++
#include<iostream>

const int N = 1e5 + 10;

int a[N];

int main() {
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);
    }
    
    for (int i = 0; i < n; i++) {
        int result = 0;
        for (int j = 0; j < 32; j++) {
            result += a[i] >> j & 1;
        }
        printf("%d ", result);
    }
    
    return 0;
}
```

## 离散化

### 模板

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
    return left + 1; // 映射到1, 2, ...n
}
```

### [AcWing 802. 区间和](https://www.acwing.com/problem/content/description/804/)

```C++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

typedef pair<int, int> PII;
const int N = 3e5 + 10;

vector<PII> add, query;
vector<int> alls;

int a[N], s[N];

int find(int x) {
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
    return left + 1;
}

int main() {
    int n, m;
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i++) {
        int x, c;
        scanf("%d%d", &x, &c);
        add.push_back({x, c});
        alls.push_back(x);
    }
    for (int i = 0; i < m; i++) {
        int l, r;
        scanf("%d%d", &l, &r);
        query.push_back({l, r});
        alls.push_back(l);
        alls.push_back(r);
    }
    
    sort(alls.begin(), alls.end());
    alls.erase(unique(alls.begin(), alls.end()), alls.end());
    
    for (auto item: add) {
        int x = item.first;
        a[find(x)] += item.second;
    }
    
    for (int i = 1; i <= alls.size(); i++) {
        s[i] = s[i - 1] + a[i];
    }
    
    for (auto item: query) {
        int left = item.first, right = item.second;
        printf("%d\n", s[find(right)] - s[find(left) - 1]);
    }
    
    return 0;
}
```

## 区间合并

### 模板

```C++
// 将所有存在交集的区间合并
void merge(vector<PII> &segs) {
    vector<PII> res;

    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs) {
        if (ed < seg.first) {
            if (st != -2e9) { //最开始还未形成区间，只更新区间，不加入结果
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

### [AcWing 803. 区间合并](https://www.acwing.com/problem/content/805/)

```C++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

typedef pair<int, int> PII;

int main() {
    int n;
    scanf("%d", &n);
    vector<PII> segs;
    for (int i = 0; i < n; i++) {
        int l, r;
        scanf("%d%d", &l, &r);
        segs.push_back({l, r});
    }
    
    sort(segs.begin(), segs.end());
    int left = -2e9, right = -2e9, result = 0;
    for (auto seg: segs) {
        if (seg.first > right) {
            if (right != -2e9) {
                result++;
            }
            left = seg.first;
            right = seg.second;
        }
        else {
            right = max(right, seg.second);
        }
    }
    if (right != -2e9) {
        result++;
    }
    
    printf("%d", result);
    return 0;
}
```

# 第二讲 数据结构

## 单链表

### 模板

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

### ==[AcWing 826. 单链表](https://www.acwing.com/problem/content/828/)==

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int head, e[N], ne[N], idex;

void init() {
    head = -1;
    idex = 0;
}

void insert(int x) {
    e[idex] = x, ne[idex] = head, head = idex++;
}

void delete_k(int k) {
    if (k == 0) {
        head = ne[head];
    }
    else {
        ne[k - 1] = ne[ne[k - 1]];
    }
}

void insert_k(int k, int x) {
    e[idex] = x, ne[idex] = ne[k - 1], ne[k - 1] = idex++;
}

int main() {
    int m;
    init();
    cin >> m; //本题不适合用scanf
    for (int i = 0; i < m; i++) {
        char c;
        int k, x;
        cin >> c;
        if (c == 'H') {
            cin >> x;
            insert(x);
        }
        else if (c == 'I') {
            cin >> k >> x;
            insert_k(k, x);
        }
        else {
            cin >> k;
            delete_k(k);
        }
    }
    
    for (int i = head; i != -1; i = ne[i]) {
        cout << e[i] << " ";
    }
    
    return 0;
}
```

## 双链表

### 模板

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

### [AcWing 827. 双链表](https://www.acwing.com/problem/content/829/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int e[N], l[N], r[N], index;

void init() {
    r[0] = 1, l[1] = 0;
    index = 2;
}

void insert(int k, int x) {
    e[index] = x;
    l[index] = k, r[index] = r[k];
    l[r[k]] = index, r[k] = index++;
}

void remove(int k) {
    l[r[k]] = l[k];
    r[l[k]] = r[k];
}

int main() {
    int m;
    cin >> m;
    init();
    for (int i = 0; i < m; i++) {
        string op;
        int k, x;
        cin >> op;
        if (op == "L") {
            cin >> x;
            insert(0, x);
        }
        else if (op == "R") {
            cin >> x;
            insert(l[1], x);
        }
        else if (op == "D") {
            cin >> k;
            remove(k + 1);
        }
        else if (op == "IL") {
            cin >> k >> x;
            insert(l[k + 1], x);
        }
        else {
            cin >> k >> x;
            insert(k + 1, x);
        }
    }
    
    for (int i = r[0]; i != 1; i = r[i]) {
        cout << e[i] << " ";
    }
    
    return 0;
}
```

## 栈

### 模板

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

### [AcWing 828. 模拟栈](https://www.acwing.com/problem/content/830/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int st[N], tt;

int main() {
    int m;
    cin >> m;
    for (int i = 0; i < m; i++) {
        string op;
        int x;
        cin >> op;
        if (op == "push") {
            cin >> x;
            st[++tt] = x;
        }
        else if (op == "pop") {
            tt--;
        }
        else if (op == "query") {
            cout << st[tt] << endl;
        }
        else {
            if (tt > 0) {
                cout << "NO" << endl;
            }
            else {
                cout << "YES" << endl;
            }
        }
    }
    
    return 0;
}
```

### ==[AcWing 3302. 表达式求值](https://www.acwing.com/problem/content/3305/)==

```C++
// 中缀表达式求值
#include<iostream>
#include<stack>
#include<unordered_map>

using namespace std;

stack<int> num;
stack<char> op;

void eval() {
    int b = num.top();
    num.pop();
    int a = num.top();
    num.pop();
    char c = op.top();
    op.pop();
    int x;
    if (c == '+') {
        x = a + b;
    }
    else if (c == '-') {
        x = a - b;
    }
    else if (c == '*') {
        x = a * b;
    }
    else {
        x = a / b;
    }
    num.push(x);
}

int main() {
    string str;
    cin >> str;
    
    unordered_map<char, int> pr{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}};
    for (int i = 0; i < str.length(); i++) {
        if (str[i] >= '0' && str[i] <= '9') {
            int j = i, x = 0;
            while (j < str.length() && str[j] >= '0' && str[j] <= '9') {
                x = x * 10 + str[j] - '0';
                j++;
            }
            i = j - 1;
            num.push(x);
        }
        else if (str[i] == '(') {
            op.push('(');
        }
        else if (str[i] == ')') {
            while (op.top() != '(') {
                eval();
            }
            op.pop();
        }
        else {
            while (!op.empty() && op.top() != '(' && pr[op.top()] >= pr[str[i]]) {
                eval();
            }
            op.push(str[i]);
        }
    }
    while (!op.empty()) {
        eval();
    }
    
    cout << num.top() << endl;
    
    return 0;
}
```

## 队列

### 模板

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

### [AcWing 829. 模拟队列](https://www.acwing.com/problem/content/831/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int que[N], hh = 0, tt = 0;

int main() {
    int m;
    cin >> m;
    for (int i = 0; i < m; i++) {
        string op;
        int x;
        cin >> op;
        if (op == "push") {
            cin >> x;
            que[tt++] = x;
        }
        else if (op == "pop") {
            hh++;
        }
        else if (op == "empty") {
            if (hh == tt) {
                cout << "YES" << endl;
            }
            else {
                cout << "NO" << endl;
            }
        }
        else {
            cout << que[hh] << endl;
        }
    }
    return 0;
}
```

## 单调栈

### 模板

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

### [AcWing 830. 单调栈](https://www.acwing.com/problem/content/832/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int st[N], tt = 0;

int main() {
    int n;
    cin >> n;
    st[++tt] = -1;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        while (tt > 1 && x <= st[tt]) {
            tt--;
        }
        cout << st[tt] << " ";
        st[++tt] = x;
    }
    
    return 0;
}
```

## 单调队列

### 模板

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

### [AcWing 154. 滑动窗口](https://www.acwing.com/problem/content/description/156/)

```C++
#include<iostream>

using namespace std;

const int N = 1e6 + 10;

int a[N], q[N], hh = 0, tt = -1;

int main() {
    int n, k;
    cin >> n >> k;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    
    for (int i = 0; i < n; i++) {
        if (hh <= tt && i - q[hh] >= k) {
            hh++;
        }
        while (hh <= tt && a[q[tt]] >= a[i]) {
            tt--;
        }
        q[++tt] = i;
        if (i - k + 1 >= 0) {
            cout << a[q[hh]] << " ";
        }
    }
    cout << endl;
    
    for (int i = 0; i < n; i++) {
        q[i] = 0;
    }
    hh = 0, tt = -1;
    for (int i = 0; i < n; i++) {
        if (hh <= tt && i - q[hh] >= k) {
            hh++;
        }
        while (hh <= tt && a[q[tt]] <= a[i]) {
            tt--;
        }
        q[++tt] = i;
        if (i - k + 1 >= 0) {
            cout << a[q[hh]] << " ";
        }
    }
    
    return 0;
}
```

## KMP

### 模板

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

### [AcWing 831. KMP字符串](https://www.acwing.com/problem/content/833/)

```C++
#include<iostream>

using namespace std;

const int N = 1e6 + 10;

char p[N], s[N];
int ne[N];

int main() {
    int n, m;
    cin >> n >> p + 1 >> m >> s + 1;
    
    for (int i = 2, j = 0; i <= n; i++) {
        while (j && p[i] != p[j + 1]) {
            j = ne[j];
        }
        if (p[i] == p[j + 1]) {
            j++;
        }
        ne[i] = j;
    }
    for (int i = 1, j = 0; i <= m; i++) {
        while (j && s[i] != p[j + 1]) {
            j = ne[j];
        }
        if (s[i] == p[j + 1]) {
            j++;
        }
        if (j == n) {
            cout << i - n << " ";
            j = ne[j];
        }
    }
    
    return 0;
}
```

## Trie

### 模板

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

### [AcWing 835. Trie字符串统计](https://www.acwing.com/problem/content/837/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int son[N][16], cnt[N], index = 0;
char str[N];

void insert() {
    int p = 0;
    for (int i = 0; str[i]; i++) {
        int u = str[i] - 'a';
        if (!son[p][u]) {
            son[p][u] = ++index;
        }
        p = son[p][u];
    }
    cnt[p]++;
}

int query() {
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

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        char op;
        cin >> op >> str;
        if (op == 'I') {
            insert();
        }
        else {
            cout << query() << endl;
        }
    }
    
    return 0;
}
```

### [AcWing 143. 最大异或对](https://www.acwing.com/problem/content/145/)

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 1e5 + 10, M = 31 * N;

int son[M][2], index = 0;

void insert(int x) {
    int p = 0;
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        if (!son[p][u]) {
            son[p][u] = ++index;
        }
        p = son[p][u];
    }
}

int query(int x) {
    int p = 0, result = 0;
    for (int i = 30; i >= 0; i--) {
        int u = x >> i & 1;
        if (son[p][!u]) {
            result = result * 2 + !u;
            p = son[p][!u];
        }
        else {
            result = result * 2 + u;
            p = son[p][u];
        }
    }
    return result;
}

int main() {
    int n, x, result = 0;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> x;
        insert(x);
        int t = query(x);
        result = max(result, t ^ x);
    }
    
    cout << result << endl;
    
    return 0;
}
```

## 并查集

### 模板

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

### [AcWing 836. 合并集合](https://www.acwing.com/problem/content/838/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int p[N];

void init(int n) {
    for (int i = 1; i <= n; i++) {
        p[i] = i;
    }
}

int find(int x) {
    if (x != p[x]) {
        p[x] = find(p[x]);
    }
    return p[x];
}

int main() {
    int n, m;
    cin >> n >> m;
    init(n);
    for (int i = 0; i < m; i++) {
        char c;
        int a, b;
        cin >> c >> a >> b;
        if (c == 'M') {
            p[find(a)] = find(b);
        }
        else {
            if (find(a) == find(b)) {
                cout << "Yes" << endl;
            }
            else {
                cout << "No" << endl;
            }
        }
    }
    
    return 0;
}
```

### [AcWing 837. 连通块中点的数量](https://www.acwing.com/problem/content/839/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int p[N], cnt[N];

void init(int n) {
    for (int i = 1; i <= n; i++) {
        p[i] = i;
        cnt[i] = 1;
    }
}

int find(int x) {
    if (p[x] != x) {
        p[x] = find(p[x]);
    }
    return p[x];
}

int main() {
    int n, m;
    cin >> n >> m;
    init(n);
    for (int i = 0; i < m; i++) {
        string op;
        int a, b;
        cin >> op;
        if (op == "C") {
            cin >> a >> b;
            int pa = find(a), pb = find(b);
            if (pa != pb) {
                cnt[pb] += cnt[pa];
                p[pa] = pb;
            }
        }
        else if (op == "Q1") {
            cin >> a >> b;
            if (find(a) == find(b)) {
                cout << "Yes" << endl;
            }
            else {
                cout << "No" << endl;
            }
        }
        else {
            cin >> a;
            cout << cnt[find(a)] << endl;
        }
    }
    
    return 0;
}
```

### [AcWing 240. 食物链](https://www.acwing.com/problem/content/242/)

```C++
#include<iostream>

using namespace std;

const int N = 5e4 + 10;

int p[N], d[N];

void init(int n) {
    for (int i = 1; i <= n; i++) {
        p[i] = i;
    }
}

int find(int x) {
    if (x != p[x]) {
        int t = find(p[x]);
        d[x] += d[p[x]];
        p[x] = t;
    }
    return p[x];
}

int main() {
    int n, k;
    cin >> n >> k;
    init(n);
    int result = 0;
    for (int i = 0; i < k; i++) {
        int c, x, y;
        cin >> c >> x >> y;
        if (x > n || y > n) {
            result++;
            continue;
        }
        int px = find(x), py = find(y);
        if (c == 1) {
            if (px == py && (d[x] - d[y]) % 3) {
                result++;
            }
            else if (px != py) {
                p[px] = py;
                d[px] += d[y] - d[x];
            }
        }
        else {
            if (px == py && (d[x] - d[y] - 1) % 3) {
                result++;
            }
            else if (px != py) {
                p[px] = py;
                d[px] += d[y] + 1 - d[x];
            }
        }
    }
    
    cout << result << endl;
    
    return 0;
}
```

## 堆

### 模板

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

### [AcWing 838. 堆排序](https://www.acwing.com/problem/content/840/)

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 1e5 + 10;

int h[N], Size;

void down(int u) {
    int t = u;
    if (2 * u <= Size && h[2 * u] < h[t]) {
        t = 2 * u;
    }
    if (2 * u + 1 <= Size && h[2 * u + 1] < h[t]) {
        t = 2 * u + 1;
    }
    if (t != u) {
        swap(h[t], h[u]);
        down(t);
    }
}

int main() {
    int n, m;
    cin >> n >> m;
    Size = n;
    for (int i = 1; i <= n; i++) {
        cin >> h[i];
    }
    for (int i = n / 2; i > 0; i--) {
        down(i);
    }
    
    for (int i = 0; i < m; i++) {
        cout << h[1] << " ";
        swap(h[1], h[Size]);
        Size--;
        down(1);
    }
    
    return 0;
}
```

### [AcWing 839. 模拟堆](https://www.acwing.com/problem/content/841/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int h[N], ph[N], hp[N], Size, index;

void swap_heap(int a, int b) {
    swap(ph[hp[a]], ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(h[a], h[b]);
}

void down(int u) {
    int t = u;
    if (u * 2 <= Size && h[u * 2] < h[t]) {
        t = u * 2;
    }
    if (u * 2 + 1 <= Size && h[u * 2 + 1] < h[t]) {
        t = u * 2 + 1;
    }
    if (t != u) {
        swap_heap(t, u);
        down(t);
    }
}

void up(int u) {
    while (u / 2 && h[u / 2] > h[u]) {
        swap_heap(u / 2, u);
        u >>= 1;
    }
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        string op;
        cin >> op;
        int k, x;
        if (op == "I") {
            cin >> x;
            h[++Size] = x;
            ph[++index] = Size;
            hp[Size] = index;
            up(Size);
        }
        else if (op == "PM") {
            cout << h[1] << endl;
        }
        else if (op == "DM") {
            swap_heap(1, Size);
            Size--;
            down(1);
        }
        else if (op == "D") {
            cin >> k;
            k = ph[k];
            swap_heap(k, Size);
            Size--;
            up(k), down(k);
        }
        else {
            cin >> k >> x;
            k = ph[k];
            h[k] = x;
            up(k), down(k);
        }
    }
    return 0;
}
```

## 哈希表

### 模板

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

### [AcWing 840. 模拟散列表](https://www.acwing.com/problem/content/842/)

```C++
/* 拉链法 */
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int h[N], e[N], ne[N], index;

void init(int n) {
    for (int i = 0; i < N; i++) {
        h[i] = -1;
    }
}

void insert(int x) {
    int k = (x % N + N) % N;
    e[index] = x;
    ne[index] = h[k];
    h[k] = index++;
}

bool query(int x) {
    int k = (x % N + N) % N;
    for (int i = h[k]; i != -1; i = ne[i]) {
        if (e[i] == x) {
            return true;
        }
    }
    return false;
}

int main() {
    int n;
    cin >> n;
    init(n);
    for (int i = 0; i < n; i++) {
        char c;
        int x;
        cin >> c >> x;
        if (c == 'I') {
            insert(x);
        }
        else {
            if (query(x)) {
                cout << "Yes" << endl;
            }
            else {
                cout << "No" << endl;
            }
        }
    }
    
    return 0;
}
```

```C++
/* 开放寻址法 */
#include<iostream>
#include<cstring>

using namespace std;

const int N = 2e5 + 10, INF = 0x3f3f3f3f;

int h[N];

int find(int x) {
    int k = (x % N + N) % N;
    while (h[k] != INF && h[k] != x) {
        k++;
        if (k == N) {
            k = 0;
        }
    }
    return k;
    
}

int main() {
    int n;
    cin >> n;
    memset(h, 0x3f, sizeof(h));
    for (int i = 0; i < n; i++) {
        char c;
        int x;
        cin >> c >> x;
        if (c == 'I') {
            h[find(x)] = x;
        }
        else {
            if (h[find(x)] == x) {
                cout << "Yes" << endl;
            }
            else {
                cout << "No" << endl;
            }
        }
    }
    
    return 0;
}
```

### [AcWing 841. 字符串哈希](https://www.acwing.com/problem/content/843/)

```C++
#include<iostream>

using namespace std;

typedef unsigned long long ULL;
const int N = 1e5 + 10, P = 13331;

int h[N], p[N];
char s[N];

ULL get(int l, int r) {
    return h[r] - h[l - 1] * p[r - l + 1];
}

int main() {
    int n, m;
    cin >> n >> m >> s + 1;
    p[0] = 1;
    for (int i = 1; i <= n; i++) {
        h[i] = h[i - 1] * P + s[i];
        p[i] = p[i - 1] * P;
    }
    for (int i = 0; i < m; i++) {
        int l1, r1, l2, r2;
        cin >> l1 >> r1 >> l2 >> r2;
        ULL h1 = get(l1, r1), h2 = get(l2, r2);
        if (h1 == h2) {
            cout << "Yes" << endl;
        }
        else {
            cout << "No" << endl;
        }
    }
    
    return 0;
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

# 第三讲 搜索与图论

## 模板





























# 第六讲 贪心

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
