# :cat2: Clearest-LeetCode-Cpp-Solutions
Clearest LeetCode C++ Solutions. This project is intended to clarify the problem solving ideas. 我们追求的目标：清晰简单的思路 + 畅快巧妙的代码

# 前言
- 本项目意在收集 leetcode 中各大题型最清晰的解题思路 😉，帮助短时间内捋顺 C++ 编程知识体系、掌握常规通用、简单有效的解题方法，理解并记忆一些避免代码冗余的黑科技模块。
- 项目持续更新中，优先使用 C++，不支持的题目使用 C 代替，如果您有希望分享的清晰解法欢迎联系更新~ [直接发issue 或 fork，记得留下署名和联系方式 :bear:] 鉴于追求的主题，本项目收录题解需满足：1.思路简单清晰，容易理解 2.代码轻巧，不冗余 3.执行效率较高，时间复杂度低
- 水平有限，若您发现已存在的代码中如有冗余部分，欢迎 issue 或 PR。
- 另外这里有一份[ 🐍 Python 最短题解](https://github.com/cy69855522/Shortest-LeetCode-Python-Solutions)，带您体验 python 中各种让人叹为观止的奇巧解法，如果您对俩门语言都感兴趣的话，同时服用效果更佳。
- 欢迎加入QQ交流群：902025048 [∷二维码](QR.png) 群内提供更多相关资料~
# 专题探索
![](思维导图.jpg)

以上是一张互联网公司面试中经常考察的问题类型总结的思维导图，此栏目将根据 LeetCode 中文版探索板块给出的路线制作题解，各专栏将尽力覆盖各大知识要点并总结知识点和套路。相比于[题库解析](#题库解析)部分追求代码的绝对精简，本专题追求以**高可读性**呈现各大专题的**常规思路**，为后续的题库解析部分做铺垫。俩部分题目可能重复，但专题部分会有更详细的解析，且可能运用不同解法。

## 数据结构，说难也不难
### [队列 & 栈](https://leetcode-cn.com/explore/learn/card/queue-stack/)
- :black_joker:【知识卡片】**队列**中的数据呈线性排列，就和“队列”这个名字一样，把它想象成排成一 队的人更容易理解。在队列中，处理总是从第一名开始往后进行，而新来的人只能排在队尾。像队列这种最先进去的数据最先被取来，即“先进先出”的结构，我们称为 First In First Out，简称 FIFO
- :black_joker:【知识卡片】**栈**也是一种数据呈线性排列的数据结构，不过在这种结构中，我们只能访问最新添加的数 据。栈就像是一摞书，拿到新书时我们会把它放在书堆的最上面，取书时也只能从最上面的新书开始取。Last In First Out，简称 LIFO
- :black_joker:【知识卡片】**广度优先搜索 BFS **是一种对图进行搜索的算法。假设我们一开始位于某个顶点（即起点），此 时并不知道图的整体结构，而我们的目的是从起点开始顺着边搜索，直到到达指定顶点（即终 点）。在此过程中每走到一个顶点，就会判断一次它是否为终点。广度优先搜索会优先从离起点近的顶点开始搜索，这样由近及广的搜索方式也使得。根据 BFS 的特性，其常常被用于 `遍历` 和 `搜索最短路径`
- :tophat:【套路】**广度优先搜索一般流程**
	```
    # 1.初始化队列
    # 2.选择合适的根节点压入队列

    # 3.使用 while 进入队列循环，直到搜索完毕
    # {
    #   4.取出一个节点
    #   5.放入这个节点周围的节点
    # }
	```

**队列：先入先出的数据结构**
#### [622. 设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)
```cpp
代码
```
- 解析

**队列和广度优先搜索**
#### [200. 岛屿的个数](https://leetcode-cn.com/problems/number-of-islands/)
```cpp
代码
```
- 解析

#### [752. 打开转盘锁  队列+广度优先搜索](https://leetcode-cn.com/problems/open-the-lock/submissions/)
```cpp
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        set<string> dead(deadends.begin(),deadends.end()); //收录死锁的数据
        set<string> done; //用来记录已经遍历过的数字
        string beginstr = "0000"; //记录起始数据
        done.insert(beginstr);
        
        // BFS 从这里开始--------------------------------------------------------------
        queue<pair<int,string>>record; //创建队列记录当前深度和节点
        record.push({0,beginstr});
        
        if (dead.count(beginstr)) return -1; //如果死锁数据中包含起始数据，直接返回-1
        
        while(!record.empty()) //用BSF遍历所有可能的数据
        {
            auto tmp=record.front(); //提取队列中第一个数据
            record.pop();
            
            if (tmp.second==target) return tmp.first; //将目标的判断放在加减操作之前，还可以判断起始数据是否是目标数据，较严谨
            for (int i=0; i<4; i++) //对提取出来的数据的每一位做+1和-1的操作
            {
                string spls = tmp.second;
                string ssbs = tmp.second;
                spls[i] = (spls[i] - '0' + 1) % 10 +'0'; //用mod来处理循环的数字
                ssbs[i] = ((ssbs[i] - '0') + 9) % 10 + '0';
                if (!dead.count(spls) && !done.count(spls)) //若加一后的数据不存在死锁集合中，且不是之前遍历过的数据
                {
                    record.push({tmp.first+1, spls}); //加到record队列中
                    done.insert(spls);
                }
                if (!dead.count(ssbs) && !done.count(ssbs)) //若减一后的数据不存在死锁集合中，且，不是之前遍历过的数据
                {
                    record.push({tmp.first+1, ssbs}); // 加到record队列中
                    done.insert(ssbs);
                }
            }
        }
        return -1; //若无法得到目标密码，返回-1
    }
};

```
- 使用BFS解题的关键：1.根节点是什么？周围节点是哪些？什么时候停止？
- 本题通过使用BFS，将问题转化为一个图，每一个点都与其它八个点相连接e.g. 0000 与0001，0009，0010，0090，0100，0900，1000，9000八个点相连，通过对每一个点的每一位数进行+1和-1的操作获得新的点，判断新的点是否在死锁集合内
- dead集合的作用有两点：①题目给的deadends变量是数组类型的，每次判断某个目标是否在其中需要遍历一边数组，时间复杂度O(N),而dead变量是set类型的，内部实现是哈希表，每次根据key取value值的时间复杂度是O(1),快非常多。  ②因为题目给出的deadends中有很多重复的内容，所以判断的时候重复的值也搜索一遍更慢了，使用set可以把所有重复的值去掉。
- 若不设置集合储存遍历过的点，会产生无限循环。
- 本题使用mod和ASCII码来处理9->0和0->9的转换；也可设置两个flag1，flag2，当flag==‘9’+1或‘0’-1时，对答案进行重新赋值。
- 可以通过first和second的调用来分别访问队列对中的数据； continue仅跳出一层循环一次。
#### [279. 完全平方数  队列+广度优先搜索](https://leetcode.com/problems/perfect-squares/)
```cpp
class Solution {
public:
    int numSquares(int n) {
        vector <int> f (n+1,-1); //初始化 构造一个数组，容量为n+1，所有空间初始化为-1
        f[0] = 0; //组成0的完全平方数的个数为0
        
        //BFS从这里开始-------------------------
        queue <int> q; 构造一个队列储存nodes，等待处理
        q.push(0);
        while (!q.empty())
        {
            int m = q.front(); //取出第一个数据
            q.pop();
            for(int i = 1; i*i+m <=n; i++) //最小的完全平方数是1
            {
                if(f[i*i+m] == -1) //若i*i+m这个数字还没有出现过
                {
                    f[i*i+m] = f[m] + 1; //加一次完全平方数，步骤加一
                    q.push(i*i+m); //放入队列，接着搜索
                }
            }
        }
        return f[n];
    }
};
```
- 本题使用队列+广度优先搜索来解决， 思维过程：以n=5为例，通过构建树形图形，root是0，通过第一次for循环，构建第一层node，此时m为0。如图
- ![](pics/完全平方数.png)
- 因为队列FIFO的性质，先达到目标的必是最少的个数

**栈：后入先出的数据结构**
#### [155. Min Stack 栈](https://leetcode.com/problems/min-stack/)
```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int>s; //主数据栈
    stack<int>minn; //辅助数据栈
    MinStack() {
        
    }
    
    void push(int x) {
        s.push(x);
        if(minn.empty()||x<=minn.top()) //关键点，见总结
            minn.push(x);
    }
    
    void pop() {
        if(s.top() == minn.top()) //判断数据栈的顶部是否是当前主数据栈的最小值，若是，需要将两个栈的top都pop掉
            minn.pop();
        s.pop();
    }
    
    int top() {
        return s.top(); //返回栈的顶部
    }
    
    int getMin() {
        return minn.top(); //返回最小值
    }
};
```
- 本题使用双栈，利用栈的后入先出的特性，将最小值推入minn栈，使minn栈的top始终为s栈的最小值。
- 关键点：判断条件为当minn栈为空，或当前数据小于等于minn栈的top值。条件若为true，则将当前数据也push到minn栈中。若判断条件改为‘小于’minn栈的top值，而不是‘小于等于’minn栈的top值时，代码提交失败。问题在于，测试数据可能有大于1个的最小值，若s栈中的最小值个数多于1个，当minn栈和s栈的最小值被pop一个后，s栈中还有之前的最小值，而minn栈中的最小值变得比s栈中的最小值大，答案error。
#### [20. Valid parentheses 栈](https://leetcode.com/problems/valid-parentheses/)
```cpp
class Solution {
public:
    bool isValid(string s) {
        if(s.size() % 2 != 0) return false; //若字符串s的字符数不为偶数，直接返回false.  不过没有也可
        stack <char> a; 
        map <char,char> bra; //建立右括号和左括号之间的映射
        bra.insert(pair<char,char>(')','('));
        bra.insert(pair<char,char>(']','['));
        bra.insert(pair<char,char>('}','{'));
        int lo;
        lo = s.size();
        for(int i = 0; i < lo; i++)
        {
            if(s[i] == '(' || s[i] == '[' || s[i] == '{') //若是左括号，直接推入栈
                a.push(s[i]);
            if(s[i] == ')' || s[i] == ']' || s[i] == '}') //若是右括号，先判断栈内是否有左括号
            {
                if(a.empty()) //见总结*
                    return false;
                if(bra[s[i]] == a.top()) //若右括号与栈顶的左括号匹配，pop掉栈a最上方的左括号，否则返回false
                {
                    a.pop();
                }
                else
                    return false;
            }
        }
        if(a.empty()) //若操作之后，栈内还有字符，返回false
            return true;
        else
            return false;
    }
};
```
- 有效的括号，通过建立左右括号之间的映射。使用栈后入先出的性质，压入左括号，若右括号和栈中顶部的左括号匹配，且最终栈内没有字符，返回true
- 当操作右括号前，要判断栈是否为空，若缺少这一步，当栈内为空，右括号需要与空栈的top判断段是否相等的时候，会报错。
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
## 队列
### [933. Number of Recent Calls 队列](https://leetcode-cn.com/problems/number-of-recent-calls/)
```cpp
class RecentCounter {
    queue<int> tco;
public:
    RecentCounter() { 
    }
    int ping(int t) {
        while(!tco.empty() && tco.front() <t- 3000)
        {
            tco.pop();
        }
        tco.push(t);
        return tco.size();
    }
};
```
- 因为t的值会逐渐增大，只需要判断队列首位是否小于t-3000.
- 返回队列中剩余元素的个数
## 深度优先搜索（DFS）
### [200. Number of Islands 深度优先搜索算法](https://leetcode.com/problems/number-of-islands/)
```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>&grid) {//本题地图上的'0','1'被当作字符录入
        if (grid.empty()) return 0;
        int m=grid.size();
        int n=grid[0].size();  //以上两行求得地图的边界
        int ans=0;  //答案初始化为零
        for (int y=0;y<m;y++)
            for (int x = 0; x < n; x++)
            {
                ans=grid[y][x]-'0'+ans;   //'0','1'的ASCII码值相差一，详情看代码后解析
                dfs(grid,x,y,m,n); //深度优先搜索算法
            }
        return ans;
    }
private:
    void dfs(vector<vector<char>>&grid,int x,int y,int m,int n)
    {
        if(x<0||y<0||y>=m||x>=n||grid[y][x]=='0')  //遍历的点不能超出地图边界
            return;
        grid[y][x]='0';  //将岛的一部分'1',转换为0
        dfs(grid,x+1,y,m,n);  //遍历该点的上下左右
        dfs(grid,x-1,y,m,n);
        dfs(grid,x,y+1,m,n);
        dfs(grid,x,y-1,m,n);
            
        
    }
};
```
- 本题用深度搜索算法来解答，当遭遇一座岛‘1’时，我们通过遍历这座岛每个部分（其它与这个点横竖相连的1）并将它们同化为‘0’，以防止这座岛被重复遍历
- 这里有一个关键点，本题地图上的点‘1’，‘0’作为字符录入计算机，在计算机中这两个字符会被转化为ASCII码，‘1’的ASCII码比‘0’的数值多1（分别是49，48）。
# 解法汇总贡献者
注：此处贡献名单仅代表汇总搜集贡献，不代表全部原创，欢迎所有更短的解法🤓
- [Knife丶](https://github.com/cy69855522) [QQ1272068154  微信ly18597591102]
- [YouLookDeliciousC](https://github.com/YouLookDeliciousC) [QQ210021997  微信Ccxj_1013]
