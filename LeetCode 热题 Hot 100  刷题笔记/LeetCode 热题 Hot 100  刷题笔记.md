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
        int left = 0, right = height.size() - 1; //left和right初始化为左右两端
        int maxArea = 0; //记录最大面积
        while (left < right) {
            int area = min(height[left], height[right]) * (right - left); //计算当前面积
            maxArea = max(maxArea, area); //更新最大面积
            if (height[left] <= height[right]) { //若左端较小，则右移left
                left++;
            }
            else { //若右端较小，则左移right
                right--;
            }
        }
        return maxArea;
    }
};
```

## [15. 三数之和](https://leetcode.cn/problems/3sum/)

### 解法1：排序+双指针

$$
O(n^2)+O(1)
$$

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end()); //排序，为后续操作做准备

        vector<vector<int>> result; //记录结果
        for(int i = 0; i <nums.size(); i++) {
            if (nums[i] > 0) { //当前访问的数大于0，说明后续已不会再出现三数之和等于0，可以直接退出
                break;
            }
            if (i > 0 && nums[i] == nums[i - 1]) { //避免重复
                continue;
            }
            int j = i + 1, k = nums.size() - 1; //从i位后面的数中开始找与nums[i]相加,和为0的两个数
            while (j < k) { //当j和k相等时，遍历结束
                if (nums[i] + nums[j] + nums[k] == 0) { //三数之和为0
                    result.push_back(vector<int>{nums[i], nums[j], nums[k]}); //记录到结果中
                    j++, k--; //j后移，k前移
                    while (j < k && nums[j] == nums[j - 1]) { //避免重复
                        j++;
                    }
                    while (j < k && nums[k] == nums[k + 1]) { //避免重复
                        k--;
                    }
                }
                else if (j < k && nums[i] + nums[j] + nums[k] < 0) { //和太小，需要增大，j后移
                    j++;
                }
                else { //和太大，需要减小，k前移
                    k--;
                }
            }
        }

        return result;
    }
};
```

## ==[42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)==

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.size() <= 2) { //柱子数量小于等于2个，无法接水
            return 0;
        }

        vector<int> leftHeight = vector<int>(height.size(), 0); //记录当前位置及其左边的最高柱子
        leftHeight[0] = height[0]; //初始化第一个位置及其左边的最高柱子为height[0]
        for (int i = 1; i < height.size(); i++) {
            leftHeight[i] = max(height[i], leftHeight[i - 1]);
        }

        vector<int> rightHeight = vector<int>(height.size(), 0); //记录当前位置及其右边的最高柱子
        rightHeight[height.size() - 1] = height[height.size() - 1]; //初始化最后一个位置及其右边的最高柱子为height[height.size() - 1]
        for (int i = height.size() - 2; i >= 0; i--) {
            rightHeight[i] = max(height[i], rightHeight[i + 1]);
        }

        int sum = 0; //记录雨水量
        for (int i = 0; i < height.size(); i++) {
            sum += min(leftHeight[i], rightHeight[i]) - height[i]; //当前位置能接的雨水量等于当前位置左边的最高柱子和右边的最高柱子中的较矮的柱子，与当前位置柱子高度的差
        }
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
        stack<int> st; //单调栈，记录坐标
        int sum = 0; //记录雨水量
        for (int i = 0; i < height.size(); i++) {
            while (!st.empty() && height[i] > height[st.top()]) { //出现凹槽，一直加水至凹槽不存在为止
                int mid = st.top();
                st.pop();
                if (!st.empty()) {
                    int h = min(height[i], height[st.top()]) - height[mid]; //水槽高度
                    int w = i - st.top() - 1; //水槽宽度
                    sum += h * w;
                }
            }
            st.push(i); //坐标入栈
        }
        return sum;
    }
};
```

### 解法3：双指针

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int left = 0, right = height.size() - 1; //初始化左右指针位置
        int leftMax = 0, rightMax = 0; //记录当前位置及其左边的最高柱子和当前位置及其右边的最高柱子
        int sum = 0; //记录雨水量
        while (left < right) { //左右指针相遇，则结束循环
            leftMax = max(leftMax, height[left]); //记录当前位置及其左边的最高柱子
            rightMax = max(rightMax, height[right]); //记录当前位置及其右边的最高柱子
            if (leftMax <= rightMax) { //右边柱子高，说明当前位置只能积左边最高柱子减当前位置柱子的水
                sum += leftMax - height[left];
                left++; //移动左指针
            }
            else { //左边柱子高，说明当前位置只能积右边最高柱子减当前位置柱子的水
                sum += rightMax - height[right];
                right--; //移动右指针
            }
        }
        return sum;
    }
};
```

# 滑动窗口

## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

### 解法1：滑动窗口+哈希

$$
O(n)+O(C)//C为字符集的大小
$$

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> hash; //记录当前字符出现的次数
        int i, j; //双指针，i表示当前位置，也是子串的末位，j表示子串的首位
        int maxLength = 0, length = 0; //分别记录最长子串的长度和当前子串的长度
        for (int j = 0; j < s.length(); j++) {
            while (hash[s[j]] > 0) { //右移i直到当前子串中没有重复的字符
                hash[s[i]]--;
                i++;
                length--;
            }
            hash[s[j]] = 1;
            length++;
            maxLength = max(maxLength, length); //若当前子串长度大于最长子串长度，更新最长子串长度
        }
        return maxLength;
    }
};
```

## [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

### 解法1：滑动窗口+哈希

$$
O(lenS+lenP)//lenS和lenP为字符串s和p的长度
$$

```C++
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int lenS = s.length(), lenP = p.length(); //记录两个字符串长度
        if (lenS < lenP) { //字符串s长度小于字符串p，则必然没有子串
            return vector<int>();
        }
        int count[26] = {0}; //记录当前子串各字符在s中出现的次数-p中出现的次数（字符差）
        int differ = 0; //记录当前子串和p中有差异的字符的个数

        for (int i = 0; i < lenP; i++) { //从0开始的子串中各字符差
            count[s[i] - 'a']++;
            count[p[i] - 'a']--;
        }

        for (int i = 0; i < 26; i++) { //统计开始子串和p有差异的字符个数
            if (count[i] != 0) {
                differ++;
            }
        }

        vector<int> result; //记录所有p的异位词子串的起始索引

        if (differ == 0) {
            result.push_back(0); //differ为0，说明开始子串就是p的异位词
        }

        for (int i = 0; i < lenS -lenP; i++) { //考察i+1至i+lenP的子串是否为p的异位词
            //上一循环结束的子串为i至i+lenP-1，该子串移出第i位，加入第i+lenP位，为要考察的i+1至i+lenP的子串
            if (count[s[i] - 'a'] == 0) { //第i位字符在i至i+lenP-1的子串中出现次数刚好和p中相同，则移出后differ+1，即有差异的字符个数加一
                differ++;
            }
            else if (count[s[i] - 'a'] == 1) { //第i位字符在i至i+lenP-1的子串中出现次数刚好比p中错一次，则移出后differ-1，即有差异的字符个数减一
                differ--;
            }
            count[s[i] - 'a']--;

            if (count[s[i + lenP] - 'a'] == 0) { //移入第i+lenP位字符，操作原因同上
                differ++;
            }
            else if (count[s[i + lenP] - 'a'] == -1) {
                differ--;
            }
            count[s[i + lenP] - 'a']++;

            if (differ == 0) { //differ为0，说明要考察的i+1至i+lenP的子串就是p的异位词
                result.push_back(i + 1);
            }
        }

        return result;
    }
};
```

# 子串

## [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

### 解法1：前缀和

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> count; //记录前缀和出现的次数
        count[0] = 1; //避免前缀和等于k时后续操作出错；
        int pre = 0; //前缀和
        int result = 0; //记录符合要求的连续子数组个数
        for (int i = 0; i < nums.size(); i++) {
            pre += nums[i];
            result += count[pre - k]; //若之前出现过 当前前缀和-k 的前缀和，则加上该前缀和出现的次数
            count[pre]++;
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

## [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

### 解法1：双指针

$$
O(n)+O(C)
$$

```C++
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<int, int> Count; //记录各个字符在当前访问的s子串和t中出现的次数的差
        unordered_map<int, bool> st; //记录t中出现的字符

        for (int i = 0; i < t.length(); i++) { //记录t中各个字符出现的次数（以负数记录）并标记其出现过
            Count[t[i]]--;
            st[t[i]] = true;
        }
        int differ = 0; //记录子串中仍然缺少的字符数
        for (auto it:Count) {
            if (it.second < 0) { //出现负数，缺少的字符数加一
                differ++;
            }
        }

        int i = 0, j = 0; //双指针，表示当前访问子串
        int minI = s.length(), minJ = -1; //记录最小子串的起始位置和结束为止
        
        while (i < s.length()) {
            Count[s[i]]++; //当前字符出现次数加一
            if (Count[s[i]] == 0 && st[s[i]] == true) { //加一后该字符出现次数的大于等于零且该字符属于t中字符
                differ--;
            }
            if (differ > 0) { //当前子串仍未涵盖t中所有字符，移动i指针后继续下一循环
                i++;
                continue;
            }
            else { //当前子串已涵盖t中所有字符，开始移动j指针缩短子串长度
                while (differ == 0) {
                    if (i - j < minI - minJ) {
                        minI = i;
                        minJ = j;
                    }
                    Count[s[j]]--; //j指针右移后，s[j]被移出，字符出现次数减一
                    if (Count[s[j]] == -1 && st[s[j]] == true) { //减一后该字符出现次数的差为-1且该字符属于t中字符
                        differ++;
                        i++;
                    }
                    j++;
                }
            }
        }

        if (minJ != -1) {
            return s.substr(minJ, minI - minJ + 1);
        }
        else {
            return "";
        }
    }
};
```

# 普通数组

## [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int> dp(nums.size() + 1, 0); //dp数组，含义为以当前位置结尾的最大和的连续子数组
        int result = -10010; //记录结果
        for (int i = 0; i < nums.size(); i++) {
            dp[i + 1] = max(nums[i], nums[i] + dp[i]); //若前一位的最大和大于零，则当前位接上去，否则只取当前位
            result = max(result, dp[i + 1]); //更新结果
        }
        return result;
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
        int sum = 0; //记录当前位置的最大连续子数组和
        int result = -10010; //记录结果
        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i]; //更新sum
            result = max(result, sum); //更新result
            if (sum < 0) { //如果sum小于零，则重新开始计算最大和
                sum = 0;
            }
        }
        return result;
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
    static bool cmp(const vector<int>& u,const vector<int>& v) {
        if (u[0] != v[0]) { //若左边界不相等，则按左边界从小到大排
            return u[0] < v[0];
        }
        else { //若左边界相等，则按右边界从小到大排
            return u[1] < v[1];
        }
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp); //将所有区间排序
        vector<vector<int>> result; //存储合并后的区间

        int left = intervals[0][0], right = intervals[0][1]; //存储合并区间的左右边界
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] > right) { //形成一个合并区间
                result.push_back({left, right}); //存储合并区间
                left = intervals[i][0];
                right = intervals[i][1];
            }
            else { //可以将当前区间加入合并区间，扩大合并区间边界
                right = max(right, intervals[i][1]);
            }
        }

        result.push_back({left, right}); //循环结束后还有最后一个合并区间需要收入

        return result;
    }
};
```

## [189. 轮转数组](https://leetcode.cn/problems/rotate-array/)

### 解法1：三次翻转

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k = k % nums.size(); //当k大于数组个数时，轮转结果和k取余后是一样的
        for (int i = 0; i < nums.size() / 2; i++) { //对数组整体翻转
            swap(nums[i], nums[nums.size() - 1 - i]);
        }
        for (int i = 0; i < k / 2; i++) { //翻转前k个数
            swap(nums[i], nums[k - 1 - i]);
        }
        for (int i = k; i <(nums.size() + k) / 2; i++) { //对后nums.size()-k个数进行翻转
            swap(nums[i], nums[nums.size() - 1 - (i - k)]);
        }
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
        vector<int> left(nums.size(), 1), right(nums.size(), 1); //两个数组分别存储对应位置左侧的乘积和右侧的乘积
        vector<int> result(nums.size()); //记录结果
        for (int i = 1; i < nums.size(); i++) {
            left[i] = left[i - 1] * nums[i - 1];
        }
        for (int i = nums.size() - 2; i >= 0; i--) {
            right[i] = right[i + 1] * nums[i + 1];
        }
        for (int i = 0; i < nums.size(); i++) {
            result[i] = left[i] * right[i];
        }
        
        return result;
    }
};
```

### 解法2：动态规划（空间复杂度优化）

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> result(nums.size(), 1); //存储结果
        for (int i = 1; i < nums.size(); i++) { //先乘左侧乘积
            result[i] = result[i - 1] * nums[i - 1];
        }
        int temp = 1;
        for (int i = nums.size() - 2; i >= 0; i--) { //再乘右侧乘积
            temp *= nums[i + 1];
            result[i] *= temp;
        }
        return result;
    }
};
```

## [41. 缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/)

### 解法1：哈希

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        vector<bool> st(nums.size() + 1, false); //记录已经出现的正整数

        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0 && nums[i] <= nums.size()) { //标记在1到nums.size()范围内出现的正数
                st[nums[i]] = true;
            }
        }

        for (int i = 1; i <= nums.size(); i++) { //寻找在1到nums.size()范围内未出现的正数
            if (st[i] == false) {
                return i;
            }
        }

        return nums.size() + 1; ////1到nums.size()范围内的正数都已出现，则返回nums.size()+1
    }
};
```

### 解法2：置换

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i++) {
            while (nums[i] > 0 && nums[i] <= nums.size() && nums[i] != nums[nums[i] - 1]) { //交换nums[i]和nums[nums[i] - 1]，直到两者相等，说明nums[i]已经被放在正确的位置上
                swap(nums[nums[i] - 1], nums[i]);
            }
        }

        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != i + 1) { //若当前位置上的数不在1到nums.size()的范围内，则说明i+1为缺失的最小正数
                return i + 1;
            }
        }
        return nums.size() + 1; //所有位置上的数都在1到nums.size()的范围内，则nums.size()+1为缺失的最小正数
    }
};
```

# 矩阵

## [73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

### 解法1：使用矩阵第一行和第一列作为标记变量

$$
O(mn)+O(1)
$$

```C++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size(); //记录行数
        int n = matrix[0].size(); //记录列数
        bool flagRow = false, flagCol = false; //记录第一行和第一列是否出现0

        //算法的整体思路是借助矩阵的第一行和第一列记录含有0的行和列
        for (int i = 0; i < m; i++) { //记录第一列是否出现0
            if (matrix[i][0] == 0) {
                flagCol = true;
                break;
            }
        }
        for (int i = 0; i < n; i++) { //记录第一行是否出现0
            if (matrix[0][i] == 0) {
                flagRow = true;
                break;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        //置0
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (flagRow) { //第一行置0
            for (int i = 0; i < n; i++) {
                matrix[0][i] = 0;
            }
        }
        if (flagCol) { //第一列置0
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }

    }
};
```

## [54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)

### 解法1：模拟

$$
O(mn)+O(1)
$$

```C++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
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

## [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)

### 解法1：模拟

$$
O(n^2)+O(1)
$$

```C++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for(int i = 0; i < n / 2; i++){
            for(int j = i; j < n - 1 - i; j++){ //每次从对角线往右走
                int temp                     = matrix[i][j];
                matrix[i][j]                 = matrix[n - 1 - j][i];
                matrix[n - 1 - j][i]         = matrix[n - 1 - i][n - 1 - j];
                matrix[n - 1 - i][n - 1 - j] = matrix[j][n - 1 - i];
                matrix[j][n - 1 - i]         = temp;
            }
        }
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
        if (matrix.size() == 0 || matrix[0].size() == 0) { //矩阵宽或高为0，则直接返回false
            return false;
        }
        int i = 0, j = matrix[0].size() - 1; //从右上角开始找
        while (i < matrix.size() && j >= 0) {
            if (matrix[i][j] == target) { //找到了，退出循环，并返回true
                return true;
            }
            else if(matrix[i][j] < target) { //当前值比目标值小，往下走
                i++;
            }
            else { //当前值比目标值大，往左走
                j--;
            }
        }
        return false; //没找到，返回false
    }
};
```

# 链表

## [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

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
        ListNode *p = headA, *q = headB; //用于遍历两个单链表
        int m = 0, n = 0; //记录链表A和链表B的结点个数
        while (p != NULL) { //记录链表A的结点个数
            p = p->next;
            m++;
        }
        while (q != NULL) { //记录链表B的结点个数
            q = q->next;
            n++;
        }
        int k; //记录两个链表的结点个数之差
        if (m >= n) { //p重置为个数多的那个链表的头结点，q重置为个数少的那个链表的头结点
            p = headA;
            q = headB;
            k = m - n;
        }
        else {
            p = headB;
            q = headA;
            k = n - m;
        }
        for (int i = 0; i < k; i++) { //p往后移动k个结点，控制p和q离相交节点的结点个数相同
            p = p->next;
        }
        while (p != q) { //向后同步移动p和q，直到二者相遇或同时为NULL
            p = p->next;
            q = q->next;
        }
        return p;
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
            //若fast最终不为NULL，则链表结点数为奇数，slow位置为[前半段，中间结点，后半段]，slow位置为中间结点
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
        if (head == NULL || head->next == NULL) { //若节点数少于两个，则必为回文链表
            return true;
        }

        ListNode* dummyHead1 = new ListNode();
        dummyHead1->next = head;
        ListNode* slow = dummyHead1, * fast = dummyHead1; //快慢指针
        while (fast != NULL && fast->next != NULL) { //当fast后继指针不足两个时，退出循环
            slow = slow->next; //慢指针走一步
            fast = fast->next->next; //快指针走两步
        }
        
        //使用头插法将原链表后半段逆置
        ListNode* p = slow->next, * q;
        ListNode* dummyHead2 = new ListNode();
        while (p != NULL) {
            q = p;
            p = p->next;
            q->next = dummyHead2->next;
            dummyHead2->next = q;
        }
        
        //完成逆置后，两个链表除了第一个链表可能多一个节点，其他节点的值必须相同
        p = dummyHead1->next;
        q = dummyHead2->next;
        while (q != NULL) {
            if (p->val != q->val) { //当节点值不相同时，退出循环
                break;
            }
            p = p->next;
            q = q->next;
        }
        

        if (q == NULL) { //第二个链表走到了最后，说明是回文链表
            return true;
        }
        else { //第二个链表没走到最后，说明中途出现了值不同的结点，不是回文链表
            return false;
        }
    }
};
```

## ==[141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)==

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
        if (head == NULL || head->next == NULL) { //节点数少于两个，则无环
            return false;
        }

        ListNode* slow = head, * fast = head; //快慢指针均从头节点处出发
        do {
            if (fast == NULL || fast->next == NULL) { //说明是无环单链表
                return false;
            }
            slow = slow->next; //慢指针一次走一步
            fast = fast->next->next; //快指针一次走两步
        } while (slow != fast);
        return slow; //正常退出循环，说明存在环
    }
};
```

## ==[142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)==

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
        if (head == NULL || head->next == NULL) { //节点数少于两个，则无环
            return NULL;
        }

        ListNode* slow = head, * fast = head; //快慢指针均从头节点处出发
        do {
            if (fast == NULL || fast->next == NULL) { //说明是无环单链表
                return NULL;
            }
            slow = slow->next; //慢指针一次走一步
            fast = fast->next->next; //快指针一次走两步
        } while (slow != fast);

        slow = head;
        while (slow != fast) {
            slow = slow->next;
            fast = fast->next;
        }

        return slow; //正常退出循环，说明存在环
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
        ListNode* dummyHead = new ListNode(), * p = dummyHead; //使用尾插法进行归并
        while (list1 != NULL && list2 != NULL) { //当两个链表都还有元素时
            if (list1->val < list2->val) { //链表1当前的元素更小
                p->next = list1;
                list1 = list1->next;
            }
            else { //链表2当前的元素更小
                p->next = list2;
                list2 = list2->next;
            }
            p = p->next;
        }
        if (list1 != NULL) { //链表1还没到链表尾端
            p->next = list1;
        }
        else { //链表2还没到链表尾端
            p->next = list2;
        }

        return dummyHead->next;
    }
};
```

## [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

### 解法1：大数加法+头插法

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
        ListNode * dummyHead = new ListNode(0), * cur = dummyHead; //虚拟头结点统一操作
        int t = 0; //存储进位
        ListNode * p = l1, * q = l2;
        while (p || q || t) { //遍历l1和l2
            int x = t; //计算当前位的和
            if (p) {
                x += p->val;
                p = p->next;
            }
            if (q) {
                x+= q->val;
                q = q->next;
            }
            t = x / 10; //保留进位
            x = x % 10; //取出当前位；
            ListNode * now = new ListNode(x); //构造当前节点
            cur->next = now; //尾插法
            cur = cur->next;
        }
        return dummyHead->next;
    }
};
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

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
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * dummyHead = new ListNode(0), * pre = dummyHead; //增加虚拟头节点统一操作
        dummyHead->next = head;
        ListNode * low = head, * fast = head; //双指针，一个走快点
        while (n--) { //快指针先走n步
            fast = fast ->next;
        }
        while (fast) { //快指针走到底时，慢指针走到倒数第n个节点
            pre = pre->next;
            low = low->next;
            fast = fast->next;
        }
        pre->next = low->next; //删除倒数第n个节点
        low->next = NULL;
        return dummyHead->next;
    }
};
```

## [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

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
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == NULL || head->next == NULL) { //少于两个节点，不需要交换，直接返回head
            return head;
        }
        ListNode * dummyHead = new ListNode(0); //增加虚拟头节点统一操作
        dummyHead->next = head;
        ListNode * pre = dummyHead, * p1 = pre->next, * p2 = p1->next;
        while (p1 && p2) {
            p1->next = p2->next; //交换
            pre->next = p2;
            p2->next = p1;
            pre = pre->next->next; //移动pre，p1和p2
            p1 = pre->next;
            if (p1) {
                p2 = p1->next;
            }
        }
        return dummyHead->next;
    }
};
```

## [25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

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
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode * dummyHead = new ListNode(0); //增加虚拟头节点统一操作
        dummyHead->next = head;
        ListNode * pre = dummyHead, * cur;
        while (true) {
            cur = pre;
            for (int i = 0; i < k; i++) { //cur往pre后移动k个节点
                cur = cur->next;
                if (cur == NULL) { //cur为空，说明已不足k个节点，可以返回结果
                    return dummyHead->next;
                }
            }
            //以下操作将pre后k个节点逆置
            ListNode * newPre = NULL, * newCur = pre->next; //断链，取出pre后面k个节点
            pre->next = cur->next;
            cur->next = NULL;
            while (newCur) {
                newPre = newCur;
                newCur = newCur->next;
                newPre->next = pre->next; //头插法实现逆置
                pre->next = newPre;
            }
            for (int i = 0; i < k; i++) { //pre往后移动k个节点
                pre = pre->next;
            }
        }
        return NULL; //实际不会执行到这一步
    }
};
```

## [138. 复制带随机指针的链表](https://leetcode.cn/problems/copy-list-with-random-pointer/)

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
        else if (cachedNode.count(head) == NULL) {
            Node * newHead = new Node(head->val);
            cachedNode[head] = newHead;
            newHead->next = copyRandomList(head->next);
            newHead->random = copyRandomList(head->random);
        }
        return cachedNode[head];
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
        for (Node * node = head; node != NULL; node = node->next->next) {
            Node * newNode = new Node(node->val);
            newNode->next = node->next;
            node->next = newNode;
        }
        for (Node * node = head; node != NULL;node = node->next->next) {
            Node * newNode = node->next;
            if (node->random) {
                newNode->random = node->random->next;
            }
            else {
                newNode->random = NULL;
            }
        }
        Node * newHead = head->next;
        for (Node * node = head; node != NULL; node = node->next) {
            Node * newNode = node->next;
            node->next = newNode->next;
            if (node->next) {
                newNode->next = node->next->next;
            }
            else {
                newNode->next = NULL;
            }
        }
        return newHead;
    }
};
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
                ListNode* head1 = cur;
                for (int i = 1; i < subLength && cur->next; i++) {//"cur->next"作为判断条件是为了让head2至少有一个结点
                    cur = cur->next;
                }
                //取出第二段链表
                ListNode* head2 = cur->next;
                cur->next = NULL;//断链
                cur = head2;//重置cur别忘了
                for (int i = 1; i < subLength && cur; i++) {//"cur"作为判断条件
                    cur = cur->next;
                }
                ListNode* next = NULL;//next用于记录取出两段链表后剩下的那段（用于下一次处理），避免锻炼
                if (cur){
                    next = cur->next;
                    cur->next = NULL;//断链
                }
                cur = next;//重置cur别忘了
                pre->next = merge(head1, head2);//将两段归并后的链表连在pre后面
                for (pre = pre->next; pre->next; pre = pre->next);//pre移动到新链表的末尾
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
        ListNode* dummyHead = new ListNode(), * p = dummyHead;
        while (true) {
            int index; //用于检查是否所有链表都已经完成归并
            for (index = 0; index < lists.size(); index++) {
                if (lists[index]) {
                    break;
                }
            }
            if (index == lists.size()) { //所有链表都已归并，可以直接退出循环
                break;
            }
            index = -1; //此时index用于记录头节点值最小的链表的下标
            for (int i = 0; i < lists.size(); i++) {
                if (lists[i] == NULL) { //第i个链表已经全部归并，不需要继续访问
                    continue;
                }
                else {
                    if (index == -1) {
                        index = i;
                    }
                    else {
                        index = (lists[index]->val < lists[i]->val ? index : i);
                    }
                }
            }
            p->next = lists[index];
            lists[index] = lists[index]->next;
            p = p->next;
        }
        p->next = NULL;
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
    struct cmp { //cmp结构体用于实现小顶堆
        bool operator()(ListNode * a, ListNode * b) {
            return a->val > b->val; //注意是大于，不是小于
        }
    };

    //优先队列定义的三个必要参数：<单个元素，实现容器（一般为vector），比较结构体>
    priority_queue<ListNode*, vector<ListNode*>, cmp> que; //优先队列，即堆

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        for (auto node:lists) { //将链表的所有头节点加入堆
            if (node) {
                que.push(node);
            }
        }
        ListNode* dummyHead = new ListNode(), * p = dummyHead;
        while (!que.empty()) {
            ListNode* node = que.top(); //访问堆顶，并弹出堆顶
            que.pop();
            p->next = node; //堆顶对应的节点的值最小，直接加入新链表中
            p = p->next;
            if (node->next) { //若弹出的堆顶节点有后继节点，则将其加入堆（就是在实现对每条链表的遍历）
                que.push(node->next);
            }
        }
        return dummyHead->next;
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

## [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

### 解法1：哈希+双链表

$$
O(1)+O(n)
$$

```C++
struct DLinkedNode { //双链表
    int key, value; //键和值
    DLinkedNode * prev, * next; //前驱结点和后继节点
    DLinkedNode(): key(0), value(0), prev(NULL), next(NULL) {};
    DLinkedNode(int _key, int _value): key(_key), value(_value), prev(NULL), next(NULL) {};
};

class LRUCache {
public:
    unordered_map<int, DLinkedNode*> hash; //哈希表，用于根据键找到双链表节点
    DLinkedNode* dummyHead, * dummyTail; //虚拟头节点和虚拟尾节点
    int size, capacity; //size记录当前LRU缓存大小，capacity记录LRU缓存上限
    LRUCache(int capacity) {
        //初始化虚拟头节点和虚拟尾节点
        dummyHead = new DLinkedNode();
        dummyTail = new DLinkedNode();
        dummyHead->next = dummyTail;
        dummyTail->prev = dummyHead;
        //初始化size和capacity
        this->size = 0;
        this->capacity = capacity;
    }
    
    int get(int key) {
        if (hash.count(key) == 0) {
            return -1;
        }
        else {
            DLinkedNode* node = hash[key];
            //将该节点从双链表中取出，并将其前驱节点和后继节点重新连接
            node->prev->next = node->next;
            node->next->prev = node->prev;
            //将该结点插入双链表头部
            node->prev = dummyHead;
            node->next = dummyHead->next;
            dummyHead->next->prev = node;
            dummyHead->next = node;
            return node->value;
        }
    }
    
    void put(int key, int value) {
        if (hash.count(key) == 0) {
            DLinkedNode* node = new DLinkedNode(key, value);
            //将该结点插入双链表头部
            node->prev = dummyHead;
            node->next = dummyHead->next;
            dummyHead->next->prev = node;
            dummyHead->next = node;
            hash[key] = node;
            size++;
            if (size > capacity) {
                //将与尾节点连接的节点从双链表中驱逐出去
                DLinkedNode* tailNode = dummyTail->prev;
                tailNode->prev->next = tailNode->next;
                tailNode->next->prev = tailNode->prev;
                hash.erase(tailNode->key);
                size--;
            }
        }
        else {
            DLinkedNode* node = hash[key];
            node->value = value;
            //将该节点从双链表中取出，并将其前驱节点和后继节点重新连接
            node->prev->next = node->next;
            node->next->prev = node->prev;
            //将该结点插入双链表头部
            node->next = dummyHead->next;
            dummyHead->next->prev = node;
            dummyHead->next = node;
            node->prev = dummyHead;
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

# 二叉树

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
    void inOrder(TreeNode* cur, vector<int>& result) {
        if (cur == NULL) {
            return;
        }
        inOrder(cur->left, result);
        result.push_back(cur->val);
        inOrder(cur->right, result);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result; //存储结果
        inOrder(root, result);
        return result;
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
        vector<int> result; //存储结果
        stack<TreeNode*> st; //辅助站实现迭代访问
        TreeNode* cur = root; //访问的当前元素
        while (cur || !st.empty()) {
            if (cur) {
                st.push(cur); //当前元素压栈
                cur = cur->left;
            }
            else {
                cur = st.top(); //访问栈顶元素
                st.pop();
                result.push_back(cur->val);
                cur = cur->right;
            }
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
        if (root == NULL) { //空树，返回深度为0
            return 0;
        }
        return 1 + max(maxDepth(root->left), maxDepth(root->right)); //否则，树的深度为左子树和右子树的最大深度中的较大值+1
    }
};
```

## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

### 解法1：递归

$$
O(n)+O(h)
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
        if (root == NULL) {
            return root;
        }
        invertTree(root->left), invertTree(root->right); //翻转左右子树
        TreeNode *tmp = root->left; //交换左右子树
        root->left = root->right;
        root->right = tmp;
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

## [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

### 解法1：递归

$$
O(n)+O(h)
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
    bool compare(TreeNode *left_node, TreeNode *right_node) {
        if (left_node == NULL || right_node == NULL) {
            return left_node == NULL && right_node == NULL;
        }
        else if (left_node->val != right_node->val) {
            return false;
        }
        else {
            return compare(left_node->left,right_node->right) && compare(left_node->right,right_node->left);
        }
    }
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) {
            return true;
        }
        return compare(root->left, root->right);
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

## [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

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
    int maxLength = 0;
    int depth(TreeNode* node) {
        if (node == NULL) {
            return 0;
        }
        int leftDepth = depth(node->left); //计算左子树深度
        int rightDepth = depth(node->right); //计算右子树深度
        maxLength = max(maxLength, leftDepth + rightDepth); //更新最大直径
        return max(leftDepth, rightDepth) + 1; //当前节点深度为左右子树深度较大值+1
    }
    int diameterOfBinaryTree(TreeNode* root) {
        depth(root);
        return maxLength;
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
        vector<vector<int>> result; //存放结果
        queue<TreeNode*> que;
        if (root) {
            que.push(root); //根结点入队列
        }
        while (!que.empty()) {
            int Size = que.size(); //提前记录改行的节点个数
            vector<int> vec;
            for(int i = 0; i < Size; i++) {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) {
                    que.push(node->left);
                }
                if (node->right) {
                    que.push(node->right);
                }
            }
            result.push_back(vec);
        }

        return result;
    }
};
```

## [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

### 解法1：递归

$$
O(n)+O(\log n)
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
    TreeNode *traversal(vector<int> &nums,int left,int right)
    {
        if (left>right) { //递归出口
            return NULL;
        }
        int mid = left + right >> 1; //树的根结点下标
        TreeNode *node = new TreeNode(nums[mid]);
        node->left = traversal(nums, left, mid - 1); //递归处理左子树
        node->right = traversal(nums, mid + 1, right); //递归处理右子树
        return node;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return traversal(nums, 0, nums.size() - 1);
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
    vector<int> vec; //存储中序遍历结果的数组
    bool isValidBST(TreeNode* root) {
        if (root == NULL) {
            return true; //认为空树符合二叉搜索树定义
        }
        bool leftFlag = isValidBST(root->left); //检查左子树是否是二叉搜索树
        if (vec.size() > 0 && root->val <= vec.back()) { //若左子树节点的最大值大于等于根结点的值，则说明不符合二叉搜索树定义
            return false;
        }
        vec.push_back(root->val); //将根节点的值加入数组中
        bool rightFlag = isValidBST(root->right); //检查右子树是否是二叉搜索树
        return leftFlag && rightFlag; //左子树和右子树都符合二叉搜索树定义，才能推出整体是一棵二叉搜索树
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

## [230. 二叉搜索树中第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)

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
    vector<int> vec; //存储中序遍历结果的数组
    void inOrder(TreeNode* root) {
        if (root == NULL) {
            return; //空树为递归出口
        }
        inOrder(root->left); //递归遍历左子树
        vec.push_back(root->val); //将根节点的值加入数组
        inOrder(root->right); //递归遍历右子树
    }
    int kthSmallest(TreeNode* root, int k) {
        inOrder(root);
        return vec[k - 1];
    }
};
```

## [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

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
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        queue<TreeNode*> que;
        if (root) {
            que.push(root);
        }
        while (!que.empty()) {
            int Size = que.size();
            for (int i = 0; i < Size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (i == Size - 1) {
                    result.push_back(node->val);
                }
                if (node->left) {
                    que.push(node->left);
                }
                if(node->right) {
                    que.push(node->right);
                }
            }
        }

        return result;
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
        if (node == NULL) {
            return;
        }
        vec.push_back(node);
        traversal(node->left);
        traversal(node->right);
    }
    void flatten(TreeNode* root) {
        if (root == NULL) {
            return;
        }
        traversal(root);
        for (int i = 0; i < vec.size() - 1; i++) {
            vec[i]->left = NULL;
            vec[i]->right = vec[i + 1];
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
        if (root == NULL) {
            return;
        }
        stack<TreeNode*> st;
        vector<TreeNode*> vec;
        TreeNode* node = root;
        while (node || !st.empty()){
            if (node) {
                st.push(node);
                vec.push_back(node);
                node = node->left;
            }
            else {
                node = st.top();
                st.pop();
                node = node->right;
            }
        }
        for (int i = 0; i < vec.size() - 1; i++){
            vec[i]->left = NULL;
            vec[i]->right = vec[i + 1];
        }
    }
};
```

##[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

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
    TreeNode* traversal(int left1, int right1, int left2, int right2, const vector<int>& preorder, const vector<int>& inorder) {
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
        TreeNode *node = new TreeNode(preorder[left1]);
        node->left = traversal(left1 + 1, left1 + len1, left2, mid - 1, preorder, inorder);
        node->right = traversal(left1 + len1 + 1, right1, mid + 1, right2, preorder, inorder);
        return node;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int left1 = 0, right1 = preorder.size() - 1, left2 = 0, right2 = inorder.size() - 1;
        TreeNode *root = traversal(left1, right1, left2, right2, preorder, inorder);
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
    map<int,int> index;
    TreeNode* traversal(int left1, int right1, int left2, int right2, const vector<int>& preorder, const vector<int>& inorder) {
        if (left1 > right1 || left2 > right2) {
            return NULL;
        }
        int mid = index[preorder[left1]];
        int len1 = mid - left2;
        TreeNode *node = new TreeNode(preorder[left1]);
        node->left = traversal(left1 + 1, left1 + len1, left2, mid - 1, preorder, inorder);
        node->right = traversal(left1 + len1 + 1, right1, mid + 1, right2, preorder, inorder);
        return node;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for (int i = 0; i < inorder.size(); i++) {
            index[inorder[i]]=i;
        }
        int left1 = 0, right1 = preorder.size() - 1, left2 = 0, right2 = inorder.size() - 1;
        TreeNode *root = traversal(left1, right1, left2, right2, preorder, inorder);
        return root;
    }
};
```

## [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

### 解法1：前缀和

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
    int count = 0; //记录符合条件的路径的条数
    long long curSum = 0; //记录当前和
    unordered_map<long long, int> prefixSum; //记录前缀和的个数
    void DFS(TreeNode* node, int targetSum) {
        if (node == NULL) {
            return;
        }
        curSum += node->val;
        count += prefixSum[curSum - targetSum];
        prefixSum[curSum]++;
        DFS(node->left, targetSum);
        DFS(node->right, targetSum);
        prefixSum[curSum]--;
        curSum -= node->val;
    }
    int pathSum(TreeNode* root, int targetSum) {
        prefixSum[0] = 1;
        DFS(root, targetSum);
        return count;
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
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL) {
            return NULL;
        }
        if (root == p || root == q) {
            return root;
        }
        TreeNode *left = lowestCommonAncestor(root->left, p, q);
        TreeNode *right = lowestCommonAncestor(root->right, p, q);
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
        hash[root->val]=NULL;
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
        int leftSum1 = INT_MIN / 3, leftSum2 = INT_MIN / 3;
        if (node->left) {
            traversal(node->left, leftSum1, leftSum2);
        }
        int rightSum1 = INT_MIN / 3, rightSum2 = INT_MIN / 3;
        if (node->right) {
            traversal(node->right, rightSum1, rightSum2);
        }
        sum1 = max(node->val, max(node->val + leftSum1, node->val + rightSum1));
        sum2 = max(node->val + leftSum1 + rightSum1, max(max(leftSum1, leftSum2), max(rightSum1, rightSum2)));
    }
    int maxPathSum(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        int sum1 = INT_MIN / 3, sum2 = INT_MIN / 3;
        traversal(root, sum1, sum2);
        return max(sum1, sum2);
    }
};
```

# 图论

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
        grid[i][j] = '0';
        int dx[4] = {0, 1, 0, -1};
        int dy[4] = {1, 0, -1, 0};
        for (int k = 0; k < 4; k++){
            int x = i + dx[k], y = j + dy[k];
            if(x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == '1') BFS(x, y, grid);
        }
    }
    int numIslands(vector<vector<char>>& grid) {
        int count = 0;
        m = grid.size(), n = grid[0].size();
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                if (grid[i][j] == '1'){
                    BFS(i, j, grid);
                    count++;
                }
            }
        }
        return count;
    }
};
```

## [994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/)

### 解法1：BFS

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    int maxTime = 0;
    void dijkstra(vector<vector<int>>& grid, int x, int y, int m, int n) {
        queue<pair<int, int>> que;
        que.push({x, y});
        int dx[4] = {1, 0, -1, 0};
        int dy[4] = {0, 1, 0, -1};
        while (!que.empty()) {
            pair<int, int> index = que.front();
            que.pop();
            for (int i = 0; i < 4; i++) {
                int newX = index.first + dx[i];
                int newY = index.second + dy[i];
                if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                    if (grid[newX][newY] == 0) {
                        continue;
                    }
                    else {
                        if (grid[newX][newY] == 1) {
                            grid[newX][newY] = grid[index.first][index.second] + 1;
                            que.push({newX, newY});
                        }
                        else {
                            if (grid[index.first][index.second] + 1 < grid[newX][newY]) {
                                grid[newX][newY] = grid[index.first][index.second] + 1;
                                que.push({newX, newY});
                            }
                        }
                    }
                }
            }
        }
    }
    int orangesRotting(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size(); //m和n分别表示网格的行数和列数
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) {
                    dijkstra(grid, i, j, m, n);
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    return -1;
                }
                if (grid[i][j] >= 2) {
                    maxTime = max(maxTime, grid[i][j] - 2);
                }
            }
        }
        return maxTime;
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

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

# 回溯

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
    void backtracking(const vector<int> &nums) {
        if (path.size() == nums.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (used[nums[i] + 10]) {
                continue;
            }
            used[nums[i] + 10] = true;
            path.push_back(nums[i]);
            backtracking(nums);
            used[nums[i] + 10] = false;
            path.pop_back();
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        memset(used, false, sizeof(used));
        backtracking(nums);
        return result;
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
    void backtracking(const vector<int> &nums, int index) {
        result.push_back(path);
        for (int i = index; i < nums.size(); i++) {
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        result.clear();
        path.clear();
        backtracking(nums, 0);
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

## [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

### 解法1：回溯

$$
O(3^m·4^n)+O(m+n)
$$

```C++
class Solution {
public:
    vector<string> telephone = {"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    vector<string> result;
    string path;
    void backtracking(const string& digits, int index) {
        if (index == digits.size()) {
            result.push_back(path);
            return;
        }
        string s = telephone[digits[index] - '0' - 2];
        for (int i = 0; i < s.size(); i++) {
            path.push_back(s[i]);
            backtracking(digits, index + 1);
            path.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        if (digits.size() == 0) {
            return {};
        }
        backtracking(digits, 0);
        return result;
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
    int sum = 0;
    void backtracking(const vector<int>& candidates, int target, int index) {
        if (sum == target) {
            result.push_back(path);
            return;
        }
        for (int i = index; i < candidates.size(); i++) {
            if (sum + candidates[i] <= target) {
                sum += candidates[i];
                path.push_back(candidates[i]);
                backtracking(candidates, target, i); //最后一个参数是i而不是i+1，因为元素可以无限制重复被选取
                sum -= candidates[i];
                path.pop_back();
            }
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        backtracking(candidates, target, 0);
        return result;
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
    void backtracking(string s, int left, int right, int n) {
        if (left == n && right == n) {
            result.push_back(s);
            return;
        }
        if (left + 1 <= n) {
            backtracking(s + "(", left + 1, right, n);
        }
        if (left > right) {
            backtracking(s + ")", left, right + 1, n);
        }
    }
    vector<string> generateParenthesis(int n) {
        backtracking("", 0, 0, n);
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
        if (index == word.length() - 1 && board[x][y] == word[index]) {
            return true;
        }
        else if (board[x][y] != word[index]) {
            return false;
        }
        int dx[]={0,1,0,-1};
        int dy[]={1,0,-1,0};
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

## [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

### 解法1：回溯

$$
略
$$

```C++
class Solution {
public:
    vector<vector<string>> result;
    vector<string> path;
    bool isPalindrome(const string& s, int left, int right) {
        for (int i = left, j = right; i < j; i++, j--) {
            if (s[i] != s[j]) {
                return false;
            }
        }
        return true;
    }
    void backtracking(const string& s, int index) {
        if (index == s.size()) {
            result.push_back(path);
            return;
        }
        for (int i = index; i < s.size(); i++) {
            if (isPalindrome(s, index, i)) {
                path.push_back(s.substr(index, i - index + 1));
                backtracking(s, i + 1);
                path.pop_back();
            }
        }
    }
    vector<vector<string>> partition(string s) {
        result.clear();
        path.clear();
        backtracking(s, 0);
        return result;
    }
};
```

## [51. N 皇后](https://leetcode.cn/problems/n-queens/)

### 解法1：回溯

$$
O(n!)+O(n)
$$

```C++
class Solution {
public:
    vector<vector<string>> result;
    vector<string> path;
    vector<bool> visit;
    vector<int> col;
    void backtracking(int index, int n) {
        if (index == n) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < n; i++) { //[index, i]
            if (visit[i] == false) {
                bool flag = true;
                for (int j = 0; j < index; j++) { //[j, col[j]]
                    if (abs(index - j) == abs(i - col[j])) {
                        flag = false;
                        break;
                    }
                }
                if (flag == true) {
                    visit[i] = true;
                    col[index] = i;
                    string s = "";
                    for (int j = 0; j < n; j++) {
                        if (j == i) {
                            s += 'Q';
                        }
                        else {
                            s +='.';
                        }
                    }
                    path.push_back(s);
                    backtracking(index + 1, n);
                    visit[i] = false;
                    path.pop_back();
                }
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        for (int i = 0; i < n; i++) {
            visit.push_back(false);
            col.push_back(0);
        }
        backtracking(0, n);
        return result;
    }
};
```

# 二分查找

## [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

### 解法1：二分查找

$$
O(\log n)+O(1)
$$

```C++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0, right = nums.size(); //因为target可能插在数组的后面，因此right取nums.size()
        while (left < right) {
            int mid = left + right >> 1;
            if (nums[mid] >= target) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

## [74. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

### 解法1：贪心

$$
O(m+n)+O(1)
$$

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size(); //m为行数，n为列数
        int x = 0, y = n - 1; //从右上角开始找起
        while (x < m && y >= 0) {
            if (matrix[x][y] == target) {
                return true;
            }
            else if (matrix[x][y] < target) { //比目标小，往下移
                x++;
            }
            else { //比目标大，往左移
                y--;
            }
        }
        return false;
    }
};
```

### 解法2：二分查找

$$
O(\log (mn))+O(1)
$$

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size(); //m为行数，n为列数
        int left = 0, right = m * n - 1;
        while (left < right) {
            int mid = left + right >> 1;
            if (matrix[mid / n][mid % n] >= target) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }
        return matrix[left / n][left % n] == target;
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
        vector<int> leftAndRight;
        if (nums.size() == 0) { //数组长度为0，则肯定找不到
            leftAndRight.push_back(-1), leftAndRight.push_back(-1);
            return leftAndRight;
        }
        
        //找第一个位置，即最左端点
        int left = 0, right = nums.size() - 1;
        while (left < right) {
            int mid = left + right >> 1;
            if (nums[mid] >= target) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }

        if (nums[left] == target) { //第一个位置对应的元素和target相等，说明数组中存在target
            leftAndRight.push_back(left);
        }
        else {  //第一个位置对应的元素和target不相等，说明数组中不存在target
            leftAndRight.push_back(-1), leftAndRight.push_back(-1);
            return leftAndRight;
        }

        //先最后一个位置，即最右端点
        left = 0, right = nums.size() - 1;
        while (left < right) {
            int mid = left + right + 1 >> 1;
            if (nums[mid] <= target) {
                left = mid;
            }
            else {
                right = mid - 1;
            }
        }
        leftAndRight.push_back(left);

        return leftAndRight;
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
        //先找出旋转后的最小数的位置
        int left = 0, right = nums.size() - 1;
        int beginNum = nums[0], endNum = nums[nums.size() - 1];
        while (left < right) {
            int mid = left + right >> 1;
            if (nums[mid] <= endNum) {
                right=mid;
            }
            else {
                left = mid + 1;
            }
        }
        int minNum = nums[left];

        //minNum将数组分为两段，根据target的大小确定在哪段中找target
        if (left > 0 && target >= beginNum) {
            right = left - 1, left = 0;
        }
        else {
            right = nums.size() - 1;
        }
        while (left < right) {
            int mid = left + right >> 1;
            if (nums[mid] >= target) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }

        if (nums[left] == target) {
            return left;
        }
        else {
            return -1;
        }
    }
};
```

## [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

### 解法1：二分查找

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        int beginNum = nums[0], endNum = nums[nums.size() - 1];
        while (left < right) {
            int mid = left + right >> 1;
            if (nums[mid] <= endNum) {
                right=mid;
            }
            else {
                left = mid + 1;
            }
        }
        return nums[left];
    }
};
```

## ==[4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)==

### 解法1：二分查找（左闭右开）

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size() > nums2.size()) {
            swap(nums1, nums2);
        }
        int m = nums1.size(), n = nums2.size();
        //按左闭右开查
        int left = 0, right = m;
        /*
         左闭右开+此mid公式:
         偶数个元素，定位到中线右侧，因此totalLeft刚好为 左半元素 的个数
         奇数个元素，定位到中间右侧，因此totalLeft刚好为 左半元素+中位数 的个数
        */
        int totalLeft = m + n + 1 >> 1;
        /*
         二分完后：
         偶数个元素，定位到中线右侧，因此 left或totalLeft-left中的一个 刚好为 右半元素 的第一个元素的位置
         奇数个元素，定位到中间右侧，因此 left或totalLeft-left中的一个 刚好为 中位数右侧第一个数 的位置
        */
        while (left < right){
            int i = left + right + 1 >> 1;
            int j = totalLeft - i;
            if (nums1[i - 1] > nums2[j]) {
                right = i - 1;
            }
            else {
                left = i;
            }
        }
        int i = left, j = totalLeft - i;
        int leftMaxNums1 = (i == 0 ? INT_MIN : nums1[i - 1]);
        int leftMaxNums2 = (j == 0 ? INT_MIN : nums2[j - 1]);
        int rightMinNums1 = (i == m ? INT_MAX : nums1[i]);
        int rightMinNums2 = (j == n ? INT_MAX : nums2[j]);

        if ((m + n) % 2) {
            return (double)max(leftMaxNums1, leftMaxNums2);
        }
        else {
            return (double)(max(leftMaxNums1, leftMaxNums2) + min(rightMinNums1, rightMinNums2)) / 2.0;
        }

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

# 栈

## [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

### 解法1：栈

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char, char> maps;
        maps['('] = ')', maps['['] = ']', maps['{'] = '}'; //构建映射表
        stack<char> st; //栈
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(' || s[i] == '[' || s[i] == '{') { //遇见左括号，入栈
                st.push(s[i]);
            }
            else { //遇见右括号，查看栈顶左括号是否匹配
                if (st.empty() || (maps[st.top()] != s[i])) { //栈空，或栈顶左括号不匹配
                    return false;
                }
                else { //栈顶左括号匹配
                    st.pop();
                }
            }
        }
        if (!st.empty()) { //有左括号未匹配
            return false;
        }
        else {
            return true;
        }
    }
};
```

## [155. 最小栈](https://leetcode.cn/problems/min-stack/)

### 解法1：栈

$$
O(n)+O(n)
$$

```C++
class MinStack {
    stack<int> st,min_st; //分别作为正常的栈和存储最小元素的栈
public:
    MinStack() {
        while (!st.empty()) { //栈非空时将栈清空
            st.pop();
        }
        while (!min_st.empty()) { //最小栈非空时将最小栈清空
            min_st.pop();
        }
    }
    
    void push(int val) {
        st.push(val); //将元素压栈
        if (min_st.empty() || val <= min_st.top()) { //最小栈空或者当前元素比栈顶元素更小，则压入最小栈
            min_st.push(val);
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
    
    int getMin() {
        if (!min_st.empty()) { //访问栈内最小元素，即最小栈栈顶元素
            return min_st.top();
        }
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

## [394. 字符串解码](https://leetcode.cn/problems/decode-string/)

### 解法1：模拟

$$
5
$$

```C++
class Solution {
public:
    string decodeString(string s) {
        vector<string> temp(30); //存储中间字符串
        vector<int> num(30, 0); //记录重复的次数
        int tempNum = 0; //记录当前temp字符串的重复次数
        int index = 0; //记录当前temp和num的下标
        for (int i = 0; i < s.length(); i++) {
            if (s[i] >= '0' && s[i] <= '9') {
                tempNum = tempNum * 10 + s[i] - '0';
            }
            else if (s[i] == '[') {
                index++;
                num[index] = tempNum;
                tempNum = 0;
            }
            else if (s[i] >= 'a' && s[i] <= 'z') {
                temp[index] += s[i];
            }
            else {
                for (int i = 0; i < num[index]; i++) {
                    temp[index - 1] += temp[index];
                }
                temp[index] = "";
                num[index] = 0;
                index--;
            }
        }
        return temp[0];
    }
};
```

## [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)

### 解法1：单调栈

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        vector<int> result(temperatures.size()); //存储结果
        stack<int> st; //单调栈，存temperatures下标，栈内相应的temperature递减
        for (int i = 0; i < temperatures.size(); i++) {
            while (!st.empty() && temperatures[st.top()] < temperatures[i]) { //栈非空且栈顶元素对应的temperatures元素小于当前正在访问的temperatures元素，则将元素出栈
                result[st.top()] = i - st.top(); //更新出栈元素的result值，即下一个更高温度出现在几天后
                st.pop();
            }
            st.push(i);
        }
        return result;
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
        stack<int> st; //单调栈
        heights.insert(heights.begin(), 0); //在最前面插入0，保证算法可运行
        heights.push_back(0); //在最后面插入0，保证算法可运行
        int maxArea = 0;
        for (int i = 0; i < heights.size(); i++) {
            while (!st.empty() && heights[i] < heights[st.top()]) { //当前访问元素小于栈顶元素，逐一取出栈顶元素并求值
                int mid = heights[st.top()];
                st.pop();
                if (!st.empty()) {
                    maxArea = max(maxArea, mid * (i - st.top() - 1));
                }
            }
            st.push(i);
        }
        return maxArea;
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
        vector<int> left(heights.size(), 0), right(heights.size(), 0);
        int maxArea = 0;
        for (int i = 0; i < heights.size(); i++) {
            left[i] = i - 1;
            while (left[i] >= 0 && heights[left[i]] >= heights[i]) {
                left[i] = left[left[i]];
            }
        }
        for (int i = heights.size() - 1; i >= 0; i--) {
            right[i] = i + 1;
            while (right[i] < heights.size() && heights[right[i]] >= heights[i]) {
                right[i] = right[right[i]];
            }
        }
        for (int i = 0; i < heights.size(); i++) {
            maxArea = max(maxArea, heights[i] * (right[i] - left[i] - 1));
        }
        return maxArea;
    }
};
```

# 堆

## [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

### 解法1：快速查找

$$
O(n)+O(\log n)
$$

```C++
class Solution {
public:
    void quickSearch(vector<int>& nums,int l,int r,int k){
        if (l >= r) {
            return; //递归出口
        }
        int i = l - 1, j = r + 1,x = nums[l + r >> 1];
        while (i < j) { //快排核心
            while (nums[++i] < x);
            while (nums[--j] > x);
            if (i < j) {
                swap(nums[i],nums[j]);
            }
        }
        if (j >= nums.size() - k) { //第k个最大的元素在左区间
            quickSearch(nums, l, j, k);
        }
        else { //第k个最大的元素在右区间
            quickSearch(nums, j + 1, r, k);
        }
    }
    int findKthLargest(vector<int>& nums, int k) {
        quickSearch(nums, 0, nums.size() - 1, k);
        return nums[nums.size() - k];
    }
};
```

## [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

### 解法1：堆

$$
O(n\log k)+O(n)
$$

```C++
class Solution {
public:
    struct cmp { //比较结构体
        bool operator()(const pair<int, int>& pair1, const pair<int, int>& pair2) { //加入const和&
            return pair1.second < pair2.second; //大顶堆写法
        }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> hash; //哈希表记录每个元素出现的次数
        for (auto num : nums) {
            hash[num]++;
        }
        priority_queue<pair<int, int>, vector<pair<int, int>>, cmp> que; //实现大顶堆
        for (auto item : hash) {
            que.push(item);
        }
        vector<int> result(k); //取出出现频率前k高的元素
        for (int i = 0; i < k; i++) {
            result[i] = que.top().first;
            que.pop();
        }
        return result;
    }
};
```

## [295. 数据流的中位数](https://leetcode.cn/problems/find-median-from-data-stream/)

### 解法1：堆

$$
addNum:O(n)
$$

$$
findMedian:O(1)
$$

$$
空间复杂度:O(n)
$$

```C++
class MedianFinder {
public:
    priority_queue<int, vector<int>, less<int>> queMin; //大顶堆，记录小于等于中位数的数
    priority_queue<int, vector<int>, greater<int>> queMax; //小顶堆，记录大于中位数的数
    MedianFinder() {

    }
    
    void addNum(int num) {
        if (queMin.empty() || num <= queMin.top()) { //当Min堆空或num小于等于Min堆顶元素时，将num放入Min堆中
            queMin.push(num);
            while (queMin.size() > queMax.size() + 1) { //调整两个堆的元素数量
                queMax.push(queMin.top());
                queMin.pop();
            }
        }
        else { //当num大于Min堆顶元素时，将num放入Max堆中
            queMax.push(num);
            while (queMin.size() < queMax.size()) { //调整两个堆的元素数量
                queMin.push(queMax.top());
                queMax.pop();
            }
        }
    }
    
    double findMedian() {
        if (queMin.size() > queMax.size()) { //若Min堆元素多一个，则中位数为Min堆堆顶元素
            return queMin.top();
        }
        else { //若Min堆和Max堆元素数量相等，则中位数为Min堆堆顶元素和Max堆堆顶元素的平均数
            return (queMin.top() + queMax.top()) / 2.0;
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

# 贪心算法

## [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

### 解法1：动态规划

$$
O(n)+O(n)
$$

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        vector<vector<int>> dp(prices.size(), vector<int>(2,0)); //核心数组，也是解决股票系列问题的核心
        dp[0][0] = -prices[0], dp[0][1] = 0; //初始化，其中第一个数组表示到第i天为止买入股票收益的最大值，第二个数组表示到第i天为止卖出股票收益的最大值
        for (int i = 1; i < prices.size(); i++){ //核心递推公式
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
        int minPrice = prices[0], result=0;
        for (int i = 0; i < prices.size(); i++){
            result = max(result, prices[i] - minPrice);
            minPrice = min(minPrice, prices[i]);
        }
        return result;
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
        int k = 0;
        for (int i = 0; i < nums.size(); i++){
            if (k < i) {
                return false;
            }
            k = max(k, i + nums[i]);
        }
        return true;
    }
};
```

## [45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

### 解法1：贪心

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int jump(vector<int>& nums) {
        int k1 = 0, k2 = nums[0], count = 0;
        for (int i = 0; i < nums.size(); i++){
            if (i > k1){
                count++;
                k1 = k2;
            }
            k2 = max(k2, i + nums[i]);
        }
        return count;
    }
};
```

## [763. 划分字母区间](https://leetcode.cn/problems/partition-labels/)

### 解法1：合并区间

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    //cmp最前面的static不能省，两个比较的对象加上const和&
    static bool cmp(const vector<int>& u, const vector<int>& v){ //sort函数的比较函数cmp的写法
        if (u[0] != v[0]) { //左边界不同，则区间按左边界大小排
            return u[0] < v[0];
        }
        else { //左边界相同，则区间按右边界大小排
            return u[1] < v[1];
        }
    }
    vector<int> partitionLabels(string s) {
        vector<vector<int>> count(26); //记录每个字母最早出现的位置和最迟出现的位置
        for (int i = 0; i < 26; i++) {
            count[i].push_back(500); //用500填充，若最后左边界认为500.说明该字母未出现
            count[i].push_back(500);
        }
        for (int i = 0; i < s.length(); i++){
            if (count[s[i]-'a'][0] == 500) { //更新左边界（仅第一次遇见该字母更新）
                count[s[i]-'a'][0] = i;
            }
            count[s[i]-'a'][1] = i; //更新右边界
        }

        sort(count.begin(), count.end(), cmp); //将每个字母的区间排序
        int length = 0;
        for (int i = 0; i < 26 && count[i][0] != 500; i++) { //统计出现的字母个数，也是区间的个数
            length++;
        }

        vector<int> result; //记录结果
        int left = count[0][0], right = count[0][1]; //left和right分别记录当前合并区间的左边界和右边界
        for (int i = 1; i < length; i++) {
            if (count[i][0] > right) { //此时要访问的区间的左边界已经大于合并区间的右边界，则当前合并区间可以加入result中，并更新左边界和右边界
                result.push_back(right - left + 1);
                left = count[i][0];
                right = count[i][1];
            }
            else { //否则，合并区间可以继续合并要访问的区间，则只需要更新右边界
                right = max(right, count[i][1]);
            }
        }
        result.push_back(right - left + 1); //循环结束后，再把最后一个区间加入result中
        return result;
    }
};
```

# 动态规划

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
        dp[0] = 1, dp[1] = 1; //初始化前两项
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 2] + dp[i - 1]; //第i级台阶可由第i-2级台阶走2步，或第i-1级台阶走1步走上
        }
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
        int pre = 1, now = 1;
        while (--n) {
            int temp = now;
            now = now + pre;
            pre = temp;
        }
        return now;
    }
};
```

## [118. 杨辉三角](https://leetcode.cn/problems/pascals-triangle/)

### 解法1：模拟

$$
O(n^2)+O(1)
$$

```C++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> result(numRows);
        for (int i = 0; i < numRows; i++) {
            result[i].resize(i + 1); //resize函数了解下
            result[i][0] = result[i][i] = 1;
            for (int j = 1; j < i; j++) {
                result[i][j] = result[i - 1][j - 1] + result[i - 1][j];
            }
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
        if (nums.size() == 1) { //只有一家可以偷
            return nums[0];
        }
        vector<int> dp(nums.size(),0);
        dp[0]=nums[0], dp[1] = max(nums[0],nums[1]); //初始化前两项
        for (int i = 2; i < nums.size(); i++) {
            /*
            过第i家能偷到的金额最大值为:
            1、如果偷第i家，则最大金额为过第i-2家获得的最大金额+偷第i家获得的金额
            2、如果不偷第i家，则最大金额为过第i-1家获得最大金额
            以上两者的较大值
             */
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }
        return dp[nums.size() - 1];
    }
};
```

## [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

### 解法1：动态规划

$$
O(n\sqrt n)+O(n)
$$

```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> num(101, 0);
        for (int i = 0; i <= 100; i++) {
            num[i] = i * i;
        }
        vector<int> dp(n + 1, INT_MAX / 2);
        dp[0] = 0;
        for (int i = 0; i <= 100; i++) {
            for (int j = num[i]; j <= n; j++) {
                dp[j] = min(dp[j], dp[j - num[i]] + 1);
            }
        }
        return dp[n];
    }
};
```

## [322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

### 解法1：动态规划

$$
时间复杂度，O(Sn)，其中 S 是金额，n 是面额数
$$

$$
空间复杂度：O(S)
$$

```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, INT_MAX / 2);
        dp[0] = 0;
        //使用背包模型
        for (int i = 0; i < coins.size(); i++) { //行：第i种硬币
            for (int j = coins[i]; j <= amount; j++) { //列：总金额（硬币数量不限，因此从coins[i]开始递增，否则从amount开始递减）
                dp[j] = min(dp[j], dp[j - coins[i]] + 1);
            }
        }
        if (dp[amount] >= INT_MAX / 2) {
            return -1;
        }
        else {
            return dp[amount];
        }
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
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end()); //直接将字符串列表填充到集合里
        vector<bool> dp(s.length() + 1, false); //dp数组，表示以第i个字符结尾的s的子串能否用字符串列表中的字符串表示出来
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < wordDict.size(); j++) {
                if (i >= wordDict[j].length()) {
                    string str = s.substr(i - wordDict[j].length(), wordDict[j].length());
                    if (dp[i - wordDict[j].length()] == true && wordSet.find(str) != wordSet.end()) {
                        dp[i]=true;
                    }
                }
            }
        }
        return dp[s.length()];
    }
};
```

## [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

### 解法1：动态规划

$$
O(n^2)+O(n)
$$

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp(nums.size(), 1);
        int MAX = 1;
        for (int i = 0; i < nums.size(); i++){
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            MAX = max(MAX, dp[i]);
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
        vector<int> d; //数组d[i]表示长度为i+1的最长上升子序列的末尾元素的最小值
        for (int i = 0; i < nums.size(); i++){
            if (i == 0 || d[d.size() - 1] < nums[i]) { //当前的nums[i]比d数组中的最大数还大时，扩充d数组
                d.push_back(nums[i]);
            }
            else { //否则，使用二分查找在d数组中找到第一个大于等于nums[i]的数，并将其替换成nums[i]
                int l = 0, r = d.size() - 1;
                while (l < r) {
                    int mid = l + r >> 1;
                    if (d[mid] >= nums[i]) {
                        r = mid;
                    }
                    else {
                        l = mid + 1;
                    }
                }
                d[l] = nums[i];
            }
        }
        return d.size();
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
        vector<int> minDp(nums.size()), maxDp(nums.size()); //分别记录当前位置的最小乘积和最大乘积，其中最小乘积可用于解决存在负数的问题
        minDp[0] = maxDp[0] = nums[0];
        int result = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            //最小乘积和最大乘积都从这三个数中选：nums[i]，minDp[i - 1] * nums[i]和maxDp[i - 1] * nums[i]，其中如果minDp[i - 1]和nums[i]均为负数，则相乘可能为最大乘积
            minDp[i] = min(nums[i], min(minDp[i - 1] * nums[i], maxDp[i - 1] * nums[i]));
            maxDp[i] = max(nums[i], max(minDp[i - 1] * nums[i], maxDp[i - 1] * nums[i]));
            result = max(result, maxDp[i]);
        }
        return result;
    }
};
```

### ==笔记==

是不是连续子序列，动态规划数组的意义都是“取以当前位置为结尾得到的最优结果”？

## [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

### 解法1：动态规划

$$
时间复杂度，O(Sn)，其中 S 是总和，n 是nums数组的元素数量。
$$

$$
空间复杂度：O(S)
$$

```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0; //记录总和
        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i];
        }
        if (sum % 2) { //和为奇数，则肯定无法分成两个子集
            return false;
        }
        vector<int> dp(sum / 2 + 1, 0);
        //使用背包模型
        for (int i = 0; i < nums.size(); i++) { //行：第i个nums数
            for (int j = sum / 2; j >= nums[i]; j--) { //列：总和（单个nums数只能用一次，因此从sum/2开始递减，否则从nums[i]开始递增）
                dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);
            }
        }
        if (dp[sum / 2] == sum / 2) {
            return true;
        }
        else {
            return false;
        }
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
        vector<int> dp(s.length(), 0); //dp数组表示以当前位结尾的最长子串的长度
        int maxLength = 0; //记录最长子串的长度
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(') {
                dp[i] = 0;
            }
            else {
                if (i > 0 && i - dp[i - 1] -1 >= 0){
                    if (s[i - dp[i - 1] -1] == '('){
                        //举个例子：(()())，此时dp[i-1]为4，s[i-dp[i-1]-1]为左括号，则此时dp[i]刚好用一个左括号和右括号把dp[i-1]包裹起来，则dp[i]比dp[i-1]大2
                        dp[i] = dp[i - 1] + 2;
                        //举个例子：()(()())，上一步完成后发现前面还有一对括号，则合法子串又可扩充长度，即加上dp[i - dp[i]]
                        if (i - dp[i] >= 0) {
                            dp[i] += dp[i - dp[i]];
                        }
                    }
                }
            }
            maxLength = max(maxLength, dp[i]); //更新最长子串的长度
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
        int maxLength = 0; //记录最长子串长度
        stack<int> st; //栈，记录入栈括号的下标
        st.push(-1); //将-1入栈避免算法出错
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(') { //左括号直接入栈
                st.push(i);
            }
            else {
                st.pop();
                if (st.empty()) { //右括号下标入栈是为了表示上一子串的最右端
                    st.push(i);
                }
                else { //更新最长子串的长度
                    maxLength = max(maxLength, i - st.top());
                }
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
        int left = 0, right = 0, maxLength = 0; //left和right分别记录扫描到当前位时的左括号数目和右括号数目，maxLength记录最长子串的长度
        for (int i = 0; i < s.length(); i++) { //从左到右扫描，但无法对“左括号数永远大于右括号数”这种情况得到正确答案
            if (s[i] == '(') {
                left++;
            }
            else {
                right++;
            }
            if (left == right) { //当左右括号数量相同时，可以更新最长子串的长度
                maxLength = max(maxLength, 2 * left);
            }
            else if (left < right) { //当右括号更多时，重新开始求子串长度
                left = right = 0;
            }
        }
        left = right = 0;
        for (int i = s.length() - 1; i >= 0; i--) { //从右到左扫描，但无法对“右括号数永远大于左括号数”这种情况得到正确答案
            if (s[i] == '(') {
                left++;
            }
            else {
                right++;
            }
            if (left == right) {
                maxLength = max(maxLength, 2 * left);
            }
            else if (left > right) {
                left = right = 0;
            }
        }
        return maxLength;
    }
};
```

# 多维动态规划

## [62. 不同路径](https://leetcode.cn/problems/unique-paths/)

### 解法1：动态规划

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        dp[1][0] = 1;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
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
        vector<int> dp(n + 1);
        dp[1] = 1;
        for (int i = 1; i <= m; i++) {
            for (int j = 2; j <= n; j++) {
                dp[j] = dp[j] + dp[j - 1];
            }
        }
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
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i && j) {
                    dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
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
        int m = grid.size(), n = grid[0].size();
        vector<int> dp(n, 0);
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                if (i && j) {
                    dp[j] = min(dp[j], dp[j - 1]) + grid[i][j];
                }
                else if (j) {
                    dp[j] = dp[j - 1] + grid[i][j];
                }
                else {
                    dp[j] += grid[i][j];
                }
            }
        }
        return dp[n - 1];
    }
};
```

## [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

### 解法1：动态规划

$$
O(n^2)+O(n^2)
$$

```C++
class Solution {
public:
    string longestPalindrome(string s) {
        vector<vector<bool>> dp(s.length(), vector<bool>(s.length(), false));
        int maxLength = 0;
        string result;
        for (int i = s.length() - 1; i >= 0; i--) { //注意遍历顺序
            for (int j = i; j < s.length(); j++) {
                if (j - i <= 1) {
                    dp[i][j] = (s[i] == s[j]);
                }
                else if (s[i] == s[j]) {
                    dp[i][j] = dp[i + 1][j - 1];
                }
                else dp[i][j] = false;
                if (dp[i][j] && maxLength < j - i + 1){
                    maxLength = j - i + 1;
                    result = s.substr(i, j - i + 1);
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
        int maxLength = 0;
        string result;
        for (int i = 0; i < s.length(); i++) {
            int left = i, right = i;
            while (true) {
                if (left == 0 || right == s.length() - 1 || s[left - 1] != s[right + 1]) {
                    break;
                }
                else {
                    left--;
                    right++;
                }
            }
            if (right - left + 1 > maxLength) {
                maxLength = right - left + 1;
                result = s.substr(left, right - left + 1);
            }
            left = i, right = i;
            if (i + 1 < s.length() && s[i] == s[i + 1]) {
                right++;
                while (true) {
                    if (left == 0 || right == s.length() - 1 || s[left - 1] != s[right + 1]) {
                        break;
                    }
                    else {
                        left--;
                        right++;
                    }
                }
                if (right - left + 1 > maxLength) {
                    maxLength = right - left + 1;
                    result = s.substr(left, right - left + 1);
                }
            }
        }
        return result;
    }
};
```

## [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

### 解法1：动态规划

$$
O(mn)+O(mn)
$$

```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size(), vector<int>(text2.size(), 0));
        for (int i = 0; i < text1.length(); i++) {
            if (text1[i] == text2[0]) {
                dp[i][0] = 1;
            }
            else if (i) {
                dp[i][0] = dp[i - 1][0];
            }
        }
        for (int i = 0; i < text2.length(); i++) {
            if (text1[0] == text2[i]) {
                dp[0][i] = 1;
            }
            else if (i) {
                dp[0][i] = dp[0][i-1];
            }
        }
        for (int i = 1; i < text1.length(); i++) {
            for (int j = 1; j < text2.length(); j++) {
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                if (text1[i] == text2[j]) {
                    dp[i][j] = max(dp[i][j],dp[i - 1][j - 1] + 1);
                }
            }
        }
        return dp[text1.length()-1][text2.length()-1];
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
        vector<vector<int>> dp(word1.length() + 1, vector<int>(word2.length() + 1, 0));
        for (int i = 0; i <= word2.length(); i++) {
            dp[0][i] = i;
        }
        for (int i = 0; i <= word1.length(); i++) {
            dp[i][0] = i;
        }
        for (int i = 1; i <= word1.length(); i++) {
            for (int j = 1; j <= word2.length(); j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else {
                    dp[i][j] = min(min(dp[i - 1][j - 1], dp[i - 1][j]), dp[i][j - 1]) + 1;
                }
            }
        }
        return dp[word1.length()][word2.length()];
    }
};
```

# 技巧

## [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

### 解法1：异或

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result = 0;
        for (int i = 0; i < nums.size(); i++) {
            result ^= nums[i];
        }
        return result;
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
        int result = nums[0], count = 1;
        for (int i = 1; i < nums.size(); i++) {
            if (count == 0) { //count归零，则更新选中的nums[i]元素并重新开始计数
                result = nums[i];
            }
            if (result == nums[i]) { //相同，计数加1
                count++;
            }
            else { //不同，计数减1
                count--;
            }
        }
        return result;
    }
};
```

## [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

### 解法1：荷兰国旗问题解法

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int i = 0, j = 0, k = nums.size() - 1;
        while (j <= k) {
            switch(nums[j]) {
                case 0:
                    swap(nums[i], nums[j]), i++, j++;
                    break;
                case 1:
                    j++;
                    break;
                case 2:
                    swap(nums[j], nums[k]), k--;
                    break;
            }
        }
    }
};
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
        int i = nums.size() - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) { //从后向前查找第一个 相邻升序 的元素对
            i--;
        }
        if (i >= 0){ 
            int j = nums.size() - 1;
            while (j >= 0 && nums[i] >= nums[j]) { //下标i后面的元素必然是递减的，从后往前寻找第一个大于i对应元素的元素
                j--;
            }
            swap(nums[i], nums[j]); //将二者交换，即用尽可能小的数与下标i对应的元素交换
        }
        reverse(nums.begin() + i + 1, nums.end()); //上一步完成后i后面仍未逆序，逆置即可
    }
};
```

## [287. 寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)

### 解法1：Flyod判圈法

$$
O(n)+O(1)
$$

```C++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        //Floyd判圈法
        int slow = 0, fast = 0;
        do {
            slow = nums[slow]; //慢指针一次走一步
            fast = nums[nums[fast]]; //快指针一次走两步
        } while (slow != fast);
        slow = 0; //慢指针回到出发点
        while (slow != fast) { //当快慢指针相遇时，即为答案
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
};
```

# 其他

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





