#### leetCode 刷题

#### [数组中最大数对和的最小值](https://leetcode-cn.com/problems/minimize-maximum-pair-sum-in-array/)

一个数对 (a,b) 的 数对和 等于 a + b 。最大数对和 是一个数对数组中最大的 数对和 。

比方说，如果我们有数对 (1,5) ，(2,3) 和 (4,4)，最大数对和 为 max(1+5, 2+3, 4+4) = max(6, 5, 8) = 8 。
给你一个长度为 偶数 n 的数组 nums ，请你将 nums 中的元素分成 n / 2 个数对，使得：

nums 中每个元素 恰好 在 一个 数对中，且
最大数对和 的值 最小 。
请你在最优数对划分的方案下，返回最小的 最大数对和 。

```c++
class Solution {
public:
    int minPairSum(vector<int> &nums)
    {
        sort(nums.begin(), nums.end());
        int len = nums.size();
        int right = len / 2;
        int left = right - 1;
        int res = INT_MIN;
        while (right < len) {
            res = max(res, (nums[left] + nums[right]));
            right++;
            left--;
        }
        return res;
    }
};
```

#### [圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

0,1,···,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

```c++
class Solution {
public:
    int lastRemaining(int n, int m)
    {
       if(n==1) return 0;
       return (lastRemaining(n-1,m)+m)%n;
    }
};
```

#### [扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

```c++

class Solution {
public:
    bool isStraight(vector<int> &nums)
    {
        sort(nums.begin(), nums.end());
        int queue = 0;
        int len = nums.size();
        for (int i = 1; i < len; i++) {
            if (nums[i] != 0 && nums[i] == nums[i - 1]) {
                return false;
            }
            if (nums[i] == 0)
                queue++;
        }
        if (nums[0] == 0)
            queue++;
        //cout<<queue<<" "<<len<<endl;
        if (nums[len - 1] - nums[0 + queue] <= len - 1) {
            return true;
        }
        return false;
    }
};
```

#### [滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

给定一个数组 `nums` 和滑动窗口的大小 `k`，请找出所有滑动窗口里的最大值。

```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int>res;
        deque<int>q;
        for(int i = 0; i < nums.size(); i++){
            if(!q.empty() && i-q.front() >= k)//判断队头是否需要出队
                q.pop_front();
            while(!q.empty()&&nums[q.back()]<nums[i])//维护队列单调性
                q.pop_back();
            q.push_back(i);
            if(i >= k-1){
                res.push_back(nums[q.front()]);//取队头作为窗口最大元素
            }
        }
        return res;
    }
};
```

#### [矩阵置零](https://leetcode-cn.com/problems/set-matrix-zeroes/)

给定一个 `*m* x *n*` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **[原地](http://baike.baidu.com/item/原地算法)** 算法

进阶：

一个直观的解决方案是使用  O(mn) 的额外空间，但这并不是一个好的解决方案。
一个简单的改进方案是使用 O(m + n) 的额外空间，但这仍然不是最好的解决方案。
你能想出一个仅使用常量空间的解决方案吗？

```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
            int m = matrix.size(), n = matrix[0].size();
            vector<int> mArr(m, 1);
            vector<int> nArr(n, 1);
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    if (matrix[i][j] == 0) {
                        mArr[i] = 0;
                        nArr[j] = 0;
                    }
                }
            }

            for (int i = 0; i < m; i++) {
                if (mArr[i] == 0) {
                    for (int j = 0; j < n; j++)
                        matrix[i][j] = 0;
                }
            }

            for (int j = 0; j < n; j++) {
                if (nArr[j] == 0) {
                    for (int i = 0; i < m; i++)
                        matrix[i][j] = 0;
                }
            }
    }
};
```

#### [在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

统计一个数字在排序数组中出现的次数

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
    if(nums.size()==0) return 0;
    auto it =lower_bound(nums.begin(),nums.end(),target);
    int res=0;
    //cout<<*it<<endl;
    while (it<nums.end()&&*it==target) {
        it++;
        res++;
    }
    return res;
    }
};
```

#### [螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix`

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
    vector<vector<int>> res(n,vector<int>(n,0));
    vector<vector<bool>> visited(n,vector<bool>(n,false));
    int dirx[]={0,1,0,-1};
    int diry[]={1,0,-1,0};
    int idx=1,dir=0;
    int i=0,j=0;
    int maxval=n*n;
    //cout<<maxval<<endl;
    while (idx<=maxval) {
        //cout<<"i = "<<i<<" j = "<<j<<endl;
        res[i][j]=idx;
        int tmpi=i,tmpj=j;
        //cout<<" i  =  "<<i<<"  j = "<<j<<"   "<<res[i][j]<<endl;
        i+=dirx[dir];
        j+=diry[dir];
        if(i<0||i>=n||j<0||j>=n||visited[i][j]){
            i-=dirx[dir];
            j-=diry[dir];
            dir=(dir+1)%4;
            i+=dirx[dir];
            j+=diry[dir];
        }
        visited[tmpi][tmpj]=true;
        idx++;
    }
    return res;
    }
};
```

#### [减小和重新排列数组后的最大元素](https://leetcode-cn.com/problems/maximum-element-after-decreasing-and-rearranging/)

给你一个正整数数组 arr 。请你对 arr 执行一些操作（也可以不进行任何操作），使得数组满足以下条件：

arr 中 第一个 元素必须为 1 。
任意相邻两个元素的差的绝对值 小于等于 1 ，也就是说，对于任意的 1 <= i < arr.length （数组下标从 0 开始），都满足 abs(arr[i] - arr[i - 1]) <= 1 。abs(x) 为 x 的绝对值。
你可以执行以下 2 种操作任意次：

减小 arr 中任意元素的值，使其变为一个 更小的正整数 。
重新排列 arr 中的元素，你可以以任意顺序重新排列。
请你返回执行以上操作后，在满足前文所述的条件下，arr 中可能的 最大值 。

```c++
class Solution {
public:
    int maximumElementAfterDecrementingAndRearranging(vector<int>& arr) {
    sort(arr.begin(),arr.end());
    int len=arr.size();
    arr[0]=1;
    for(int i=1;i<len;i++){
        int diff=arr[i]-arr[i-1];
        if(diff>1){
            arr[i]=arr[i-1]+1;
        }
    }
    return arr[len-1];
    }
};
```

#### [绝对差值和](https://leetcode-cn.com/problems/minimum-absolute-sum-difference/)

给你两个正整数数组 nums1 和 nums2 ，数组的长度都是 n 。

数组 nums1 和 nums2 的 绝对差值和 定义为所有 |nums1[i] - nums2[i]|（0 <= i < n）的 总和（下标从 0 开始）。

你可以选用 nums1 中的 任意一个 元素来替换 nums1 中的 至多 一个元素，以 最小化 绝对差值和。

在替换数组 nums1 中最多一个元素 之后 ，返回最小绝对差值和。因为答案可能很大，所以需要对 109 + 7 取余 后返回。

|x| 定义为：

如果 x >= 0 ，值为 x ，或者
如果 x <= 0 ，值为 -x

```
输入：nums1 = [1,7,5], nums2 = [2,3,5]
输出：3
解释：有两种可能的最优方案：
- 将第二个元素替换为第一个元素：[1,7,5] => [1,1,5] ，或者
- 将第二个元素替换为第三个元素：[1,7,5] => [1,5,5]
两种方案的绝对差值和都是 |1-2| + (|1-3| 或者 |5-3|) + |5-5| = 3
```

```c++
class Solution {
public:
    int minAbsoluteSumDiff(vector<int>& nums1, vector<int>& nums2) {
        vector<int> assist(nums1);
    sort(assist.begin(),assist.end());
    long res=0;
    int maxReplace=0;
    int len=nums1.size();
    for(int i=0;i<len;i++)
    {
        int diff=abs(nums1[i]-nums2[i]);
        res+=diff;
        int j=lower_bound(assist.begin(),assist.end(),nums2[i])-assist.begin();
        if(j<len){
            maxReplace=max(maxReplace,diff-(assist[j]-nums2[i]));
        }
        if(j>0){
            maxReplace=max(maxReplace,diff-(nums2[i]-assist[j-1]));
        }
    }
    return (int)((res-maxReplace)%1000000007);        
    }
};
```

#### [反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表 。

```c++
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
    queue<int> headQ;
    queue<int> tailQ;
    stack<int> sRev;
    int status=0;
    int idx=0;
    if(left==right) return head;
    while (head!=nullptr) {
        idx++;
        if(idx==left) status=1;
        else if(idx==right) {
            status=2;
            sRev.push(head->val);
            head=head->next;
            continue;
        }
        if(status==0) headQ.push(head->val);
        else if(status==1) sRev.push(head->val);
        else tailQ.push(head->val);
        head=head->next; 
    }
    ListNode* dump=new ListNode(-1,nullptr);
    ListNode* tmp=dump;
    while (!headQ.empty()) {
         ListNode* tmp2=new ListNode(headQ.front());
         tmp->next=tmp2;
         tmp=tmp2;
         headQ.pop();
    }
    while (!sRev.empty()) {
        //cout<<sRev.top()<<endl;
         ListNode* tmp2=new ListNode(sRev.top());
         tmp->next=tmp2;
         tmp=tmp2;
         sRev.pop();
    }
    while (!tailQ.empty()) {
         ListNode* tmp2=new ListNode(tailQ.front());
         tmp->next=tmp2;
         tmp=tmp2;
         tailQ.pop();
    }
    return dump->next;
    }
};
```

#### [ 字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

```c++
class Solution {
public:
    int firstUniqChar(string s) {
    unordered_map<char,int> dic;
    int len=s.size();
    for(int i=0;i<len;i++){
        if(dic.count(s[i])) dic[s[i]]++;
        else dic.insert({s[i],1});
    }
    int res=INT_MAX;
    for(auto item : dic){
        if(item.second==1) res=min(res,(int)s.find(item.first));
    }
    return res==INT_MAX?-1:res;
    }
};
```

#### [有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

```
输入: s = "anagram", t = "nagaram"
输出: true

输入: s = "rat", t = "car"
输出: false
```

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size()!=t.size()) return false;
        sort(s.begin(),s.end());
        sort(t.begin(),t.end());
        int len=s.size();
        for(int i=0;i<len;i++){
            if(s[i]!=t[i]) return false;
        }
        return true;
    }
};
```

#### [最小栈](https://leetcode-cn.com/problems/min-stack/)

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。

```c++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> dataStack;
    stack<int> minNumStack;
    MinStack() {

    }
    
    void push(int val) {
        dataStack.push(val);
        if(minNumStack.empty()||val<=minNumStack.top()) minNumStack.push(val);
    }
    
    void pop() {
        int val=dataStack.top();
        dataStack.pop();
        if(val==minNumStack.top()) minNumStack.pop();
    }
    
    int top() {
        return dataStack.top();
    }
    
    int getMin() {
        return minNumStack.top();
    }
};
```

#### [二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        int targetP=min((*p).val,(*q).val);
        int targetQ=max((*p).val,(*q).val);
        return dfs(root,targetP,targetQ);
    }

int getMax(TreeNode* root){
    if(root->right==nullptr)
        return root->val;
    else
        return getMax(root->right);
}

int getMin(TreeNode* root){
    if(root->left==nullptr)
        return root->val;
    else
        return getMin(root->left);
}


TreeNode* dfs(TreeNode* root,int targetP,int targetQ){
    if(root->val==targetP||root->val==targetQ) return root;
    if(root->left==nullptr){
        return dfs(root->right,targetP,targetQ);
    }
    if(root->right==nullptr){
        return dfs(root->left,targetP,targetQ);
    }
    int leftMax=getMax(root->left);
    int rightMin=getMin(root->right);
    if(targetP<=leftMax&&rightMin<=targetQ){
        return root;
    }
    else if(targetP>leftMax){
        return dfs(root->right,targetP,targetQ);
    }
    else{
        return dfs(root->left,targetP,targetQ);
    }
}
};
```

#### [位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)

编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为[汉明重量](https://baike.baidu.com/item/汉明重量)）。

```
class Solution {
public:
    int hammingWeight(uint32_t n) {
    int res=0;
    for(int i=0;i<32;i++){
        if(n&(1<<i)) res++;
    }
    return res;    
    }
};
```

#### [验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明：**本题中，我们将空字符串定义为有效的回文串。

```
输入: "A man, a plan, a canal: Panama"
输出: true
解释："amanaplanacanalpanama" 是回文串
```

```
class Solution {
public:
    bool isPalindrome(string s) {
    string newStr="";
    for(int i=0;i<s.size();i++)
    {
        if('a'<=s[i]&&s[i]<='z'){
            newStr+=s[i];
        }else if('A'<=s[i]&&s[i]<='Z'){
            newStr+=tolower(s[i]);
        }else if('0'<=s[i]&&s[i]<='9')
        {
             newStr+=s[i];
        }
    }
    int last=newStr.size()-1,start=0;
    while (start<last) {
        if(newStr[start]!=newStr[last]){
            return false;
        }
        start++;
        last--;
    }
    return true;
    }
};
```

#### [杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/)

给定一个非负整数 *numRows，*生成杨辉三角的前 *numRows* 行。

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
    vector<vector<int>> res;
    if(numRows==0) return res;
    vector<int> preVector={1};
    res.push_back(preVector);
    for(int i=1;i<numRows;i++)
    {
        vector<int> currVector;
        currVector.push_back(1);
        int len=preVector.size();
        for(int i=0;i<len-1;i++)
        {
            currVector.push_back(preVector[i]+preVector[i+1]);
        }
        currVector.push_back(1);
        res.push_back(currVector);
        preVector=currVector;
    }
    return res;
    }
};
```

#### [二进制求和](https://leetcode-cn.com/problems/add-binary/)

给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 **非空** 字符串且只包含数字 `1` 和 `0`。

```c++
class Solution {
public:
    int addChar(char a,char b,int adds,string* c){
    if(a=='0'&&b=='0') {
        if(adds==1) *c='1';
        else *c='0';
        return 0;
    }
    if(a=='1'&&b=='1') {
        if(adds==1) *c='1';
        else *c='0';
        return 1;
    }
    if(adds==1)
    {
        *c='0';
        return 1;
    }else{
        *c='1';
        return 0;
    }
}
    string addBinary(string a, string b) {
        string res="";
    int addRes=0;
    string c;
    int aLen=a.size()-1,bLen=b.size()-1;
    while (aLen>=0&&bLen>=0) {
        addRes = addChar(a[aLen],b[bLen],addRes,&c);
        res.insert(0,c);
        aLen--;
        bLen--;
    }
    while (aLen>=0) {
        addRes = addChar(a[aLen],'0',addRes,&c);
        res.insert(0,c);
        aLen--;
    }
    while (bLen>=0) {
        addRes = addChar('0',b[bLen],addRes,&c);
        res.insert(0,c);
        bLen--;
    }
    if(addRes==1) res.insert(0,"1");
    //cout<<res<<endl;
    return res;
    }
};
```

#### [ 整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)

罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给你一个整数，将其转为罗马数字。

```c++
class Solution {
public:
    string intToRoman(int num) {
        string res="";
    while (num>=1000) {
        num-=1000;
        res+="M";
        //res.insert(res.size(),"M");
    }
    while (num>=900) {
        num-=900;
        res+="CM";
    }
    while (num>=500) {
        num-=500;
        res+="D";
    }
    while (num>=400) {
        num-=400;
        res+="CD";
    }
    while (num>=100) {
        num-=100;
        res+="C";
    }
    while (num>=90) {
        num-=90;
        res+="XC";
    }
    while (num>=50) {
        num-=50;
         res+="L";
    }
    while (num>=40) {
        num-=40;
        res+="XL";
    }
    while (num>=10) {
        num-=10;
        res+="X";
    }
    while (num>=9) {
        num-=9;
        res+="IX";
    }
    while (num>=5) {
        num-=5;
        res+="V";
    }
    while (num>=4) {
        num-=4;
        res+="IV";
    }
    while (num>0) {
        num-=1;
        res+="I";
    }
    //cout<<res<<endl;
    return res;
    }
};
```

#### [H 指数](https://leetcode-cn.com/problems/h-index/)

给你一个整数数组 citations ，其中 citations[i] 表示研究者的第 i 篇论文被引用的次数。计算并返回该研究者的 h 指数。

h 指数的定义：h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （n 篇论文中）总共有 h 篇论文分别被引用了至少 h 次。且其余的 n - h 篇论文每篇被引用次数 不超过 h 次。

例如：某人的 h 指数是 20，这表示他已发表的论文中，每篇被引用了至少 20 次的论文总共有 20 篇。

提示：如果 h 有多种可能的值，h 指数 是其中最大的那个。

```
输入：citations = [3,0,6,1,5]
输出：3 
解释：给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3。
```

```c++
class Solution {
public:
 int hIndex(vector<int>& citations) {
        int res =0,len=citations.size();
        sort(citations.begin(),citations.end());
        for(int i=0;i<len;i++){
           int refTime=len-i;
           if(citations[i]>=refTime) res=max(res,refTime);
        }
        //cout<<res<<endl;
        return res;
}
};
```

#### [字符串解码](https://leetcode-cn.com/problems/decode-string/)

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

```c++
class Solution {
public:
    string decodeString(string s) {
        return dfs(s);
    }
    string dfs(string s){
    int leftCount=0,len=s.size();
    int leftIndex=-1,rightIndex=-1;
    for(int i=0;i<len;i++){
        if(s[i]=='[') {
            leftCount++;
            if(leftCount==1) leftIndex=i;
        }
        else if(s[i]==']') {
            leftCount--;
            if(leftCount==0) {
                rightIndex=i;
                break;
            }
        }
    }
    if(leftIndex!=-1){
        int movenum=1;
        while (0<=s[leftIndex-movenum]&&s[leftIndex-movenum]<='9') {
            movenum++;
            if(leftIndex-movenum<0) break;
        } 
        movenum--;
        //cout<<movenum<<endl;
        int num=0;
        for(int i=movenum;i>=1;i--) num=num*10+s[leftIndex-i]-'0';
        string tmp(s.begin()+leftIndex+1,s.begin()+rightIndex);
        //cout<<"tmp = "<<tmp<<endl;
        string newTmp=dfs(tmp);
        string addS;
        for(int i=0;i<num;i++) addS+=newTmp;
        //cout<<"adds = "<<addS<<endl;
        s.erase(s.begin()+leftIndex-movenum,s.begin()+rightIndex+1);
        //cout<<" s1 = "<<s<<endl;
        s.insert(leftIndex-movenum,addS);
    }else{
        return s;
    }
    return dfs(s);
}
};
```

#### [主要元素](https://leetcode-cn.com/problems/find-majority-element-lcci/)

数组中占比超过一半的元素称之为主要元素。给你一个 整数 数组，找出其中的主要元素。若没有，返回 -1 。请设计时间复杂度为 O(N) 、空间复杂度为 O(1) 的解决方案。

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
         int num=nums[0],numoftime=1,len=nums.size();
    for(int i=1;i<len;i++)
    {
        if(numoftime==0) {
            num=nums[i];
            numoftime++;
        }
        else if(nums[i]==num) numoftime++;
        else numoftime--;
    }
    int count=0;
    for(int i=0;i<len;i++){
        if(nums[i]==num) count++;
    }
    if(count>len/2) return num;
    return -1;
    }
};
```

#### [分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

```
输入: g = [1,2,3], s = [1,1]
输出: 1
解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
```

```
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        int gLen=g.size(),sLen=s.size(),gIdx=0,sIdx=0;
        int res=0;
        while(gIdx<gLen&&sIdx<sLen){
            if(s[sIdx]>=g[gIdx]){
                res++;
                sIdx++;
                gIdx++;
            }else{
                sIdx++;
            }
        }
        return res;
    }
};
```

#### [二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。

路径和 是路径中各节点值的总和。

给你一个二叉树的根节点 root ，返回其 最大路径和

```
class Solution {
public:
    int res=INT_MIN; 
    int maxPathSum(TreeNode* root) {
        dfs(root);
        return res;
    }
    int dfs(TreeNode* root)
    {
        if(root==nullptr) return 0;
        int left=dfs(root->left);
        int right =dfs(root->right);
        int tmp=0;
        if(left>0) tmp+=left;
        if(right>0) tmp+=right;
        tmp+=root->val;
        res=max(res,tmp);
        return root->val+max(max(left,right),0);
    }
};
```

