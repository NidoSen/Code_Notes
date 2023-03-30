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

