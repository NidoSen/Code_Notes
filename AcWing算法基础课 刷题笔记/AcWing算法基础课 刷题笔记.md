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

高精度压位的做法：

```C++
#include <iostream>
#include <vector>

using namespace std;

const int base = 1000000000;

vector<int> add(vector<int> &A, vector<int> &B) {
    if (A.size() < B.size()) {
        return add(B, A);
    }

    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i++){
        t += A[i];
        if (i < B.size()) {
            t += B[i];
        }
        C.push_back(t % base);
        t /= base;
    }

    if (t) {
        C.push_back(t);
    }
    return C;
}

int main() {
    string a, b;
    vector<int> A, B;
    cin >> a >> b;

    for (int i = a.size() - 1, s = 0, j = 0, t = 1; i >= 0; i--) {
        s += (a[i] - '0') * t;
        j++;
        t *= 10;
        if (j == 9 || i == 0) {
            A.push_back(s);
            s = j = 0;
            t = 1;
        }
    }
    for (int i = b.size() - 1, s = 0, j = 0, t = 1; i >= 0; i--) {
        s += (b[i] - '0') * t;
        j ++, t *= 10;
        if (j == 9 || i == 0) {
            B.push_back(s);
            s = j = 0;
            t = 1;
        }
    }

    auto C = add(A, B);

    cout << C.back();
    for (int i = C.size() - 2; i >= 0; i--) {
        printf("%09d", C[i]);
    }
    cout << endl;

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

## DFS

### [AcWing 842. 排列数字](https://www.acwing.com/problem/content/844/)

```C++
#include<iostream>

using namespace std;

const int N = 10;
int st[N], path[N];

void backtracking(int index, int n) {
    if (index == n + 1) {
        for (int i = 1; i <= n; i++) {
            cout << path[i] << " ";
        }
        cout << endl;
        return;
    }
    
    for (int i = 1; i <= n; i++) {
        if (st[i] == true) {
            continue;
        }
        st[i] = true;
        path[index] = i;
        backtracking(index + 1, n);
        st[i] = false;
    }
}

int main() {
    int n;
    cin >> n;
    
    backtracking(1, n);
    
    return 0;
}
```

### [AcWing 843. n-皇后问题](https://www.acwing.com/problem/content/description/845/)

```C++
#include<iostream>

using namespace std;

const int N = 10;
int st[N], path[N];

void backtracking(int index, int n) {
    if (index == n + 1) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (j == path[i]) {
                    cout << 'Q';
                }
                else {
                    cout << '.';
                }
            }
            cout << endl;
        }
        cout << endl;
        return;
    }
    for (int i = 1; i <= n; i++) {
        if (st[i]) {
            continue;
        }
        int j;
        for (j = 1; j < index; j++) {
            if (index - j == i - path[j] || index - j == path[j] - i ) {
                break;
            }
        }
        if (j < index) {
            continue;
        }
        st[i] = true;
        path[index] = i;
        backtracking(index + 1, n);
        st[i] = false;
    }
}

int main() {
    int n;
    cin >> n;
    
    backtracking(1, n);
    
    return 0;
}
```

## BFS

### [AcWing 844. 走迷宫](https://www.acwing.com/problem/content/description/846/)

```C++
#include<iostream>
#include<queue>

using namespace std;
typedef pair<int, int> PII;

const int N = 110;
int n, m;
int q[N][N], num[N][N];

int dx[4] = {1, 0, -1, 0};
int dy[4] = {0, 1, 0, -1};

bool judge(int i, int j) {
    if (i <= 0 || i > n || j <= 0 || j > m || q[i][j] == 1) {
        return false;
    } else {
        return true;
    }
}

void BFS() {
    queue<PII> que;
    num[1][1] = 1;
    que.push({1, 1});
    
    while (!que.empty()) {
        PII t = que.front();
        que.pop();
        for (int i = 0; i < 4; i++) {
            int x = t.first + dx[i], y = t.second + dy[i];
            if (judge(x, y) && num[x][y] == 0) {
                num[x][y] = num[t.first][t.second] + 1;
                que.push({x, y});
            }
        }
    }
}

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> q[i][j];
        }
    }
    
    BFS();
    
    cout << num[n][m] - 1 << endl;
    
    return 0;
}
```

### ==[AcWing 845. 八数码](https://www.acwing.com/problem/content/847/)==

```C++
#include<iostream>
#include<queue>
#include<unordered_map>

using namespace std;

int BFS(string start) {
    string end = "12345678x";
    
    queue<string> que;
    unordered_map<string, int> umap;
    que.push(start);
    umap[start] = 0;
    
    int dx[4] = {1, 0, -1, 0};
    int dy[4] = {0, 1, 0, -1};
    
    while (!que.empty()) {
        string cur = que.front();
        if (cur == end) {
            return umap[cur];
        }
        que.pop();
        int index = cur.find('x');
        int row = index / 3, col = index % 3;
        for (int i = 0; i < 4; i++) {
            int x = row + dx[i], y = col + dy[i];
            if (x >= 0 && x < 3 && y >= 0 && y < 3) {
                int distance = umap[cur];
                int newIndex = x * 3 + y;
                swap(cur[index], cur[newIndex]);
                if (umap.count(cur) == 0) { // 注意判断一个元素cur是否在映射集里出现，不是用umap[cur] == 0，而是用umap.count(cur)
                    que.push(cur);
                    umap[cur] = distance + 1;
                }
                swap(cur[index], cur[newIndex]);
            }
        }
    }
    
    return -1;
}

int main() {
    char s[2];
    string start;
    for (int i = 0; i < 9; i++) {
        cin >> s;
        start += s[0];
    }
    
    cout << BFS(start) << endl;
    
    return 0;
}
```

## 树与图的深度优先遍历

### 模板

```C++
树是一种特殊的图，与图的存储方式相同。
对于无向图中的边ab，存储两条有向边a->b, b->a。
因此我们可以只考虑有向图的存储。
    
(1) 邻接矩阵：g[a][b] 存储边a->b
    
(2) 邻接表：
// 对于每个点k，开一个单链表，存储k所有可以走到的点。h[k]存储这个单链表的头结点
const int N = 1e5, M = 2 * N;
int h[N], e[M], ne[M], idx;

// 添加一条边a->b
void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

// 初始化
idx = 0;
memset(h, -1, sizeof h);
```

深度优先遍历：时间复杂度$O(n+m)$，$n$表示点数，$m$表示边数

```C++
int dfs(int u) {
    st[u] = true; // st[u] 表示点u已经被遍历过
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (!st[j]) {
            dfs(j);
        }
    }
}
```

### ==[AcWing 846. 树的重心](https://www.acwing.com/problem/content/848/)==

```C++
#include<iostream>
#include<cstring> //memset函数在这里面
#include<algorithm>

using namespace std;

const int N = 1e5 + 10, M = 2 * N;
int h[N], e[M], ne[M], idx;

int n, st[N], ans = N;

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

int DFS(int u) {
    st[u] = true;
    int nodeNum = 1, res = 0;
    for (int i = h[u]; i != -1; i = ne[i]) {
        int t = e[i];
        if (st[t]) {
            continue;
        }
        st[t] = true;
        int s = DFS(t);
        res = max(res, s);
        nodeNum += s;
    }
    res = max(res, n - nodeNum);
    ans = min(ans, res);
    
    return nodeNum;
}

int main() {
    memset(h, -1, sizeof(h)); // 初始化不能忘记
    
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a, b;
        cin >> a >> b;
        add(a, b);
        add(b, a);
    }
    
    DFS(1);
    cout << ans << endl;
    
    return 0;
}
```

## 树与图的广度优先遍历

### 模板

广度优先遍历：时间复杂度$O(n+m)$，$n$表示点数，$m$表示边数

```C++
queue<int> q;
st[1] = true; // 表示1号点已经被遍历过
q.push(1);

while (!q.empty()) {
    int t = q.front();
    q.pop();

    for (int i = h[t]; i != -1; i = ne[i]) {
        int j = e[i];
        if (!st[j]) {
            st[j] = true; // 表示点j已经被遍历过
            q.push(j);
        }
    }
}
```

### [AcWing 847. 图中点的层次](acwing.com/problem/content/849/)

```C++
#include<iostream>
#include<cstring>
#include<queue>

using namespace std;

const int N = 1e5 + 10;
int h[N], e[N], ne[N], idx;

int n, m, d[N];

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void BFS(int u) {
    queue<int> que;
    que.push(u);
    d[u] = 0;
    
    while (!que.empty()) {
        int t = que.front();
        que.pop();
        for (int i = h[t]; i != -1; i = ne[i]) {
            if (d[e[i]] >= 0) {
                continue;
            }
            que.push(e[i]);
            d[e[i]] = d[t] + 1;
        }
    }
}

int main() {
    memset(h, -1, sizeof(h));
    memset(d, -1, sizeof(d));
    
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        add(a, b);
    }
    
    BFS(1);
    
    cout << d[n] << endl;
    
    return 0;
}
```

## 拓扑排序

### 模板

拓扑排序：时间复杂度$O(n+m)$，$n$表示点数，$m$表示边数

```C++
int q[N], d[N];

bool topsort() {
    int hh = 0, tt = -1;

    // d[i] 存储点i的入度
    for (int i = 1; i <= n; i++) {
        if (d[i] == 0) {
            q[++tt] = i;
        }
    }

    while (hh <= tt) {
        int t = q[hh++];

        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (--d[j] == 0) {
                q[++tt] = j;
            }
        }
    }

    // 如果所有点都入队了，说明存在拓扑序列；否则不存在拓扑序列。
    return tt == n - 1;
}
```

### [AcWing 848. 有向图的拓扑序列](https://www.acwing.com/problem/content/850/)

```C++
#include<iostream>
#include<cstring>

using namespace std;

const int N = 1e5 + 10;
int h[N], e[N], ne[N], idx;
int q[N], d[N];

int n, m;

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

bool topSort() {
    int hh = 0, tt = -1;
    
    for (int i = 1; i <= n; i++) {
        if (d[i] == 0) {
            q[++tt] = i;
        }
    }
    
    while (hh <= tt) {
        int t = q[hh++];
        for (int i = h[t]; i != -1; i = ne[i]) {
            if (--d[e[i]] == 0) {
                q[++tt] = e[i];
            }
        }
    }
    
    return tt == n - 1;
}

int main() {
    memset(h, -1, sizeof(h));
    
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        add(a, b);
        d[b]++;
    }
    
    if (topSort()) {
        for (int i = 0; i < n; i++) {
            cout << q[i] << " ";
        }
    } else {
        cout << -1 << endl;
    }
    
    return 0;
}
```

## Dijkstra

### 模板

朴素Dijkstra算法：时间复杂度$O(n^2+m)$，$n$表示点数，$m$表示边数

```C++
int g[N][N];  // 存储每条边，别忘了用memset初始化
int dist[N];  // 存储1号点到每个点的最短距离
bool st[N];   // 存储每个点的最短路是否已经确定

// 求1号点到n号点的最短路，如果不存在则返回-1
int dijkstra() {
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0; // 没有st[1] = true;

    for (int i = 0; i < n - 1; i++) {
        int t = -1;     // 在还未确定最短路的点中，寻找距离最小的点
        for (int j = 1; j <= n; j++) {
            if (!st[j] && (t == -1 || dist[t] > dist[j])) {
                t = j;
            }
        }

        // 用t更新其他点的距离
        for (int j = 1; j <= n; j++) {
            dist[j] = min(dist[j], dist[t] + g[t][j]);
        }

        st[t] = true;
    }

    if (dist[n] == 0x3f3f3f3f) {
        return -1;
    } else {
        return dist[n];
    }
}
```

堆优化版Dijkstra算法：时间复杂度$O(m\log n)$，$n$表示点数，$m$表示边数

```C++
typedef pair<int, int> PII;

int n;      // 点的数量
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N];        // 存储所有点到1号点的距离
bool st[N];     // 存储每个点的最短距离是否已确定

// 求1号点到n号点的最短距离，如果不存在，则返回-1
int dijkstra() {
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, 1});      // first存储距离，second存储节点编号

    while (heap.size()) {
        auto t = heap.top();
        heap.pop();

        int ver = t.second, distance = t.first;

        if (st[ver]) {
            continue;
        }
        st[ver] = true;

        for (int i = h[ver]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > distance + w[i]) {
                dist[j] = distance + w[i];
                heap.push({dist[j], j});
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) {
        return -1;
    }
    return {
        dist[n];
    }
}
```

### [AcWing 849. Dijkstra求最短路 I](https://www.acwing.com/problem/content/851/)

```C++
#include<iostream>
#include<cstring>

using namespace std;

const int N = 510;
int g[N][N], dist[N];
bool st[N];

int n, m;

int dijkstra() {
    dist[1] = 0;
    
    for (int i = 0; i < n - 1; i++) {
        int t = -1;
        for (int j = 1; j <= n; j++) {
            if (!st[j] && (t == -1 || dist[j] < dist[t])) {
                t = j;
            }
        }
        
        st[t] = true;
        
        for (int j = 1; j <= n; j++) {
            dist[j] = min(dist[j], dist[t] + g[t][j]);
        }
    }
    
    if (dist[n] == 0x3f3f3f3f) {
        return -1;
    } else {
        return dist[n];
    }
}

int main() {
    memset(g, 0x3f, sizeof(g));
    memset(dist, 0x3f, sizeof(dist));
    
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int x, y, d;
        cin >> x >> y >> d;
        g[x][y] = min(g[x][y], d);
    }
    
    cout << dijkstra() << endl;
    
    return 0;
}
```

### [AcWing 850. Dijkstra求最短路 II](https://www.acwing.com/activity/content/problem/content/919/)

```C++
#include<iostream>
#include<cstring>
#include<queue>

using namespace std;

const int N = 2e5;
typedef pair<int, int> PII;

int h[N], e[N], ne[N], w[N], idx;
int dist[N];
bool st[N];

int n, m;

void add(int a, int b, int d) {
    e[idx] = b, w[idx] = d, ne[idx] = h[a], h[a] = idx++;
}

int dijkstra() {
    memset(dist, 0x3f, sizeof(dist));
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    
    dist[1] = 0;
    heap.push({0, 1});
    
    while (!heap.empty()) {
        PII t = heap.top();
        heap.pop();
        int v = t.second, d = t.first;
        
        if (st[v]) {
            continue;
        }
        st[v] = true;
        
        for (int i = h[v]; i != -1; i = ne[i]) {
            if (dist[e[i]] > d + w[i]) {
                dist[e[i]] = d + w[i];
                heap.push({dist[e[i]], e[i]});
            }
        }
    }
    
    if (dist[n] == 0x3f3f3f3f) {
        return -1;
    } else {
        return dist[n];
    }
}

int main() {
    memset(h, -1, sizeof(h));
    
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int x, y, d;
        cin >> x >> y >> d;
        add(x, y, d);
    }
    
    cout << dijkstra() << endl;
    
    return 0;
}
```

## bellman-ford

### 模板

bellman-ford算法：时间复杂度$O(nm)$，$n$表示点数，$m$表示边数

```C++
int n, m; // n表示点数，m表示边数
int dist[N], backup[N]; // dist[x]存储1到x的最短路距离

struct Edge { // 边，a表示出点，b表示入点，w表示边的权重
    int a, b, w;
}edges[M];

// 求1到n的最短路距离，如果无法从1走到n，则返回-1。
int bellman_ford() {
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    // 如果第n次迭代仍然会松弛三角不等式，就说明存在一条长度是n+1的最短路径，由抽屉原理，路径中至少存在两个相同的点，说明图中存在负权回路。
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            int a = edges[j].a, b = edges[j].b, w = edges[j].w;
            if (dist[b] > dist[a] + w) {
                dist[b] = dist[a] + w;
            }
        }
    }
    
    // 避免串联的做法
    /*
    for (int i = 0; i < n; i++) {
        memcpy(back, dist, sizeof(dist));
        for (int j = 0; j < m; j++) {
            int a = edges[j].a, b = edges[j].b, w = edges[j].w;
            dist[b] = min(dist[b], backup[a] + w)
        }
    }
    */

    if (dist[n] > 0x3f3f3f3f / 2) {
        return -1;
    }
    return {
        dist[n];
    }
}
```

### [AcWing 853. 有边数限制的最短路](https://www.acwing.com/problem/content/description/855/)

```C++
#include<iostream>
#include<cstring>

using namespace std;

const int N = 510, M = 10010;

int dist[N], backup[N];
struct edge {
    int a, b, w;
}edges[M];

int n, m, k;

void bellman_ford() {
    memset(dist, 0x3f, sizeof(dist));
    dist[1] = 0;
    
    for (int i = 0; i < k; i++) {
        memcpy(backup, dist, sizeof(dist));
        for (int j = 0; j < m; j++) {
            int a = edges[j].a, b = edges[j].b, w = edges[j].w;
            dist[b] = min(dist[b], backup[a] + w);
        }
    }
}

int main() {
    cin >> n >> m >> k;
    for (int i = 0; i < m; i++) {
        cin >> edges[i].a >> edges[i].b >> edges[i].w;
    }
    
    bellman_ford();
    
    if (dist[n] > 0x3f3f3f3f / 2) {
        cout << "impossible" << endl;
    } else {
        cout << dist[n] << endl;
    }
    
    return 0;
}
```

## spfa

### 模板

spfa算法：时间复杂度平均情况下$O(m)$，最坏情况下$O(nm)$，$n$表示点数，$m$表示边数

```C++
int n;      // 总点数
int h[N], w[N], e[N], ne[N], idx;       // 邻接表存储所有边
int dist[N];        // 存储每个点到1号点的最短距离
bool st[N];     // 存储每个点是否在队列中

// 求1号点到n号点的最短路距离，如果从1号点无法走到n号点则返回-1
int spfa() {
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    queue<int> q;
    q.push(1);
    st[1] = true;

    while (q.size()) {
        auto t = q.front();
        q.pop();

        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                if (!st[j]) { // 如果队列中已存在j，则不需要将j重复插入
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) {
        return -1;
    } else {
        return dist[n];
    }
}
```





# 第四讲 数学知识

## 质数

### 模板

```C++
// 试除法判定质数
bool is_prime(int x) {
    if (x < 2) {
        return false;
    }
    for (int i = 2; i <= x / i; i++) {
        if (x % i == 0) {
            return false;
        }
    }
    return true;
}
```

```C++
// 试除法分解质因数
void divide(int x) {
    for (int i = 2; i <= x / i; i++) {
        if (x % i == 0) {
            int s = 0;
            while (x % i == 0) {
                x /= i;
                s++;
            }
            cout << i << ' ' << s << endl;
        }
    }
    if (x > 1) {
        cout << x << ' ' << 1 << endl;
    }
    cout << endl;
}
```

```C++
// 朴素筛法求质数
int primes[N], cnt; // primes[]存储所有质数
bool st[N]; // st[x]存储x是否被筛掉

void get_primes(int n) {
    for (int i = 2; i <= n; i++) {
        if (st[i]) {
            continue;
        }
        primes[cnt++] = i;
        for (int j = i + i; j <= n; j += i) {
            st[j] = true;
        }
    }
}
```

```C++
// 线性筛法求质数
int primes[N], cnt; // primes[]存储所有素数
bool st[N]; // st[x]存储x是否被筛掉

void get_primes(int n) {
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            primes[cnt++] = i;
        }
        for (int j = 0; primes[j] <= n / i; j++) {
            st[primes[j] * i] = true; //primes[j] * i一定会被它的最小质因子primes[j]筛掉
            if (i % primes[j] == 0) {
                break;
            }
        }
    }
}
```

### [AcWing 866. 试除法判定质数](https://www.acwing.com/problem/content/868/)

```C++
#include<iostream>

using namespace std;

bool is_prime(int x) {
    if (x < 2) {
        return false;
    }
    for (int i = 2; i <= x / i; i++) {
        if (x % i == 0) {
            return false;
        }
    }
    return true;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        if (is_prime(a)) {
            cout << "Yes" << endl;
        } else {
            cout << "No" << endl;
        }
    }
    
    return 0;
}
```

### [AcWing 867. 分解质因数](https://www.acwing.com/problem/content/869/)

```C++
#include<iostream>

using namespace std;

void divide(int x) {
    for (int i = 2; i <= x / i; i++) {
        if (x % i != 0) {
            continue;
        }
        int s = 0;
        while (x % i == 0) {
            x /= i;
            s++;
        }
        cout << i << " " << s << endl;
    }
    if (x > 1) {
        cout << x << " " << 1 << endl;
    }
    cout << endl;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        divide(a);
    }
    
    return 0;
}
```

### [AcWing 868. 筛质数](https://www.acwing.com/problem/content/870/)

```C++
#include<iostream>

using namespace std;

const int N = 1e6 + 10;

int primes[N], cnt;
bool st[N];
int n;

void get_primes() {
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            primes[cnt++] = i;
        }
        for (int j = 0; primes[j] <= n / i; j++) {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) {
                break;
            }
        }
    }
}

int main() {
    cin >> n;
    
    get_primes();
    
    cout << cnt << endl;
    
    return 0;
}
```

## 约数

### 模板

```C++
// 试除法求所有约数
vector<int> get_divisors(int x) {
    vector<int> res;
    for (int i = 1; i <= x / i; i++) {
        if (x % i == 0) {
            res.push_back(i);
            if (i != x / i) {
                res.push_back(x / i);
            }
        }
    }
    sort(res.begin(), res.end());
    return res;
}
```
```C++
// 约数个数和约数之和
如果 N = p1^c1 * p2^c2 * ... *pk^ck
约数个数： (c1 + 1) * (c2 + 1) * ... * (ck + 1)
约数之和： (p1^0 + p1^1 + ... + p1^c1) * ... * (pk^0 + pk^1 + ... + pk^ck)
```

```C++
// 求约数个数
long long get_divisors_num(int x) {
    unordered_map<int, int> primes;
    for (int i = 2; i <= x / i; i++) {
        while (x % i == 0) {
            x /= i;
            primes[i]++;
        }
    }
    if (x > 1) {
        primes[x]++;
    }
    
    long long res = 1;
    for (auto p: primes) {
        res = res * (p.second + 1) % mod;
    }
    
    return res;
}
```

```C++
// 求约数之和
long long get_divisors_sum(int x) {
    unordered_map<int, int> primes;
    for (int i = 2; i <= x / i; i++) {
        while (x % i == 0) {
            x /= i;
            primes[i]++;
        }
    }
    if (x > 1) {
        primes[x]++;
    }
    
    long long res = 1;
    for (auto p: primes) {
        long long a = p.first, b = p.second, t = 1;
        while (b--) {
            t = (t * a + 1) % mod;
        }
        res = res * t % mod;
    }
    
    return res;
}
```


```C++
// 欧几里得算法
int gcd(int a, int b) {
    return b ? gcd(b, a % b) : a;
}
```

### [AcWing 869. 试除法求约数](https://www.acwing.com/problem/content/871/)

```C++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

vector<int> get_divisors(int x) {
    vector<int> res;
    for (int i = 1; i <= x / i; i++) {
        if (x % i != 0) {
            continue;
        }
        res.push_back(i);
        if (i != x / i) {
            res.push_back(x / i);
        }
    }
    sort(res.begin(), res.end());
    return res;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        vector<int> res = get_divisors(a);
        for (int j = 0; j < res.size(); j++) {
            cout << res[j] << " ";
        }
        cout << endl;
    }
    return 0;
}
```

### [AcWing 870. 约数个数](https://www.acwing.com/problem/content/872/)

```C++
#include<iostream>
#include<unordered_map>

using namespace std;

const int mod = 1e9 + 7;

int main() {
    int n;
    cin >> n;
    
    unordered_map<int, int> primes;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        for (int j = 2; j <= a / j; j++) {
            if (a % i != 0) {
                continue;
            }
            while (a % j == 0) {
                a /= j;
                primes[j]++;
            }
        }
        if (a > 1) {
            primes[a]++;
        }
    }
    
    long long res = 1;
    for (auto p: primes) {
        res = res * (p.second + 1) % mod;
    }
    
    cout << res << endl;
    
    return 0;
}
```

### [AcWing 871. 约数之和](https://www.acwing.com/activity/content/problem/content/940/)

```C++
#include<iostream>
#include<unordered_map>

using namespace std;

const int mod = 1e9 + 7;

int main() {
    int n;
    cin >> n;
    
    unordered_map<int, int> primes;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        for (int j = 2; j <= a / j; j++) {
            if (a % j != 0) {
                continue;
            }
            while (a % j == 0) {
                a /= j;
                primes[j]++;
            }
        }
        if (a > 1) {
            primes[a]++;
        }
    }
    
    long long res = 1;
    for (auto p: primes) {
        long long a = p.first, b = p.second, t = 1;
        while (b--) {
            t = (t * a + 1) % mod;
        }
        res = res * t % mod;
    }
    
    cout << res << endl;
    
    return 0;
}
```

### [AcWing 872. 最大公约数](https://www.acwing.com/problem/content/description/874/)

```C++
#include<iostream>

using namespace std;

int gcd(int a, int b) {
    return b ? gcd(b, a % b) : a;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a, b;
        cin >> a >> b;
        cout << gcd(a, b) << endl;
    }
    
    return 0;
}
```

## 欧拉函数

```C++
// 求欧拉函数
int phi(int x) {
    int res = x;
    for (int i = 2; i <= x / i; i++) {
        if (x % i == 0) {
            res = res / i * (i - 1);
            while (x % i == 0) {
                x /= i;
            }
        }
    }
    if (x > 1) {
        res = res / x * (x - 1);
    }

    return res;
}
```

```C++
int primes[N], cnt; // primes[]存储所有素数
int euler[N]; // 存储每个数的欧拉函数
bool st[N]; // st[x]存储x是否被筛掉

void get_eulers(int n) {
    euler[1] = 1;
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            primes[cnt++] = i;
            euler[i] = i - 1;
        }
        for (int j = 0; primes[j] <= n / i; j++) {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0) {
                euler[t] = euler[i] * primes[j];
                break;
            }
            euler[t] = euler[i] * (primes[j] - 1);
        }
    }
}
```

### [AcWing 873. 欧拉函数](https://www.acwing.com/problem/content/description/875/)

```C++
#include<iostream>

using namespace std;

int phi(int x) {
    int res = x;
    for (int i = 2; i <= x / i; i++) {
        if (x % i == 0) {
            res = res / i * (i - 1);
            while (x % i == 0) {
                x /= i;
            }
        }
    }
    if (x > 1) {
        res = res / x * (x - 1);
    }
    return res;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        cout << phi(a) << endl;
    }
    
    return 0;
}
```

### [AcWing 874. 筛法求欧拉函数](https://www.acwing.com/problem/content/876/)

```C++
#include<iostream>

using namespace std;

const int N = 1e6 + 10;

int primes[N], cnt;
int phi[N];
bool st[N];

void get_eulers(int n) {
    phi[1] = 1;
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            primes[cnt++] = i;
            phi[i] = i - 1;
        }
        for (int j = 0; primes[j] <= n / i; j++) {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0) {
                phi[t] = phi[i] * primes[j];
                break;
            }
            phi[t] = phi[i] * (primes[j] - 1);
        }
    }
}

int main() {
    int n;
    cin >> n;
    
    get_eulers(n);
    long long res = 0;
    for (int i = 1; i <= n; i++) {
        res += phi[i];
    }
    
    cout << res << endl;
    
    return 0;
}
```

## 快速幂

### 模板

```C++
// 求 m^k mod p，时间复杂度 O(logk)。

long long qmi(int m, int k, int p) {
    long res = 1, t = m;
    while (k) {
        if (k & 1) {
            res = res * t % p;
        }
        t = t * t % p;
        k >>= 1;
    }
    return res;
}
```

乘法逆元：若整数 $a$， $m$ 互质，并且对于任意的整数 $n$，如果满足 $a|n$ （含义是 $n$ 能被 $a$ 整除，即存在整数 $k$ 满足 $n=ka$ ），则存在一个整数 $x$，使得 $n/a≡n×x(mod\ m)$，则称 $x$ 为 $a$ 的模 $m$ 乘法逆元，记为 $a^{-1}\ (mod\ m)$。

$a$ 存在乘法逆元的充要条件是 $a$ 与模数 $m$ 互质。

当**模数 $m$ 为质数**时，$a^{m−2}$ 即为 $a$ 的乘法逆元，即 `qmi(a, m - 2, m)`。

当 **$\gcd(a,m)$ 能被 $b$ 整除**时，可使用扩展欧几里得算法得到 $a$ 的逆元 $x$（该逆元满足 $a×x≡b(mod\ m)$），即 `exgcd(a, m, x, y)`（注意这里的 $x$ 和前面不是同一个，因为扩展欧几里得算法求出来的 $x$ 和 $y$ 是方程 $ax+my=\gcd(a,m)$ 的解），函数运行后 $x×b/\gcd(a,m)\%m$ 即为 $a$ 的逆元。

### [AcWing 875. 快速幂](https://www.acwing.com/problem/content/description/877/)

```C++
#include<iostream>

using namespace std;

long long qmi(int m, int k, int p) {
    long long res = 1, t = m;
    while (k) {
        if (k & 1) {
            res = res * t % p;
        }
        t = t * t % p;
        k >>= 1;
    }
    return res;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a, b, p;
        cin >> a >> b >> p;
        cout << qmi(a, b, p) << endl;
    }
    return 0;
}
```

### [AcWing 876. 快速幂求逆元](acwing.com/problem/content/878/)

```C++
#include<iostream>

using namespace std;

long long qmi(int m, int k, int p) {
    long long res = 1, t = m;
    while (k) {
        if (k & 1) {
            res = res * t % p;
        }
        t = t * t % p;
        k >>= 1;
    }
    return res;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a, p;
        cin >> a >> p;
        if (a % p == 0) {
            cout <<  "impossible" << endl;
        }
        else {
            cout << qmi(a, p - 2, p) << endl;
        }
    }
    
    return 0;
}
```

## 扩展欧几里得算法

### 模板

```C++
// 求x, y，使得ax + by = gcd(a, b)
int exgcd(int a, int b, int &x, int &y) {
    if (!b) {
        x = 1; y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= (a/b) * x;
    return d;
}
```

### [AcWing 877. 扩展欧几里得算法](https://www.acwing.com/problem/content/879/)

```C++
#include<iostream>

using namespace std;

int exgcd(int a, int b, int& x, int& y) {
    if (b == 0) {
        x = 1, y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return d;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a, b, x, y;
        cin >> a >> b;
        exgcd(a, b, x, y);
        cout << x << " " << y << endl;
    }
    return 0;
}
```

### [AcWing 878. 线性同余方程](https://www.acwing.com/problem/content/880/)

```C++
#include<iostream>

using namespace std;

int exgcd(int a, int b, int& x, int& y) {
    if (b == 0) {
        x = 1, y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return d;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a, b, m, x, y;
        cin >> a >> b >> m;
        int d = exgcd(a, m, x, y);
        if (b % d == 0) {
            cout << (long long)x * b / d % m<< endl;
        } else {
            cout << "impossible" << endl;
        }
    }
    
    return 0;
}
```

## 中国剩余定理

### 模板

中国剩余定理及其解：

问题一般化：假设整数 $m_1,m_2,……,m_n$ 两两互素，则对于任意的整数 $a_1,a_2,……,a_n$ ，方程组
$$
\begin{cases}
x≡a_1 \ (mod\ m_1)\\
x≡a_2 \ (mod\ m_2)\\
……\\
x≡a_n \ (mod\ m_n)
\end{cases}
$$
都存在整数解，且若 $X$， $Y$ 都满足该方程组，则必有 $X≡Y\ (mod\ N)$，其中 $N=\prod^{n}_{i=1}{m_i}$。

具体而言，$x≡\sum^{n}_{i=1}{a_i×\frac{N}{m_i}×[(\frac{N}{m_i})^{-1}]_{m_i}}(mod\ N)$。

### [AcWing 204. 表达整数的奇怪方式](https://www.acwing.com/problem/content/206/)

```C++
#include<iostream>

using namespace std;

long long exgcd(long long a, long long b, long long& x, long long& y) {
    if (b == 0) {
        x = 1, y = 0;
        return a;
    }
    long long d = exgcd(b, a % b, y, x);
    y -= -a / b * x;
    return d;
}

int main() {
    int n;
    cin >> n;
    
    long long a1, m1;
    cin >> a1 >> m1;
    bool flag = true;
    for (int i = 0; i < n - 1; i++) {
        long long a2, m2;
        cin >> a2 >> m2;
        long long k1, k2;
        long long d = exgcd(a1, a2, k1, k2);
        if ((m2 - m1) % d != 0) {
            flag = false;
            break;
        }
        k1 *= (m2 - m1) / d; // 更新状态
        k1 = (k1 % (a2 / d) + a2 / d) % (a2 / d); // 将解变成一个最小的正整数解
        m1 = k1 * a1 + m1;
        a1 = abs(a1 / d * a2);
    }
    
    if (flag) {
        cout << (m1 % a1 + a1) % a1 << endl;
    } else {
        cout << -1 << endl;
    }
    
    return 0;
}
```

## 高斯消元

### 模板

```C++
// a[N][N]是增广矩阵
int gauss() {
    int c, r;
    for (c = 0, r = 0; c < n; c++) {
        int t = r;
        for (int i = r; i < n; i++) { // 找到绝对值最大的行
            if (fabs(a[i][c]) > fabs(a[t][c])) {
                t = i;
            }
        }

        if (fabs(a[t][c]) < eps) {
            continue;
        }

        for (int i = c; i <= n; i++) { // 将绝对值最大的行换到最顶端
            swap(a[t][i], a[r][i]);
        }
        for (int i = n; i >= c; i--) { // 将当前行的首位变成1
            a[r][i] /= a[r][c];
        }
        for (int i = r + 1; i < n; i++) { // 用当前行将下面所有的列消成0
            if (fabs(a[i][c]) > eps) {
                for (int j = n; j >= c; j--) {
                    a[i][j] -= a[r][j] * a[i][c];
                }
            }
        }
        
        r++;
    }

    if (r < n) {
        for (int i = r; i < n; i++) {
            if (fabs(a[i][n]) > eps) {
                return 2; // 无解
            }
        }
        return 1; // 有无穷多组解
    }

    for (int i = n - 1; i >= 0; i--) {
        for (int j = i + 1; j < n; j++) {
            a[i][n] -= a[i][j] * a[j][n];
        }
    }

    return 0; // 有唯一解
}
```

### [AcWing 883. 高斯消元解线性方程组](https://www.acwing.com/problem/content/885/)

```C++
#include<iostream>
#include<cmath>

using namespace std;

const int N = 110;
const double eps = 1e-8;

double a[N][N];
int n;

int gauss() {
    int r, c;
    for (r = 0, c = 0; c < n; c++) {
        int t = r;
        for (int i = r; i < n; i++) {
            if (fabs(a[i][c]) > fabs(a[t][c])) {
                t = i;
            }
        }
        
        if (fabs(a[t][c]) < eps) {
            continue;
        }
        
        for (int i = c; i <= n; i++) {
            swap(a[t][i], a[r][i]);
        }
        
        for (int i = n; i >= c; i--) {
            a[r][i] /= a[r][c];
        }
        
        for (int i = r + 1; i < n; i++) {
            if (fabs(a[i][c]) < eps) {
                continue;
            }
            for (int j = n; j >= c; j--) {
                a[i][j] -= a[i][c] * a[r][j];
            }
        }
        
        r++;
    }
    
    if (r < n) {
        for (int i = r; i < n; i++) {
            if (fabs(a[i][n]) > eps) {
                return 2;
            }
        }
        return 1;
    }
    else {
        for (int i = n - 1; i >= 0; i--){
            for (int j = i + 1; j < n; j++) {
                a[i][n] -= a[i][j] * a[j][n];
            }
        }
        return 0;
    }
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= n; j++) {
            cin >> a[i][j];
        }
    }
    
    int res = gauss();
    
    if (res == 0) {
        for (int i = 0; i < n; i++) {
            printf("%.2lf\n", a[i][n]);
        }
    }
    else if (res == 1) {
        cout << "Infinite group solutions" << endl;
    }
    else {
        cout << "No solution" << endl;
    }
    return 0;
}
```

### [AcWing 884. 高斯消元解异或线性方程组](https://www.acwing.com/problem/content/886/)

```C++
#include<iostream>

using namespace std;

const int N = 110;

int m[N][N], n;

int gauss() {
    int r, c;
    for (r = 0, c = 0; c < n; c++) {
        int t = r;
        for (int i = r; i < n; i++) {
            if (m[i][c] == 1) {
                t = i;
                break;
            }
        }
        
        if (m[t][c] == 0) {
            continue;
        }
        
        for (int i = c; i <= n; i++) {
            swap(m[r][i], m[t][i]);
        }
        
        for (int i = r + 1; i < n; i++) {
            if (m[i][c] == 0) {
                continue;
            }
            for (int j = c; j <= n; j++) {
                m[i][j] ^= m[r][j];
            }
        }
        
        r++;
    }
    
    if (r < n) {
        for (int i = r; i < n; i++) {
            if (m[i][n] == 1) {
                return 2;
            }
        }
        return 1;
    }
    else {
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                m[i][n] ^= m[i][j] * m[j][n];
            }
        }
        return 0;
    }
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= n; j++) {
            cin >> m[i][j];
        }
    }
    
    int res = gauss();
    
    if (res == 0) {
        for (int i = 0; i < n; i++) {
            cout << m[i][n] << endl;
        }
    }
    else if (res == 1) {
        cout << "Multiple sets of solutions" << endl;
    }
    else {
        cout << "No solution" << endl;
    }
    return 0;
}
```

## 求组合数

### 模板

```C++
// 递推法求组合数
// c[a][b] 表示从a个苹果中选b个的方案数
for (int i = 0; i < N; i++) {
    for (int j = 0; j <= i; j++) {
        if (j == 0) {
            c[i][j] = 1;
        }
        else {
            c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;
        }
    }
}
```

```C++
// 通过预处理逆元的方式求组合数
// 首先预处理出所有阶乘取模的余数fact[N]，以及所有阶乘取模的逆元infact[N]
// 如果取模的数是质数，可以用费马小定理求逆元
long long qmi(int a, int k, int p) { // 快速幂模板
    long long res = 1, t = a;
    while (k) {
        if (k & 1) {
            res = res * a % p;
        }
        t = t * t % p;
        k >>= 1;
    }
    return res;
}

// 预处理阶乘的余数和阶乘逆元的余数
fact[0] = infact[0] = 1;
for (int i = 1; i < N; i ++ ) {
    fact[i] = fact[i - 1] * i % mod;
    infact[i] = infact[i - 1] * qmi(i, mod - 2, mod) % mod;
}
```

```C++
// Lucas定理
// 若p是质数，则对于任意整数 1 <= m <= n，有：
//  C(n, m) = C(n % p, m % p) * C(n / p, m / p) (mod p)
long long qmi(int a, int k, int p) { // 快速幂模板
    long long res = 1, t = a;
    while (k) {
        if (k & 1) {
            res = res * a % p;
        }
        t = t * t % p;
        k >>= 1;
    }
    return res;
}

long long C(long long a, long long b, int p) { // 通过定理求组合数C(a, b)
    if (a < b) {
        return 0;
    }

    long long x = 1, y = 1;  // x是分子，y是分母
    for (long long i = a, j = 1; j <= b; i--, j++) {
        x = x * i % p;
        y = y * j % p;
    }

    return x * qmi(y, p - 2, p) % p;
}

long long lucas(long long a, long long b, int p) {
    if (a < p && b < p) {
        return C(a, b, p);
    }
    return C(a % p, b % p, p) * lucas(a / p, b / p, p) % p;
}
```

```C++
// 分解质因数法求组合数
// 当我们需要求出组合数的真实值，而非对某个数的余数时，分解质因数的方式比较好用：
//    1. 筛法求出范围内的所有质数
//    2. 通过 C(a, b) = a! / b! / (a - b)! 这个公式求出每个质因子的次数。 n!中p的次数是 n / p + n / p^2 + n / p^3 + ...
//    3. 用高精度乘法将所有质因子相乘
int primes[N], cnt; // 存储所有质数
int sum[N]; // 存储每个质数的次数
bool st[N]; // 存储每个数是否已被筛掉

void get_primes(int n) { // 线性筛法求素数
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            primes[cnt++] = i;
        }
        for (int j = 0; primes[j] <= n / i; j++) {
            st[primes[j] * i] = true;
            if (i % primes[j] == 0) {
                break;
            }
        }
    }
}

int get(int n, int p) { // 求n！中的次数
    int res = 0;
    while (n) {
        res += n / p;
        n /= p;
    }
    return res;
}

vector<int> mul(vector<int> a, int b) { // 高精度乘低精度模板
    vector<int> c;
    int t = 0;
    for (int i = 0; i < a.size(); i++) {
        t += a[i] * b;
        c.push_back(t % 10);
        t /= 10;
    }
    
    while (t) {
        c.push_back(t % 10);
        t /= 10;
    }
    
    return c;
}

get_primes(a); // 预处理范围内的所有质数

for (int i = 0; i < cnt; i++) { // 求每个质因数的次数
    int p = primes[i];
    sum[i] = get(a, p) - get(b, p) - get(a - b, p);
}

vector<int> res;
res.push_back(1);

for (int i = 0; i < cnt; i++) { // 用高精度乘法将所有质因子相乘
    for (int j = 0; j < sum[i]; j++) {
        res = mul(res, primes[i]);
    }
}
```

求组合数的策略：

|      | n范围 | a，b范围                 | 方法       | 时间复杂度         |
| ---- | ----- | ------------------------ | ---------- | ------------------ |
| 1    | 10万  | $1\le b\le a\le 2000$    | 递推       | $O(n^2)$           |
| 2    | 1万   | $1\le b\le a\le 10^5$    | 预处理     | $O(n\log n)$       |
| 3    | 20    | $1\le b\le a\le 10^{18}$ | Lucas定理  | $O(P\log n\log P)$ |
| 4    | 10万  | $1\le b\le a\le 2000$    | 分解质因数 |                    |


卡特兰数：给定 $n$ 个 $0$ 和 $n$ 个 $1$，它们按照某种顺序排成长度为 $2n$ 的序列，满足任意前缀中 $0$ 的个数都不少于 $1$ 的个数的序列的数量为 $Cat(n) = \frac{1}{n+1}C^n_{2n}$。

### [AcWing 885. 求组合数 I](https://www.acwing.com/problem/content/887/)

```C++
#include<iostream>

using namespace std;

const int N = 2010, mod = 1e9 + 7;
int C[N][N];

int main() {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j <= i; j++) {
            if (j == 0) {
                C[i][j] = 1;
            }
            else {
                C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % mod;
            }
        }
    }
    
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a, b;
        cin >> a >> b;
        cout << C[a][b] << endl;
    }
    
    return 0;
}
```

### [AcWing 886. 求组合数 II](https://www.acwing.com/problem/content/888/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10, mod = 1e9 + 7;

long long fact[N], infact[N];

long long qmi(int a, int k, int p) {
    long long res = 1, t = a;
    while (k) {
        if (k & 1) {
            res = res * t % p;
        }
        t = t * t % p;
        k >>= 1;
    }
    return res;
}

int main() {
    fact[0] = infact[0] = 1;
    for (int i = 1; i < N; i++) {
        fact[i] = fact[i - 1] * i % mod;
        infact[i] = infact[i - 1] * qmi(i, mod - 2, mod) % mod;
    }
    
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a, b;
        cin >> a >> b;
        long long res = fact[a] * infact[b] % mod *infact[a - b] % mod;
        cout << res << endl;
    }
    
    return 0;
}
```

### [AcWing 887. 求组合数 III](https://www.acwing.com/problem/content/889/)

```C++
#include<iostream>

using namespace std;

long long qmi(long long a, long long b, long long p) {
    long long res = 1;
    while (b) {
        if (b & 1) {
            res = res * a % p;
        }
        a = a * a % p;
        b >>= 1;
    }
    return res;
}

long long C(long long a, long long b, long long p) {
    if (a < b) {
        return 0;
    }
    
    long long x = 1, y = 1;
    for (long long i = a, j = 1; j <= b; i--, j++) {
        x = x * i % p;
        y = y * j % p;
    }
    
    return x * qmi(y, p - 2, p) % p;
}

long long lucas(long long a, long long b, long long p) {
    if (a < p && b < p) {
        return C(a, b, p);
    }
    else {
        return C(a % p, b % p, p) * lucas(a / p, b / p, p) % p;
    }
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        long long a, b, p;
        cin >> a >> b >> p;
        cout << lucas(a, b, p) << endl;
    }
    return 0;
}
```

### [AcWing 888. 求组合数 IV](https://www.acwing.com/problem/content/890/)

```C++
#include<iostream>
#include<vector>

using namespace std;

const int N = 5010;

int primes[N], cnt;
bool st[N];

void get_primes(int n) {
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            primes[cnt++] = i;
        }
        for (int j = 0; primes[j] <= n / i; j++) {
            st[i * primes[j]] = true;
            if (i % primes[j] == 0) {
                break;
            }
        }
    }
}

int sum[N];

int get(int n, int p) {
    int res = 0;
    while (n) {
        res += n / p;
        n /= p;
    }
    return res;
}

vector<int> mul(const vector<int>& a, int b) {
    vector<int> res;
    int t = 0;
    for (int i = 0; i < a.size(); i++) {
        t += a[i] * b;
        res.push_back(t % 10);
        t /= 10;
    }
    while (t) {
        res.push_back(t % 10);
        t /= 10;
    }
    return res;
}

int main() {
    int a, b;
    cin >> a >> b;
    
    get_primes(a);
    for (int i = 0; i < cnt; i++) {
        int p = primes[i];
        sum[i] = get(a, p) - get(b, p) - get(a - b, p);
    }
    vector<int> res;
    res.push_back(1);
    for (int i = 0; i < cnt; i++) {
        for (int j = 0; j < sum[i]; j++) {
            res = mul(res, primes[i]);
        }
    }
    
    for (int i = res.size() - 1; i >= 0; i--) {
        cout << res[i];
    }
    
    return 0;
}
```

### [AcWing 889. 满足条件的01序列](https://www.acwing.com/problem/content/891/)

```C++
#include<iostream>

using namespace std;

const int N = 2e5 + 10, mod = 1e9 + 7;

long long fact[N], infact[N];

long long qmi(int a, int b, int p) {
    long long res = 1, t = a;
    while (b) {
        if (b & 1) {
            res = res * t % p;
        }
        t = t * t % p;
        b >>= 1;
    }
    return res;
}

int main() {
    fact[0] = infact[0] = 1;
    for (int i = 1; i < N; i++) {
        fact[i] = fact[i - 1] * i % mod;
        infact[i] = infact[i - 1] * qmi(i, mod - 2, mod) % mod;
    }
    
    int n;
    cin >> n;
    cout << fact[2 * n] * infact[n] % mod * infact[n] % mod * qmi(n + 1, mod - 2, mod) % mod;
    
    return 0;
}
```

## 容斥原理

### [AcWing 890. 能被整除的数](https://www.acwing.com/problem/content/892/)

```C++
#include<iostream>

using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    int p[20];
    for (int i = 0; i < m; i++) {
        cin >> p[i];
    }
    
    int res = 0;
    for (int i = 1; i < 1 << m; i++) {
        int t = 1, cnt = 0;
        for (int j = 0; j < m; j++) {
            if (i >> j & 1) {
                if ((long long)t * p[j] > n) {
                    t = -1;
                    break;
                }
                t *= p[j];
                cnt++;
            }
            
        }
        if (t == -1) {
            continue;
        }
        else if (cnt % 2 == 1) {
            res += n / t;
        }
        else {
            res -= n / t;
        }
    }
    
    cout << res << endl;
    
    return 0;
}
```

## 博弈论

### 模板

**NIM游戏**

给定 $N$ 堆物品，第 $i$ 堆物品有 $A_i$ 个。两名玩家轮流行动，每次可以任选一堆，取走任意多个物品，可把一堆取光，但不能不取。取走最后一件物品者获胜。两人都采取最优策略，问先手是否必胜。

我们把这种游戏称为NIM博弈。把游戏过程中面临的状态称为局面。整局游戏第一个行动的称为先手，第二个行动的称为后手。若在某一局面下无论采取何种行动，都会输掉游戏，则称该局面必败。

所谓采取最优策略是指，若在某一局面下存在某种行动，使得行动后对面面临必败局面，则优先采取该行动。同时，这样的局面被称为必胜。我们讨论的博弈问题一般都只考虑理想情况，即两人均无失误，都采取最优策略行动时游戏的结果。
NIM博弈不存在平局，只有先手必胜和先手必败两种情况。

定理： NIM博弈先手必胜，当且仅当 $A_1\bigoplus A_2\bigoplus … \bigoplus A_n != 0$

**公平组合游戏ICG**

若一个游戏满足：

1. 由两名玩家交替行动；
2. 在游戏进程的任意时刻，可以执行的合法行动与轮到哪名玩家无关；
3. 不能行动的玩家判负。

则称该游戏为一个公平组合游戏。

NIM博弈属于公平组合游戏，但城建的棋类游戏，比如围棋，就不是公平组合游戏。因为围棋交战双方分别只能落黑子和白子，胜负判定也比较复杂，不满足条件2和条件3。

**Mex运算**

设S表示一个非负整数集合。定义 $mex(S)$ 为求出不属于集合 $S$ 的最小非负整数的运算，即：$mex(S) = min{x}$, $x$ 属于自然数，且 $x$ 不属于 $S$。

**SG函数**

在有向图游戏中，对于每个节点 $x$，设从 $x$ 出发共有 $k$ 条有向边，分别到达节点 $y_1, y_2, …, y_k$，定义 $SG(x)$ 为 $x$ 的后继节点$y_1, y_2, …, y_k$ 的 $SG$ 函数值构成的集合再执行 $mex(S)$ 运算的结果，即：

$SG(x) = mex({SG(y_1), SG(y_2), …, SG(y_k)})$

特别地，整个有向图游戏 $G$ 的 $SG$ 函数值被定义为有向图游戏起点 $s$ 的 $SG$ 函数值，即 $SG(G) = SG(s)$。

**有向图游戏的和**

设 $G_1, G_2, …, G_m$ 是 $m$ 个有向图游戏。定义有向图游戏 $G$，它的行动规则是任选某个有向图游戏 $G_i$，并在 $G_i$ 上行动一步。$G$ 被称为有向图游戏 $G_1, G_2, …, G_m$ 的和。

有向图游戏的和的 $SG$ 函数值等于它包含的各个子游戏 $SG$ 函数值的异或和，即：

$SG(G) = SG(G1) \bigoplus SG(G2) \bigoplus … \bigoplus SG(Gm)$

**定理**

有向图游戏的某个局面必胜，当且仅当该局面对应节点的 $SG$ 函数值大于0。

有向图游戏的某个局面必败，当且仅当该局面对应节点的 $SG$ 函数值等于0。

### [AcWing 891. Nim游戏](https://www.acwing.com/problem/content/893/)

```C++
#include<iostream>

using namespace std;

int main() {
    int res = 0;
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        res ^= x;
    }
    
    if (res == 0) {
        cout << "No" << endl;
    }
    else {
        cout << "Yes" << endl;
    }
    
    return 0;
}
```

### [AcWing 892. 台阶-Nim游戏](https://www.acwing.com/problem/content/894/)

```C++
#include<iostream>

using namespace std;

int main() {
    int n;
    cin >> n;
    int res = 0;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        if (i % 2 == 0) {
            res ^= a;
        }
    }
    
    if (res != 0) {
        cout << "Yes" << endl;
    }
    else {
        cout << "No" << endl;
    }
    
    return 0;
}
```

### [AcWing 893. 集合-Nim游戏](https://www.acwing.com/problem/content/895/)

```C++
#include<iostream>
#include<unordered_set>
#include<cstring>

using namespace std;

const int N = 110, M = 1e4 + 10;

int n, k;
int s[N], f[M];

int sg(int x) {
    if (f[x] != -1) {
        return f[x];
    }
    
    unordered_set<int> uset;
    for (int i = 0; i < n; i++) {
        int sum = s[i];
        if (x >= s[i]) {
            uset.insert(sg(x - sum));
        }
    }
    
    for (int i = 0; ; i++) {
        if (uset.count(i) == 0) {
            return f[x] = i;
        }
    }
}

int main() {
    memset(f, -1, sizeof(f));
    
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> s[i];
    }
    cin >> k;
    int res = 0;
    for (int i = 0; i < k; i++) {
        int h;
        cin >> h;
        res ^= sg(h);
    }
    
    if (res > 0) {
        cout << "Yes" << endl;
    }
    else {
        cout << "No" << endl;
    }
    
    return 0;
}
```

### [AcWing 894. 拆分-Nim游戏](https://www.acwing.com/problem/content/896/)

```C++
#include<iostream>
#include<unordered_set>
#include<cstring>

using namespace std;

const int N = 110;

int f[N];
int n;

int sg(int x) {
    if (f[x] != -1) {
        return f[x];
    }
    
    unordered_set<int> uset;
    for (int i = 0; i < x; i++) {
        for (int j = 0; j <= i; j++) {
            uset.insert(sg(i) ^ sg(j));
        }
    }
    
    for (int i = 0; ; i++) {
        if (uset.count(i) == 0) {
            return f[x] = i;
        }
    }
}

int main() {
    memset(f, -1, sizeof(f));
    
    cin >> n;
    int res = 0;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        res ^= sg(a);
    }
    
    if (res > 0) {
        cout << "Yes" << endl;
    }
    else {
        cout << "No" << endl;
    }
    
    return 0;
}
```

# 第五讲 动态规划

## 背包问题

### 模型与模板

（动态规划的笔记参考AcWing用户 卢盼盼，https://www.acwing.com/blog/content/4022/）

#### 01背包问题

![DP-01背包问题](动态规划\DP-01背包.jpg)

$n$ 个物品，每个物品的体积是 $v_i$，价值是 $w_i$，背包的容量是 $m$

若**每个物品最多只能装一个**，且不能超过背包容量，则背包的最大价值是多少？

**模版**

```C++
int n;              // 物品总数
int m;              // 背包容量
int v[N];           // 重量 
int w[N];           // 价值

// ---------------二维形式---------------
int f[N][M];    // f[i][j]表示在考虑前i个物品后，背包容量为j条件下的最大价值
for(int i = 1; i <= n; ++i) 
    for(int j = 1; j <= m; ++j)
        if(j < v[i]) f[i][j] = f[i-1][j];   //  当前重量装不进，价值等于前i-1个物品   
        else f[i][j] = max(f[i-1][j], f[i-1][j-v[i]] + w[i]); // 能装，需判断  
cout << f[n][m];

// ---------------一维形式---------------
int f[M];   // f[j]表示背包容量为j条件下的最大价值
for(int i = 1; i <= n; ++i) 
    for(int j = m; j >= v[i]; --j)
        f[j] = max(f[j], f[j - v[i]] + w[i]);           // 注意是倒序，否则出现写后读错误
cout << f[m];           // 注意是m不是n
```

**说明**

1. 注意 $f[i][j]$ 的含义：在考虑 $i$ 个物品后，背包容量为 $j$ 条件下的最大价值，而不是选择了 $i$ 个物品的最大价值，实际上选择的物品数 $≤i$，$f[j]$ 表示背包容量为 $j$ 条件下的最大价值
2. 二维压缩成一维，实际上是寻找避开写后续错误的方法
   1. $f[i][j]$ 始终只用上一行的数据 $f[i-1][...]$ 更新（迭代更新的基础，如果还需要用上上行数据则不可压缩）
   2. $f[i][j]$ 始终用靠左边的数据 $f[i-1][≤j]$ 更新（决定了只能倒序更新）
3. 显然 $i=0$ 时， $f(i,j)=0$，而初始化自动赋0，故不必单独处理第0行

#### 完全背包问题

![DP-完全背包](动态规划\DP-完全背包.jpg)

假设背包容量为 $j$ 时，最多可以装入 $k$ 个物品 $i$，则有

$f(i,j)=\max(f(i-1,j),f(i-1,j-v_i)+w_i,f(i-1,j-2v_i)+2w_i,...,f(i-1,j-kv_i)+kw_i)$

考虑

$f(i,j-v_i)=\max(f(i-1,j-v_i),f(i-1,j-2v_i)+w_i,f(i-1,j-3v_i)+2w_i,...,f(i-1,j-kv_i)+(k-1)w_i)$

上式变形得

$f(i,j-v_i)+w_i=\max(f(i-1,j-v_i)+w_i,f(i-1,j-2v_i)+2w_i,f(i-1,j-3v_i)+3w_i,...,f(i-1,j-kv_i)+kw_i)$

综上可得

$f(i,j)=\max(f(i-1,j),f(i,j-v_i)+w_i)$

这样就得优化后的迭代公式，和01背包问题非常相似，但后者用的是

$f(i,j)=\max(f(i-1,j),f(i-1,j-v_i)+w_i)$

**模版**

```C++
int n;              // 物品总数
int m;              // 背包容量
int v[N];           // 重量 
int w[N];           // 价值

// ---------------二维形式---------------
// 未优化
int f[N][M];    // f[i][j]表示在考虑前i个物品后，背包容量为j条件下的最大价值
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= m; j++)
        for (int k = 0; k * v[i] <= j; k++)
            f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);


// 已优化
int f[N][M];    // f[i][j]表示在考虑前i个物品后，背包容量为j条件下的最大价值
for(int i = 1; i <= n; ++i) 
    for(int j = 1; j <= m; ++j)
        if(j < v[i]) f[i][j] = f[i-1][j];   //  当前重量装不进，价值等于前i-1个物品   
        else f[i][j] = max(f[i-1][j], f[i][j-v[i]] + w[i]); // 能装，需判断  
cout << f[n][m];

// ---------------一维形式---------------
int f[M];   // f[j]表示背包容量为j条件下的最大价值
for(int i = 1; i <= n; ++i) 
    for(int j = v[i]; j <= m; ++j)
        f[j] = max(f[j], f[j - v[i]] + w[i]);           // 注意是倒序，否则出现写后读错误
cout << f[m];           // 注意是m不是n
```

**说明**

1. 形式上和01背包差不多，在二维数组表示下，主要差别在
   1. 在选择第 $i$ 物品时，用的是 $f[i][j-v]+w$，而不是 $f[i-1][j-v]+w$
   2. 上述条件决定了在每次迭代时，必须**正向**遍历，而不是反向遍历
2. 在一维数组表示下，主要差别只表现为迭代的顺序（正向或反向）
3. 在一维数组表示下，01背包只能反向是因为它主要用到上一行的数据来更新当前行数据，如果正向遍历，则会修改上一行的数据，出现写后读错误；完全背包只能正向是因为它需要用到当前行的数据更新，如果反向遍历，使用的是上一行的数据，则不符合公式

#### 多重背包问题

第 $i$ 个物品至多拿 $s_i$ 件

假设背包容量为 $j$ 时，最多可以装入 $k$ 个物品 $i$，则有

$f(i,j)=\max(f(i-1,j),f(i-1,j-v_i)+w_i,f(i-1,j-2v_i)+2w_i,...,f(i-1,j-s_iv_i)+s_iw_i)$

而

$f(i,j-v_i)=\max(f(i-1,j-v_i),f(i-1,j-2v_i)+w_i,f(i-1,j-3v_i)+2w_i,...,f(i-1,j-(s_i+1)v_i)+s_iw_i)$

变形后得

$f(i,j-v_i)+w_i=\max(f(i-1,j-v_i)+w_i,f(i-1,j-2v_i)+2w_i,f(i-1,j-3v_i)+3w_i,...,f(i-1,j-(s_i+1)v_i)+(s_i+1)w_i)$

多了一项 $f(i-1,j-(s_i+1)v_i)+(s_i+1)w_i)$，因此无法按照完全背包的方式优化

**二进制优化**

已知 $1,2,...,2^k$ 可以由系数0和1线性组合出 $0$ 至 $2^{k+1}-1$。考虑更一般的情况，若想线性组合出 $0$ 至 $S$，$S<2^{k+2}$， 则猜测可由 $1,2,...,2^k,C$ 组合出，其中 $C<2^{k+1}$， 显然，在 $C$ 一定存在的情况下，可得到的数的范围为 $C$ 至 $S$。由于 $C<2^{k+1}$，则 $C≤2^{k+1}-1$，故 $[0,2^{k+1}-1]\cup [C,S]\supseteq [0,2^{k+1}-1]\cup [2^{k+1}-1,S]=[0,S]$，即可用 $1,2,...,2^k,C$ 表示任何小于 $2^{k+2}$ 的数

因此对于有 $s[i]$ 件的某个物品 $i$，可以打包成 $\lceil \log s[i] \rceil$ 个物品，每包有 $1,2,...,2^k,C$ 件物品 $i$， 其中 $k=\lceil \log s[i] \rceil-1$

**模版**

```C++
int n;              // 物品总数
int m;              // 背包容量
int v[N];           // 重量 
int w[N];           // 价值

// -----------------未优化（完全背包模板）----------------------
int f[N][M];    // f[i][j]表示在考虑前i个物品后，背包容量为j条件下的最大价值
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= m; j++)
        for (int k = 0; k <= s[i] && k * v[i] <= j; k++)
            f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);

// -----------------------二进制优化---------------------------
// 读入物品个数时顺便打包
int k = 1;      // 当前包裹大小
while (k <= s)
{
    cnt ++ ;            // 实际物品种数
    v[cnt] = a * k;
    w[cnt] = b * k;
    s -= k;
    k *= 2;             // 倍增包裹大小
}
if (s > 0)
{
    // 不足的单独放一个，即C
    cnt ++ ;
    v[cnt] = a * s;
    w[cnt] = b * s;
}
n = cnt;        // 更新物品种数

// 转换成01背包问题
for (int i = 1; i <= n; i ++ )
    for (int j = m; j >= v[i]; j -- )
        f[j] = max(f[j], f[j - v[i]] + w[i]);

cout << f[m] << endl;
```

**说明**

1. 用二进制优化后，注意物品种数变成 $N×\log M$ ，问题转换成01背包问题
2. 时间复杂度为 $O(nm\log s)$

#### 分组背包问题

每组物品中至多拿一个

实际上是带有约束的01背包问题，状态计算为 $f(i,j)=\max(f(i−1,j),f(i−1,j−v(i,k))+w(i,k))$

**模板**

```C++
int n;              // 物品总数
int m;              // 背包容量
int v[N][S];         // 重量 
int w[N][S];         // 价值
int s[N];           // 各组物品种数

// 读入数据
 for (int i = 1; i <= n; i ++ )
 {
     cin >> s[i];
     for (int j = 1; j <= s[i]; j ++ )
         cin >> v[i][j] >> w[i][j];
 }

// 处理数据
for (int i = 1; i <= n; i ++ )
    for (int j = m; j >= 1; j -- )
        for (int k = 1; k <= s[i]; k ++ )
            if (v[i][k] <= j)
                f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);

cout << f[m] << endl;
```

### [AcWing 2. 01背包问题](https://www.acwing.com/problem/content/description/2/)

```C++
#include<iostream>

using namespace std;

const int N = 1010;

int v[N], w[N], dp[N][N];

int main() {
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i];
    }
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (v[i] > j) {
                dp[i][j] = dp[i - 1][j];
            } else {
                dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - v[i]] + w[i]);
            }
        }
    }
    
    cout << dp[n][m];
    
    return 0;
}
```

空间压缩版本：

```C++
#include<iostream>

using namespace std;

const int N = 1010;

int v[N], w[N], dp[N];

int main() {
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i];
    }
    
    for (int i = 1; i <= n; i++) {
        for (int j = m; j >= v[i]; j--) {
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
        }
    }
    
    cout << dp[m] << endl;
    
    return 0;
}
```

### [AcWing 3. 完全背包问题](https://www.acwing.com/problem/content/description/3/)

```C++
#include<iostream>

using namespace std;

const int N = 1010;

int v[N], w[N], dp[N][N];

int main() {
    int n, m;
    cin >> n >> m;
    
    for (int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i];
    }
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (v[i] > j) {
                dp[i][j] = dp[i - 1][j];
            } else {
                dp[i][j] = max(dp[i - 1][j], dp[i][j - v[i]] + w[i]);
            }
        }
    }
    
    cout << dp[n][m] << endl;
    
    return 0;
}
```

空间压缩版本：

```C++
#include<iostream>

using namespace std;

const int N = 1010;

int v[N], w[N], dp[N];

int main() {
    int n, m;
    cin >> n >> m;
    
    for (int i = 1; i <= n; i++) {
        cin >> v[i] >> w[i];
    }
    
    for (int i = 1; i <= n; i++) {
        for (int j = v[i]; j <= m; j++) {
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
        }
    }
    
    cout << dp[m] << endl;
    
    return 0;
}
```

### [AcWing 4. 多重背包问题](https://www.acwing.com/problem/content/4/) 和 [AcWing 5. 多重背包问题 II](https://www.acwing.com/problem/content/5/)

```C++
#include<iostream>

using namespace std;

const int N = 25000;

int v[N], w[N], dp[N];

int main() {
    int n, m;
    cin >> n >> m;
    int cnt = 1;
    for (int i = 0; i < n ; i++) {
        int a, b, s;
        cin >> a >> b >> s;
        int k = 1;
        while (k <= s) {
            v[cnt] = a * k;
            w[cnt] = b * k;
            s -= k;
            k *= 2;
            cnt++;
        }
        if (s > 0) {
            v[cnt] = a * s;
            w[cnt] = b * s;
            cnt++;
        }
    }
    
    n = cnt;
    for (int i = 1; i <= cnt; i++) {
        for (int j = m; j >= v[i]; j--) {
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
        }
    }
    
    cout << dp[m] << endl;
    
    return 0;
}
```

### [AcWing 9. 分组背包问题](https://www.acwing.com/problem/content/9/)

```C++
#include<iostream>

using namespace std;

const int N = 110;

int s[N], v[N][N], w[N][N], dp[N];

int main() {
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> s[i];
        for (int j = 0; j < s[i]; j++) {
            cin >> v[i][j] >> w[i][j];
        }
    }
    
    for (int i = 1; i <= n; i++) {
        for (int j = m; j > 0; j--) {
            for (int k = 0; k < s[i]; k++) {
                if (j >= v[i][k]) {
                    dp[j] = max(dp[j], dp[j - v[i][k]] + w[i][k]);
                }
            }
        }
    }
    
    cout << dp[m] << endl;
    
    return 0;
}
```

## 线性DP

### 模型与模板

#### 数字三角形

![数字三角形](动态规划\线性DP-数字三角形.jpg)

**核心代码**

```C++
// 自顶向下（未压缩`f`)
const int INF = 1e9;

for (int i = 0; i <= n; i ++ )
    for (int j = 0; j <= i + 1; j ++ )
        f[i][j] = -INF;

f[1][1] = a[1][1];
    for (int i = 2; i <= n; i ++ )
        for (int j = 1; j <= i; j ++ )
            f[i][j] = max(f[i - 1][j - 1] + a[i][j], f[i - 1][j] + a[i][j]);

int res = -INF;
for (int j = 1; j <= n; j ++ ) res = max(res, f[n][j]);
```

**说明**

1. 实际上可压缩 $f$，此时只能反向遍历行
2. 还可**自底向上**实现，若压缩，只能正向遍历行
3. 可以用 `0x80` 初始化 $f$，使得元素都小于 `-2e9`
4. 时间复杂度= $O(状态×转移)$ = $O(1×n^2)$ = $O(n^2)$

#### 最长上升子序列

![最长上升子序列](动态规划\线性DP-最长上升子序列.jpg)

**核心代码**

```C++
// 朴素法
for (int i = 1; i <= n; i ++ )          // 求以a[i]为终点的上升序列中，序列元素个数的最大值，i∈[1,n]
{
    f[i] = 1;       // 初始化f[i]为1，因为{a[i]}也是一条合法的上升序列

    // 状态计算（考虑前i-1个元素）
    for (int j = 1; j < i; j ++ )
        if (a[j] < a[i])                   // 转移条件（升序条件）
            f[i] = max(f[i], f[j] + 1);     // 状态转移
}

// f[n]表示的是以a[n]为终点的上升序列的最大值，而题目求的是分别以a[1],a[2],...,a[n]为终点的上升序列的最大值
int res = 0;
for (int i = 1; i <= n; i ++ ) res = max(res, f[i]);

// 二分优化
vector<int> stk;            //模拟栈
stk.push_back(arr[0]);

for (int i = 1; i < n; ++i) {
    if (arr[i] > stk.back())        
        stk.push_back(arr[i]);          // 如果该元素大于栈顶元素,将该元素入栈
    else
        *lower_bound(stk.begin(), stk.end(), arr[i]) = arr[i];  // 替换掉第一个大于或者等于这个数字的那个数
}
cout << stk.size() << endl;

// yxc二分优化
int len = 0;        // 最长上升子序列长度（数组q的长度）
for (int i = 0; i < n; i ++ )
{
    // 在数组q中二分查找第1个>= a[i]的数（结果）
    int l = 0, r = len;
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (q[mid] < a[i]) l = mid;
        else r = mid - 1;
    }
    q[l + 1] = a[i];            // 在长度为l的上升序列中，最小末尾元素是a[i]
    len = max(len, l + 1);      // q[l] < a[i] < q[l + 1]，更新数组q的长度（如果在q末尾插入，则按l + 1更新len
}

printf("%d\n", len);
```

**说明**

1. 朴素法时间复杂度= $O(状态×转移)$ = $O(n×n)$ = $O(n^2)$
2. 改进（贪心+二分）
   1. 令数组 $q$ 保存长度为 $i$ 的上升子序列末尾元素的最小值，例如 `125` 和 `123` 优先保存 `123` 的 `3`，因为它更能接上的后缀种类更多
   2. $q$ 是单调递增的，否则存在 $q[i-1]>q[i]$，说明长度为 $i$ 的上升子序列的最小末尾元素比长度为 $i-1$ 的还小，这与 $q[i-1]$ 的定义不符
   3. 为了使上升子序列最长，应在 $q$ 中找到 $<a[i]$ 的最大 $q[j]$ ，使得 $q[j]<a[i]<q[j+1]$，此时子序列的长度为 $j+1$，且 $q[j+1]=a[i]$。这步用整数二分实现
   4. 改进后，时间复杂度变为$O(n\log n)$

#### 最长公共子序列

![最长公共子序列](动态规划\线性DP-最长公共子序列.jpg)

$00$ 表示 $i$ 和 $j$ 都不选，$01$ 表示不选 $i$ 选 $j$，$10$ 表示选 $i$ 不选 $j$，$11$ 表示 $i$ 和 $j$ 都选

**核心代码**

```C++
char a[N], b[N];
int f[N][N];

for (int i = 1; i <= n; i ++ )
    for (int j = 1; j <= m; j ++ )
    {
        f[i][j] = max(f[i - 1][j], f[i][j - 1]);
        if (a[i] == b[j]) f[i][j] = max(f[i][j], f[i - 1][j - 1] + 1);
    }

printf("%d\n", f[n][m]);
```

**说明**

1. 难点
   1. $f(i−1,j)$ 并不是 $01$ 的等价形式，但 $01\subseteq f(i-1,j)\subseteq f(i,j)$， 因此 $\max f(i-1,j)$ 包含了 $\max(01)$，且剩余的部分也是属于 $f(i,j)$ 的，故可用 $f(i-1,j)$ 代替 $01$。同理$f(i-1,j)$ 可代替 $10$
   2. 若$C\subseteq A\cap B$，则 $\max (A,B,C)=max(A,B)$。由于$f(i-1,j-1)\subseteq f(i-1,j)\cup f(i,j-1)$，故无需考虑 $00$ 的情形，而只需考虑 $01,10和11$ 的情况
2. 时间复杂度 = $O(状态×转移)$ = $O(n^2×1)$ = $O(n^2)$

#### 最短编辑距离

给定两个字符串 $A$ 和 $B$，只允许对 $A$ 进行字符插入，字符删除和字符替换，求把 $A$ 变成 $B$ 的最少操作次数

![最短编辑距离](动态规划\线性DP-最短编辑距离.jpg)

**核心代码**

```C++
// 初始化边界
for (int i = 0; i <= m; i ++ ) f[0][i] = i;         // 把B变成空串需要删除字符的次数
for (int i = 0; i <= n; i ++ ) f[i][0] = i;         // 把空串B扩充成A需要插入字符的次数

for (int i = 1; i <= n; i ++ )
    for (int j = 1; j <= m; j ++ )
    {
        f[i][j] = min(f[i - 1][j] + 1, f[i][j - 1] + 1);
        if (a[i] == b[j]) f[i][j] = min(f[i][j], f[i - 1][j - 1]);
        else f[i][j] = min(f[i][j], f[i - 1][j - 1] + 1);
    }

printf("%d\n", f[n][m]);
```

**说明**

1. 时间复杂度= $O(状态×转移)$ = $O(n^2×3)$= $O(n^2)$

### [AcWing 898. 数字三角形](https://www.acwing.com/problem/content/900/)

```C++
#include<iostream>

using namespace std;

const int N = 510;

int a[N][N], dp[N][N];

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            cin >> a[i][j];
        }
    }
    
    for (int i = 1; i <= n; i++) {
        dp[i][0] = dp[i][i + 1] = -1e9;
    }
    dp[1][1] = a[1][1];
    for (int i = 2; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            dp[i][j] = max(dp[i - 1][j - 1], dp[i - 1][j]) + a[i][j];
        }
    }
    int res = -1e9;
    for (int i = 1; i <= n; i++) {
        res = max(res, dp[n][i]);
    }
    
    cout << res << endl;
    
    return 0;
}
```

### [AcWing 895. 最长上升子序列](https://www.acwing.com/problem/content/description/897/)

```C++
#include<iostream>

using namespace std;

const int N = 1010;

int a[N], dp[N];

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    
    int res = 0;
    for (int i = 1; i <= n; i++) {
        dp[i] = 1;
        for (int j = 1; j <= i; j++) {
            if (a[i] > a[j]) {
                dp[i] = max(dp[i], dp[j] + 1);
            }
        }
        res = max(res, dp[i]);
    }
    
    cout << res << endl;
    
    return 0;
}
```

### [AcWing 896. 最长上升子序列 II](https://www.acwing.com/problem/content/898/)

```C++
#include<iostream>

using namespace std;

const int N = 1e5 + 10;

int a[N], q[N];

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    
    int len = 0;
    for (int i = 1; i <= n; i++) {
        int left = 0, right = len;
        while (left < right) {
            int mid = left + right + 1 >> 1;
            if (q[mid] < a[i]) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        q[left + 1] = a[i];
        len = max(len, left + 1);
    }
    
    cout << len << endl;
    
    return 0;
}
```

### [AcWing 897. 最长公共子序列](https://www.acwing.com/problem/content/899/)

```C++
#include<iostream>

using namespace std;

const int N = 1010;
char a[N], b[N];
int dp[N][N];

int main() {
    int n, m;
    cin >> n >> m;
    cin >> a + 1 >> b + 1;
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            if (a[i] == b[j]) {
                dp[i][j] = max(dp[i][j], dp[i - 1][j - 1] + 1);
            }
        }
    }
    
    cout << dp[n][m] << endl;
    
    return 0;
}
```

### [AcWing 902. 最短编辑距离](https://www.acwing.com/problem/content/904/)

```C++
#include<iostream>

using namespace std;

const int N = 1010;

char a[N], b[N];
int dp[N][N];

int main() {
    int n, m;
    cin >> n >> a + 1 >> m >> b + 1;
    
    for (int i = 1; i <= n; i++) {
        dp[i][0] = i;
    }
    for (int j = 1; j <= m; j++) {
        dp[0][j] = j;
    }
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + 1;
            if (a[i] == b[j]) {
                dp[i][j] = min(dp[i][j], dp[i - 1][j - 1]);
            } else {
                dp[i][j] = min(dp[i][j], dp[i - 1][j - 1] + 1);
            }
        }
    }
    
    cout << dp[n][m] << endl;
    
    return 0;
}
```

### [AcWing 899. 编辑距离](https://www.acwing.com/problem/content/description/901/)

```C++
#include<iostream>
#include<cstring>

using namespace std;

const int N1 = 1010, N2 = 15;

char a[N1][N2], b[N1];
int dp[N1][N2];

int edit_distance(char a[], char b[]) {
    int n = strlen(a + 1), m = strlen(b + 1);
    
    for (int i = 1; i <= n; i++) {
        dp[i][0] = i;
    }
    for (int i = 1; i <= m; i++) {
        dp[0][i] = i;
    }
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + 1;
            dp[i][j] = min(dp[i][j], dp[i - 1][j - 1] + (a[i] != b[j]));
        }
    }
    
    return dp[n][m];
}

int main() {
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> a[i] + 1;
    }
    for (int i = 0; i < m; i++) {
        int k;
        cin >> b + 1 >> k;
        int cnt = 0;
        for (int j = 0; j < n; j++) {
            if (edit_distance(a[j], b) <= k) {
                cnt++;
            }
        }
        cout << cnt << endl;
    }
    
    return 0;
}
```

## 区间DP

### 模型与模板

石子合并：相邻两堆石子可以合并，代价为二者石子数的和，求最小代价

![石子合并](E:\Code_Notes\AcWing算法基础课 刷题笔记\动态规划\区间DP-石子合并.jpg)

**核心代码**

```C++
int s[N];           // 前缀和
int f[N][N];        

for (int i = 1; i <= n; i ++ ) s[i] += s[i - 1];        // 初始化前缀和

for (int len = 2; len <= n; len ++ )        // len=1时不合并（类似归并排序的merge）
    // 固定窗口大小，从小到大遍历
    for (int i = 1; i + len - 1 <= n; i ++ )
    {
        // 固定窗口左端点，则可确定窗口右端点，注意边界
        int l = i, r = i + len - 1;
        // 窗口内划分
        f[l][r] = 0x7f7f7f7;    // 初始化为无穷大
        for (int k = l; k < r; k ++ )
            f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
    }

printf("%d\n", f[1][n]);
```

**说明**

1. 时间复杂度 $O(n^3)$

### [AcWing 282. 石子合并](https://www.acwing.com/problem/content/284/)

```C++
#include<iostream>

using namespace std;

const int N = 310;

int a[N], s[N], dp[N][N];

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        s[i] = s[i - 1] + a[i];
    }
    
    for (int len = 2; len <= n; len++) {
        for (int i = 1; i + len - 1 <= n; i++) {
            int l = i, r = i + len - 1;
            dp[l][r] = 0x3f3f3f3f;
            for (int j = i; j < r; j++) {
                dp[l][r] = min(dp[l][r], dp[l][j] + dp[j + 1][r] + s[r] - s[l - 1]);
            }
        }
    }
    
    cout << dp[1][n] << endl;
    
    return 0;
}
```

## 计数类DP

### 模型与模板

把n拆分成1~n的和的方案数（不考虑顺序）

**完全背包解法**

![整数划分](动态规划\计数类DP-整数划分（完全背包）.jpg)

可以看做有 $n$ 种物品，第 $i$ 种物品的体积为 $i$，背包的容量为 $n$，每个物品可以拿无数次，求装满背包的方案数

假设当前背包容量为 $j$，则第 $i$ 个物品至多装 $k=⌊\frac{j}{i}⌋$个

已知$f(i,j)=f(i−1,j)+f(i−1,j−i)+f(i−1,j−2i)+⋯+f(i−1,j−ki)$

当 $j<i$ 时，$j-i<0$， $f(i,j)=f(i−1,j)$

当 $j≥i$ 时，$j-i≥0$，有$f(i,j−i)=f(i−1,j−i)+f(i−1,j−2i)+f(i−1,j−3i)+⋯+f(i−1,j−ki)$

对比可得状态转移方程
$$
f(i,j)=f(i-1,j)+f(i,j-1)
$$
综上，当 $j<i$ 时， $f(i,j)=f(i-1,j)$；当 $j≥i$ 时，$f(i,j)=f(i-1,j)+f(i,j-i)$。特别的 $f(0,*)=1$

**核心代码**

```C++
// 未压缩f
f[0][0] = 1;
for (int i = 1; i <= n; i ++)
    for (int j = 0; j <= n; j ++)
        if (j >= i) f[i][j] = (f[i - 1][j] + f[i][j - i]) % mod;
        else f[i][j] = f[i - 1][j];
cout << f[n][n] << endl;

// 压缩f
f[0] = 1;
for (int i = 1; i <= n; i ++ )
    for (int j = i; j <= n; j ++ )
        f[j] = f[j] + f[j - i];
```

**其他解法**

![证书划分](动态规划\计数类DP-整数划分（其它算法）.jpg)

当 $j$ 个数中最小值是 $1$ ，则方案数等价于去掉 $1$ 个 $1$ 时的方案数 $f(i−1,j−1)$ ，此时总和为 $i−1$ ，共有 $j−1$ 个数

当 $j$ 个数中最小值 $>1$ ，则方案数等价于 $j$ 个数全都减 $1$ 的方案数 $f(i−j,j)$

**核心代码**

```C++
f[1][1] = 1;
for (int i = 2; i <= n; i ++ )
    for (int j = 1; j <= i; j ++ )
        f[i][j] = (f[i - 1][j - 1] + f[i - j][j]) % mod;

int res = 0;
for (int i = 1; i <= n; i ++ ) res = (res + f[n][i]) % mod;

cout << res << endl;
```

### [AcWing 900. 整数划分](https://www.acwing.com/problem/content/902/)

完全背包模型解法：

```C++
#include<iostream>

using namespace std;

const int N = 1010, mod = 1e9 + 7;

int dp[N][N];

int main() {
    int n;
    cin >> n;
    
    dp[0][0] = 1;
    
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= n; j++) {
            if (i <= j) {
                dp[i][j] = (dp[i - 1][j] + dp[i][j - i]) % mod;
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    
    cout << dp[n][n] << endl;
    
    return 0;
}
```

完全背包模型压缩版本：

```C++
#include<iostream>

using namespace std;

const int N = 1010, mod = 1e9 + 7;

int dp[N];

int main() {
    int n;
    cin >> n;
    
    dp[0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= n; j++) {
            if (i <= j) {
                dp[j] = (dp[j] + dp[j - i]) % mod;
            }
        }
    }
    
    cout << dp[n] << endl;
    
    return 0;
}
```

其他解法：

```C++
#include<iostream>

using namespace std;

const int N = 1010, mod = 1e9 + 7;

int dp[N][N];

int main() {
    int n;
    cin >> n;
    
    dp[1][1] = 1;
    for (int i = 2; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            dp[i][j] = (dp[i - 1][j - 1] + dp[i - j][j]) % mod;
        }
    }
    int res = 0;
    for (int i = 1; i <= n; i++) {
        res = (res + dp[n][i]) % mod;
    }
    
    cout << res << endl;
    
    return 0;
}
```

## 数位统计DP

### 模型与模板

给定两个整数 $a$ 和 $b$ ，求 $a$ 和 $b$ 之间的所有数字中 $0$ ~ $9$ 的出现次数

**思路**

$count(n, x)$ 表示 $1$ 到 $n$ 中，数字 $x$ 出现的次数 $0≤x≤9$

考虑数 $x$ 在 $n=abcdefg$ 的第 $4$ 位 $d$ 出现的次数，不妨把 $n$ 看成 $yyyxzzz$

1. 当$000≤yyy≤(abc−1)$ 时， $000≤zzz≤999$
   1. 当 $x≠0$ 时，此时共有 $abc×1000$ 次
   2. 当 $x=0$ 时，由于 $abcd$ 不能全为 $0$ ，因此此时共有 $(abc−1)×1000$ 次
2. 当 $yyy=abc$ 时
   1. 当 $d<x$ ，$abcdefg<abcxzzz$ ，此时出现 $0$ 次
   2. 当 $d=x$ ，$000≤yyy≤efg$ ，此时出现 $efg+1$ 次
   3. 当 $d>x$ ，$000≤yyy≤999$ ，此时出现 $1000$ 次

特殊处理

1. 当求的是最左边那位出现的次数时，$abc$ 不存在，因此此时只需考虑第2种情况
2. 当 $x=0$ ，且遍历到最左边那位时，根据上述讨论，需要把 $n$ 看成 $xyyyzzz=0yyyzzz$ ，这是不合法的，因此当 $x=0$ 时，从左起第2位开始遍历

**核心代码**

```C++
int get(vector<int> num, int l, int r)
{
    int res = 0;
    for (int i = l; i >= r; i -- ) res = res * 10 + num[i];
    return res;
}

int power10(int x)
{
    int res = 1;
    while (x -- ) res *= 10;
    return res;
}

int count(int n, int x)
{
    if (!n) return 0;       // 特判

    // 拆分
    vector<int> num;
    while (n)
    {
        num.push_back(n % 10);
        n /= 10;
    }
    n = num.size();

    // 核心
    int res = 0;
    for (int i = n - 1 - !x; i >= 0; i -- )     // 当x=0时，从左起第2位开始遍历
    {
        // 考虑左起第1位时不存在abc，跳过
        if (i < n - 1)
        {
            res += get(num, n - 1, i + 1) * power10(i);
            if (!x) res -= power10(i);      // x=0，abc不能全为0，排除这种情况
        }

        // 尽管左起第1位不存在abc，但存在efg，因此保留这部分
        if (num[i] == x) res += get(num, i - 1, 0) + 1;
        else if (num[i] > x) res += power10(i);
    }

    return res;
}

for (int i = 0; i <= 9; i ++ )
    cout << count(b, i) - count(a - 1, i) << ' ';       // 类似前缀和思想
```

### [AcWing 338. 计数问题](https://www.acwing.com/problem/content/340/)

```C++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 10;

int get(vector<int> num, int left, int right) {
    int res = 0;
    for (int i = left; i >= right; i--) {
        res = res * 10 + num[i];
    }
    return res;
}

int power10(int x) {
    int res = 1;
    while (x--) {
        res *= 10;
    }
    return res;
}

int count(int n, int x) {
    if (!n) {
        return 0;
    }

    vector<int> num;
    while (n) {
        num.push_back(n % 10);
        n /= 10;
    }
    n = num.size();

    int res = 0;
    for (int i = n - 1 - !x; i >= 0; i--) {
        if (i < n - 1) {
            res += get(num, n - 1, i + 1) * power10(i);
            if (!x) {
                res -= power10(i);
            }
        }

        if (num[i] == x) {
            res += get(num, i - 1, 0) + 1;
        } else if (num[i] > x) {
            res += power10(i);
        }
    }

    return res;
}

int main() {
    int a, b;
    while (cin >> a >> b , a) {
        if (a > b) {
            swap(a, b);
        }

        for (int i = 0; i <= 9; i++) {
            cout << count(b, i) - count(a - 1, i) << ' ';
        }
        cout << endl;
    }

    return 0;
}
```

## 状态压缩DP

### 模型与模板

**蒙德里安的梦想**

在给定大小的网格放入 $1×2$ 和 $2×1$ 的矩形

![蒙德里安的梦想](动态规划\状态压缩DP-蒙德里安的梦想.jpg)

**状态压缩**是通过用**二进制**数表示状态实现的

当确定横向矩形的位置后，竖向矩形的位置就确定了，因此只需考虑横向矩形的放置方法。

$j$ 表示当前列的状态，它记录了上一列放入横向矩形的行号，也可理解为当前列被上一列横向矩形捅出的行号，这些行号不可用。例如 $10010$ 表示上一列第1行和第4行放有横向矩形，即当前列的第1行和第4行不可用。$k$ 表示上一列的状态，与 $j$ 类似。

假设有 $n$ 行，则状态 $j$ 表示某列 $n$ 个格子的使用情况，格子被占用记为1，空闲记为0。因此这 $n$ 个格子一共有 $2^n$ 情况，且 $0≤j≤2^n−1$

集合根据上一列的状态 $k$ 划分，因此共有 $2^n$ 种状态划分方式，当满足横向矩阵不碰撞以及剩余部分能放完竖向矩阵两个条件是， $f(i,j)$ 能转移到 $f(i-1,k)$

**核心代码**

```C++
const int N = 12, M = 1 << N;       // 最大行数，每列的最大状态数
long long f[N][M];
bool st[M];

// 穷举第i列和第i+1列能放完竖向矩形的情形（找到所有不存在连续奇数个0的情况）
// 预处理，减少重复计算的代价
for (int i = 0; i < 1 << n; i ++ )
{
    int cnt = 0;
    st[i] = true;
    for (int j = 0; j < n; j ++ )
        if (i >> j & 1)
        {
            if (cnt & 1) {
                st[i] = false;
                break;
            }
            cnt = 0;
        }
        else cnt ++ ;
    if (cnt & 1) st[i] = false;
}

f[0][0] = 1;            // 全部放竖向矩形，1种方案
for (int i = 1; i <= m; i ++ )                  // 遍历1~m列
    for (int j = 0; j < 1 << n; j ++ )          // 穷举当前列的可能状态
        for (int k = 0; k < 1 << n; k ++ )      // 穷举上一列的可能状态
            if ((j & k) == 0 && st[j | k])      // 判断是否满足状态转移条件（不碰撞，能放满）
                f[i][j] += f[i - 1][k];         // 状态转移

cout << f[m][0] << endl;                        // 当放完第0~m-1列后，第m列没有捅出的横向矩形的情形就是最终结果
```

说明

1. 时间复杂度 $O(n×2^n×2^n)=O(n4^n)$，但 $n=11$，实际上只需计算 $4×10^7$ 次，能在1s内完成

**最短Hamilton路径**

给定一张 $n$ 个点的带权无向图，点从 $0$ ~ $n−1$ 标号，求起点 $0$ 到终点 $n−1$ 的最短 $Hamilton$ 路径。 $Hamilton$ 路径的定义是从 $0$ 到 $n−1$ 不重不漏地经过每个点恰好一次。

![最短Hamilton路径](动态规划\状态压缩DP-最短Hamilton路径.jpg)

将 $f[i][j]$ 所表示的集合中的所有路线，按**倒数第二个点**分成若干类，其中第 $k$ 类是指倒数第二个点是 $k$ 的所有路线。那么 $f[i][j]$ 的值就是每一类的最小值，再取个 $min$。而第 $k$ 类的最小值就是$f[i - (1 << j)][k] + w[k][j]$。

从定义出发，最后 $f[(1 << n) - 1][n - 1]$ 就表示所有“经过了所有点，且最后位于点 $n-1$ 的路线”的长度的最小值，也就是我们要求的答案。

```C++
memset(f, 0x3f, sizeof f);              // 初始化为无穷大
f[1][0] = 0;    // 表示只有起点0且最后位于起点0的路线的长度是0，此时点集i的最后一位是1，其余为0，因为点集只有起点0，故i=1

for (int i = 0; i < 1 << n; i ++ )          // 穷举所有可能的点集
    for (int j = 0; j < n; j ++ )           // 从当前点集找一个点（二进制串中位为1的位置）
        if (i >> j & 1)
            for (int k = 0; k < n; k ++ )    // 从当前点集找另外一个点（可以和之前找的相同）
                if (i >> k & 1)
                    f[i][j] = min(f[i][j], f[i - (1 << j)][k] + w[k][j]);   // 尝试从后找的点到达点j

cout << f[(1 << n) - 1][n - 1];     // 所有点都在点集，且终点是n-1
```

### [AcWing 291. 蒙德里安的梦想](https://www.acwing.com/problem/content/293/)

```C++
#include<iostream>
#include<cstring>

using namespace std;

const int N = 12, M = 1 << N;

bool st[M];
long long dp[N][M];

int main() {
    int n, m;
    cin >> n >> m;
    while (n > 0 || m > 0) {
        for (int i = 0; i < 1 << n; i++) {
            st[i] = true;
            int cnt = 0;
            for (int j = 0; j < n; j++) {
                if (i >> j & 1) {
                    if (cnt & 1) {
                        st[i] = false;
                        break;
                    }
                    cnt = 0;
                } else {
                    cnt++;
                }
            }
            if (cnt & 1) {
                st[i] = false;
            }
        }
        
        memset(dp, 0, sizeof(dp));
        dp[0][0] = 1;
        for (int i = 1; i <= m; i++) {
            for (int j = 0; j < 1 << n; j++) {
                for (int k = 0; k < 1 << n; k++) {
                    if ((j & k) == 0 && st[j | k]) {
                        dp[i][j] += dp[i - 1][k];
                    }
                }
            }
        }
        
        cout << dp[m][0] << endl;
        
        cin >> n >> m;
    }
    
    return 0;
}
```

### [AcWing 91. 最短Hamilton路径](https://www.acwing.com/problem/content/93/)

```C++
#include<iostream>
#include<cstring>

using namespace std;

const int N = 20, M = 1 << N;

int w[N][N], dp[M][N];

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> w[i][j];
        }
    }
    
    memset(dp, 0x3f, sizeof(dp));
    dp[1][0] = 0;
    for (int i = 0; i < 1 << n; i++) {
        for (int j = 0; j < n; j++) {
            if (i >> j & 1) {
                for (int k = 0; k < n; k++) {
                    if (i >> k & 1) {
                        dp[i][j] =  min(dp[i][j], dp[i - (1 << j)][k] + w[k][j]);
                    }
                }
            }
        }
    }
    
    cout << dp[(1 << n) - 1][n - 1];
    
    return 0;
}
```

## 树形DP

### 模型与模板

给定一个带结点值的树，求一个结点集合，使得集合里任意两个结点都不相邻，且结点值的和最大

![没有上司的舞会](动态规划\树形DP-没有上司的舞会.jpg)

根据有无上司划分集合

1. 若上司存在，则不考虑所有直接下属，只考虑所有间接下属
2. 若上司存在，则考虑直接下属，在有无下属中选择幸福值最大的那个方案

**核心代码**

```C++
int n;
int h[N], e[N], ne[N], idx;     
int happy[N];
int f[N][2];
bool has_fa[N];         // 标记是否存在父节点

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx ++ ;
}

void dfs(int u)
{
    f[u][1] = happy[u];     // 选取根节点的初值为自身的幸福度

    // 遍历子树
    for (int i = h[u]; ~i; i = ne[i])
    {
        int j = e[i];       // 子结点
        dfs(j);             // 递归子结点

        // 状态转移
        f[u][1] += f[j][0]; 
        f[u][0] += max(f[j][0], f[j][1]);
    }
}

// 核心
memset(h, -1, sizeof h);            // 初始化邻接表头指针

// 读入树结构
for (int i = 0; i < n - 1; i ++ )
{
    int a, b;
    scanf("%d%d", &a, &b);
    add(b, a);                      // 尽管是无向图，但只需要保留一条边（上司指向下属）
    has_fa[a] = true;               // 标记存在父节点
}

// 找树根，不存在父节点的就是树根
int root = 1;
while (has_fa[root]) root ++ ;

dfs(root);      // 从根节点开始遍历

printf("%d\n", max(f[root][0], f[root][1]));
```

**说明**

1. 使用**邻接表**表示树
2. DFS+动态规划

### [AcWing 285. 没有上司的舞会](https://www.acwing.com/problem/content/287/)

```C++
#include<iostream>

using namespace std;

const int N = 6010;

int h[N], e[N], ne[N], idx = 1;

int n, happy[N];
bool hasFather[N];
int dp[N][2];

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void DFS(int u) {
    dp[u][1] = happy[u];
    
    for (int i = h[u]; i > 0; i = ne[i]) {
        int v = e[i];
        DFS(v);
        dp[u][0] += max(dp[v][0], dp[v][1]);
        dp[u][1] += dp[v][0];
    }
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> happy[i];
    }
    for (int i = 1; i < n; i++) {
        int l, k;
        cin >> l >> k;
        add(k, l);
        hasFather[l] = true;
    }
    
    int root;
    for (int i = 1; i <= n; i++) {
        if (!hasFather[i]) {
            root = i;
            break;
        }
    }
    DFS(root);
    
    cout << max(dp[root][0], dp[root][1]) << endl;
    
    return 0;
}
```

## 记忆化搜索

### 模型与模板

给一个矩阵，求一条路径，使得它的长度最长，且路径上的值是递减的。（只能往上下左右移动）

![滑雪](动态规划\记忆化搜索-滑雪.jpg)

**记忆化搜索**指保存中间结果，避免重复计算，用空间换时间

**核心代码**

```C++
int g[N][N];
int f[N][N];
int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

int dp(int x, int y)
{
    if (f[x][y] != -1) return f[x][y];      // 已经计算过，不必重复计算（记忆化搜索）

    f[x][y] = 1;
    for (int i = 0; i < 4; i++) {
        int a = x + dx[i], b = y + dy[i];
        if (g[a][b] < g[x][y]) 
            f[x][y] = max(f[x][y], dfs(a, b) + 1);
    }

    return f[x][y];
}

memset(g, 0x3f, sizeof g);      // 边界为无穷大，不能滑到，可以省去边界判断
memset(f, -1, sizeof f);

int res = 0;
for (int i = 1; i <= n; i ++ )
    for (int j = 1; j <= m; j ++ )
        res = max(res, dp(i, j));
```

### [AcWing 901. 滑雪](https://www.acwing.com/problem/content/903/)

```C++
#include<iostream>
#include<cstring>

using namespace std;

const int N = 310;

int r, c;
int g[N][N], dp[N][N];
int res = 0;
int dx[4] = {-1, 0, 1, 0};
int dy[4] = {0, 1, 0, -1};

int DFS(int i, int j) {
    if (dp[i][j] != 0) {
        return dp[i][j];
    }
    
    dp[i][j] = 1;
    for (int k = 0; k < 4; k++) {
        int x = i + dx[k], y = j + dy[k];
        if (g[x][y] < g[i][j]) {
            dp[i][j] = max(dp[i][j], DFS(x, y) + 1);
        }
    }
    
    return dp[i][j];
}

int main() {
    memset(g, 0x3f, sizeof(g));
    
    cin >> r >> c;
    for (int i = 1; i <= r; i++) {
        for (int j = 1; j <= c; j++) {
            cin >> g[i][j];
        }
    }
    
    for (int i = 1; i <= r; i++) {
        for (int j = 1; j <= c; j++) {
            res = max(res, DFS(i, j));
        }
    }
    
    cout << res << endl;
    
    return 0;
}
```

# 第六讲 贪心

## 区间问题

### [AcWing 905. 区间选点](https://www.acwing.com/problem/content/907/)和[AcWing 908. 最大不相交区间数量](https://www.acwing.com/problem/content/910/)

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 1e5 + 10;

struct Range {
    int l, r;
}ranges[N];

bool cmp(const Range& range1, const Range& range2) {
    return range1.r < range2.r;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> ranges[i].l >> ranges[i].r;
    }
    
    sort(ranges, ranges + n, cmp);
    int ed = -2e9, res = 0;
    for (int i = 0; i < n; i++) {
        if (ranges[i].l > ed) {
            res++;
            ed = ranges[i].r;
        }
    }
    
    cout << res << endl;
    
    return 0;
}
```

### [AcWing 906. 区间分组](https://www.acwing.com/problem/content/908/)

1. 将所有区间按左端点从小到大排序
2. 从前往后处理每个区间，判断能否将其放到某个现有的组中
   1. 如果不存在这样的组**（所有组的最小右端点仍比该区间左端点大）**，则开新组
   2. 如果存在这样的组，将其放进去，并更新当前组的max_right

```C++
#include<iostream>
#include<algorithm>
#include<queue>

using namespace std;

const int N = 1e5 + 10;

struct range {
    int l, r;
}ranges[N];

bool cmp(const range& u, const range& v) {
    if (u.l != v.l) {
        return u.l < v.l;
    } else {
        return v.l < v.r;
    }
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> ranges[i].l >> ranges[i].r;
    }
    
    sort(ranges, ranges + n, cmp);
    priority_queue<int, vector<int>, greater<int>> heap;
    for (int i = 0; i < n; i++) {
        if (heap.empty() || heap.top() >= ranges[i].l) {
            heap.push(ranges[i].r);
        } else {
            heap.pop();
            heap.push(ranges[i].r);
        }
    }
    
    cout << heap.size() << endl;
    
    return 0;
}
```

### [AcWing 907. 区间覆盖](https://www.acwing.com/problem/content/909/)

题意：

​		给出N个闭区间以及一个线段区间[start，end]，选择尽可能少的区间，将线段区间完全覆盖。若存在，输出所需最少区间数，如果无解，输出-1。

思路：

1. 将左端点从小到大排序；
2. 从前往后依次枚举每个区间，在所有能覆盖start的区间中，选择右端点最大的区间，然后将start更新成右端点的最大值。

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 1e5 + 10;

struct range {
    int l, r;
}ranges[N];

bool cmp(const range& u, const range& v) {
    if (u.l != v.l) {
        return u.l < v.l;
    } else {
        return u.r < v.r;
    }
}

int main() {
    int s, t;
    cin >> s >> t;
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> ranges[i].l >> ranges[i].r;
    }
    
    sort(ranges, ranges + n, cmp);
    int i = 0, j, res = 0;
    while (i < n) {
        j = i;
        int r = -2e9;
        while (j < n && ranges[j].l <= s) {
            r = max(r, ranges[j].r);
            j++;
        }
        res ++;
        if (r < s) {
            cout << -1 << endl;
            return 0;
        } else if (r >= t) {
            cout << res << endl;
            return 0;
        } else {
            i = j;
            s = r;
        }
    }
    cout << -1 << endl;
    
    return 0;
}
```

## Huffman树

### [AcWing 148. 合并果子](https://www.acwing.com/problem/content/150/)

```C++
#include<iostream>
#include<queue>

using namespace std;

int main() {
    int n;
    cin >> n;
    priority_queue<int, vector<int>, greater<int>> heap;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        heap.push(a);
    }
    
    int res = 0;
    for (int i = 1; i < n; i++) {
        int a = heap.top();
        heap.pop();
        int b = heap.top();
        heap.pop();
        res += a + b;
        heap.push(a + b);
    }
    
    cout << res << endl;
    
    return 0;
}
```

## 排序不等式

### [AcWing 913. 排队打水](https://www.acwing.com/problem/content/description/915/)

题意：

​		有 n 个人排队到 11 个水龙头处打水，第 i 个人装满水桶所需的时间是 t~i~，请问如何安排他们的打水顺序才能使所有人的等待时间之和最小？

思路：

​		按照从小到大的顺序排队，时间最小。

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 1e5 + 10;

int t[N];

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> t[i];
    }
    
    sort(t, t + n);
    long long res = 0;
    for (int i = 0; i < n; i++) {
        res += t[i] * (n - 1 - i);
    }
    
    cout << res << endl;
    
    return 0;
}
```

## 绝对值不等式

### [AcWing 104. 货仓选址](https://www.acwing.com/problem/content/description/106/)

题意：

​		在一条数轴上有 N家商店，它们的坐标分别为 A~1~-A~N~。现在需要在数轴上建立一家货仓，每天清晨，从货仓到每家商店都要运送一车商品。为了提高效率，求把货仓建在何处，可以使得货仓到每家商店的距离之和最小。

思路：

​		尽可能往中间放。

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 1e5 + 10;

int a[N];

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    
    int res = 0;
    sort(a, a + n);
    for (int i = 0; i < n; i++) {
        res += abs(a[i] - a[n / 2]);
    }
    
    cout << res << endl;
    
    return 0;
}
```

## 推公式

### [AcWing 125. 耍杂技的牛](https://www.acwing.com/problem/content/127/)

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

const int N = 5e4 + 10;

struct cow {
    int w, s;
}cows[N];

bool cmp(const cow& u, const cow& v) {
    return u.w + u.s < v.w + v.s;
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> cows[i].w >> cows[i].s;
    }
    
    int res = -2e9, sum = 0;
    sort(cows, cows + n, cmp);
    for (int i = 0; i < n; i++) {
        res = max(res, sum - cows[i].s);
        sum += cows[i].w;
    }
    
    cout << res << endl;
    
    return 0;
}
```
