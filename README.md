# :cat2: Clearest-LeetCode-Cpp-Solutions
Clearest LeetCode C++ Solutions. This project is intended to clarify the problem solving ideas. 我们追求的目标：清晰简单的思路 + 畅快巧妙的代码

# 前言
- 本项目意在收集 leetcode 中各大题型最清晰的解题思路 😉，帮助短时间内捋顺 C++ 编程知识体系、掌握常规通用、简单有效的解题方法，理解并记忆一些避免代码冗余的黑科技模块。
- 项目持续更新中，优先使用 C++，不支持的题目使用 C 代替，如果您有希望分享的清晰解法欢迎联系更新~ [直接发issue 或 fork，记得留下署名和联系方式 :bear:] 鉴于追求的主题，本项目收录题解需满足：1.思路简单清晰，容易理解 2.代码轻巧，不冗余 3.执行效率较高，时间复杂度低
- 水平有限，若您发现已存在的代码中如有冗余部分，欢迎 issue 或 PR。
- 另外这里有一份[ 🐍 Python 最短题解](https://github.com/cy69855522/Shortest-LeetCode-Python-Solutions)，带您体验 python 中各种让人叹为观止的奇巧解法，如果您对俩门语言都感兴趣的话，同时服用效果更佳。
- 欢迎加入QQ交流群：902025048

    ![](QR.png)


# 专题探索
![](思维导图.jpg)

以上是一张互联网公司面试中经常考察的问题类型总结的思维导图，此栏目将根据 LeetCode 中文版探索板块给出的路线制作题解，目标追求思路的常规性，代码的高可读性，各专栏将尽力覆盖各大知识要点。
## 数据结构，说难也不难
### [队列 & 栈](https://leetcode-cn.com/explore/learn/card/queue-stack/)

**队列：先入先出的数据结构**
#### [622. 设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)
```cpp
此处为代码
```
- 此处为解析

**队列和广度优先搜索**
#### [200. 岛屿的个数](https://leetcode-cn.com/problems/number-of-islands/)
```cpp
此处为代码
```
- 此处为解析


# 题库解析
默认已经看过题目 🤡 点击标题可跳转对应题目网址。
## 数组
### [238. Product of Array Except Self 双指针](https://leetcode.com/problems/product-of-array-except-self/)
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n, 1);
        
        int l = 1;
        for(int i=0; i<n; ++i){
            res[i] *= l;
            l *= nums[i];
        }
        
        int r = 1;
        for(int j=n-1; j>=0; --j){
            res[j] *= r;
            r *= nums[j];
        }
        
        return res;
    }
};
```
- 本题利用双指针，新数组每个位置上的值应该等于数组左边所有数字的乘积 × 数组右边所有数字的乘积
- 1.初始化一个新的数组res（result），包含n个1

  2.初始化变量l（left）代表左边的乘积，从左到右遍历数组，每次都让新数组的值乘以它左边数字的乘积l，然后更新l。此时新数组里的所有数字就代表了nums数组中对应位置左边所有数字的乘积
  
  3.再从右往左做一遍同样的操作，最终`res[i] = 1 * nums中i左边所有数字的乘积 * nums中i右边所有数字的乘积`
### [448. Find All Numbers Disappeared in an Array 伪哈希](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        for(int i=0; i<nums.size(); ++i){
            nums[abs(nums[i])-1] = -abs(nums[abs(nums[i])-1]);
        }
        
        vector<int> r;
        for(int i=0; i<nums.size(); ++i){
            if(nums[i] > 0){
                r.push_back(i+1);
            }
        }
        return r;
    }
};
```
- 应题目进阶要求，O(N)时间，无额外空间，此解实际上是利用索引把数组自身当作哈希表处理
- 将 nums 中所有正数作为索引i，置 nums[i] 为负值。那么，仍为正数的位置即为（未出现过）消失的数字
    - 原始数组：[4,3,2,7,8,2,3,1]
    - 重置后为：[-4,-3,-2,-7,`8`,`2`,-3,-1]
    - 结论：[8,2] 分别对应的index为[5,6]（消失的数字）
## 哈希表
### [36. Valid Sudoku 哈希表]()
```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        int n = board.size();
        vector<unordered_map<char, int>> row(n), col(n);
        vector<vector<unordered_map<char, int>>> sub(n/3, vector<unordered_map<char, int>>(n/3));
        
        for(int i=0; i<n; ++i){
            for(int j=0; j<n; ++j){
                char c = board[i][j];
                if(c == '.') continue;
                if(row[i][c]++ > 0 || col[j][c]++ > 0 || sub[i/3][j/3][c]++ > 0) return false;
            }
        }
        return true;
    }
};
```
- O(N)时间复杂度，为每个分区建立一张映射表，每遍历一个元素就在所属的所有对应分区中记录该值，若发现值已经被更改则证明数独表无效
## 链表
### [206. Reverse Linked List 前后指针](https://leetcode.com/problems/reverse-linked-list/)
```cpp
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
        ListNode *pre = NULL;
        while(head){
            ListNode *next = head -> next;
            head -> next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
};
```
## 数学
### [268. Missing Number 等差数列](https://leetcode.com/problems/missing-number/)
```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        
        int sum = 0;
        for(int i=0; i<n; ++i){
            sum += nums[i];
        }
        
        n++;
        return n * (n - 1) / 2 - sum;
    }
};
```
- 缺失数字 = 0 加到 n+1 的总和 - 数组中所有数字的总和
- 计算 0 加到 n+1 的总和，可利用等差数列求和公式，此题可理解为`总和 = (元素个数 / 2) * (首尾两数字之和)`

## 双指针
### [344. Reverse String 双向指针](https://leetcode.com/problems/reverse-string/)
```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i = -1, j = s.size();
        while(++i < --j){
            swap(s[i], s[j]);
        }
    }
};
```
## 字符串
### [13. Roman to Integer 哈希表](https://leetcode.com/problems/roman-to-integer/)
```cpp
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<string, int> m = {{"I", 1}, {"IV", 3}, {"IX", 8}, {"V", 5}, {"X", 10}, {"XL", 30}, {"XC", 80}, {"L", 50}, {"C", 100}, {"CD", 300}, {"CM", 800}, {"D", 500}, {"M", 1000}};
        
        int r = m[s.substr(0, 1)];
        for(int i=1; i<s.size(); ++i){
            string two = s.substr(i-1, 2);
            string one = s.substr(i, 1);
            r += m[two] ? m[two] : m[one];
        }
        return r;
    }
};
```
- 构建一个哈希表记录所有罗马数字子串，注意长度为2的子串记录的值是（实际值-子串内左边罗马数字代表的数值）
- 遍历整个s的时候判断当前位置和前一个位置的两个字符组成的字符串是否在字典内，如果在就记录值，不在就说明当前位置不存在小数字在前面的情况，直接记录当前位置字符对应值。整个过程时间复杂度为O(N)
- 举个例子，遍历经过IV的时候先记录I的对应值1再往前移动一步记录IV的值3，加起来正好是IV的真实值4

## 二分查找
- :paperclip:【知识卡片】二分查找利用已排好序的数组，每一次查找都可以将查找范围减半。查找范围内只剩一个数据时查找结束。数据量为 n 的数组，将其长度减半 log2n 次后，其中便只剩一个数据了。也就是说，在二分查找中重复执行“将目标数据和数组中间的数据进行比较后将查找范围减半”的操作 log2n 次后，就能找到目标数据（若没找到则可以得出数据不存在的结论），因此它的时间复杂度为 O(logn)
### [162. Find Peak Element 二分查找](https://leetcode.com/problems/find-peak-element/)
```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 0;
        int r = nums.size() - 1;
        
        while(l < r){
            int m = (l + r) / 2;
            
            if(m > 0 and nums[m-1] > nums[m]){
                r = m - 1;
            }else if(m < nums.size() and nums[m+1] > nums[m]){
                l = m + 1;
            }else{
                return m;
            }
        }
        return l;
    }
};
```
- 初始化搜索范围为[0, len(nums)-1]，初始搜索位置为中间位置 m，如果 m 左边存在值比 nums[m] 大，说明[0, m-1]一定存在峰值，我们缩小搜索范围；否则如果 m 右边存在值比 nums[m] 大，说明[m+1, len(nums)-1]一定存在峰值，我们缩小范围；否则 m 就是峰值
## 分治算法
### [973. K Closest Points to Origin 快速选择](https://leetcode.com/problems/k-closest-points-to-origin/)
```cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        int p = pow(points[0][0], 2) + pow(points[0][1], 2);
        vector<vector<int>> l, m, r;
        for(int i=0; i<points.size(); ++i){
            int v = pow(points[i][0], 2) + pow(points[i][1], 2);
            if(v < p){
                l.push_back(points[i]);
            }else if(v == p){
                m.push_back(points[i]);
            }else{
                r.push_back(points[i]);
            }
        }
        
        if(K <= l.size()){
            return kClosest(l, K);
        }else if(K <= l.size() + m.size()){
            l.insert(l.end(), m.begin(), m.begin() + K - l.size());
            return l;
        }else{
            r = kClosest(r, K - l.size() - m.size());
            l.insert(l.end(), m.begin(), m.end());
            l.insert(l.end(), r.begin(), r.end());
            return l;
        }
    }
};
```
- 快速选择的一般流程，计算lmr，组合
## 位运算
### [461. Hamming Distance 异或](https://leetcode.com/problems/hamming-distance/submissions/)
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        return bitset<32>(x ^ y).count();
    }
};
```
# 解法汇总贡献者
注：此处贡献名单仅代表汇总搜集贡献，不代表全部原创，欢迎所有更短的解法🤓
- [Knife丶](https://github.com/cy69855522)[QQ1272068154  微信ly18597591102]
