## [1. 两数之和](https://leetcode.cn/problems/two-sum/)

### 解法1：哈希

$$
O(n)+O(n)
$$

```c
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        for(int i=0;i<nums.size();i++)
        {
            auto it=hash.find(target-nums[i]);
            if(it!=hash.end()) return {it->second,i};
            hash.insert({nums[i],i});
        }
        return vector<int>();
    }
};
```

## [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

### 解法1：

$$
O(m+n)+O(1)
$$

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *p1=l1,*p2=l2,*dummyHead=new ListNode(0),*p=dummyHead;
        int carry=0;
        while(p1&&p2){
            int t=(p1->val+p2->val+carry)%10;
            carry=(p1->val+p2->val+carry)/10;
            ListNode* newNode=new ListNode(t);
            p->next=newNode;
            p=p->next;
            p1=p1->next;
            p2=p2->next;
        }
        if(p2) p1=p2;
        while(p1){
            int t=(p1->val+carry)%10;
            carry=(p1->val+carry)/10;
            ListNode* newNode=new ListNode(t);
            p->next=newNode;
            p=p->next;
            p1=p1->next;
        }
        if(carry){
            ListNode* newNode=new ListNode(carry);
            p->next=newNode;
            p=p->next;
        }

        return dummyHead->next;
    }
};
```

## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

### 解法1：滑动窗口+哈希

$$
O(n)+O(C)//C为字符集的大小
$$

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char,int> hash;
        int i=0,j=0;
        int max_length=0,count=0;
        for(j=0;j<s.length();j++){
            while(hash[s[j]]>0){
                hash[s[i]]--;
                i++;
                count--;
            }
            count++;
            hash[s[j]]=1;
            max_length=max(max_length,count);
        }
        return max_length;
    }
};
```

## ==[4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)==

### 解法1：二分查找（左闭右开）

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if(nums1.size()>nums2.size()) swap(nums1,nums2);
        int m=nums1.size(),n=nums2.size();
        int left=0,right=m;
        int totalLeft=(m+n+1)>>1;
        while(left<right){
            int i=(left+right+1)>>1;
            int j=totalLeft-i;
            if(nums1[i-1]>nums2[j]) right=i-1;
            else left=i;
        }
        int i=left,j=totalLeft-i;
        int leftMaxNums1=(i==0?INT_MIN:nums1[i-1]);
        int leftMaxNums2=(j==0?INT_MIN:nums2[j-1]);
        int rightMinNums1=(i==m?INT_MAX:nums1[i]);
        int rightMinNums2=(j==n?INT_MAX:nums2[j]);

        if((m+n)%2) return (double)max(leftMaxNums1,leftMaxNums2);
        else return (double)(max(leftMaxNums1,leftMaxNums2)+min(rightMinNums1,rightMinNums2))/2.0;

        return 0;
    }
};
```

### ==笔记：整数二分查找中的左闭右闭和左闭右开==

**情况一：数组长度为偶数**

长度为8的数组，$[0,1,2,3,4,5,6,7]$，从中间切分为$[0,1,2,3]$和$[4,5,6,7]$

如果按**左闭右闭**查找，那么 $left=0,right=7$，若划分为 $[left,mid]$ 和 $[mid+1,right]$ 两个区间，则 $mid=\frac{left+right}{2}=3$，划分结果为$[0,1,2,3]$和$[4,5,6,7]$ ；若划分为 $[left,mid-1]$ 和 $[mid,right]$ 两个区间，则 $mid=\frac{left+right+1}{2}=4$，划分结果也为$[0,1,2,3]$和$[4,5,6,7]$ 

如果按**左闭右开**查找，那么 $left=0,right=8$，必然划分为 $[left,mid)$ 和 $[mid,right)$ 两个区间，若 $mid=\frac{left+right}{2}=4$，划分结果为$[0,1,2,3]$和$[4,5,6,7]$ ；若 $mid=\frac{left+right+1}{2}=4$，划分结果也为$[0,1,2,3]$和$[4,5,6,7]$ ；两种情况相同

**情况一：数组长度为奇数**

长度为9的数组，$[0,1,2,3,4,5,6,7,8]$，从中间切分为$[0,1,2,3]$,$[4]$和$[5,6,7,8]$

如果按**左闭右闭**查找，那么 $left=0,right=8$，若划分为 $[left,mid]$ 和 $[mid+1,right]$ 两个区间，则 $mid=\frac{left+right}{2}=4$，划分结果为$[0,1,2,3,4]$和$[5,6,7,8]$ ；若划分为 $[left,mid-1]$ 和 $[mid,right]$ 两个区间，则 $mid=\frac{left+right+1}{2}=4$，划分结果为$[0,1,2,3]$和$[4,5,6,7,8]$ 

如果按**左闭右开**查找，那么 $left=0,right=9$，必然划分为 $[left,mid)$ 和 $[mid,right)$ 两个区间，若 $mid=\frac{left+right}{2}=4$，划分结果为$[0,1,2,3]$和$[4,5,6,7,8]$ ；若 $mid=\frac{left+right+1}{2}=5$，划分结果为$[0,1,2,3,4]$和$[5,6,7,8]$ 

**总结**

数组长度为偶数，左闭右闭，mid的两个公式分别定位到中线左边和右边，划分结果都是对半开；左闭右开，mid的两个公式都定位到中线右边，划分结果都是对半开

数组长度为奇数，左闭右闭，mid的两个公式都定位到中间；左闭右开，mid的两个公式，一个定位到中间，一个定位到中间右偏一个位置

## [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

### 解法1：动态规划

$$
O(n^2)+O(n^2)
$$

```C++
class Solution {
public:
    string longestPalindrome(string s) {
        vector<vector<bool>> dp(s.length(),vector<bool>(s.length(),false));
        int maxLength=0;
        string result;
        for(int i=s.length()-1;i>=0;i--){
            for(int j=i;j<s.length();j++){
                if(j-i<=1) dp[i][j]=(s[i]==s[j]);
                else if(s[i]==s[j]) dp[i][j]=dp[i+1][j-1];
                else dp[i][j]=false;
                if(dp[i][j]&&maxLength<j-i+1){
                    maxLength=j-i+1;
                    result=s.substr(i,j-i+1);
                }
            }
        }
        return result;
    }
};
```

### 解法2：中心扩散

$$
O(n^2)+O(1)
$$

```C++
class Solution {
public:
    string longestPalindrome(string s) {
        int maxLength=0;
        string result;
        for(int i=0;i<s.length();i++){
            int left=i,right=i;
            while(true){
                if(left==0||right==s.length()-1||s[left-1]!=s[right+1]) break;
                else left--,right++;
            }
            if(right-left+1>maxLength){
                maxLength=right-left+1;
                result=s.substr(left,right-left+1);
            }
            left=i,right=i;
            if(i+1<s.length()&&s[i]==s[i+1]){
                right++;
                while(true){
                    if(left==0||right==s.length()-1||s[left-1]!=s[right+1]) break;
                    else left--,right++;
                }
                if(right-left+1>maxLength){
                    maxLength=right-left+1;
                    result=s.substr(left,right-left+1);
                }
            }
        }
        return result;
    }
};
```

## [10. 正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/)

### 解法1：动态规划

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m=s.size(),n=p.size();
        vector<vector<bool>> dp(m+1,vector<bool>(n+1,false));
        dp[0][0]=true;
        for(int i=0;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(i>=1&&(s[i-1]==p[j-1]||p[j-1]=='.')) dp[i][j]=dp[i-1][j-1];
                else if(p[j-1]=='*'&&j>=2){
                    dp[i][j]=dp[i][j]||dp[i][j-2];
                    if(i>=1&&(s[i-1]==p[j-2]||p[j-2]=='.')) dp[i][j]=dp[i][j]||dp[i-1][j];
                }
            }
        }
        return dp[m][n];
    }
};
```

## [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

### 解法1：双指针

```C++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left=0,right=height.size()-1;
        int result=0;
        while(left<right){
            int area=min(height[left],height[right])*(right-left);
            result=max(result,area);
            if (height[left]<=height[right]) left++;
            else right--;
        }
        return result;
    }
};
```

## [15. 三数之和](https://leetcode.cn/problems/3sum/)

### 解法1：排序+三指针

$$
O(n^2)+O(1)
$$

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> ans;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]>0) return ans;
            if(i>0&&nums[i]==nums[i-1]) continue;
            int j=i+1,k=nums.size()-1;
            while(j<k)
            {
                if(nums[i]+nums[j]+nums[k]==0)
                {
                    ans.push_back({nums[i],nums[j],nums[k]}),j++,k--;
                    while(j<k&&nums[j]==nums[j-1]) j++;
                    while(k>j&&nums[k]==nums[k+1]) k--;
                }
                else if(nums[i]+nums[j]+nums[k]<0) j++;
                else k--;
            }
        }
        return ans;
    }
};
```

## [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

### 解法1：回溯

$$
O(3^m·4^n)+O(m+n)
$$

```C++
class Solution {
public:
    vector<string> telephone={"abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
    vector<string> result;
    string path;
    void backtracking(int index,string digits)
    {
        if(index==digits.size())
        {
            result.push_back(path);
            return;
        }
        string s=telephone[digits[index]-'0'-2];
        for(int i=0;i<s.size();i++)
        {
            path.push_back(s[i]);
            backtracking(index+1,digits);
            path.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        if(digits.size()==0) return {};
        backtracking(0,digits);
        return result;
    }
};
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

### 解法1：快慢指针

$$
O(n)+O(1)
$$

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummyHead=new ListNode();
        dummyHead->next=head;
        ListNode *pre=dummyHead,*p=head,*q=head;
        while(n--) q=q->next;
        while(q!=NULL) pre=pre->next,p=p->next,q=q->next;
        pre->next=p->next;
        return dummyHead->next;
    }
};
```

## [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

### 解法1：栈

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    bool isValid(string s) {
        map<char,char> m;
        m['(']=')',m['[']=']',m['{']='}';
        stack<char> st;
        for(int i=0;i<s.length();i++)
        {
            if(s[i]=='('||s[i]=='['||s[i]=='{') st.push(s[i]);
            else
            {
                if(st.empty()||m[st.top()]!=s[i]) return false;
                st.pop();
            }
        }
        if(st.empty()) return true;
        else return false;
    }
};
```

## [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

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
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode *dummyHead=new ListNode(0),*p=dummyHead;
        while(list1&&list2){
            if(list1->val<=list2->val){
                p->next=list1;
                list1=list1->next;
            }
            else{
                p->next=list2;
                list2=list2->next;
            }
            p=p->next;
        }
        if(list1) p->next=list1;
        else p->next=list2;
        return dummyHead->next;
    }
};
```

## [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/)

### 解法1：回溯

$$
略
$$

```C++
class Solution {
public:
    vector<string> result;
    void backtracking(string s,int left,int right,int n){
        if(left==n&&right==n){
            result.push_back(s);
            return;
        }
        if(left+1<=n) backtracking(s+"(",left+1,right,n);
        if(left>right) backtracking(s+")",left,right+1,n);
    }
    vector<string> generateParenthesis(int n) {
        backtracking("",0,0,n);
        return result;
    }
};
```

## ==[23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)==

### 解法1：k阶归并

$$
O(nk^2)+O(1)
$$

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* dummyHead=new ListNode(),* p=dummyHead;
        while(true){
            int i;
            for(i=0;i<lists.size();i++){
                if(lists[i]) break;
            }
            if(i==lists.size()) break;
            int minList=-1;
            for(int i=0;i<lists.size();i++){
                if(lists[i]){
                    if(minList==-1) minList=i;
                    else minList=(lists[minList]->val<=lists[i]->val?minList:i);
                }
                else continue;
            }
            p->next=lists[minList];
            p=p->next;
            lists[minList]=lists[minList]->next;
        }
        p->next=NULL;
        return dummyHead->next;
    }
};
```

### 解法2：堆

$$
O(nk(\log k))+O(\log k)
$$

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    struct cmp {
        bool operator()(ListNode* a, ListNode* b) {
            return a->val > b->val;
        }
    };

    priority_queue<ListNode*, vector<ListNode*>, cmp> q;

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        for (auto node:lists) {
            if(node) q.push(node);
        }
        ListNode* head = new ListNode();
        ListNode* tail = head;
        while (!q.empty()) {
            ListNode* node = q.top();
            q.pop();
            tail->next = node; 
            tail = tail->next;
            if (node->next) q.push(node->next);
        }
        return head->next;
    }
};
```

### ==优先队列`priori_queue`自定义比较==

方法1：结构体中重载<符号

```C++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <string>
#include <queue>
using namespace std;

struct fruit
{
	int price;

	// 自定义比较函数的第一种方式：重载<符号
	// 注意此处一定要声明为const，否则编译时无法通过
	// const修饰的是类函数隐藏的第一个参数 this指针，这表明this指针只读，也即类成员不可修改
	// 注意该用法只能是成员函数，要是类的静态函数或者是非成员函数就不可以在函数名后面加上const
	bool operator<(const fruit& f1) const {
		return this->price < f1.price;//大顶堆的写法
	}

	// 如果不使用const函数，使用以下友元函数也可行
	/*friend bool operator<(const fruit& f1, const fruit& f2)
	{
		return f1.price < f2.price;
	}*/
};

int main()
{
	fruit a;
	fruit b;
	fruit c;

	a.price = 11;
	b.price = 12;
	c.price = 4;

	// 第一种方式：结构体内重载<符号
	priority_queue<fruit> q;	
	q.push(a);
	q.push(b);
	q.push(c);
	while (!q.empty())
	{
		cout << "q1 top price = " << q.top().price << endl;
		q.pop();
	}

	return 0;
}
```

结果：

```C++
q1 top price = 12
q1 top price = 11
q1 top price = 4
```

方法2：创建一个结构体，重载()符号

```C++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <string>
#include <queue>
using namespace std;

// 自定义比较函数的第二种方式：结构体
struct cmp {
	bool operator()(const fruit& f1, const fruit& f2) {
		return f1.price > f2.price;//小顶堆的写法
	}
};

int main()
{
	fruit a;
	fruit b;
	fruit c;

	a.price = 11;
	b.price = 12;
	c.price = 4;

	// 第二种方式：自定义结构体，重载()符号
	priority_queue<fruit, vector<fruit>, cmp> q2; 
	a.price = 3;
	b.price = 2;
	c.price = 7;

	q2.push(a);
	q2.push(b);
	q2.push(c);
	while (!q2.empty())
	{
		cout << "q2 top price = " << q2.top().price << endl;
		q2.pop();
	}

	return 0;
}
```

结果：

```C++
q2 top price = 2
q2 top price = 3
q2 top price = 7
```

## [31. 下一个排列](https://leetcode.cn/problems/next-permutation/)

### 解法1：两次扫描

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int i=nums.size()-2;
        while(i>=0&&nums[i]>=nums[i+1]) i--;
        if (i>=0){
            int j=nums.size()-1;
            while(j>=0&&nums[i]>=nums[j]) j--;
            swap(nums[i], nums[j]);
        }
        reverse(nums.begin()+i+1,nums.end());
    }
};
```

## [32. 最长有效括号](https://leetcode.cn/problems/longest-valid-parentheses/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        vector<int> dp(s.length(),0);
        int maxLength=0;
        for(int i=0;i<s.length();i++){
            if(s[i]=='(') dp[i]=0;
            else{
                if(i>0&&i-dp[i-1]-1>=0){
                    if(s[i-dp[i-1]-1]=='('){
                        dp[i]=dp[i-1]+2;
                        if(i-dp[i]>=0) dp[i]+=dp[i-dp[i]];
                    }
                }
            }
            maxLength=max(maxLength,dp[i]);
        }
        return maxLength;
    }
};
```

### 解法2：栈

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        int maxLength=0;
        stack<int> st;
        st.push(-1);
        for(int i=0;i<s.length();i++){
            if(s[i]=='(') st.push(i);
            else{
                st.pop();
                if(st.empty()) st.push(i);
                else maxLength=max(maxLength,i-st.top());
            }
        }
        return maxLength;
    }
};
```

### 解法3：两次扫描

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        int left=0,right=0,maxLength=0;
        for(int i=0;i<s.length();i++){
            if(s[i]=='(') left++;
            else right++;
            if(left==right) maxLength=max(maxLength,2*left);
            else if(left<right) left=right=0;
        }
        left=right=0;
        for(int i=s.length()-1;i>=0;i--){
            if(s[i]=='(') left++;
            else right++;
            if(left==right) maxLength=max(maxLength,2*left);
            else if(left>right) left=right=0;
        }
        return maxLength;
    }
};
```

## [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

### 解法1：二分查找

$$
O(\log n)+O(1)
$$

```C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left=0,right=nums.size()-1;
        int beginNum=nums[0],endNum=nums[nums.size()-1];
        while(left<right){
            int mid=left+right>>1;
            if(nums[mid]<=endNum) right=mid;
            else left=mid+1;
        }
        int minNum=nums[left];
        if(left>0&&target>=beginNum) right=left-1,left=0;
        else right=nums.size()-1;
        while(left<right){
            int mid=left+right>>1;
            if(nums[mid]>=target) right=mid;
            else left=mid+1;
        }
        if(nums[left]==target) return left;
        else return -1;
    }
};
```

## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

### 解法1：二分查找

$$
O(\log n)+O(1)
$$

```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> lr;
        if(nums.size()==0)
        {
            lr.push_back(-1),lr.push_back(-1);
            return lr;
        }
        
        int l=0,r=nums.size()-1;
        while(l<r)
        {
            int mid=l+(r-l>>1);
            if(nums[mid]>=target) r=mid;
            else l=mid+1;
        }
        if(nums[l]==target) lr.push_back(l);
        else
        {
            lr.push_back(-1),lr.push_back(-1);
            return lr;
        }

        l=0,r=nums.size()-1;
        while(l<r)
        {
            int mid=l+(r-l+1>>1);
            if(nums[mid]<=target) l=mid;
            else r=mid-1;
        }
        lr.push_back(l);

        return lr;
    }
};
```

## [39. 组合总和](https://leetcode.cn/problems/combination-sum/)

### 解法1：回溯

$$
略
$$

```c++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    int sum=0;
    void backtracking(int index,vector<int> &candidates,int target)
    {
        if(sum==target)
        {
            result.push_back(path);
            return;
        }
        for(int i=index;i<candidates.size();i++)
        {
            if(sum+candidates[i]<=target)
            {
                sum+=candidates[i];
                path.push_back(candidates[i]);
                backtracking(i,candidates,target);
                sum-=candidates[i];
                path.pop_back();
            }
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        backtracking(0,candidates,target);
        return result;
    }
};
```

## [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        if(height.size()<=2) return 0;
        int sum=0;
        vector<int> left_height(height.size(),0),right_height(height.size(),0);
        left_height[0]=height[0],right_height[height.size()-1]=height[height.size()-1];
        for(int i=1;i<height.size();i++) left_height[i]=max(height[i],left_height[i-1]);
        for(int i=height.size()-2;i>=0;i--) right_height[i]=max(height[i],right_height[i+1]);
        for(int i=1;i<=height.size()-2;i++) sum+=min(left_height[i],right_height[i])-height[i];
        return sum;
    }
};
```

### 解法2：单调栈

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        stack<int> st;
        int sum=0;
        for(int i=0;i<height.size();i++){
            while(!st.empty()&&height[i]>height[st.top()]){
                int mid=st.top();
                st.pop();
                if(!st.empty()){
                    int h=min(height[i],height[st.top()])-height[mid];
                    int w=i-st.top()-1;
                    sum+=h*w;
                }
            }
            st.push(i);
        }
        return sum;
    }
};
```

### 解法3：双指针

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int left=0,right=height.size()-1;
        int leftMax=0,rightMax=0;
        int sum=0;
        while(left<right){
            leftMax=max(leftMax,height[left]);
            rightMax=max(rightMax,height[right]);
            if(leftMax<rightMax){
                sum+=leftMax-height[left];
                left++;
            }
            else{
                sum+=rightMax-height[right];
                right--;
            }
        }
        return sum;
    }
};
```

## [46. 全排列](https://leetcode.cn/problems/permutations/)

### 解法1：回溯

$$
O(n×n!)+O(n)
$$

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    bool used[21];
    void backtracking(const vector<int> &nums)
    {
        if(path.size()==nums.size())
        {
            result.push_back(path);
            return;
        }
        for(int i=0;i<nums.size();i++)
        {
            if(used[nums[i]+10]) continue;
            used[nums[i]+10]=true;
            path.push_back(nums[i]);
            backtracking(nums);
            used[nums[i]+10]=false;
            path.pop_back();
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        memset(used,false,sizeof(used));
        backtracking(nums);
        return result;
    }
};
```

## [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)

### 解法1

$$
O(n^2)+O(1)
$$

```C++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n=matrix.size();
        for(int i=0;i<n/2;i++){
            for(int j=i;j<n-1-i;j++){
                int temp=matrix[i][j];
                matrix[i][j]=matrix[n-1-j][i];
                matrix[n-1-j][i]=matrix[n-1-i][n-1-j];
                matrix[n-1-i][n-1-j]=matrix[j][n-1-i];
                matrix[j][n-1-i]=temp;
            }
        }
    }
};
```



