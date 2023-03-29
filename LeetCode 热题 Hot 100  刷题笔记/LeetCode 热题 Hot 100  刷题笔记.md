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

