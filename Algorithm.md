# 算法梗概
## 递归算法
递归过程一般通过函数或者子过程来实现。
```C++
int function(int x){
    if(n == x)return 1;
    ...;
    return function(x);
}
```
## 排序算法
核心思想是将无序表排序成有序表。
### 冒泡算法
```C++
for(int i = 0; i < nums.size(); i++)
    for(int j = 0; j < nums.size() - i; j++)
        if(nums[j] > nums[j+1])
            swap(&nums[j], &nums[j+1])
```
### 快速排序算法
```C++
while(i != j) {
    while(array[j] >= temp && i < j) {
        j--;
    }
	while(array[i] <= temp && i < j) {
        i++;
    }
	if(i < j) {
        swap(array[i], array[j]);
    }
}
```
## 二分查找算法
核心思想是首先选取**有序表**中间位置的记录，将其关键字与给定关键字 key 进行比较，若相等，则査找成功；若 key 值比该关键字值大，则要找的元素一定在右子表中，则继续对右子表进行折半查找；若 key 值比该关键宇值小，则要找的元素一定在左子表中，继续对左子表进行折半査找。如此递推，直到査找成功或査找失败（或査找范围为 0）。

## 搜索算法
### 深度优先算法DFS
### 广度优先算法BFS

## 哈希算法
经典的利用空间换时间的算法。任意长度输入 -> 固定长度哈希值，同样的输入得到同样的输出，输入只要改一点则输出则完全不同。

## 贪心算法

## 分支算法

## 回溯算法

## 动态规划算法
### DP

## 字符串匹配算法
### KMP算法
KMP 是一个解决模式串在文本串是否出现过，如果出现过，最早出现的位置的经典算法。

Knuth-Morris-Pratt 字符串查找算法，简称为 “KMP 算法”，常用于在一个文本串 S 内查找一个模式串 P 的出现位置，这个算法由*Donald Knuth*、*Vaughan Pratt*、*James H. Morris* 三人于 1977 年联合发表，故取这 3 人的姓氏命名此算法。

KMP 方法算法就利用之前判断过的信息，通过一个 next 数组，保存模式串中**前后最长公共子序列**的长度，每次回溯时，通过 next 数组找到，前面匹配过的位置，省去了大量的计算时间。

#### 最大公共前后缀
1. 字符串的前缀是指**不包含最后一个字符的所有以第一个字符（索引为0）开头的连续子串**
	> 比如字符串"ABABA"的前缀有：A, AB, ABA, ABAB

2. 字符串的后缀是指**不包含开头一个字符的所有以最后一个字符字符结尾的连续子串**
	> 比如字符串"ABABA"的后缀有：A, BA, ABA, BABA

3. 公共前后缀就是指一个字符串的**所有前缀连续子串**和**所有后缀连续子串**中相等的子串
	> 比如字符串"ABABA"的公共前后缀有：A, ABA

4. 最长公共前后缀又是指公共前后缀中的**长度最长**的子串
	> 比如字符串"ABABA"的最大公共前后缀有：ABA

#### 部分匹配表 —— next 数组
```C
// 1. 构建 next 数组
int n = strlen(match);
int m = strlen(pattern);
int next[m];
next[0] = 0;
for (int i = 1, j = 0; i < m; i++) {
    while (j > 0 && pattern[i] != pattern[j])	// 失败回退
        j = next[j - 1];
    if (pattern[i] == pattern[j])				// 匹配扩展
        j++;
    next[i] = j;
}
```
失败回退：当前缀和后缀不匹配时，找到*上一个子串索引的最长公共前后缀*作为参考拼接

匹配扩展：当前缀和后缀匹配时，在上一个子串的最长公共前后缀长度的基础上 + 1

#### 部分匹配表搜索字符串匹配
```C
for (int i = 0, j = 0; i < n; i++) {
    while (j > 0 && match[i] != pattern[j]) {
        j = next[j - 1];
    }
    if (match[i] == pattern[j]) {
        j++;
    }
    if (j == m) {
        return i - m + 1;
    }
}
```

## 二叉树算法
二叉树是一种重要的数据结构，它是由节点组成的，每个节点最多有两个子节点，分别是左子节点和右子节点。二叉树可以为空，也可以只有一个根节点，或者有一个根节点和两个子节点。二叉树的特点是每个节点最多只能有两个子节点，这两个子节点被称为左子树和右子树。
### 二叉树的性质
* 在二叉树的第`k`层，最多有`2^(k-1)`个节点。
* 深度`k`的二叉树最多有`2^k-1`个节点。
* 包含`n`个节点的二叉树的高度至少为`log2(n+1)`。
### 二叉树的数据结构
```C++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};
```
### 二叉树的前序、中序、后序遍历
* 前序遍历：根前后
    ```C++
    void preorder(TreeNode* root, vector<int>& res){
        if(!root)return;
        res.push_back(root->val);
        preorder(root->left, res);
        preorder(root->right, res);
    }
    ```
* 中序遍历：前根后
    ```C++
    void inorder(TreeNode* root, vector<int>& res){
        if(!root)return;
        inorder(root->left, res);
        res.push_back(root->val);
        inorder(root->right, res);
    }
    ```
* 后序遍历：前后根
    ```C++
    void postorder(TreeNode* root, vector<int>& res){
        if(!root)return;
        postorder(root->left, res);
        postorder(root->right, res);
        res.push_back(root->val);
    }
    ```