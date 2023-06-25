# 字符串

## [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

### 解法1：模拟

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    string replaceSpace(string s) {        
        string array;
        for (auto c : s) {
            if (c == ' ') {
                array += "%20";
            }
            else {
                array.push_back(c);
            }
        }
        return array;
    }
};
```

## [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode.cn/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

### 解法1：模拟

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        s = s.substr(n) + s.substr(0, n);
        return s;
    }
};
```

## ==[剑指 Offer 20. 表示数值的字符串](https://leetcode.cn/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)==（本题适合死记硬背）

### 解法1：模拟

```C++
class Solution {
public:
    bool isNumber(string s) {
        //先去掉s前后的空格
        string str;
        int i = 0, j = s.length() - 1;
        while (i < s.length() && s[i] == ' ') {
            i++;
        }
        while (j >= 0 && s[j] == ' ') {
            j--;
        }
        for (int k = i; k <= j; k++) {
            str += s[k];
        }
        if (str.length() == 0) {
            return false;
        }

        //若字符串开始为+/-号，后移一位
        int index=0;
        if (str[index] == '+' || str[index] == '-') {
            index++;
        }
        if (index == str.length()) {
            return false;//字符串只有一个+/-号，结果为false
        }

        //检查字符串直到遇见第一个./e/E符号退出
        while (index < str.length() && str[index] != '.' && str[index] != 'e' && str[index]!='E') {
            if (str[index] < '0' || str[index] > '9') {
                return false;//中途出现空格或其他符号,结果为false
            }
            else {
                index++;
            }
        }

        //根据第一个循环结果决定下一步操作
        if (index == str.length()) {
            return true;//字符串表示整数，结果为true
        }
        else if (str[index] == '.') {//遇见小数点.
            if (index >0 && str[index - 1] >= '0' && str[index - 1] <= '9');//小数点前面至少一位数字
            else if (index + 1 == str.length() || str[index + 1] < '0' || str[index + 1] > '9') {
                return false;//小数点前面没数字，后面也没数字（两种情况：后面没字符，或字符为空格或其他符号）
            }
            index++;
        }

        //如果第一个环结束在e/E，则该循环不执行；如果第一个循环结束在小数点并跳一位后，则继续寻找e/E
        while (index < str.length() && str[index] != 'e' && str[index] != 'E') {
            if (str[index] < '0' || str[index] > '9') {
                return false;//中途出现空格或其他符号,结果为false
            }
            else {
                index++;
            }
        }

        //根据第二个循环结果决定下一步操作
        if (index == str.length()) {
            return true;//字符串表示整数或小数（不带e/E）
        }
        else {
            if (index > 0 && (str[index - 1] >= '0' && str[index - 1] <= '9' || str[index - 1] == '.'));//e/E前面至少一位数字或小数点
            else if (index >= 0 || index == str.length() - 1) {
                return false;//e/E前面有不是数字或小数点的字符，或e/E后面连一位数字都没有
            }
            index++;
        }

        if (str[index] == '+' || str[index] == '-') {
            index++;//e/E后面是+/-号，后移一位
        }
        if (index == str.length()) {
            return false;//e/E后面只有一个+/-号，结果为false
        }
        //第三个循环，确定e/E后面是整数，否则为false
        while (index < str.length()) {
            if (str[index] < '0' || str[index] > '9') {
                return false;
            }
            else {
                index++;
            }
        }
        return true;
    }
};
```

## [剑指 Offer 67. 把字符串转换成整数](https://leetcode.cn/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

### 解法1：模拟

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int strToInt(string str) {
        int index = 0;
        while (str[index] == ' ') {
            index++;
        }
        if (index == str.length() || !(str[index] >= '0' && str[index] <= '9' || str[index] == '+' || str[index] == '-')) { //字符串都是空格 或者 第一个有效字符不是数字或正负号
            return 0;
        }

        int flag = true;
        if (str[index] == '+' || str[index] == '-') { //如果第一个有效字符为正负号，则判断结果为正还是负
            flag = str[index] == '+' ? true : false;
            index++;
        }
        int num = 0;
        while (str[index] >= '0' && str[index] <= '9') {
            if (flag == true) { //为正数
                if (num < INT_MAX / 10 || num == INT_MAX / 10 && str[index] - '0' <= INT_MAX % 10) { //当继续加数字上去不会大于INT_MAX时
                    num = num * 10 + (str[index] - '0');
                }
                else {
                    return INT_MAX;
                }
            }
            else { //为负数
                if (num > INT_MIN / 10 || num == INT_MIN / 10 && str[index] - '0' <= -(INT_MIN % 10)) {  //当继续加数字上去不会小于INT_MIN时
                    num = num * 10 - (str[index] - '0');
                }
                else {
                    return INT_MIN;
                }
            }
            index++;
        }
        return num;
    }
};
```

# 链表

## ==[剑指 Offer 06. 从尾到头打印链表](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)==

### 解法1：模拟

$$
O(n)+O(1)
$$

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        ListNode* node = head;
        vector<int> result;
        while (node != NULL) {
            result.push_back(node->val);
            node = node->next;
        }
        reverse(result.begin(), result.end()); //reverse函数在vector容器中的使用
        return result;
    }
};
```

### ==笔记：`reverse()`==

```C++
//翻转数组
//头文件
#include <algorithm>
//使用方法
reverse(a, a+n);//n为数组中的元素个数

//翻转字符串
//用法为
reverse(str.begin(), str.end());

//翻转向量
//用法
reverse(vec.begin(), vec.end());
```

## [剑指 Offer 24. 反转链表](https://leetcode.cn/problems/fan-zhuan-lian-biao-lcof/)

### 解法1：头插法

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* dummyHead1 = new ListNode(), * dummyHead2 = new ListNode(); //设置虚拟头结点，方便后续操作
        dummyHead1->next = head;
        ListNode* p = head;
        while (p != NULL) { //头插法
            //将当前节点取出后重新链接原单链表
            ListNode* q = p;
            p = p->next;
            dummyHead1->next = p;
            //将当前节点按头插法加入dummyHead2所在链表
            q->next = dummyHead2->next;
            dummyHead2->next = q;
        }
        return dummyHead2->next;
    }
};
```

## [剑指 Offer 35. 复杂链表的复制](https://leetcode.cn/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

### 解法1：回溯+哈希

$$
O(n)+O(1)
$$

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    unordered_map<Node*, Node*> cachedNode;
    Node* copyRandomList(Node* head) {
        if (head == NULL) {
            return NULL;
        }
        if (cachedNode[head] != NULL) {
            return cachedNode[head];
        }
        Node* newHead = new Node(head->val);
        cachedNode[head] = newHead;
        newHead->next = copyRandomList(head->next);
        newHead->random = copyRandomList(head->random);
        return newHead;
    }
};
```

### 解法2：迭代+节点拆分

$$
O(n)+O(1)
$$

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == NULL) {
            return NULL;
        }
        for (Node* node = head; node != NULL; node = node->next->next) {
            Node* newNode = new Node(node->val);
            newNode->next = node->next;
            node->next = newNode;
        }
        for (Node* node = head; node != NULL; node = node->next->next) {
            Node* newNode = node->next;
            if (node->random != NULL) {
                newNode->random = node->random->next;
            }
        }
        Node* newHead = head->next;
        for (Node* node = head; node != NULL; node = node->next) {
            Node* newNode = node->next;
            node->next = newNode->next;
            if (node->next != NULL) {
                newNode->next = node->next->next;
            }
        }
        return newHead;
    }
};
```

# 双指针

## [剑指 Offer 18. 删除链表的节点](https://leetcode.cn/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

### 解法1：题意

$$
O(n)+O(1)
$$

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* pre = dummyHead, * p = head;
        while (p) {
            if (p->val == val) {
                pre->next = p->next;
                break;
            }
            pre = pre->next;
            p = p->next;
        }
        return dummyHead->next;
    }
};
```

## [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

### 解法1：双指针

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* p = head, * q = head;
        while (k-- && q != NULL) {
            q = q->next;
        }
        if (k >= 0) {
            return NULL;
        }
        while (q != NULL) {
            p = p->next;
            q = q->next;
        }
        return p;
    }
};
```

## [剑指 Offer 25. 合并两个排序的链表](https://leetcode.cn/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

### 解法1：归并

$$
O(n)+O(1)
$$

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummyHead = new ListNode(0), * p = dummyHead;
        while (l1 && l2) {
            if (l1->val <= l2->val) {
                p->next = l1;
                l1 = l1->next;
            }
            else {
                p->next = l2;
                l2 = l2->next;
            }
            p = p->next;
        }
        if (l1) {
            p->next = l1;
        }
        else {
            p->next = l2;
        }
        return dummyHead->next;
    }
};
```

## [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode.cn/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

### 解法1：双指针

$$
O(n)+O(1)
$$

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* p = headA, * q = headB;
        int countA = 0, countB = 0;
        while (p) {
            p = p->next;
            countA++;
        }
        while (q) {
            q = q->next;
            countB++;
        }
        p = headA, q = headB;
        if (countA < countB) {
            swap(p,q);
        }
        int count = abs(countA - countB);
        while (count--) {
            p = p->next;
        }
        while (p != q) {
            p = p->next;
            q = q->next;
        }
        return p;
    }
};
```

## [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode.cn/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

### 解法1：双指针

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int i = 0, j = nums.size() - 1;
        while (true) {
            while (i < j && nums[i] % 2 == 1) {
                i++;
            }
            while (i < j && nums[j] % 2 == 0) {
                j--;
            }
            if (i < j) {
                swap(nums[i], nums[j]);
            }
            else {
                break;
            }
        }
        return nums;
    }
};
```

## [剑指 Offer 57. 和为s的两个数字](https://leetcode.cn/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

### 解法1：双指针

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int i = 0, j = nums.size() - 1;
        while (i < j) {
            if (nums[i] + nums[j] == target) {
                break;
            }
            else if (nums[i] + nums[j] < target) {
                i++;
            }
            else {
                j--;
            }
        }
        return vector<int>{nums[i], nums[j]};
    }
};
```

## [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode.cn/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

### 解法1：题意

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    string reverseWords(string s) {
        string result, word; //result存储结果，word存储单个单词
        int left = 0, right = s.length() - 1; //去掉左右空格后的左边界和右边界
        while (left < s.length() && s[left] == ' ') {
            left++;
        }
        while (right >= 0 && s[right] == ' ') {
            right--;
        }
        for (int i = left ;i <= right; i++) {
            if (s[i] != ' ') { //遇到非空字符，插入word扩充单词
                word += s[i];
            }
            else {
                if (i > 0 && s[i - 1] != ' ') { //只有前一个字符是非空字符的空格，才是word形成的标志
                    result = " " + word + result;
                    word = ""; //清空word，为下一次循环做准备
                }
            }
        }
        result = word + result;
        return result;
    }
};
```

# 栈与队列

## [剑指 Offer 09. 用两个栈实现队列](https://leetcode.cn/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

### 解法1：模拟

$$
O(1)+O(n)
$$

```C++
class CQueue {
    stack<int> st1, st2; //初始化两个栈，st1用于出队，st2用于入队
public:
    CQueue() {
        //清空两个栈
        while (!st1.empty()) {
            st1.pop();
        }
        while (!st2.empty()) {
            st2.pop();
        }
    }
    
    void appendTail(int value) {
        st2.push(value); //将元素压入入队栈
    }
    
    int deleteHead() {
        if (!st1.empty()) { //出队栈非空，直接出队
            int x = st1.top();
            st1.pop();
            return x;
        }
        else if (!st2.empty()) { //出队栈空，但入队栈非空，则将入队栈所有元素压入出队栈后再出队
            while (!st2.empty()) {
                st1.push(st2.top());
                st2.pop();
            }
            int x = st1.top();
            st1.pop();
            return x;
        }
        else { //入队栈和出队栈都空，说明队空，返回-1
            return -1;
        }
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```

## [剑指 Offer 30. 包含min函数的栈](https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/)

### 解法1：模拟

$$
O(1)+O(n)
$$

```C++
class MinStack {
    stack<int> st, min_st; //分别作为正常的栈和存储最小元素的栈
public:
    /** initialize your data structure here. */
    MinStack() {
        while (!st.empty()) { //栈非空时将栈清空
            st.pop();
        }
        while (!min_st.empty()) { //最小栈非空时将最小栈清空
            min_st.pop();
        }
    }
    
    void push(int x) {
        st.push(x); //将元素压栈
        if (min_st.empty() || x <= min_st.top()) { //最小栈空或者当前元素比栈顶元素更小，则压入最小栈
            min_st.push(x);
        }
    }
    
    void pop() {
        if (!min_st.empty() && st.top() == min_st.top()) { //最小栈非空且最小栈栈顶元素和栈顶元素相同，则最小栈栈顶元素同时出栈
            min_st.pop();
        }
        if (!st.empty()) { //栈非空，则栈顶元素出栈
            st.pop();
        }
    }
    
    int top() {
        if (!st.empty()) { //访问栈顶元素
            return st.top();
        }
        return -1;
    }
    
    int min() {
        if (!min_st.empty()) { //访问栈内最小元素，即最小栈栈顶元素
            return min_st.top();
        }
        return -1;
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```

## [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode.cn/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

### 解法1：单调队列

$$
O(n)+O(k)
$$

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> q; //双端队列，存位置；双端队列头部永远存的是当前滑动窗口的最大值
        vector<int> result; //存放各滑动窗口的最大值
        
        for (int i = 0; i < nums.size(); i++) {
            //访问 0或i-k+1 到 i 的滑动窗口的数组
            if (!q.empty() && q.front() < i - k + 1) { //若双端队列第一个数的位置不在范围内，将其移出
                q.pop_front();
            }
            while (!q.empty() && nums[q.back()] <= nums[i]) { //当双端队列最后一个数比当前位置的数要小时，则后续滑动窗口继续右移时，滑动窗口内最大的数必然大于等于当前位置的数，故可以直接将双端队列最后的数移出
                q.pop_back();
            }
            q.push_back(i); //将当前位置的数移入双端队列
            if (i >= k - 1) { //当滑动敞口大小为k时，将最大值存入结果数组
                result.push_back(nums[q.front()]);
            }
        }
        ;

        return result;
    }
};
```

## [剑指 Offer 59 - II. 队列的最大值](https://leetcode.cn/problems/dui-lie-de-zui-da-zhi-lcof/)

### 解法1：单调队列

$$
O(1)+O(n)
$$

```C++
class MaxQueue {
    queue<int> que;
    deque<int> maxQue;
public:
    MaxQueue() {
        while (!que.empty()) {
            que.pop();
        }
        while (!maxQue.empty()) {
            maxQue.pop_front();
        }
    }
    
    int max_value() {
        if (maxQue.empty()) {
            return -1;
        }
        else {
            return maxQue.front();
        }
    }
    
    void push_back(int value) {
        que.push(value);
        while (!maxQue.empty() && maxQue.back() < value) {
            maxQue.pop_back();
        }
        maxQue.push_back(value);
    }
    
    int pop_front() {
        if (que.empty()) {
            return -1;
        }
        else {
            if (que.front() == maxQue.front()) {
                maxQue.pop_front();
            }
            int x = que.front();
            que.pop();
            return x;
        }
    }
};

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue* obj = new MaxQueue();
 * int param_1 = obj->max_value();
 * obj->push_back(value);
 * int param_3 = obj->pop_front();
 */
```

# 模拟

## [剑指 Offer 29. 顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

### 解法1：模拟

$$
O(mn)+O(1)
$$

```C++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return vector<int>{};
        }
        int m = matrix.size(), n = matrix[0].size(); //记录矩阵的行数和列数
        vector<int> v; //结果数组
        int u = 0, d = m - 1, l = 0, r = n - 1; //记录当前要处理的最上行，最下行，最左行，最右行
        while (u <= d && l <= r) { //当最上行和最下行，最左行和最右行未相遇时，继续处理
            for (int i = l; i <= r; i++) { //处理最上行
                v.push_back(matrix[u][i]);
            }
            if (++u > d) { //最上行下移，且当与最下行相遇时，退出循环
                break;
            }
            for (int i = u; i <= d; i++) { //处理最右行
                v.push_back(matrix[i][r]);
            }
            if (--r < l) { //最右行左移，且当与最左行相遇时，退出循环
                break;
            }
            for (int i = r; i >= l; i--) { //处理最下行
                v.push_back(matrix[d][i]);
            }
            if (--d < u) {
                break; //最下行上移，且当与最上行相遇时，退出循环
            }
            for (int i = d; i >= u; i--) { //处理最左行
                v.push_back(matrix[i][l]);
            }
            if (++l > r) {
                break; //最左行右移，且当与最右行相遇时，退出循环
            }
        }

        return v;
    }
};
```

## [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode.cn/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

### 解法1：模拟

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> st;
        int i, j;
        for (i = 0, j = 0; i < pushed.size(); i++) {
            if (pushed[i] == popped[j]) {
                j++;
                while (!st.empty() && st.top() == popped[j]) {
                    st.pop();
                    j++;
                }
            }
            else {
                st.push(pushed[i]);
            }
        }
         while (!st.empty() && st.top() == popped[j]) {
            st.pop();
            j++;
        }
        if (j == popped.size()) {
            return true;
        }
        else {
            return false;
        }
    }
};
```

# 查找算法

##==[剑指 Offer 03. 数组中重复的数字](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)==

### 解法1：哈希

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        vector<int> count(100010, 0);
        for (int i = 0; i < nums.size(); i++) {
            if (count[nums[i]]++) {
                return nums[i];
            }
        }
        return 0;
    }
};
```

```C++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        unordered_set<int> Set;
        for (int i = 0; i < nums.size(); i++) {
            if (Set.find(nums[i]) != Set.end()) {
                return nums[i];
            }
            else {
                Set.insert(nums[i]);
            }
        }
        return -1;
    }
};
```

### ==笔记：常见的三种哈希结构（数组，set，map）==

数组略。

在C++中，set 和 map 分别提供以下三种数据结构，其底层实现以及优劣如下表所示：

| 集合               | 底层实现 | 是否有序 | 数值是否可以重复 | 能够更改数值 | 查询效率 | 增删效率 |
| :----------------- | -------- | -------- | ---------------- | ------------ | -------- | -------- |
| std::set           | 红黑树   | 有序     | 否               | 否           | O(log n) | O(log n) |
| std::multiset      | 红黑树   | 有序     | 是               | 是           | O(log n) | O(log n) |
| std::unordered_set | 红黑树   | 无序     | 否               | 否           | O(1)     | O(1)     |

std::unordered_set底层实现为哈希表，std::set 和std::multiset 的底层实现是红黑树，红黑树是一种平衡二叉搜索树，所以key值是有序的，但key不可以修改，改动key值会导致整棵树的错乱，所以只能删除和增加。

| 集合               | 底层实现 | 是否有序 | 数值是否可以重复 | 能够更改数值 | 查询效率 | 增删效率 |
| :----------------- | -------- | -------- | ---------------- | ------------ | -------- | -------- |
| std::map           | 红黑树   | key有序  | key不可重复      | key不可修改  | O(logn)  | O(logn)  |
| std::multimap      | 红黑树   | key有序  | key可重复        | key不可修改  | O(log n) | O(log n) |
| std::unordered_map | 哈希表   | key无序  | key不可重复      | key不可修改  | O(1)     | O(1)     |

std::unordered_map 底层实现为哈希表，std::map 和std::multimap 的底层实现是红黑树。同理，std::map 和std::multimap 的key也是有序的（这个问题也经常作为面试题，考察对语言容器底层的理解）。

当我们要使用集合来解决哈希问题的时候，优先使用unordered_set，因为它的查询和增删效率是最优的，如果需要集合是有序的，那么就用set，如果要求不仅有序还要有重复数据的话，那么就用multiset。

那么再来看一下map ，在map 是一个key value 的数据结构，map中，对key是有限制，对value没有限制的，因为key的存储方式使用红黑树实现的。

其他语言例如：java里的HashMap ，TreeMap 都是一样的原理。可以灵活贯通。

set容器和map容器的用法

```C++
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
    count()
    和上面类似，增删改查的时间复杂度是 O(1)
    不支持 lower_bound()/upper_bound()， 迭代器的++，--
```

set的使用举例（另外两种容器类似）

```C++
#include <iostream>
#include <set>
using namespace std;
int main() {
    set<int> s; // 定义⼀个空集合s
    s.insert(1); // 向集合s⾥⾯插⼊⼀个1
    cout << *(s.begin()) << endl; // 输出集合s的第⼀个元素 (前⾯的星号表示要对指针取值)
    for (int i = 0; i < 6; i++) {
        s.insert(i); // 向集合s⾥⾯插⼊i
    }
    for (auto it = s.begin(); it != s.end(); it++) { // ⽤迭代器遍历集合s⾥⾯的每⼀个元素
        cout << *it << " ";
    }
    cout << endl << (s.find(2) != s.end()) << endl; // 查找集合s中的值，如果结果等于s.end()表示未找到 (因为s.end()表示s的最后⼀个元素的下⼀个元素所在的位置)
    cout << (s.find(10) != s.end()) << endl; // s.find(10) != s.end()表示能找到10这个元素
    s.erase(1); // 删除集合s中的1这个元素
    cout << (s.find(1) != s.end()) << endl; // 这时候元素1就应该找不到啦～
    return 0;
}
```

map的使用举例（另外两种容器类似）

```C++
#include <iostream>
#include <map>
#include <string>
using namespace std;
int main() {
    map<string, int> m; // 定义⼀个空的map m，键是string类型的，值是int类型的
    m["hello"] = 2; // 将key为"hello", value为2的键值对(key-value)存⼊map中
    cout << m["hello"] << endl; // 访问map中key为"hello"的value, 如果key不存在，则返回0
    cout << m["world"] << endl;
    m["world"] = 3; // 将"world"键对应的值修改为3
    m[","] = 1; // 设⽴⼀组键值对，键为"," 值为1
    // ⽤迭代器遍历，输出map中所有的元素，键⽤it->first获取，值⽤it->second获取
    for (auto it = m.begin(); it != m.end(); it++) {
        cout << it->first << " " << it->second << endl;
    }
    // 访问map的第⼀个元素，输出它的键和值
    cout << m.begin()->first << " " << m.begin()->second << endl;
    // 访问map的最后⼀个元素，输出它的键和值
    cout << m.rbegin()->first << " " << m.rbegin()->second << endl;
    // 输出map的元素个数
    cout << m.size() << endl;
    return 0;
}
```

## [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

### 解法1：二分查找

$$
O(\log n)=O(1)
$$

```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.size() == 0) {
            return 0;
        }
        int left1 = 0, right1 = nums.size() - 1;
        while (left1 < right1) { //查找该数出现的第一个位置
            int mid = left1 + right1 >> 1;
            if (nums[mid] >= target) {
                right1 = mid;
            }
            else {
                left1 = mid + 1;
            }
        }
        if (nums[left1] != target) { //找到的数不是目标数，说明数组中目标数出现的次数为0
            return 0;
        }
        int left2 = 0, right2 = nums.size() - 1;
        while (left2 < right2) {
            int mid = left2 + right2 + 1 >> 1;
            if (nums[mid] <= target) {
                left2 = mid;
            }
            else {
                right2 = mid - 1;
            }
        }
        return left2 - left1 + 1;
    }
};
```

## [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/)

### 解法1：直接遍历

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i++) {
            if (i != nums[i]) {
                return i;
            }
        }
        return nums.size();
    }
};
```

## ==[剑指 Offer 04. 二维数组中的查找](https://leetcode.cn/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)==

### 解法1：二分查找

$$
O(m\log n)+O(1)
$$

```C++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        for (const auto& row: matrix) {
            auto it = lower_bound(row.begin(), row.end(), target);
            if (it != row.end() && *it == target) {
                return true;
            }
        }
        return false;
    }
};
```

### 解法2：Z字形查找

$$
O(m+n)+O(1)
$$

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return false;
        }
        int i = 0, j = matrix[0].size() - 1;
        while (i < matrix.size() && j >= 0) {
            if (matrix[i][j] == target) {
                return true;
            }
            else if (matrix[i][j] < target) {
                i++;
            }
            else {
                j--;
            }
        }
        return false;
    }
};
```

### ==笔记：vector容器的遍历方法==

参考：https://blog.csdn.net/HW140701/article/details/78833486?ops_request_misc=&request_id=&biz_id=102&utm_term=vector%20%E9%81%8D%E5%8E%86&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-78833486.142^v73^wechat,201^v4^add_ask,239^v2^insert_chatgpt&spm=1018.2226.3001.4187

```C++
#include <vector>
#include <iostream>

using namespace std;

int main()
{
	vector<int> vec(6, 6);
	//第一种遍历方式，下标
	for (int i = 0; i < vec.size(); ++i) {
		cout << vec[i] << endl;
	}
	//第二种遍历方式，迭代器
	for (vector<int>::iterator iter = vec.begin(); iter != vec.end(); iter++) {
		cout << *iter << endl;
	}
	//第三种遍历方式，auto关键字
	for (auto iter = vec.begin(); iter != vec.end(); iter++) {
		cout << *iter << endl;
	}
	//第四种遍历方式，auto关键字的另一种方式
	for (auto i : vec) {
		cout << i << endl;
	}
	return 0;
}
```

### ==笔记：整数二分查找模板==

```C++
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

### ==笔记：浮点数二分模板==

```C++
bool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```

### ==笔记：`lower_bound()`与`upper_bound()`==

参考：https://blog.csdn.net/weixin_42051815/article/details/115873582

1. 在数组中使用`lower_bound()`和`upper_bound()`

   用法：`int i_lower = lower_bound(arr, arr + n, val) - arr;`

   作用：`lower_bound()`函数返回数组 arr 中**大于等于** val 的第一个元素的地址，若arr中的元素均小于 val 则返回尾后插入val位置地址。

   用法：`int i_upper = lower_bound(arr, arr + n, val) - arr;`

   作用：`upper_bound()`函数返回数组 arr 中**大于** val 的第一个元素的地址，若 arr 中的元素均小于等于 val 则返回尾后插入val位置地址。

2. STL中的`lower_bound()`和`upper_bound()`

   用法与数组中基本一样，但在STL中，返回值为迭代器。但在vector中，可以是迭代器也可以是位置。

   (1) vector动态数组

   ```C++
   int main()
   {
   	vector<int> vec{ 0,1,5,8,10,11};//有序数组
   	vector<int>::iterator it1 = lower_bound(vec.begin(), vec.end(), 11);
   	bool flag1 = it1 == vec.end() - 1;
   	cout << flag1 << endl;//it1指向最后一个元素的迭代器
       unsigned i_lower = lower_bound(vec.begin(), vec.end(), 11) - vec.begin();
       cout << "i_lower:" << i_lower << endl;//位置5
   
   	vector<int>::iterator it2 = upper_bound(vec.begin(), vec.end(), 11);
   	bool flag2 = it2 == vec.end();
   	cout << flag2 << endl;//it2为尾后迭代器
       unsigned j_upper = upper_bound(vec.begin(), vec.end(), 11) - vec.begin();
       cout << "j_upper:" << j_upper << endl;//位置6
   	return 0;
   }
   ```

   运行结果：

   ```C++
   1
   i_lower:5
   1
   j_upper:6
   ```

   (2) set集合

   ```C++
   #include<algorithm>
   #include<set>
   using namespace std;
   
   int main()
   {
   	set<int> s{ 0,2,5,8,11};//原本就有序
   	set<int>::iterator it1 = s.lower_bound(2);
   	cout << *it1 << endl;
   	set<int>::iterator it2 = s.upper_bound(2);
   	cout << *it2 << endl;
   	return 0;
   }
   ```

   运行结果：

   ```C++
   2
   5
   ```

   (3) map字典

   ```C++
   #include<algorithm>
   #include<map>
   using namespace std;
   
   int main()
   {
   	map<int, int> m{ {1,2},{2,2},{9,2},{7,2} };//有序
   	map<int, int>::iterator it1 = m.lower_bound(2);
   	cout << it1->first << endl;//it1->first=2
   	map<int, int>::iterator it2 = m.upper_bound(2);
   	cout << it2->first << endl;//it2->first=7;
   	return 0;
   }
   ```

   运行结果：

   ```C++
   2
   7
   ```

## [剑指 Offer 11. 旋转数组的最小数字](https://leetcode.cn/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

### 解法1：二分查找

$$
平均时间复杂度：O(\log n)；上限时间复杂度：O(n)
$$

$$
空间复杂度：O(1)
$$

```C++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int left = 0, right = numbers.size() - 1;
        while (left < numbers.size() && numbers[left] == numbers[right]) { //若首尾元素相同，则右移left至两元素不同，避免下面的算法出错
            left++;
        }
        if (left == numbers.size()) { //最坏的情况，所有元素都相同，时间复杂度退化至O(n)
            return numbers[0];
        }
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (numbers[mid] <= numbers[numbers.size()-1]) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }
        return numbers[left];
    }
};
```

## [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode.cn/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

### 解法1：哈希

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    char firstUniqChar(string s) {
        unordered_map<char, int> hash;
        for (int i = 0; i < s.length(); i++) {
            hash[s[i]]++;
        }
        for (int i = 0; i < s.length(); i++) {
            if (hash[s[i]] == 1) {
                return s[i];
            }
        }
        return ' ';
    }
};
```

# 搜索与回溯算法

## [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

### 解法1：层序遍历

$$
O(n)+O(1)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        vector<int> res;
        queue<TreeNode*> que;
        if (root) {
            que.push(root);
        }
        while (!que.empty()) {
            TreeNode* node = que.front();
            que.pop();
            res.push_back(node->val);
            if (node->left) {
                que.push(node->left);
            }
            if (node->right) {
                que.push(node->right);
            }
        }
        return res;
    }
};
```

## [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

### 解法1：层序遍历

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> que;
        if (root) {
            que.push(root);
        }
        while (!que.empty()) {
            int size = que.size();
            vector<int> path;
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                path.push_back(node->val);
                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }
            }
            res.push_back(path);
        }
        return res;
    }
};
```

## [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

### 解法1：层序遍历

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> que;
        if (root) {
            que.push(root);
        }
        bool flag = false;
        while (!que.empty()) {
            int size = que.size();
            vector<int> path;
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                path.push_back(node->val);
                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }
            }
            if (flag) {
                reverse(path.begin(), path.end());
            }
            flag = !flag;
            res.push_back(path);
        }
        return res;
    }
};
```

## ==[剑指 Offer 26. 树的子结构](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/)==

### 解法1：递归

$$
O(mn)+O(m)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSub(TreeNode* A, TreeNode* B){
        if (B == NULL) {
            return true;
        }
        else if (A == NULL || A->val != B->val) {
            return false;
        }
        return isSub(A->left, B->left) && isSub(A->right, B->right);
    }
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (A == NULL || B == NULL) {
            return false;
        }
        return isSub(A,B) || isSubStructure(A->left, B) || isSubStructure(A->right, B);
    }
};
```

## ==[剑指 Offer 27. 二叉树的镜像](https://leetcode.cn/problems/er-cha-shu-de-jing-xiang-lcof/)==

### 解法1：递归

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if (root == NULL) {
            return NULL;
        }
        TreeNode* p = root->left;
        root->left = mirrorTree(root->right);
        root->right = mirrorTree(p);
        return root;
    }
};
```

### 解法2：层序遍历

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        queue<TreeNode*> que;
        que.push(root);
        while (!que.empty()) {
            TreeNode* node = que.front();
            que.pop();
            if (node == NULL) {
                continue;
            }
            swap(node->left, node->right);
            que.push(node->left);
            que.push(node->right);
        }
        return root;
    }
};
```

## ==[剑指 Offer 28. 对称的二叉树](https://leetcode.cn/problems/dui-cheng-de-er-cha-shu-lcof/)==

### 解法1：递归

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSym(TreeNode* leftNode,TreeNode* rightNode){
        if (leftNode == NULL && rightNode == NULL) {
            return true;
        }
        else if (leftNode == NULL || rightNode == NULL) {
            return false;
        }
        else if (leftNode->val != rightNode->val) {
            return false;
        }
        else {
            return isSym(leftNode->left,rightNode->right)&&isSym(leftNode->right,rightNode->left);
        }
    }
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) {
            return true;
        }
        return isSym(root->left, root->right);
    }
};
```

### 解法2：层序遍历

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) {
            return true;
        }
        queue<TreeNode*> que;
        que.push(root->left), que.push(root->right);
        while (!que.empty()) {
            TreeNode *left_node = que.front();
            que.pop();
            TreeNode *right_node = que.front();
            que.pop();
            if (left_node == NULL && right_node == NULL) {
                continue;
            }
            else if (left_node == NULL || right_node == NULL) {
                return false;
            }
            else if (left_node->val != right_node->val) {
                return false;
            }
            else {
                que.push(left_node->left), que.push(right_node->right);
                que.push(left_node->right), que.push(right_node->left);
            }
        }
        return true;
    }
};
```

## [剑指 Offer 12. 矩阵中的路径](https://leetcode.cn/problems/ju-zhen-zhong-de-lu-jing-lcof/)

### 解法1：回溯

$$
时间复杂度：略
$$

$$
空间复杂度：O(mn)
$$

```C++
class Solution {
public:
    bool traversal(int x,int y,int index,vector<vector<char>>& board,string word, vector<vector<bool>>& visit){
        if (index == word.length() - 1 && board[x][y] == word[index]) {
            return true;
        }
        else if (board[x][y] != word[index]) {
            return false;
        }
        int dx[] = {0, 1, 0, -1};
        int dy[] = {1, 0, -1, 0};
        for (int i = 0; i < 4; i++) {
            if (x + dx[i] >= 0 && x + dx[i] < board.size() && y + dy[i] >= 0 && y + dy[i] < board[0].size()) {
                if (visit[x + dx[i]][y + dy[i]] == false) {
                    visit[x + dx[i]][y + dy[i]] = true;
                    if (traversal(x + dx[i], y + dy[i], index + 1, board, word, visit)) {
                        return true;
                    }
                    visit[x + dx[i]][y + dy[i]] = false;
                }
            }
        }
        return false;
    }
    bool exist(vector<vector<char>>& board, string word) {
        vector<vector<bool>> visit(board.size(), vector<bool>(board[0].size(), false));
        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board[0].size(); j++) {
                if (board[i][j] == word[0]) {
                    visit[i][j] = true;
                    if (traversal(i, j, 0, board, word, visit) == true) {
                        return true;
                    }
                    visit[i][j] = false;
                }
            }
        }
        return false;
    }
};
```

## [剑指 Offer 13. 机器人的运动范围](https://leetcode.cn/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

### 解法1：BFS

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    bool judge(int x, int y, int k) {
        int sum = 0;
        while (x) {
            sum += x % 10;
            x /= 10;
        }
        while (y) {
            sum += y % 10;
            y /= 10;
        }
        if (sum <= k) {
            return true;
        }
        else {
            return false;
        }
    }
    int movingCount(int m, int n, int k) {
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        queue<pair<int, int>> que;
        que.push({0, 0});
        visited[0][0] = true;
        int count = 1;
        int dx[4] = {1, 0, -1, 0};
        int dy[4] = {0, 1, 0, -1};
        while (!que.empty()) {
            pair<int, int> now = que.front();
            que.pop();
            int x = now.first, y = now.second;
            for (int i = 0; i < 4; i++) {
                if (x + dx[i] >= 0 && x + dx[i] < m && y + dy[i] >= 0 && y + dy[i] < n) {
                    if (visited[x + dx[i]][y + dy[i]] == false && judge(x + dx[i], y + dy[i], k)) {
                        que.push({x + dx[i], y + dy[i]});
                        visited[x + dx[i]][y + dy[i]] = true;
                        count++;
                    }
                }
            }
        }
        return count;
    }
};
```

## [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode.cn/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

### 解法1：递归

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void traversal(TreeNode *node, int sum, int target) {
        if (node->left == NULL && node->right == NULL) {
            if (sum == target) {
                res.push_back(path);
            }
            return;
        }
        if (node->left) {
            path.push_back(node->left->val);
            traversal(node->left, sum + node->left->val, target);
            path.pop_back();
        }
        if (node->right) {
            path.push_back(node->right->val);
            traversal(node->right, sum + node->right->val, target);
            path.pop_back();
        }
    }
    vector<vector<int>> pathSum(TreeNode* root, int target) {
        if (root) {
            path.push_back(root->val);
            traversal(root, root->val, target);
            path.pop_back();
        }
        return res;
    }
};
```

## [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode.cn/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

### 解法1：中序遍历

$$
O(n)+O(1)
$$

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    Node* head = NULL;
    void traversal(Node* node){
        if (node == NULL) {
            return;
        }
        traversal(node->left);
        Node* nodeRight = node->right;
        if (head == NULL){
            head = node;
            head->right = head;
            head->left = head;
        }
        else {
            head->left->right = node;
            node->left = head->left;
            node->right = head;
            head->left = node;
        }
        traversal(nodeRight);
    }
    Node* treeToDoublyList(Node* root) {
        traversal(root);
        return head;
    }
};
```

## [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

### 解法1：中序遍历

$$
O(n)+O(1)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int index=1,result;
    void inOrder(TreeNode* node,int k){
        if(node==NULL) return;
        inOrder(node->right,k);
        if(index==k) result=node->val;
        index++;
        inOrder(node->left,k);
    }
    int kthLargest(TreeNode* root, int k) {
        inOrder(root,k);
        return result;
    }
};
```

## [剑指 Offer 55 - I. 二叉树的深度](https://leetcode.cn/problems/er-cha-shu-de-shen-du-lcof/)

### 解法1：递归

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```

## [剑指 Offer 55 - II. 平衡二叉树](https://leetcode.cn/problems/ping-heng-er-cha-shu-lcof/)

### 解法1：递归

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int getHeight(TreeNode* node){
        if (node == NULL) {
            return 0;
        }
        int leftHeight = getHeight(node->left);
        if (leftHeight == -1) {
            return -1;
        }
        int rightHeight = getHeight(node->right);
        if (rightHeight == -1) {
            return -1;
        }
        if(abs(leftHeight - rightHeight) > 1) {
            return -1;
        }
        else return 1 + max(leftHeight, rightHeight);
    }
    bool isBalanced(TreeNode* root) {
        int result = getHeight(root);
        if (result == -1) {
            return false;
        }
        else {
            return true;
        }
    }
};
```

## [剑指 Offer 64. 求1+2+…+n](https://leetcode.cn/problems/qiu-12n-lcof/)

### 解法1：递归

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int sumNums(int n) {
        n&&(n+=sumNums(n-1));
        return n;
    }
};
```

### 解法2：位运算

$$
O(1)+O(1)
$$

```C++
class Solution {
public:
    int sumNums(int n) {
        int ans = 0, A = n, B = n + 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        return ans >> 1;
    }
};
```

## [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

### 解法1：技巧

$$
O(n)+O(1)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (p->val > q->val) {
            swap(p,q);
        }
        TreeNode* node = root;
        while (true) {
            if (node->val < p->val) {
                node = node->right;
            }
            else if (node->val > q->val) {
                node = node->left;
            }
            else {
                return node;
            }
        }
        return NULL;
    }
};
```

## ==[剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode.cn/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)==

### 解法1：递归

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL) {
            return NULL;
        }
        if (root == p || root == q) {
            return root;
        }
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left == NULL && right == NULL) {
            return NULL;
        }
        else if (left == NULL) {
            return right;
        }
        else if (right == NULL) {
            return left;
        }
        else {
            return root;
        }
    }
};
```

### 解法2：存储父节点

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    unordered_map<int, TreeNode*> hash;
    unordered_map<int, bool> visit;
    void DFS(TreeNode* node){
        if (node == NULL) {
            return;
        }
        if (node->left) {
            hash[node->left->val] = node;
        }
        if (node->right) {
            hash[node->right->val] = node;
        }
        DFS(node->left);
        DFS(node->right);
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        hash[root->val] = NULL;
        DFS(root);
        while (p != NULL) {
            visit[p->val] = true;
            p = hash[p->val];
        }
        while (q != NULL) {
            if (visit[q->val] == true) {
                return q;
            }
            q = hash[q->val];
        }
        return NULL;
    }
};
```

## ==[剑指 Offer 37. 序列化二叉树](https://leetcode.cn/problems/xu-lie-hua-er-cha-shu-lcof/)==

### 解法1：层序遍历

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string result = "[";
        queue<TreeNode*> que;
        que.push(root);
        while (true) {
            int flag = false;
            string temp;
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (node != root) {
                    temp += ",";
                }
                if (node) {
                    flag = true;
                    temp += to_string(node->val);
                    que.push(node->left);
                    que.push(node->right);
                }
                else {
                    temp += "null";
                }
            }
            if (flag == true) {
                result += temp;
            }
            else {
                break;
            }
        }
        result += "]";
        return result;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data == "[]") {
            return NULL;
        }
        string temp;
        int count = 1;
        TreeNode* head = NULL;
        queue<TreeNode*> que;
        for (int i = 1; i < data.length(); i++) {
            if (data[i] != ',' && data[i] != ']') {
                temp += data[i];
            }
            else {
                if (temp != "null") {
                    TreeNode* node = new TreeNode(stoi(temp));
                    que.push(node);
                    if (head == NULL) {
                        head = node;
                    }
                    else {
                        TreeNode* pre = que.front();
                        if (count % 2 == 0) {
                            pre->left = node;
                        }
                        else {
                            pre->right = node;
                        }
                    }
                }
                temp = "";
                if (count != 1 && count %2 ) {
                    que.pop();
                }
                count++;
            }
        }
        return head;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```

## ==[剑指 Offer 38. 字符串的排列](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/)==

### 解法1：回溯

$$
O(n*n!)+O(n)
$$

```C++
class Solution {
public:
    vector<string> result;
    string path;
    void backtracking(const string& s, vector<bool>& used) {
        if (path.length() == s.length()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < s.length(); i++) {
            if (i > 0 && s[i] == s[i - 1] && used[i - 1] == false) {
                continue;
            }
            if (used[i] == false) {
                used[i] = true;
                path.push_back(s[i]);
                backtracking(s, used);
                path.pop_back();
                used[i] = false;
            }
        }
    }
    vector<string> permutation(string s) {
        sort(s.begin(), s.end());
        vector<bool> used(s.length(), false);
        backtracking(s, used);
        return result;
    }
};
```

### 解法2：下一个排列

$$
O(n*n!)+O(1)
$$

```C++
class Solution {
public:
    bool nextPermutation(string& s) {
        int i = s.size() - 2;
        while (i >= 0 && s[i] >= s[i + 1]) {
            i--;
        }
        if (i < 0) {
            return false;
        }
        int j = s.size() - 1;
        while (j >= 0 && s[i] >= s[j]) {
            j--;
        }
        swap(s[i], s[j]);
        reverse(s.begin() + i + 1, s.end());
        return true;
    }

    vector<string> permutation(string s) {
        vector<string> result;
        sort(s.begin(), s.end());
        do {
            result.push_back(s);
        } while (nextPermutation(s));
        return result;
    }
};
```

# 分治算法

## ==[剑指 Offer 07. 重建二叉树](https://leetcode.cn/problems/zhong-jian-er-cha-shu-lcof/)==

### 解法1：分治

$$
O(n^2)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* traversal(const vector<int>& preorder, const vector<int>& inorder, int left1, int right1, int left2, int right2) {
        if (left1 > right1 || left2 > right2) {
            return NULL;
        }
        int mid;
        for (mid = left2; mid <= right2; mid++) {
            if (inorder[mid] == preorder[left1]) {
                break;
            }
        }
        int len1 = mid - left2;
        TreeNode* node = new TreeNode(preorder[left1]);
        node->left = traversal(preorder, inorder, left1 + 1, left1 + len1, left2, mid - 1);
        node->right = traversal(preorder, inorder, left1 + len1+1 , right1, mid + 1, right2);
        return node;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int left1 = 0, right1 = preorder.size() - 1, left2 = 0, right2 = inorder.size() - 1;
        TreeNode* root = traversal(preorder, inorder, left1, right1, left2, right2);
        return root;
    }
};
```

### 解法2：分治+哈希

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    map<int, int> index; //index为map容器，用于记录中序遍历位置
    TreeNode* traversal(const vector<int>& preorder, const vector<int>& inorder, int left1, int right1, int left2, int right2) {
        if (left1 > right1 || left2 > right2) {
            return NULL;
        }
        int mid = index[preorder[left1]];
        int len1 = mid - left2;
        TreeNode* node = new TreeNode(preorder[left1]);
        node->left = traversal(preorder, inorder, left1 + 1, left1 + len1, left2, mid - 1);
        node->right = traversal(preorder, inorder, left1 + len1+1 , right1, mid + 1, right2);
        return node;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int left1 = 0, right1 = preorder.size() - 1, left2 = 0, right2 = inorder.size() - 1;
        for (int i = left2; i <= right2; i++) {
            index[inorder[i]] = i;
        }
        TreeNode* root = traversal(preorder, inorder, left1, right1, left2, right2);
        return root;
    }
};
```

### 解法3：迭代

$$
O(n)+O(n)
$$

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (preorder.size() == 0) {
            return NULL;
        }
        TreeNode* root = new TreeNode(preorder[0]);
        stack<TreeNode*> st;
        int indexInorder = 0;
        st.push(root);
        for (int i = 1; i < preorder.size(); i++) {
            TreeNode* node = st.top();
            if (node->val != inorder[indexInorder]) {
                node->left = new TreeNode(preorder[i]);
                st.push(node->left);
            }
            else {
                while (!st.empty() && st.top()->val == inorder[indexInorder]) {
                    node = st.top();
                    st.pop();
                    indexInorder++;
                }
                node->right = new TreeNode(preorder[i]);
                st.push(node->right);
            }
        }
        return root;
    }
};
```

## ==[剑指 Offer 16. 数值的整数次方](https://leetcode.cn/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)==

### 解法1：快速幂

$$
O(\log n)+O(1)
$$

```C++
class Solution {
public:
    double myPow(double x, int n) {
        double result = 1;
        bool flag = true;
        long long k = n;
        if (k < 0) {
            flag = false;
            k = -(long long)n;
        }
        while (k) {
            if (k & 1) {
                result *= x;
            }
            x *= x;
            k >>= 1;
        }
        if (flag == false) {
            result = 1 / result;
        }
        return result;
    }
};
```

### ==笔记：快速幂模板==

```C++
// 求m^k mod p, 时间复杂度为 O(log k)
long long(int m,int k,int p){
    long long res=1&p,t=m;
    while(k){
        if(k&1) res=res*t%p;
        t=t*t;
        k>>=1;
    }
    return res;
}
```

## ==[剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)==

### 解法1：分治

$$
O(n^2)+O(n)
$$

```C++
class Solution {
public:
    bool verify(vector<int>& postorder,int left,int right){
        if (left >= right) {
            return true;
        }
        int pivot = postorder[right];
        int left1 = left, right1, left2 = left, right2 = right - 1;
        while (postorder[left2] < pivot) {
            left2++;
        }
        right1 = left2 - 1;
        for (int i = left2; i <= right2; i++) {
            if (postorder[i] < pivot) {
                return false;
            }
        }
        return verify(postorder, left1, right1) && verify(postorder, left2, right2);
    }
    bool verifyPostorder(vector<int>& postorder) {
        return verify(postorder, 0, postorder.size() - 1);
    }
};
```

### 解法2：单调栈

$$
O(n)+o(n)
$$

```C++
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        int root = INT_MAX;
        stack<int> st;
        for (int i = postorder.size() - 1; i >= 0; i--) {
            if (postorder[i] > root) {
                return false;
            }
            else {
               while (!st.empty() && st.top() > postorder[i]) {
                   root = st.top();
                   st.pop();
               }
               st.push(postorder[i]);
            }
        }
        return true;
    }
};
```

## ==[剑指 Offer 17. 打印从1到最大的n位数](https://leetcode.cn/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)==

### 解法1：题意

$$
O(10^n)+O(1)
$$

```C++
class Solution {
public:
    vector<int> printNumbers(int n) {
        vector<int> res;
        int count = 0;
        for (int i = 0; i < n; i++) {
            count = count * 10 + 9;
        }
        for (int i = 1; i <= count; i++) {
            res.push_back(i);
        }
        return res;
    }
};
```

### 解法2：大数打印

$$
O(10^n)+O(10^n)
$$

```C++
class Solution {
public:
    vector<int> res;
    vector<int> printNumbers(int n) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= 9; j++) {
                dfs(1, i, to_string(j)); //dfs从1开始，因为第0位已经确定了
            }
        }
        return res;
    }
    void dfs(int k, int n, string s) {
        if (k == n) {
            res.emplace_back(stoi(s));
            return;
        }
        for (int i = 0; i < 10; i++) {
            dfs(k + 1, n, s + to_string(i));
        }
    }
};
```

## ==[剑指 Offer 51. 数组中的逆序对](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)==

### 解法1：归并排序

$$
o(n\log n)+O(n)
$$

```C++
class Solution {
public:
    int count = 0;
    void mergeSort(vector<int>& nums, vector<int>& temp, int left, int right) {
        if (left >= right) {
            return;
        }
        int mid = left + right >> 1;
        mergeSort(nums, temp, left, mid);
        mergeSort(nums, temp, mid + 1, right);
        int cur1 = left, cur2 = mid + 1, cur3 = left;
        while (cur1 <= mid && cur2 <= right) {
            if (nums[cur1] <= nums [cur2]) {
                temp[cur3++] = nums[cur1++];
            }
            else {
                count += mid - cur1 + 1;
                temp[cur3++] = nums[cur2++];
            }
        }
        while (cur1 <= mid) {
            temp[cur3++] = nums[cur1++];
        }
        while (cur2 <= right) {
            temp[cur3++] = nums[cur2++];
        }
        for (int i = left; i <= right; i++) {
            nums[i] = temp[i];
        }
    }
    int reversePairs(vector<int>& nums) {
        vector<int> temp(nums.size(), 0);
        mergeSort(nums, temp, 0 ,nums.size() - 1);
        return count;
    }
};
```

### 解法2：离散化树状数组

$$
O(n\log n)+O(n)
$$

```C++
class BIT {
private:
    vector<int> tree;
    int n;

public:
    BIT(int _n): n(_n), tree(_n + 1) {}

    static int lowbit(int x) {
        return x & (-x);
    }

    int query(int x) {
        int ret = 0;
        while (x) {
            ret += tree[x];
            x -= lowbit(x);
        }
        return ret;
    }

    void update(int x) {
        while (x <= n) {
            ++tree[x];
            x += lowbit(x);
        }
    }
};

class Solution {
public:
    int reversePairs(vector<int>& nums) {
        int n = nums.size();
        vector<int> tmp = nums;
        // 离散化
        sort(tmp.begin(), tmp.end());
        for (int& num: nums) {
            num = lower_bound(tmp.begin(), tmp.end(), num) - tmp.begin() + 1;
        }
        // 树状数组统计逆序对
        BIT bit(n);
        int ans = 0;
        for (int i = n - 1; i >= 0; --i) {
            ans += bit.query(nums[i] - 1);
            bit.update(nums[i]);
        }
        return ans;
    }
};
```

# 排序

## [剑指 Offer 45. 把数组排成最小的数](https://leetcode.cn/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

### 解法1：快速排序

$$
O(n\log n)+O(n)
$$

```C++
class Solution {
public:
    static bool cmp(const string& a,const string &b){
        return a + b < b + a;
    }
    string minNumber(vector<int>& nums) {
        vector<string> temp;
        for (int i = 0; i < nums.size(); i++) {
            temp.push_back(to_string(nums[i]));
        }
        sort(temp.begin(), temp.end(), cmp);
        string result;
        for (int i = 0; i < temp.size(); i++) {
            result += temp[i];
        }
        return result;
    }
};
```

## [面试题61. 扑克牌中的顺子](https://leetcode.cn/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

### 解法1：哈希

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        int count[14] = {0};
        int min = 14, max = 0;
        for (int i = 0; i < nums.size(); i++) {
            count[nums[i]]++;
            if (nums[i] != 0 && nums[i] < min) {
                min = nums[i];
            }
            if (nums[i] != 0 && nums[i] > max) {
                max = nums[i];
            }
        }
        if (count[0] == 5) {
            return true;
        }
        for (int i = min; i <= max; i++) {
            if (count[i] > 1) {
                return false;
            }
            else if (count[i] == 1) {
                continue;
            }
            else if (count[i] == 0 && count[0]) {
                count[0]--;
            }
            else {
                return false;
            }
        }
        return true;
    }
};
```

## ==[剑指 Offer 40. 最小的k个数](https://leetcode.cn/problems/zui-xiao-de-kge-shu-lcof/)==

### 解法1：堆排序

$$
O(n\log k)+O(n)
$$

```C++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        priority_queue<int, vector<int>, greater<int>> heap;
        for (int i = 0; i < arr.size(); i++) {
            heap.push(arr[i]);
        }
        vector<int> result;
        for (int i = 0; i < k; i++) {
            result.push_back(heap.top());
            heap.pop();
        }
        return result;
    }
};
```

```C++
class Solution {
    vector<int> heap;
    int size;
public:
    void down(int u) {
        int t = u;
        if (u * 2 <= size && heap[t] > heap[u * 2]) {
            t = u * 2;
        }
        if (u * 2 + 1 <= size && heap[t] > heap[u * 2 + 1]) {
            t = u * 2 + 1;
        }
        if (u != t) {
            swap(heap[u], heap[t]);
            down(t);
        }
    }
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        heap.resize(arr.size() + 1);
        for (int i = 0; i < arr.size(); i++) {
            heap[i + 1] = arr[i];
        }
        size = arr.size();
        for (int i = size / 2; i > 0; i--) {
            down(i);
        }
        vector<int> result(k);
        for (int i = 0; i < k; i++) {
            result[i] = heap[1];
            swap(heap[1], heap[size]);
            size--;
            down(1);
        }
        return result;
    }
};
```

### 解法2：快速排序

```C++
class Solution {
public:
    void quick_sort(vector<int>& arr, int left, int right, int k) {
        if (left >= right) {
            return;
        }
        int i = left - 1, j = right + 1, x = arr[left + right >> 1];
        while (i < j) {
            while (arr[++i] < x);
            while (arr[--j] > x);
            if (i < j) {
                swap(arr[i], arr[j]);
            }
        }
        if (j < k - 1) {
            quick_sort(arr, j + 1, right, k);
        }
        else if (j > k - 1) {
            quick_sort(arr, left, j, k);
        }
        else {
            return;
        }
    }
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        quick_sort(arr, 0, arr.size() - 1, k);
        vector<int> result(k);
        for (int i = 0; i < k; i++) {
            result[i] = arr[i];
        }
        return result;
    }
};
```

### ==笔记：容器`priority_queue`的使用==

```C++
priority_queue, 优先队列，默认是大根堆
    size()
    empty()
    push()  插入一个元素
    top()  返回堆顶元素
    pop()  弹出堆顶元素
    定义成小根堆的方式：priority_queue<int, vector<int>, greater<int>> q;
```

### ==笔记：堆的手动实现（以小根堆为例）==

```C++
// h[N]存储堆中的值, h[1]是堆顶，x的左儿子是2x, 右儿子是2x + 1
// ph[k]存储第k个插入的点在堆中的位置
// hp[k]存储堆中下标是k的点是第几个插入的
int h[N], ph[N], hp[N], size;

// 交换两个点，及其映射关系
void heap_swap(int a, int b)
{
    swap(ph[hp[a]],ph[hp[b]]);
    swap(hp[a], hp[b]);
    swap(h[a], h[b]);
}

void down(int u)
{
    int t = u;
    if (u * 2 <= size && h[u * 2] < h[t]) t = u * 2;
    if (u * 2 + 1 <= size && h[u * 2 + 1] < h[t]) t = u * 2 + 1;
    if (u != t)
    {
        heap_swap(u, t);
        down(t);
    }
}

void up(int u)
{
    while (u / 2 && h[u] < h[u / 2])
    {
        heap_swap(u, u / 2);
        u >>= 1;
    }
}

// O(n)建堆
for (int i = n / 2; i; i -- ) down(i);
```

### ==笔记：快速排序模板==

```C++
模板1：每次以q[l+r>>1]为枢轴，分成[l,j]和[j+1,r]两个区间,左区间≤枢轴，右区间≥枢轴
void quick_sort1(int q[], int l, int r)
{
	if (l >= r) return;

	int i = l - 1, j = r + 1, x = q[l + r >> 1];//i开始取l-1，j开始取r+1
	while (i < j)
	{
		while (q[++i] < x);//注意是++i
		while (q[--j] > x);//注意是--j
		if (i < j) swap(q[i], q[j]);//i<j这个条件别忘了
	}

	quick_sort1(q, l, j), quick_sort1(q, j + 1, r);//j不要写成i
}

模板2：每次以q[l]为枢轴，分成[l,i-1]与[i+1,r]两个区间和一个枢轴（位置在i）,左区间≤枢轴，右区间≥枢轴
void quick_sort2(int q[], int l, int r)
{
	if (l >= r) return;

	int i = l, j = r, x = q[l];//以第一个元素作为枢轴
	while (i < j)//多次出现的i<j，一个也不能落下，为了控制最后i=j
	{
		while (i < j && q[j] >= x) j--;//先从右边开始，遇到≥枢轴的，j左移（找第一个小于枢轴的元素）
		q[i] = q[j];//更改q[i]
		while (i < j && q[i] <= x) i++;//再从左边开始，遇到≤枢轴的，i右移（找第一个大于枢轴的元素）
		q[j] = q[i];//更改q[j]
	}
	q[i] = x;//最后放置枢轴这步别忘了

	quick_sort2(q, l, i - 1), quick_sort2(q, i + 1, r);
}
```

## [剑指 Offer 41. 数据流中的中位数](https://leetcode.cn/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

### 解法1：堆排序

$$
O(n\log n) + O(n)
$$

```C++
class MedianFinder {
    priority_queue<int, vector<int>, less<int>> maxHeap;
    priority_queue<int, vector<int>, greater<int>> minHeap;
public:
    /** initialize your data structure here. */
    MedianFinder() {

    }
    
    void addNum(int num) {
        if (maxHeap.empty() || num <= maxHeap.top()) {
            maxHeap.push(num);
            while (maxHeap.size() > minHeap.size() + 1) {
                minHeap.push(maxHeap.top());
                maxHeap.pop();
            }
        }
        else {
            minHeap.push(num);
            while (maxHeap.size() < minHeap.size()) {
                maxHeap.push(minHeap.top());
                minHeap.pop();
            }
        }
    }
    
    double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.top() * 1.0;
        }
        else {
            return (maxHeap.top() + minHeap.top()) * 1.0 / 2;
        }
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

# 动态规划

## [剑指 Offer 10- I. 斐波那契数列](https://leetcode.cn/problems/fei-bo-na-qi-shu-lie-lcof/)

### 解法1：递推

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int fib(int n) {
        int N = 1e9 + 7;
        if (n <= 1) {
            return n;
        }
        int a = 0, b = 1;
        for (int i = 2; i <= n; i++) {
            int temp = b;
            b = (a + b) % N;
            a = temp;
        }
        return b;
    }
};
```

## [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode.cn/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int numWays(int n) {
        int dp[101], N = 1e9 + 7;
        dp[0] = 1, dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = (dp[i - 2] + dp[i - 1]) % N;
        }
        return dp[n];
    }
};
```

## [剑指 Offer 63. 股票的最大利润](https://leetcode.cn/problems/gu-piao-de-zui-da-li-run-lcof/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() == 0) {
            return 0;
        }
        vector<vector<int>> dp(prices.size(), vector<int>(2, 0));
        dp[0][0] = -prices[0], dp[0][1] = 0;
        for (int i = 1; i < prices.size(); i++) {
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i][0] + prices[i]);
        }
        return dp[prices.size() - 1][1];
    }
};
```

### 解法2：贪心

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() == 0) {
            return 0;
        }
        int minPrice = prices[0], result = 0;
        for (int i = 0; i < prices.size(); i++){
            result = max(result, prices[i] - minPrice);
            minPrice = min(minPrice, prices[i]);
        }
        return result;
    }
};
```

## [剑指 Offer 42. 连续子数组的最大和](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int> dp(nums.size() + 1, 0);
        int result = -10010;
        for (int i = 1; i <= nums.size(); i++) {
            dp[i] = max(nums[i - 1], dp[i - 1] + nums[i - 1]);
            result = max(result, dp[i]);
        }
        return result;
    }
};
```

### 解法1：贪心

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result = -10010, sum = 0;
        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i];
            result = max(result, sum);
            if (sum < 0) {
                sum = 0;
            }
        }
        return result;
    }
};
```

## [剑指 Offer 47. 礼物的最大价值](https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof/)

### 解法1：动态规划

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        vector<vector<int>> dp(grid.size(), vector<int>(grid[0].size(), 0));
        int m = grid.size(), n = grid[0].size();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i && j) {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
                }
                else if (i) {
                    dp[i][j] = dp[i - 1][j] + grid[i][j];
                }
                else if (j) {
                    dp[i][j] = dp[i][j - 1] + grid[i][j];
                }
                else {
                    dp[i][j] = grid[i][j];
                }
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

## ==[剑指 Offer 46. 把数字翻译成字符串](https://leetcode.cn/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)==

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int translateNum(int num) {
        string str = to_string(num);
        vector<int> dp(str.length() + 1);
        dp[0] = 1;
        for (int i = 0; i < str.length(); i++) {
            dp[i + 1] = dp[i];
            if (i > 0 && (str[i - 1] == '1' || str[i - 1] == '2' && str[i] <= '5')) {
                dp[i + 1] += dp[i - 1];
            }
        }
        return dp[str.length()];
    }
};
```

### ==笔记：字符串和数字的相互转换==

字符串转数字：

```C++
//使用stoi()函数
string s("12345");
long long a = stoi(s);
cout << a << endl;

//使用atoi()函数
char str3[10] = "3245345";
//数字简单，所以转数字一个参数 
long long a = atoi(str3);  
cout << a << endl;
```

数字转字符串：

```C++
//使用to_string()函数
long long m = 1234566700;
string str = to_string(m);   //系统提供数字转字符 
cout << str << endl;

//使用itoa()函数
int n = 100;
char str2[10];
itoa(n,str2,10);
cout << str2 << endl;
```

## [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode.cn/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

### 解法1：滑动窗口+哈希

$$
O(n)+O(C)
$$

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> count;
        int j = 0;
        int result = 0;
        for (int i = 0; i < s.size(); i++) {
            while (j < s.size() && count[s[j]] == 0){
                count[s[j]]++;
                result = max(result, j - i + 1);
                j++;
            }
            count[s[i]]--;
        }
        return result;
    }
};
```

## ==[剑指 Offer 19. 正则表达式匹配](https://leetcode.cn/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)==

### 解法1：动态规划

```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
        dp[0][0] = true;
        for (int i = 0; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (i >= 1 && (s[i - 1] == p[j - 1] || p[j - 1] == '.')) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else if (p[j - 1] == '*' && j >= 2) {
                    dp[i][j] = dp[i][j] || dp[i][j - 2];
                    if (i >= 1 && (s[i - 1] == p[j - 2] || p[j - 2] == '.')) {
                        dp[i][j] = dp[i][j] || dp[i - 1][j];
                    }
                }
            }
        }
        return dp[m][n];
    }
};
```

## [剑指 Offer 49. 丑数](https://leetcode.cn/problems/chou-shu-lcof/)

### 解法1：堆排序

$$
O(n\log n)+O(n)
$$

```C++
class Solution {
public:
    int nthUglyNumber(int n) {
        unordered_set<long long> uset;
        priority_queue<long long, vector<long long>, greater<long long>> heap;
        heap.push(1L);
        uset.insert(1L);
        int factor[3] = {2, 3, 5};
        long long result;
        for (int i = 0; i < n; i++) {
            result = heap.top();
            heap.pop();
            for (int j = 0; j < 3; j++) {
                if (uset.find(result * factor[j]) == uset.end()) {
                    heap.push(result * factor[j]);
                    uset.insert(result * factor[j]);
                }
            }
        }
        return (int)result;
    }
};
```

### 解法2：动态规划

```C++
class Solution {
public:
    int nthUglyNumber(int n) {
        int a = 0, b = 0, c = 0;
        vector<int> dp(n, 0);
        dp[0] = 1;
        for (int i = 1; i < n; i++) {
            int n1 = dp[a] * 2, n2 = dp[b] * 3, n3 = dp[c] * 5;
            dp[i] = min(min(n1, n2), n3);
            if (dp[i] == n1) {
                a++;
            }
            if (dp[i] == n2) {
                b++;
            }
            if (dp[i] == n3) {
                c++;
            }
        }
        return dp[n - 1];
    }
};
```

## [剑指 Offer 60. n个骰子的点数](https://leetcode.cn/problems/nge-tou-zi-de-dian-shu-lcof/)

### 解法1：动态规划

$$
O(n^2)+O(n^2)
$$

```C++
class Solution {
public:
    vector<double> dicesProbability(int n) {
        vector<double> result(6, 1.0 / 6.0);
        for (int i = 2; i <= n; i++) {
            vector<double> temp(i * 5 + 1, 0);
            for (int j = 0; j < temp.size(); j++) {
                for (int k = max(j - 5, 0); k <= min(j, (int)result.size() - 1); k++) { // (int)result.size() - 1 里的 (int)不能省，否则会报错
                    temp[j] += result[k] * 1.0 / 6.0;
                }
            }
            result = temp;
        }
        return result;
    }
};
```

# 位运算

## [剑指 Offer 15. 二进制中1的个数](https://leetcode.cn/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

### 解法1：位运算

$$
O(\log n)+O(1)
$$

```C++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while (n) {
            if (n & 1) {
                count++;
            }
            n >>= 1;
        }
        return count;
    }
};
```

## [剑指 Offer 65. 不用加减乘除做加法](https://leetcode.cn/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

### 解法1：位运算

$$
O(\log n)+O(1)
$$

```C++
class Solution {
public:
    int add(int a, int b) {
        while (b) {
            unsigned carry = (unsigned)(a & b) << 1;
            a = a ^ b;
            b = carry;
        }
        return a;
    }
};
```

## [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

### 解法1：位运算

$$
O(\log n)+O(1)
$$

```C++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int ret = 0;
        for (int n: nums) {
            ret ^= n;
        }
        int div = 1;
        while ((div & ret) == 0) {
            div <<= 1;
        }
        int a = 0, b = 0;
        for (int n: nums) {
            if (div & n) {
                a ^= n;
            }
            else {
                b ^= n;
            }
        }
        return vector<int>{a, b};
    }
};
```

## [剑指 Offer 56 - II. 数组中数字出现的次数 II](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

### 解法1：位运算

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        vector<int> count(31, 0);
        for (int n: nums) {
            for (int j = 0; j < 31; j++) {
                count[j] += n & 1;
                n >>= 1;
            }
        }
        int res = 0;
        for (int j = 30; j >= 0; j--) {
            res <<= 1;
            res += count[j] % 3;
        }
        return res;
    }
};
```

# 数学

## [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode.cn/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

### 解法1：哈希

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        map<int, int> count;
        for (int i = 0; i < nums.size(); i++) {
            count[nums[i]]++;
            if (count[nums[i]] > nums.size() / 2) {
                return nums[i];
            }
        }
        return -1;
    }
};
```

### 解法2：摩尔投票

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int result = nums[0], count = 1;
        for (int i = 1; i < nums.size(); i++) {
            if (count == 0) {
                result = nums[i];
            }
            if (result == nums[i]) {
                count++;
            }
            else {
                count--;
            }
        }
        return result;
    }
};
```

## [剑指 Offer 66. 构建乘积数组](https://leetcode.cn/problems/gou-jian-cheng-ji-shu-zu-lcof/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        vector<int> left(a.size(), 1), right(a.size(), 1);
        for (int i = 1; i <a.size(); i++) {
            left[i] =left[i - 1] * a[i - 1];
        }
        for (int i = a.size() - 2; i >= 0; i--) {
            right[i] = right[i + 1] * a[i + 1];
        }
        vector<int> result(a.size(), 0);
        for (int i = 0; i < a.size(); i++) {
            result[i] = left[i] * right[i];
        }
        return result;
    }
};
```

### 解法2：左右乘积列表优化

```C++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        vector<int> result(a.size(), 1);
        for (int i = 1; i < a.size(); i++) {
            result[i] = result[i - 1] * a[i - 1];
        }
        int r = 1;
        for (int i = a.size() - 1; i >= 0; i--) {
            result[i] = result[i] * r;
            r *= a[i];
        }
        return result;
    }
};
```

## [剑指 Offer 14- I. 剪绳子](https://leetcode.cn/problems/jian-sheng-zi-lcof/)

### 解法1：数学

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int cuttingRope(int n) {
        if (n == 2) {
            return 1;
        }
        if (n == 3) {
            return 2;
        }
        if (n == 4) {
            return 4;
        }
        int result = 1;
        while (n > 4){
            result *= 3;
            n -= 3;
        }
        result *= n;
        return result;
    }
};
```

## [剑指 Offer 14- II. 剪绳子 II](https://leetcode.cn/problems/jian-sheng-zi-ii-lcof/)

### 解法1：数学

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int cuttingRope(int n) {
        int mod = 1e9 + 7;
        if (n == 2) {
            return 1;
        }
        if (n == 3) {
            return 2;
        }
        if (n == 4) {
            return 4;
        }
        int result = 1;
        while (n > 4) {
            int temp = result;
            result = (result + temp) % mod;
            result = (result + temp) % mod;
            n -= 3;
        }
        int temp = result;
        for (int i = 0; i < n - 1; i++) {
            result = (result + temp) % mod;
        }
        return result;
    }
};
```

## [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode.cn/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

### 解法1：滑动窗口

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>> result;
        int j = 1, sum = 1;
        for (int i = 1; i <= target / 2 + 1; i++) {
            while (sum < target) {
                j++;
                sum += j;
            }
            if (sum == target) {
                vector<int> temp(j - i + 1);
                for (int k = 0; k < temp.size(); k++) {
                    temp[k] = k + i;
                }
                result.push_back(temp);
            }
            sum -= i;
        }
        return result;
    }
};
```

## [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

### 解法1：数学+递归

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int lastRemaining(int n, int m) {
        if (n == 1) {
            return 0;
        }
        else {
            int x = lastRemaining(n - 1, m);
            return (m + x) % n;
        }
    }
};
```

### 解法2：数学+迭代

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int lastRemaining(int n, int m) {
        int x = 0;
        for (int i = 2; i <= n; i++) {
            x = (m + x) % i;
        }
        return x;
    }
};
```

## [剑指 Offer 43. 1～n 整数中 1 出现的次数](https://leetcode.cn/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

### 解法1：数学

$$
O(\log n)+O(C)
$$

```C++
class Solution {
public:
    int countDigitOne(int n) {
        unsigned int count[10];
        for (int i = 0; i < 10; i++) {
            count[i] = (unsigned int)pow(10, i - 1) * i;
        }
        unsigned int result = 0;
        int num[10], temp = n;
        for (int i = 0; i < 10; i++) {
            num[i] = temp % 10;
            temp /= 10;
        }
        temp = 0;
        int front = 0;
        for (int i = 9; i >= 0; i--) {
            if (num[i] == 0);
            else if (num[i] == 1) {
                result += front * (unsigned)pow(10, i) * num[i];
                result += count[i] + 1;
                front++;
            }
            else {
                result += front * (unsigned)pow(10, i) * num[i];
                result += num[i] * count[i] + (unsigned)pow(10, i);
            }
        }

        return (int)result;
    }
};
```





## [剑指 Offer 44. 数字序列中某一位的数字](https://leetcode.cn/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)（==待做==）
