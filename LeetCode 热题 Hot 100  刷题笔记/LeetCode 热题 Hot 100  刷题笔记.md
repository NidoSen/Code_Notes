#哈希
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

## ==[49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)==

### 解法1：排序+哈希

$$
O(nk\log k)+O(nk)
$$

```C++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> hash;
        string ans;
        for(auto &it:strs){
            ans=it;
            sort(ans.begin(),ans.end());
            hash[ans].push_back(it);
        }
        vector<vector<string>> strings;
        for(auto &it:hash) strings.push_back(it.second);
        return strings;
    }
};
```

### ==笔记==

使用`map`容器实现一对多，可借助`vector`

## [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

### 解法1：哈希

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_map<int,int> map;//0表示没这个数，1表示有这个数，2表示已经被访问过
        int maxLength=0;
        for(int i=0;i<nums.size();i++){//标记出现过的数字
            map[nums[i]]=1;
        }
        for(int i=0;i<nums.size();i++){
            if(map[nums[i]]==2) continue;//已经访问过，跳过
            int length=1;
            map[nums[i]]=2;//标记访问
            for(int j=1;map[nums[i]-j]==1;j++){//向前扫描
                map[nums[i]-j]=2;
                length++;
            }
            for(int j=1;map[nums[i]+j]==1;j++){//向后扫描
                map[nums[i]+j]=2;
                length++;
            }
            maxLength=max(maxLength,length);
        }
        return maxLength;
    }
};
```

# 双指针

## [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

### 解法1：双指针

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int i,j;
        for(i=0,j=0;i<nums.size();i++)
            if(nums[i])
            {
                nums[j]=nums[i];
                j++;
            }
        for(;j<nums.size();j++) nums[j]=0;
    }
};
```

## ==[11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)==

### 解法1：双指针

$$
O(n)+O(1)
$$

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















# 滑动窗口

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

### 解法1：模拟

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

## [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int> dp(nums.size()+1,0);
        int MAX=-10010;
        for(int i=1;i<=nums.size();i++){
            dp[i]=max(nums[i-1],dp[i-1]+nums[i-1]);
            MAX=max(MAX,dp[i]);
        }
        return MAX;
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
    int maxSubArray(vector<int>& nums) {
        vector<int> dp(nums.size()+1,0);
        int MAX=-10010;
        for(int i=1;i<=nums.size();i++){
            dp[i]=max(nums[i-1],dp[i-1]+nums[i-1]);
            MAX=max(MAX,dp[i]);
        }
        return MAX;
    }
};
```

## [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)

### 解法1：贪心

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int k=0;
        for(int i=0;i<nums.size();i++){
            if(k<i) return false;
            k=max(k,i+nums[i]);
        }
        return true;
    }
};
```

## [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

### 解法1：排序

$$
O(n\log n)+O(1)
$$

```C++
class Solution {
public:
    static bool cmp(const vector<int>& u,const vector<int>& v){
        if(u[0]!=v[0]) return u[0]<v[0];
        else return u[1]<v[1];
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        vector<int> tmp;
        sort(intervals.begin(),intervals.end(),cmp);
        tmp.push_back(intervals[0][0]);
        tmp.push_back(intervals[0][1]);

        for(int i=1;i<intervals.size();i++){
            if(intervals[i][0]>tmp[1]){
                result.push_back(tmp);
                tmp[0]=intervals[i][0];
                tmp[1]=intervals[i][1];
            }
            else tmp[1]=max(tmp[1],intervals[i][1]);
        }
        result.push_back(tmp);

        return result;
    }
};
```

## [62. 不同路径](https://leetcode.cn/problems/unique-paths/)

### 解法1：动态规划

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        int dp[110][110];
        for(int i=0;i<=n;i++) dp[0][i]=0;
        for(int i=0;i<=m;i++) dp[i][0]=0;
        dp[1][0]=1;
        for(int i=1;i<=m;i++)
            for(int j=1;j<=n;j++)
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
        return dp[m][n];
    }
};
```

### 解法2：动态规划+滚动数组优化

$$
O(mn)+O(n)
$$

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<int> dp(n+1);
        dp[1]=1;
        for(int i=1;i<=m;i++)
            for(int j=2;j<=n;j++)
                dp[j]=dp[j]+dp[j-1];
        return dp[n];
    }
};
```

## [64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)

### 解法1：动态规划

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        vector<vector<int>> dp(m,vector<int>(n,0));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i&&j) dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i][j];
                else if(i) dp[i][j]=dp[i-1][j]+grid[i][j];
                else if(j) dp[i][j]=dp[i][j-1]+grid[i][j];
                else dp[i][j]=grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
};
```

### 解法2：动态规划+滚动数组优化

$$
O(mn)+O(n)
$$

```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        vector<int> dp(n,0);
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i&&j) dp[j]=min(dp[j],dp[j-1])+grid[i][j];
                else if(j) dp[j]=dp[j-1]+grid[i][j];
                else dp[j]+=grid[i][j];
            }
        }
        return dp[n-1];
    }
};
```

## [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int climbStairs(int n) {
        int dp[50];
        dp[0]=1,dp[1]=1;
        for(int i=2;i<=n;i++) dp[i]=dp[i-2]+dp[i-1];
        return dp[n];
    }
};
```

### 解法2：递推

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int climbStairs(int n) {
        int pre=1,now=1;
        while(--n){
            int temp=now;
            now=now+pre;
            pre=temp;
        }
        return now;
    }
};
```

## [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

### 解法1：动态规划

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.length()+1,vector<int>(word2.length()+1,0));
        for(int i=0;i<=word2.length();i++) dp[0][i]=i;
        for(int i=0;i<=word1.length();i++) dp[i][0]=i;
        for(int i=1;i<=word1.length();i++){
            for(int j=1;j<=word2.length();j++){
                if(word1[i-1]==word2[j-1]) dp[i][j]=dp[i-1][j-1];
                else dp[i][j]=min(min(dp[i-1][j-1],dp[i-1][j]),dp[i][j-1])+1;
            }
        }
        return dp[word1.length()][word2.length()];
    }
};
```

## [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

### 解法1：三指针

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int i=0,j=0,k=nums.size()-1;
        while(j<=k){
            switch(nums[j]){
                case 0:
                    swap(nums[i],nums[j]),i++,j++;
                    break;
                case 1:
                    j++;
                    break;
                case 2:
                    swap(nums[j],nums[k]),k--;
                    break;
            }
        }
    }
};
```

## [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

### 解法1：滑动窗口

$$
O(m+n)+O(C)
$$

```C++
class Solution {
public:
    string minWindow(string s, string t) {
        bool st[52];
        int num[52];
        memset(st,false,sizeof(st)),memset(num,0,sizeof(num));
        
        for(int i=0;i<t.length();i++){
            if(t[i]>='a'&&t[i]<='z') st[t[i]-'a']=true,num[t[i]-'a']++;
            else st[t[i]-'A'+26]=true,num[t[i]-'A'+26]++;
        }
        int i,j,id=0,jd=s.length()-1;
        for(i=0,j=0;j<s.length();j++){
            if(s[j]>='a'&&s[j]<='z'&&st[s[j]-'a']==true) num[s[j]-'a']--;
            if(s[j]>='A'&&s[j]<='Z'&&st[s[j]-'A'+26]==true) num[s[j]-'A'+26]--;
            bool flag=true;
            for(int k=0;k<52;k++)
                if(st[k]==true&&num[k]>0) flag=false;
            while(flag==true){
                if(s[i]>='a'&&s[i]<='z'){
                    if(st[s[i]-'a']==false) i++;
                    else if(st[s[i]-'a']==true&&num[s[i]-'a']<0) num[s[i]-'a']++,i++;
                    else break;
                }
                if(s[i]>='A'&&s[i]<='Z'){
                    if(st[s[i]-'A'+26]==false) i++;
                    else if(st[s[i]-'A'+26]==true&&num[s[i]-'A'+26]<0) num[s[i]-'A'+26]++,i++;
                    else break;
                }
            }
            if(flag==true&&j-i<jd-id) jd=j,id=i;
        }

        int k;
        for(k=0;k<52;k++)
            if(st[k]==true&&num[k]>0) break;
        if(k==52) return s.substr(id,jd-id+1);
        else return "";
    }
};
```

## [78. 子集](https://leetcode.cn/problems/subsets/)

### 解法1：回溯

$$
O(n×2^n)+O(n)
$$

```C++
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int index,const vector<int> &nums){
        if(index==nums.size()){
            result.push_back(path);
            return;
        }
        backtracking(index+1,nums);
        path.push_back(nums[index]);
        backtracking(index+1,nums);
        path.pop_back();
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        result.clear(),path.clear();
        backtracking(0,nums);
        return result;
    }
};
```

### 解法2：二进制

$$
O(n×2^n)+O(n)
$$

```C++
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;

    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < (1 << n); i++) {
            path.clear();
            for (int j = 0; j < n; j++) {
                if (i & (1 << j)) {
                    path.push_back(nums[j]);
                }
            }
            result.push_back(path);
        }
        return result;
    }
};
```

## [79. 单词搜索](https://leetcode.cn/problems/word-search/)

### 解法1：回溯

$$
略
$$

```C++
class Solution {
public:
    bool traversal(int x,int y,int index,vector<vector<char>>& board,string word, vector<vector<bool>>& visit){
        if(index==word.length()-1&&board[x][y]==word[index]) return true;
        else if(board[x][y]!=word[index]) return false;
        int dx[]={0,1,0,-1};
        int dy[]={1,0,-1,0};
        for(int i=0;i<4;i++)
            if(x+dx[i]>=0&&x+dx[i]<board.size()&&y+dy[i]>=0&&y+dy[i]<board[0].size())
                if(visit[x+dx[i]][y+dy[i]]==false){
                    visit[x+dx[i]][y+dy[i]]=true;
                    if(traversal(x+dx[i],y+dy[i],index+1,board,word,visit)) return true;
                    visit[x+dx[i]][y+dy[i]]=false;
                }
        return false;
    }
    bool exist(vector<vector<char>>& board, string word) {
        vector<vector<bool>> visit(board.size(),vector<bool>(board[0].size(),false));
        for(int i=0;i<board.size();i++)
            for(int j=0;j<board[0].size();j++)
                if(board[i][j]==word[0]){
                    visit[i][j]=true;
                    if(traversal(i,j,0,board,word,visit)==true) return true;
                    visit[i][j]=false;
                }
        return false;
    }
};
```

## [84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)

### 解法1：单调栈

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int>st;
        heights.insert(heights.begin(), 0);
        heights.push_back(0);
        int sum=0;
        for(int i=0;i<heights.size();i++){
            while(!st.empty()&&heights[i]<heights[st.top()]){
                int mid=st.top();
                st.pop();
                if(!st.empty()) sum=max(sum,heights[mid]*(i-st.top()-1));
            }
            st.push(i);
        }
        return sum;
    }
};
```

### 解法2：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        vector<int> left(heights.size());
        vector<int> right(heights.size());
        left[0]=-1,right[0]=heights.size();
        for(int i=0;i<heights.size();i++){
            int t=i-1;
            while(t>=0&&heights[t]>=heights[i]) t=left[t];
            left[i]=t;
        }
        for(int i=heights.size()-1;i>=0;i--){
            int t=i+1;
            while(t<heights.size()&&heights[t]>=heights[i]) t=right[t];
            right[i]=t;
        }
        int sum=0;
        for(int i=0;i<heights.size();i++) sum=max(sum,(right[i]-left[i]-1)*heights[i]);
        return sum;
    }
};
```

## [85. 最大矩形](https://leetcode.cn/problems/maximal-rectangle/)

### 解法1：单调栈

$$
O(mn)+O(n)
$$

```C++
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        if(matrix.size()==0||matrix[0].size()==0) return 0;
        int m=matrix.size(),n=matrix[0].size();
        vector<int> height(n+2,0);
        int sum=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]=='1') height[j+1]+=1;
                else height[j+1]=0;
            }
            stack<int> st;
            for(int j=0;j<n+2;j++){
                while(!st.empty()&&height[j]<height[st.top()]){
                    int mid=st.top();
                    st.pop();
                    if(!st.empty()) sum=max(sum,height[mid]*(j-st.top()-1));
                }
                st.push(j);
            }
        }
        return sum;
    }
};
```

## [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

### 解法1：中序遍历（递归）

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
    void traversal(TreeNode *cur,vector<int> &vec){
        if(cur==NULL) return;
        traversal(cur->left,vec);
        vec.push_back(cur->val);
        traversal(cur->right,vec);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> vec;
        traversal(root,vec);
        return vec;
    }
};
```

### 解法2：中序遍历（迭代）

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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        TreeNode *node=root;
        while(node||!st.empty()){
            if(node){
                st.push(node);
                node=node->left;
            }
            else{
                node=st.top();
                st.pop();
                result.push_back(node->val);
                node=node->right;
            }
        }
        return result;
    }
};
```

## [96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n+1,0);
        dp[0]=1;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=i;j++)
                dp[i]+=dp[j-1]*dp[i-j];
        }
        return dp[n];
    }
};
```

## [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

### 解法1：中序遍历（递归）

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
    vector<int> vec;
    bool isValidBST(TreeNode* root) {
        if(root==NULL) return true;
        bool leftFlag=isValidBST(root->left);
        if(vec.size()>0&&root->val<=vec.back()) return false;
        vec.push_back(root->val);
        bool rightFlag=isValidBST(root->right);
        return leftFlag&&rightFlag;
    }
};
```

### 解法2：中序遍历（迭代）

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
    bool isValidBST(TreeNode* root) {
        vector<int> vec;
        stack<TreeNode*> st;
        TreeNode *node=root;
        while(node||!st.empty()){
            if(node){
                st.push(node);
                node=node->left;
            }
            else{
                node=st.top();
                st.pop();
                vec.push_back(node->val);
                if(vec.size()>1&&vec[vec.size()-2]>=vec[vec.size()-1]) return false;
                node=node->right;
            }
        }
        return true;
    }
};
```

## [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

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
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root==NULL) return true;
        queue<TreeNode*> que;
        que.push(root->left),que.push(root->right);
        while(!que.empty())
        {
            TreeNode *left_node=que.front();
            que.pop();
            TreeNode *right_node=que.front();
            que.pop();
            if(left_node==NULL&&right_node==NULL) continue;
            else if(left_node==NULL||right_node==NULL) return false;
            else if(left_node->val!=right_node->val) return false;
            que.push(left_node->left),que.push(right_node->right);
            que.push(left_node->right),que.push(right_node->left);
        }
        return true;
    }
};
```

### 解法2：递归

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
    bool compare(TreeNode *left_node,TreeNode *right_node)
    {
        if(left_node==NULL||right_node==NULL) return left_node==NULL&&right_node==NULL;
        if(left_node->val!=right_node->val) return false;
        return compare(left_node->left,right_node->right)&&compare(left_node->right,right_node->left);
    }
    bool isSymmetric(TreeNode* root) {
        if(root==NULL) return true;
        return compare(root->left,root->right);
    }
};
```

## [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

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
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> que;
        if(root) que.push(root);
        while(!que.empty()){
            int Size=que.size();
            vector<int> vec;
            for(int i=0;i<Size;i++){
                TreeNode* node=que.front();
                que.pop();
                vec.push_back(node->val);
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        return result;
    }
};
```

## [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

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
    int maxDepth(TreeNode* root) {
        if(root==NULL) return 0;
        return 1+max(maxDepth(root->left),maxDepth(root->right));
    }
};
```

## [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

### 解法1：递归+分治

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
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> pre_order,in_order;
    TreeNode* traversal(int l1,int r1,int l2,int r2)
    {
        if(l1>r1||l2>r2) return NULL;
        int mid;
        for(mid=l2;mid<=r2;mid++)
            if(in_order[mid]==pre_order[l1]) break;
        int len1=mid-l2;
        TreeNode *node=new TreeNode(pre_order[l1]);
        node->left=traversal(l1+1,l1+len1,l2,mid-1);
        node->right=traversal(l1+len1+1,r1,mid+1,r2);
        return node;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        pre_order=preorder,in_order=inorder;
        int l1=0,r1=preorder.size()-1,l2=0,r2=inorder.size()-1;
        TreeNode *root=traversal(l1,r1,l2,r2);
        return root;
    }
};
```

### 解法2：递归+分治（map容器优化）

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
    vector<int> pre_order,in_order;
    map<int,int> index;
    TreeNode* traversal(int l1,int r1,int l2,int r2)
    {
        if(l1>r1||l2>r2) return NULL;
        int mid=index[pre_order[l1]];
        int len1=mid-l2;
        TreeNode *node=new TreeNode(pre_order[l1]);
        node->left=traversal(l1+1,l1+len1,l2,mid-1);
        node->right=traversal(l1+len1+1,r1,mid+1,r2);
        return node;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        pre_order=preorder,in_order=inorder;
        for(int i=0;i<inorder.size();i++) index[inorder[i]]=i;
        int l1=0,r1=preorder.size()-1,l2=0,r2=inorder.size()-1;
        TreeNode *root=traversal(l1,r1,l2,r2);
        return root;
    }
};
```

## [114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

### 解法1：前序遍历（递归）

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
    vector<TreeNode*> vec;
    void traversal(TreeNode* node){
        if(node==NULL) return;
        vec.push_back(node);
        traversal(node->left);
        traversal(node->right);
    }
    void flatten(TreeNode* root) {
        if(root==NULL) return;
        traversal(root);
        for(int i=0;i<vec.size()-1;i++){
            vec[i]->left=NULL;
            vec[i]->right=vec[i+1];
        }
    }
};
```

### 前序遍历（迭代）

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
    void flatten(TreeNode* root) {
        if(root==NULL) return;
        stack<TreeNode*> st;
        vector<TreeNode*> vec;
        TreeNode* node=root;
        while(node||!st.empty()){
            if(node){
                st.push(node);
                vec.push_back(node);
                node=node->left;
            }
            else{
                node=st.top();
                st.pop();
                node=node->right;
            }
        }
        for(int i=0;i<vec.size()-1;i++){
            vec[i]->left=NULL;
            vec[i]->right=vec[i+1];
        }
    }
};
```

## [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        vector<vector<int>> dp(prices.size(),vector<int>(2,0));
        dp[0][0]=-prices[0],dp[0][1]=0;
        for(int i=1;i<prices.size();i++){
            dp[i][0]=max(dp[i-1][0],-prices[i]);
            dp[i][1]=max(dp[i-1][1],dp[i][0]+prices[i]);
        }
        return dp[prices.size()-1][1];
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
        int minPrice=prices[0],result=0;
        for(int i=0;i<prices.size();i++){
            result=max(result,prices[i]-minPrice);
            minPrice=min(minPrice,prices[i]);
        }
        return result;
    }
};
```

## [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

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
    void traversal(TreeNode* node,int& sum1,int& sum2){
        int leftSum1=INT_MIN/3,leftSum2=INT_MIN/3;
        if(node->left) traversal(node->left,leftSum1,leftSum2);
        int rightSum1=INT_MIN/3,rightSum2=INT_MIN/3;
        if(node->right) traversal(node->right,rightSum1,rightSum2);
        sum1=max(node->val,max(node->val+leftSum1,node->val+rightSum1));
        sum2=max(node->val+leftSum1+rightSum1,max(max(leftSum1,leftSum2),max(rightSum1,rightSum2)));
    }
    int maxPathSum(TreeNode* root) {
        if(root==NULL) return 0;
        int sum1=INT_MIN/3,sum2=INT_MIN/3;
        traversal(root,sum1,sum2);
        return max(sum1,sum2);
    }
};
```



## [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

### 解法1：异或

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result=0;
        for(int i=0;i<nums.size();i++) result^=nums[i];
        return result;
    }
};
```

## [139. 单词拆分](https://leetcode.cn/problems/word-break/)

### 解法1：动态规划

$$
O(mn)+O(n)
$$

```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(),wordDict.end());
        vector<bool> dp(s.length()+1,false);
        dp[0]=true;
        for(int i=1;i<=s.length();i++)
            for(int j=0;j<wordDict.size();j++)
                if(i>=wordDict[j].length()){
                    string str=s.substr(i-wordDict[j].length(),wordDict[j].length());
                    if(dp[i-wordDict[j].length()]==true&&wordSet.find(str)!=wordSet.end())
                        dp[i]=true;
                }
        return dp[s.length()];
    }
};
```

## [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

### 解法1：快慢指针

$$
O(n)+O(1)
$$

```c++
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
    bool hasCycle(ListNode *head) {
        if(head==NULL||head->next==NULL) return false;
        ListNode *p=head,*q=head->next;
        while(q!=NULL&&q->next!=NULL){
            if(p==q) break;
            p=p->next;
            q=q->next->next;
        }
        if(q==NULL||q->next==NULL) return false;
        else return true;
    }
};
```

## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

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
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *p=head,*q=head;
        while(q!=NULL&&q->next!=NULL){
            p=p->next;
            q=q->next->next;
            if(p==q){
                p=head;
                while(p!=q) p=p->next,q=q->next;
                return p;
            }
        }
        return NULL;
    }
};
```

## [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

### 解法1：哈希+双链表

$$
O(1)+O(n)
$$

```C++
struct DLinkedNode {
    int key, value;
    DLinkedNode* prev;
    DLinkedNode* next;
    DLinkedNode(): key(0), value(0), prev(nullptr), next(nullptr) {}
    DLinkedNode(int _key, int _value): key(_key), value(_value), prev(nullptr), next(nullptr) {}
};

class LRUCache {
public:
    unordered_map<int, DLinkedNode*> cache;
    DLinkedNode* dummyHead,* dummyTail;
    int size,capacity;
    LRUCache(int capacity) {
        dummyHead = new DLinkedNode();
        dummyTail = new DLinkedNode();
        dummyHead->next = dummyTail;
        dummyTail->prev = dummyHead;
        size = 0;
        this->capacity = capacity;
    }
    
    int get(int key) {
        if (cache.count(key) == 0){
            return -1;
        }
        DLinkedNode* node = cache[key];
        moveToHead(node);
        return node->value;
    }
    
    void put(int key, int value) {
        if (cache.count(key) != 0){
            DLinkedNode* node = cache[key];
            node->value = value;
            moveToHead(node);
        }
        else{
            size++;
            DLinkedNode* node = new DLinkedNode(key, value);
            cache[key] = node;
            addToHead(node);
            if(size > capacity){
                DLinkedNode* removed = removeTail();
                cache.erase(removed->key);//map中的erase
                size--;
            }
        }
    }

    void addToHead(DLinkedNode* node) {
        node->prev = dummyHead;
        node->next = dummyHead->next;
        dummyHead->next->prev = node;
        dummyHead->next = node;
    }
    
    void removeNode(DLinkedNode* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void moveToHead(DLinkedNode* node) {
        removeNode(node);
        addToHead(node);
    }

    DLinkedNode* removeTail() {
        DLinkedNode* node = dummyTail->prev;
        removeNode(node);
        return node;
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

## [148. 排序链表](https://leetcode.cn/problems/sort-list/)

### 解法1：归并排序（自顶向下）

$$
O(n\log n)+O(\log n)
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
    ListNode* sortList(ListNode* head){
        if (head == NULL || head->next == NULL) {
            return head;
        }
        ListNode* slow = head, * fast = head;
        while (fast->next != NULL && fast->next->next != NULL) {
        //这个条件不能写成 while (fast != NULL && fast->next != NULL)
            slow = slow->next;
            fast = fast->next->next;
        }
        fast = slow->next, slow->next = NULL;
        return merge(sortList(head), sortList(fast));
    }

    ListNode* merge(ListNode* head1, ListNode* head2){
        ListNode* dummyHead = new ListNode(), * p = dummyHead;
        ListNode* p1 = head1, * p2 = head2;
        while (p1 && p2) {
            if (p1->val < p2->val) {
                p->next = p1;
                p1 = p1->next;
            }
            else {
                p->next = p2;
                p2 = p2->next;
            }
            p = p->next;
        }
        p->next = p1 ? p1 : p2;
        return dummyHead->next;
    }
};
```

### 解法2：归并排序（自底向上）

$$
O(n\log n)+O(1)
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
    ListNode* sortList(ListNode* head){
        if (head == NULL || head->next == NULL) {
            return head;
        }
        ListNode* dummyHead = new ListNode();
        dummyHead->next = head;
        ListNode* node = head;
        int length = 0;//统计链表长度
        while (node) {
            length++;
            node = node->next;
        }
        for (int subLength = 1; subLength < length; subLength <<= 1){//自底向上，每一轮归并的长度逐渐变长
            ListNode* pre = dummyHead, * cur = dummyHead->next;//pre存储前一次归并的结果，cur为当前要处理的一段链表
            while (cur) {//每次取两段链表归并
                //取出第一段链表
                ListNode* head1;
                head1 = cur;
                for (int i = 1; i < subLength && cur->next; i++) {//"cur->next"作为判断条件是为了让head2至少有一个结点
                    cur = cur->next;
                }
                //取出第二段链表
                ListNode* head2;
                head2 = cur->next;
                cur->next = NULL;//断链
                cur = head2;
                for (int i = 1; i < subLength && cur; i++) {//"cur"作为判断条件
                    cur = cur->next;
                }
                ListNode* next = NULL;//next用于记录取出两段链表后剩下的那段（用于下一次处理），避免锻炼
                if (cur){
                    next = cur->next;
                    cur->next = NULL;//断链
                }
                cur = next;
                ListNode* merged = merge(head1, head2);
                pre->next = merged;//将两段归并后的链表连在pre后面
                for (pre = merged; pre->next; pre = pre->next);//pre移动到新链表的末尾
            }
        }
        return dummyHead->next;
    }

    //merge函数和解法1完全一致
    ListNode* merge(ListNode* head1, ListNode* head2){
        ListNode* dummyHead = new ListNode(), * p = dummyHead;
        ListNode* p1 = head1, * p2 = head2;
        while (p1 && p2) {
            if (p1->val < p2->val) {
                p->next = p1;
                p1 = p1->next;
            }
            else {
                p->next = p2;
                p2 = p2->next;
            }
            p = p->next;
        }
        p->next = p1 ? p1 : p2;
        return dummyHead->next;
    }
};
```

## ==[152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)==

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        vector<int> minDp(nums.size()),maxDp(nums.size());
        minDp[0]=maxDp[0]=nums[0];
        int result=nums[0];
        for(int i=1;i<nums.size();i++){
            minDp[i]=min(nums[i],min(minDp[i-1]*nums[i],maxDp[i-1]*nums[i]));
            maxDp[i]=max(nums[i],max(minDp[i-1]*nums[i],maxDp[i-1]*nums[i]));
            result=max(result,maxDp[i]);
        }
        return result;
    }
};
```

### ==笔记==

是不是连续子序列，动态规划数组的意义都是“取以当前位置为结尾得到的最优结果”？

## [155. 最小栈](https://leetcode.cn/problems/min-stack/)

### 解法1：栈

$$
略
$$

```C++
class MinStack {
    stack<int> st,min_st;
public:
    MinStack() {
        while(!st.empty()) st.pop();
        while(!min_st.empty()) min_st.pop();
    }
    
    void push(int val) {
        st.push(val);
        if(min_st.empty()||val<=min_st.top()) min_st.push(val);
    }
    
    void pop() {
        if(!min_st.empty()&&st.top()==min_st.top()) min_st.pop();
        if(!st.empty()) st.pop();
    }
    
    int top() {
        if(!st.empty()) return st.top();
        return -1;
    }
    
    int getMin() {
        if(!min_st.empty()) return min_st.top();
        return -1;
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

## [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

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
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *p=headA,*q=headB;
        int m=0,n=0;
        while(p!=NULL) p=p->next,m++;
        while(q!=NULL) q=q->next,n++;
        int k;
        if(m>=n) p=headA,q=headB,k=m-n;
        else p=headB,q=headA,k=n-m;
        while(k--) p=p->next;
        while(p!=q) p=p->next,q=q->next;
        if(p!=NULL) return p;
        else return NULL;
    }
};
```

## [169. 多数元素](https://leetcode.cn/problems/majority-element/)

### 解法1：摩尔投票

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int result=nums[0],count=1;
        for(int i=1;i<nums.size();i++){
            if(count==0) result=nums[i];
            if(result==nums[i]) count++;
            else count--;
        }
        return result;
    }
};
```

## [198. 打家劫舍](https://leetcode.cn/problems/house-robber/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==1) return nums[0];
        vector<int> dp(nums.size(),0);
        dp[0]=nums[0],dp[1]=max(nums[0],nums[1]);
        for(int i=2;i<nums.size();i++)
            dp[i]=max(dp[i-1],dp[i-2]+nums[i]);
        return dp[nums.size()-1];
    }
};
```

## [200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

### 解法1：BFS

$$
O(mn)+O(1)
$$

```C++
class Solution {
public:
    int m,n;
    void BFS(int i,int j,vector<vector<char>>& grid){
        grid[i][j]='0';
        int dx[4]={0,1,0,-1};
        int dy[4]={1,0,-1,0};
        for(int k=0;k<4;k++){
            int x=i+dx[k],y=j+dy[k];
            if(x>=0&&x<m&&y>=0&&y<n&&grid[x][y]=='1') BFS(x,y,grid);
        }
    }
    int numIslands(vector<vector<char>>& grid) {
        int count=0;
        m=grid.size(),n=grid[0].size();
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]=='1'){
                    BFS(i,j,grid);
                    count++;
                }
            }
        }
        return count;
    }
};
```

## [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

### 解法1：头插法

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
    ListNode* reverseList(ListNode* head) {
        ListNode* dummyHead1=new ListNode(),*dummyHead2=new ListNode();
        dummyHead1->next=head;
        ListNode* p=head;
        while(p!=NULL){
            dummyHead1->next=p->next;
            p->next=dummyHead2->next;
            dummyHead2->next=p;
            p=dummyHead1->next;
        }

        return dummyHead2->next;
    }
};
```

## ==[207. 课程表](https://leetcode.cn/problems/course-schedule/)==

### 解法1：拓扑排序

$$
O(m+n)+O(m+n)
$$

```C++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> inDegree(numCourses, 0);
        unordered_map<int, vector<int>> graph;
        for (int i = 0; i < prerequisites.size(); i++) {
            inDegree[prerequisites[i][0]]++;
            graph[prerequisites[i][1]].push_back(prerequisites[i][0]);
        }
        queue<int> que;
        int count = 0;
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                que.push(i);
                count++;
            }
        }
        while (!que.empty()) {
            int node = que.front();
            que.pop();
            for (int i = 0; i < graph[node].size(); i++) {
                if (--inDegree[graph[node][i]] == 0) {
                    que.push(graph[node][i]);
                    count++;
                }
            }
        }
        if (count == numCourses) {
            return true;
        }
        else {
            return false;
        }
    }
};
```

### ==笔记：`map`容器排序==

#### 1.默认情况

从小大大排序

#### 2.让map中的元素按照key从大到小排序

```C++
#include <map>
#include <string>
#include <iostream>
using namespace std;
int main(){
    map<string, int, greater<string> > mapStudent;  //关键是这句话
    mapStudent["LiMin"]=90;
    mapStudent["ZiLinMi"]=72;
    mapStudent["BoB"]=79;
    map<string, int>::iterator iter=mapStudent.begin();
    for(iter=mapStudent.begin();iter!=mapStudent.end();iter++)
    {
        cout<<iter->first<<" "<<iter->second<<endl;
    }
    return 0;
}
```

运行结果

```C++
[root@localhost charpter03]# g++ 0327.cpp -o 0327
[root@localhost charpter03]# ./0327
ZiLinMi 72
LiMin 90
BoB 79
```

#### 3.重定义map内部的Compare函数，按照键字符串长度大小进行排序

```C++
#include <map>
#include <string>
#include <iostream>
using namespace std;
// 自己编写的Compare，实现按照字符串长度进行排序
struct CmpByKeyLength {
    bool operator()(const string& k1, const string& k2) {  
    return k1.length() < k2.length();  
  }  
};  
int main(){
    map<string, int, CmpByKeyLength > mapStudent;  //这里注意要换成自己定义的compare
    mapStudent["LiMin"]=90;
    mapStudent["ZiLinMi"]=72;
    mapStudent["BoB"]=79;
    map<string, int>::iterator iter=mapStudent.begin();
    for(iter=mapStudent.begin();iter!=mapStudent.end();iter++){
        cout<<iter->first<<" "<<iter->second<<endl;
    }
    return 0;
}
```

运行结果

```C++
[root@localhost charpter03]# g++ 0328.cpp -o 0328
[root@localhost charpter03]# ./0328
BoB 79
LiMin 90
ZiLinMi 72
```

#### 4.key是结构体的排序

```C++
#include <map>
#include <string>
#include <iostream>
using namespace std;
typedef struct tagStudentInfo  
{  
    int iID;  
    string  strName;  
    bool operator < (tagStudentInfo const& r) const {  
        //这个函数指定排序策略，按iID排序，如果iID相等的话，按strName排序  
        if(iID < r.iID)  return true;  
        if(iID == r.iID) return strName.compare(r.strName) < 0;  
        return false;
    }  
}StudentInfo;//学生信息
int main(){
    /*用学生信息映射分数*/  
    map<StudentInfo, int>mapStudent;  
    StudentInfo studentInfo;  
    studentInfo.iID = 1;  
    studentInfo.strName = "student_one";  
    mapStudent[studentInfo]=90;
    studentInfo.iID = 2;  
    studentInfo.strName = "student_two";
    mapStudent[studentInfo]=80;
    map<StudentInfo, int>::iterator iter=mapStudent.begin();
    for(;iter!=mapStudent.end();iter++){
        cout<<iter->first.iID<<" "<<iter->first.strName<<" "<<iter->second<<endl;
    }
    return 0;
}
```

运行结果

```C++
[root@localhost charpter03]# g++ 0329.cpp -o 0329
[root@localhost charpter03]# ./0329
1 student_one 90
2 student_two 80
```

#### 5.将map按value排序

```C++
#include <algorithm>
#include <map>
#include <vector>
#include <string>
#include <iostream>
using namespace std;
typedef pair<string, int> PAIR;   
bool cmp_by_value(const PAIR& lhs, const PAIR& rhs) {  
  return lhs.second < rhs.second;  
}  
struct CmpByValue {  
  bool operator()(const PAIR& lhs, const PAIR& rhs) {  
    return lhs.second < rhs.second;  
  }  
};
int main(){  
  map<string, int> name_score_map;  
  name_score_map["LiMin"] = 90;  
  name_score_map["ZiLinMi"] = 79;  
  name_score_map["BoB"] = 92;  
  name_score_map.insert(make_pair("Bing",99));  
  name_score_map.insert(make_pair("Albert",86));  
  /*把map中元素转存到vector中*/   
  vector<PAIR> name_score_vec(name_score_map.begin(), name_score_map.end());  
  sort(name_score_vec.begin(), name_score_vec.end(), CmpByValue());  
  /*sort(name_score_vec.begin(), name_score_vec.end(), cmp_by_value);也是可以的*/
  for (int i = 0; i != name_score_vec.size(); ++i) {  
    cout<<name_score_vec[i].first<<" "<<name_score_vec[i].second<<endl;  
  }  
  return 0;  
}  
```

运行结果

```C++
[root@localhost charpter03]# g++ 0330.cpp -o 0330
[root@localhost charpter03]# ./0330
ZiLinMi 79
Albert 86
LiMin 90
BoB 92
Bing 99
```

## ==[208. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)==

### 解法1：Trie树

$$
时间复杂度：初始化为O(1)，其余操作为O(|S|)，其中|S|是每次插入或查询的字符串的长度。
$$
$$
空间复杂度：O(|T|⋅Σ)，其中∣T∣为所有插入字符串的长度之和，Σ为字符集的大小，本题Σ=26。
$$

```C++
class Trie {
private:
    bool isEnd;
    Trie* next[26];
public:
    Trie() {
        isEnd = false;
        memset(next, 0, sizeof(next));
    }
    
    void insert(string word) {
        Trie* node = this;
        for (char c : word) {
            if (node->next[c - 'a'] == NULL) {
                node->next[c - 'a'] = new Trie();
            }
            node = node->next[c-'a'];
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
            node = node->next[c-'a'];
            if (node == NULL) {
                return false;
            }
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

## [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

### 解法1：快速排序

$$
O(n)+O(\log n)
$$

```C++
class Solution {
public:
    void quicksort(vector<int>& nums,int l,int r,int k){
        if(l>=r) return;
        int i=l-1,j=r+1,x=nums[l+r>>1];
        while(i<j){
            while(nums[++i]<x);
            while(nums[--j]>x);
            if(i<j) swap(nums[i],nums[j]);
        }
        if(j>=nums.size()-k) quicksort(nums,l,j,k);
        else quicksort(nums,j+1,r,k);
    }
    int findKthLargest(vector<int>& nums, int k) {
        quicksort(nums,0,nums.size()-1,k);
        return nums[nums.size()-k];
    }
};
```

## [221. 最大正方形](https://leetcode.cn/problems/maximal-square/)

### 解法1：动态规划

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return 0;
        }
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n));
        int maxSize = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == '1') {
                    if (i == 0 || j == 0){
                        dp[i][j] = 1;
                    }
                    else {
                        dp[i][j] = min(dp[i-1][j-1], min(dp[i-1][j], dp[i][j-1])) + 1;
                    }
                }
                else {
                    dp[i][j] = 0;
                }
                maxSize = max(maxSize, dp[i][j]);
            }
        }
        return maxSize * maxSize;
    }
};
```

## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

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
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty())
        {
            TreeNode* node=que.front();
            que.pop();
            if(node==NULL) continue;
            swap(node->left,node->right);
            que.push(node->left);
            que.push(node->right);
        }
        return root;
    }
};
```

### 解法2：递归

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
    TreeNode* invertTree(TreeNode* root) {
        if(root==NULL) return root;
        invertTree(root->left),invertTree(root->right);
        TreeNode *tmp=root->left;
        root->left=root->right;
        root->right=tmp;
        return root;
    }
};
```

## ==[234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)==

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
    bool isPalindrome(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return true;
        }
        ListNode* pre = NULL, * slow = head, * fast = head;
        while (fast != NULL && fast->next != NULL) {
            pre = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        if (fast != NULL){
            //slow和fast均从head出发
            //若fast最终为NULL，则链表结点数为偶数，分[前半段，后半段]，slow位置为后半段链表第一个结点
            //若fast最终不为NULL，则链表结点数为奇数，slow位置为[前半段，中间结点，后半段]，slow位置为之间结点
            slow = slow->next;
        }
        pre->next = NULL;
        ListNode* dummyHead = new ListNode();
        while (slow != NULL) {
            ListNode* p = slow;
            slow = slow->next;
            p->next = dummyHead->next;
            dummyHead->next = p;
        }
        
        ListNode* head1 = head, * head2 = dummyHead->next;
        while (head1 != NULL && head2 != NULL) {
            if (head1->val != head2->val) {
                return false;
            }
            head1 = head1->next;
            head2 = head2->next;
        }
        return true;
    }
};
```

## [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

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
    //题目中已经指出，p != q，且p和q都存在于树中
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root==NULL) return NULL;
        if(root==p||root==q) return root;
        TreeNode *left=lowestCommonAncestor(root->left,p,q);
        TreeNode *right=lowestCommonAncestor(root->right,p,q);
        if(left==NULL&&right==NULL) return NULL;
        else if(left==NULL) return right;
        else if(right==NULL) return left;
        else return root;
    }
};
```

### 解法2：存储父结点

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
    unordered_map<int,TreeNode*> hash;
    unordered_map<int,bool> visit;
    void DFS(TreeNode* node){
        if(node==NULL) return;
        if(node->left) hash[node->left->val]=node;
        if(node->right) hash[node->right->val]=node;
        DFS(node->left);
        DFS(node->right);
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        hash[root->val]=NULL;
        DFS(root);
        while(p!=NULL){
            visit[p->val]=true;
            p=hash[p->val];
        }
        while(q!=NULL){
            if(visit[q->val]==true) return q;
            q=hash[q->val];
        }
        return NULL;
    }
};
```

## [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> left(nums.size()), right(nums.size());
        vector<int> result(nums.size());
        for (int i = 0; i < nums.size(); i++) {
            if (i > 0) {
                left[i] = left[i - 1] * nums[i - 1];
                right[nums.size() - 1 - i] = right[nums.size() - i] * nums[nums.size() - i];
            }
            else {
                left[i] = 1;
                right[nums.size() - 1 - i] = 1;
            }
        }
        for (int i = 0; i < nums.size(); i++) {
            result[i] = left[i] * right[i];
        }
        return result;
    }
};
```

### 解法2：动态规划

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> result(nums.size());
        for (int i = 0; i < nums.size(); i++) {
            if (i > 0) {
                result[i] = result[i - 1] * nums[i - 1];
            }
            else {
                result[i] = 1;
            }
        }
        int temp = 1;
        for (int i = nums.size() - 1; i >= 0; i--) {
            if (i < nums.size() - 1) {
                temp *= nums[i + 1];
                result[i] *= temp;
            }
            else {
                result[i] *= 1;
            }
        }
        return result;
    }
};
```

## ==[239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)==

### 解法1：滑动窗口

$$
O(n)+O(k)
$$

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> q;
        vector<int> maxn;
        for(int i=0;i<nums.size();i++)
        {
            while(!q.empty()&&q.front()<i-k+1) q.pop_front();
            while(!q.empty()&&nums[q.back()]<=nums[i]) q.pop_back();
            q.push_back(i);
            if(i>=k-1) maxn.push_back(nums[q.front()]);
        }
        return maxn;
    }
};
```

## [240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)

### 解法1：对角查找

$$
O(\min(m,n))+O(1)
$$

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0||matrix[0].size()==0) return false;
        int i=0,j=matrix[0].size()-1;
        while(i<matrix.size()&&j>=0){
            if(matrix[i][j]==target) return true;
            else if(matrix[i][j]<target) i++;
            else j--;
        }
        return false;
    }
};
```

## [253.会议室II](https://leetcode.cn/problems/meeting-rooms-ii/?favorite=2cktkvj)（==Plus会员题==）

## [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

### 解法1：动态规划

$$
O(n\sqrt n)+O(n)
$$

```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> num(101,0);
        for(int i=0;i<=100;i++) num[i]=i*i;
        vector<int> dp(n+1,INT_MAX/2);
        dp[0]=0;
        for(int i=0;i<=100;i++)
            for(int j=num[i];j<=n;j++)
                dp[j]=min(dp[j],dp[j-num[i]]+1);
        return dp[n];
    }
};
```



## [287. 寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0, fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }while (slow != fast);
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
};
```

## [297. 二叉树的序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/)（==待做==）



## [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

### 解法1：动态规划

$$
O(n^2)+O(n)
$$

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp(nums.size(),1);
        int MAX=1;
        for(int i=0;i<nums.size();i++){
            for(int j=0;j<i;j++)
                if(nums[i]>nums[j]) dp[i]=max(dp[i],dp[j]+1);
            MAX=max(MAX,dp[i]);
        }
        return MAX;
    }
};
```

### 解法2：贪心+二分查找

$$
O(n\log n)+O(n)
$$

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> d;
        for(int i=0;i<nums.size();i++){
            if(i==0||d[d.size()-1]<nums[i]) d.push_back(nums[i]);
            else{
                int l=0,r=d.size()-1;
                while(l<r){
                    int mid=l+r>>1;
                    if(d[mid]>=nums[i]) r=mid;
                    else l=mid+1;
                }
                d[l]=nums[i];
            }
        }
        return d.size();
    }
};
```

## [301. 删除无效的括号](https://leetcode.cn/problems/remove-invalid-parentheses/)

### 解法1：回溯

$$
O(2^n)+O(n)
$$

```C++
class Solution {
public:
    int count = 0;
    vector<string> result;
    unordered_set<string> set;
    string path;
    void backtracking(const string& s, int index, int left, int right){
        if (index == s.length()){
            if (left == right) {
                if (left + right > count) {
                    count = left + right;
                    result.clear();
                    set.clear();
                    result.push_back(path);
                    set.insert(path);
                }
                else if (left + right == count && set.find(path) == set.end()) {
                    result.push_back(path);
                    set.insert(path);
                }
            }
            return;
        }
        if (s[index] == '(') {
            path.push_back(s[index]);
            backtracking(s, index + 1, left + 1, right);
            path.pop_back();
            backtracking(s, index + 1, left, right);
        }
        else if (s[index] == ')') {
            if (left > right) {
                 path.push_back(s[index]);
                 backtracking(s, index + 1, left, right + 1);
                 path.pop_back();
            }
            backtracking(s, index + 1, left, right);
        }
        else {
            path.push_back(s[index]);
            backtracking(s, index + 1, left, right);
            path.pop_back();
        }
    }
    vector<string> removeInvalidParentheses(string s) {
        backtracking(s, 0, 0 ,0);
        return result;
    }
};
```

## [309. 最佳买卖股票时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()==1) return 0;
        vector<vector<int>> dp(prices.size(),vector<int>(2,0));
        dp[0][0]=-prices[0],dp[0][1]=0;
        dp[1][0]=max(-prices[0],-prices[1]),dp[1][1]=max(dp[0][1],dp[0][0]+prices[1]);
        for(int i=2;i<prices.size();i++){
            dp[i][0]=max(dp[i-1][0],dp[i-2][1]-prices[i]);
            dp[i][1]=max(dp[i-1][1],dp[i][0]+prices[i]);
        }
        return dp[prices.size()-1][1];
    }
};
```

## [312. 戳气球](https://leetcode.cn/problems/burst-balloons/)

### 解法1：动态规划

$$
O(n^3)+O(n^2)
$$

```C++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        nums.insert(nums.begin(), 1);
        nums.push_back(1);
        vector<vector<int>> dp(n + 2, vector<int>(n + 2, 0));
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 2; j <= n + 1; j++) {
                for (int mid = i + 1; mid < j; mid ++) {
                    int sum = dp[i][mid] + dp[mid][j] + nums[i] * nums[mid] * nums[j];
                    dp[i][j] = max(dp[i][j], sum);
                }
            }
        }
        return dp[0][n + 1];
    }
};
```

## [322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

### 解法1：动态规划

$$
时间复杂度，O(Sn)，其中 S 是金额，n 是面额数。
$$

$$
空间复杂度：O(S)。
$$

```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount+1,INT_MAX/2);
        dp[0]=0;
        for(int i=0;i<coins.size();i++)
            for(int j=coins[i];j<=amount;j++)
                dp[j]=min(dp[j],dp[j-coins[i]]+1);
        if(dp[amount]>=INT_MAX/2) return -1;
        else return dp[amount];
    }
};
```

## [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)



## [338. 比特位计数](https://leetcode.cn/problems/counting-bits/)



## [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)



## [394. 字符串解码](https://leetcode.cn/problems/decode-string/)



## [399. 除法求值](https://leetcode.cn/problems/evaluate-division/)



## [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)



## [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)



## [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)



## [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)



## [448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)



## [461. 汉明距离](https://leetcode.cn/problems/hamming-distance/)



## [494. 目标和](https://leetcode.cn/problems/target-sum/)



## [538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)



## [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)



## [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)



## [581. 最短无序连续子数组](https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/)



## [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)



## [621. 任务调度器](https://leetcode.cn/problems/task-scheduler/)



## [647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/)



## [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)





