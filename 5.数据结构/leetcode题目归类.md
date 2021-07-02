[TOC]

## 链表

### 遍历（其实很多用的也是双指针，但不是快慢指针）

#### 模板

- 1.递归遍历（前后序遍历） 不常用！！

  - ```c++
    vector<ListNode*> vec_pos;//保存遍历结果
    vector<ListNode*> vec_neg;
    void traverse(ListNode*head){//经典遍历
        if(head==nullptr)
            return;
        vec_pos.push_back(head);//前序遍历，即正序打印链表
        traverse(head->next);
        vec_neg.push_back(head);//后序遍历，即倒序打印链表
    }
    ```

- 2.迭代遍历

  - ```c++
    vector<int> res;//保存遍历结果
    void traverse(ListNode* head){
        while(head){
            res.push_back(head->val);
            head=head->next;
        }
    }
    ```

#### 题目

- [21. 合并两个有序链表（easy）](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
  - 使用哨兵确实简洁很多。
- [23. 合并K个升序链表（hard）](https://leetcode-cn.com/problems/merge-k-sorted-lists/)
  - 我使用的是最朴实的思想，几乎不使用额外内存。跟合并2个有序链表的思想一致。每个节点，遍历一个链表数目比如为n。所以是O(n*N)
- [2. 两数相加（medium）类似于合并两个有序链表](https://leetcode-cn.com/problems/add-two-numbers/)
- [83. 删除排序链表中的重复元素（easy，就是个遍历，多一个指针保存pre节点而已）](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
  - 遍历，相当于双指针pre cur
- [82. 删除排序链表中的重复元素 II（medium）](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)
  - 3个指针，pre left right，画画图。涉及到可能删除首节点的，设置一个dummy节点非常方便
- [剑指 Offer 18. 删除链表的节点（easy，遍历）](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)
  - 可能会删除头结点，加个dummy开始遍历，双指针一个pre指向dummy 一个cur指向head
  - （这个题delete删除cur节点反而还通过不了，在本地完全没错。理论上必须加上delete才对。）
- [剑指 Offer 25. 合并两个排序的链表（easy）](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)
  - 设置dummy节点
  - 其实` while(head1!=nullptr && head2!=nullptr) `还是` while(l1!=nullptr || l2!=nullptr) `都行。 &&时就是在while外继续处理，||时就是在while里处理
- [138. 复制带随机指针的链表（medium，hashmap不是最佳，最佳方法待解决）](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)
  - 用hash表。时间复杂度O(2N)，空间复杂度O(N)。这肯定不是最巧妙的方法。但是想法最简单
- 

### 反转、部分反转

#### 模板

- 迭代反转整个链表

  - ```c++
    ListNode* reverseList(ListNode* head) {
        ListNode* pre=nullptr;
        ListNode* cur=head;
        ListNode* nxt=nullptr;
        while(cur!=nullptr){
            nxt=cur->next;//保存下一个节点
            cur->next=pre;//当前节点的下一个设置为上一个
            pre=cur;//cur变pre
            cur=nxt;//nxt变cur
        }
        //退出时，cur指向nullptr,pre指向原末尾
        return pre;
    }
    ```

- 迭代反转[a,b)节点上的链表

  - 做这种题一定要切记[a,b)区间，反转以后的头部会由reverse函数返回。而尾部，就是a节点！

  - ```c++
    ListNode* reverse(ListNode*a,ListNode*b){
        ListNode* pre,*cur,*nxt;
        pre=nullptr;cur=a;nxt=nullptr;
        while(cur!=b){//跟迭代反转整个链表的区别仅在于 cur!=b还是cur!=nullptr
            nxt=cur->next;
            cur->next=pre;
            pre=cur;
            cur=nxt;
        }
        //退出时，cur指向b,pre指向原顺序中b的前一个节点
        return pre;
    }
    /*比如
    1->2->3->4->5
    left=1
    right=4
    那么a应该是1，b应该是5，因为左开右闭。[1,5)即反转 [1,4]*/
    ```

#### 题目

- [206. 反转链表（easy反转整个链表）](https://leetcode-cn.com/problems/reverse-linked-list)
- [92. 反转链表 II（medium）反转left到right上的部分链表](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
  - 反转部分链表
- [25. K 个一组翻转链表(hard)](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/).
  - 反转部分链表[a,b)
- [234. 回文链表(easy判断是否是回文链表)](https://leetcode-cn.com/problems/palindrome-linked-list/)
  - 双指针找中点+反转后半部分链表+比较
- [143. 重排链表（medium 运用了链表找中点、翻转链表等方法）](https://leetcode-cn.com/problems/reorder-list/)
  - 第一步：找到原链表的中点。（类似于回文链表里找中点）
  - 第二步：将原链表的右半端反转（翻转一部分链表）
  - 第三步：将原链表的两端合并。
- [24. 两两交换链表中的节点（medium，跟每k个一组反转链表一样）](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)
  - 遇到头结点未知的情况时，都建议用dummy节点。它比k个一组反转简单，因为不用考虑不足k个时是反转还是不反转的情况。
- [字节跳动高频题——排序奇升偶降链表](https://mp.weixin.qq.com/s/377FfqvpY8NwMInhpoDgsw)
  - 1.按奇偶位置拆分链表，得1->3->5->7->NULL和8->6->4->2->NULL。（328. 奇偶链表）
  - 2.反转偶链表，得1->3->5->7->NULL和2->4->6->8->NULL。（经典的反转整条链表的模板）
  - 3.合并两个有序链表，得1->2->3->4->5->6->7->8->NULL。（剑指 Offer 25. 合并两个排序的链表）
- 

### 双指针（快慢双指针、等速双指针）

#### 模板

#### 题目

- [876. 链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)
  - 快慢指针找中点
- [234. 回文链表(easy判断是否是回文链表)](https://leetcode-cn.com/problems/palindrome-linked-list/)
  - 双指针找中点+反转后半部分链表+比较

- [141. 环形链表(easy判断有环没环)](https://leetcode-cn.com/problems/linked-list-cycle/)
  - 快慢指针，有环的话快指针一定能追上慢指针
- [142. 环形链表 II（medium链表有环，返回环的入口）](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
  - 先判断有没有环。有环时，从slow和fast的交点处，再继续跑k-m次循环，再相遇时即环的入口。
- [面试题 02.02. 返回倒数第 k 个节点（easy）](https://leetcode-cn.com/problems/kth-node-from-end-of-list-lcci/)
  - fast先走k步，然后同速前进。fast到尾部时，slow就停在了倒数第k个节点上
- [160. 相交链表（easy，跟剑指 Offer 52. 两个链表的第一个公共节点一样）](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)
  - 双指针，每个指针都跑两圈，必然总移动距离相同
  - 参看剑指 Offer 52. 两个链表的第一个公共节点。那个写法更为简洁。
- [143. 重排链表（medium 运用了链表找中点、翻转链表等方法）](https://leetcode-cn.com/problems/reorder-list/)
  - 第一步：找到原链表的中点。（类似于回文链表里找中点）
  - 第二步：将原链表的右半端反转（翻转一部分链表）
  - 第三步：将原链表的两端合并。
- [19. 删除链表的倒数第 N 个结点（medium）类似于返回倒数第k个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
- [19. 删除链表的倒数第 N 个结点（medium）类似于返回倒数第k个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
  - 快慢指针
  - fast提前移动n步，slow将停在要删除的那个节点。需要一个Pre节点来记录它之前的那个节点
- [剑指 Offer 52. 两个链表的第一个公共节点（easy，类似于环形链表的入口）](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)
  - 两个指针来跑，跑完自己的去跑对方的
- [328. 奇偶链表（medium，有点像快慢指针）](https://leetcode-cn.com/problems/odd-even-linked-list/)
  - 双指针遍历。遇到一个大坑，之前没有设置successor，误以为while退出时head->next还是第二端起点。其实head->next早就被修改了！！一定要小心，第二段链表的起点。
- [字节跳动高频题——排序奇升偶降链表](https://mp.weixin.qq.com/s/377FfqvpY8NwMInhpoDgsw)
  - 1.按奇偶位置拆分链表，得1->3->5->7->NULL和8->6->4->2->NULL。（328. 奇偶链表）
  - 2.反转偶链表，得1->3->5->7->NULL和2->4->6->8->NULL。（经典的反转整条链表的模板）
  - 3.合并两个有序链表，得1->2->3->4->5->6->7->8->NULL。（剑指 Offer 25. 合并两个排序的链表）
- [61. 旋转链表（medium）](https://leetcode-cn.com/problems/rotate-list/)
  - 双指针，两个链表合并为一个
- [86. 分隔链表（medium）](https://leetcode-cn.com/problems/partition-list/)
  - 双指针，两个链表合并为一个

### 经验

- 双指针找中点是个非常常见的操作。
- 凡是 **涉及到多条链表合并成一个**的；**头结点不一定是谁的**；**有可能删除头节点**的。都建议使用dummy节点。
  - 用dummy时，dummy不要移动，构造一个pre=dummy，让pre去移动维系新链表。dummy->next用来在最后return
- 特别小心：链表最末尾设置nullptr、涉及到两段链表合并的一定要注意第二段链表的开头节点(后继者，successor)保存了没有
  - 例如：[328. 奇偶链表（medium，有点像快慢指针）](https://leetcode-cn.com/problems/odd-even-linked-list/)
    - 双指针遍历。遇到一个大坑，之前没有设置successor，误以为while退出时head->next还是第二端起点。其实head->next早就被修改了！！一定要小心，第二段链表的起点。
- 备注：[剑指 Offer 25. 合并两个排序的链表（easy）](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)
  - 设置dummy节点
  - 其实` while(head1!=nullptr && head2!=nullptr) `还是` while(l1!=nullptr || l2!=nullptr) `都行。 &&时就是在while外继续处理，||时就是在while里处理

## 树

### 前中后序遍历

#### 模板

- 1.二叉树递归遍历（前中后序遍历）

  - ```c++
    vector<int> res;//保存遍历结果
    void traverse(TreeNode* root){
        if(root==nullptr)
            return;
        //前序遍历 res.push_back(root->val);   (DFS，深度优先)
        traverse(root->left);
        //中序遍历 res.push_back(root->val);   (DFS，深度优先)
        traverse(root->right);
        //后序遍历 res.push_back(root->val);   (DFS，深度优先)
    }
    ```

- 2.N叉树递归遍历（前后序遍历）

  - ```c++
    /* 基本的N叉树节点 */
    class TreeNode {
        int val;
        vector<Node*> children;
    }
    
    vector<int> res;//保存遍历结果
    void traverse(TreeNode* root){
        if(root==nullptr)
            return;
        //前序遍历 res.push_back(root->val);
        for(auto child:root->children){//N叉树没有中序遍历
            traverse(child);
        }
        //后序遍历 res.push_back(root->val);
    }
    ```

- 3.二叉树的迭代遍历（前中后序遍历）

  - ```c++
    //前序
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        TreeNode* T=root;
        stack<TreeNode*> s;
    
        while(T || !s.empty())
        {
            while(T)
            {
                s.push(T);
                res.push_back(T->val);
                T=T->left;
            }
    
            T=s.top();
            s.pop();
            T=T->right;
    
        }
        return res;
    }
    
    //中序
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        TreeNode* T=root;
        stack<TreeNode*> s;
        while(T || !s.empty())
        {
            while(T)
            {
                s.push(T);
                T=T->left;
            }
            T=s.top();
            s.pop();
            res.push_back(T->val);
            T=T->right;
        }
        return res;
    }
    
    //后序
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;//根右左遍历，然后reverse
        TreeNode* T=root;
        stack<TreeNode*> s;      
        while(T || !s.empty())
        {
            while(T)
            {
                s.push(T);
                res.push_back(T->val);
                T=T->right;
            }
            T=s.top();
            s.pop();
            T=T->left;
        }
    
        reverse(res.begin(),res.end());
        return res;
    }
    ```

    

#### 题目

- [144. 二叉树的前序遍历（medium）](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)
- [94. 二叉树的中序遍历（medium）](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
- [341. 扁平化嵌套列表迭代器](https://leetcode-cn.com/problems/flatten-nested-list-iterator/)
  - N叉树的前序遍历
- [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)
  - 前序遍历
- [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

### 层序遍历类

#### 模板

- 1.`vector<vector<int>>`式，要区分每一层

  - ```c++
    vector<vector<int>> levelOrder(TreeNode* root){
        vector<vector<int>> res;
        if(root==nullptr) return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int len=q.size();//这一层一共有几个节点
            vector<int> vec;//仅仅存这一层上的值
            for(int i=0;i<len;i++){
                TreeNode* cur=q.front();
                q.pop();
                vec.push_back(cur->val);//这样写，必须得保证队列中的节点不能为空
                if(cur->left!=nullptr) q.push(cur->left);
                if(cur->right!=nullptr) q.push(cur->right);
            }
            res.push_back(vec);
        }
        return res;
    }
    ```

- 2.`vector<int>`式，不用区分每一层

  - ```c++
    vector<int> levelOrder(TreeNode* root) {
        vector<int> res;
        if(root==nullptr) return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* root=q.front();
            q.pop();
            res.push_back(root->val);//这样写，必须得保证队列中的节点不能为空
            if(root->left!=nullptr) q.push(root->left);
            if(root->right!=nullptr) q.push(root->right);
        }
        return res;
    }
    
    //备注
    /*if(root->left!=nullptr) q.push(root->left);
    if(root->right!=nullptr) q.push(root->right);
    两行顺序一颠倒就是从右往左的层序遍历了*/
    ```

#### 题目

- [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal)
- [103. 二叉树的锯齿形层次遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal)
  - 多了一个reverse操作，或者是不使用reverse，在每一层push_back的时候就直接在对应索引处赋值
- [ 199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view)
- [958. 二叉树的完全性检验](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree/)
  - nullptr也加入到queue中。遇到第一个Nullptr时退出while循环，开始检测队列里之后的元素还有没有非nullptr的节点。发现任何一个非Nullptr都不是完全二叉树。
- [662. 二叉树最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)
- [429. N 叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)
- 

### 求树高、直径、最大路径和（返回int 或者是  void traverse中有个int&，从下到上后序遍历）

#### 模板

- 算树高--1.使用返回值计算

  - ```c++
    int traverse(TreeNode* root){
        if(root==nullptr)
            return 0;
        int left=traverse(root->left);
        int right=traverse(root->right);
        int maxdep=1+(left>right?left:right);//后序
        return maxdep;
    }
    ```

- 算树高--2.使用一个变量来维系（剑指 Offer 55 - II. 平衡二叉树）

  - ```c++
    //如果traverse时除了需要返回树高，还要返回别的东西 比如true false，那么非常建议使用第二种求树高的方法。
    class Solution {
    public:
        int maxDepth(TreeNode* root) {
            int depth;
            traverse(root,depth);
            return depth;
        }
        void traverse(TreeNode* root,int& depth){//虽然我写的是void，但是在使用中一般都是有别的返回值时才使用这个模板。如果返回void，那不如使用树高模板1
            if(root==nullptr){
                depth=0;
                return;
            }
            int leftDep,rightDep;
            traverse(root->left,leftDep);
            traverse(root->right,rightDep);
            depth=1+(leftDep>rightDep?leftDep:rightDep);//后序遍历
        }
    };
    ```

#### 题目

备注：除了 剑指 Offer 55 - II. 平衡二叉树，都用模板1

备注：如果真的要返回路径，而不是路径和这种int类型，那就用普通的递归（比如[剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)）

- [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)
- [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree)
- [559. N 叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)
- [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
- [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)
- [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
  - 不是求树高了，而是求值的和。模板类似，但是要换返回值的定义
- [687. 最长同值路径](https://leetcode-cn.com/problems/longest-univalue-path/)
  - 与[124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)类似
  - 不是求树高了，而是求相同值的个数。模板类似，但是要换返回值的定义
- [剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)**（模板2）**

### 树的路径和（从上到下前序遍历，需要额外形参记录curSum）

#### 模板

```c++
//112. 路径总和
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        return traverse(root,0,targetSum);
    }

    bool traverse(TreeNode* root,int curSum,int targetSum){//所有经过本节点的路径上是否存在一条等于targetSum的路径
        if(root==nullptr)
            return false;
        curSum+=root->val;
        if(root->left==nullptr && root->right==nullptr){
            return curSum==targetSum;
        }
        else{
            return traverse(root->left,curSum,targetSum) || traverse(root->right,curSum,targetSum);
        }
    }
};
//剑指 Offer 34. 二叉树中和为某一值的路径
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int target) {
        traverse(root,0,target);
        return res;
    }
    vector<vector<int>> res;
    vector<int> curVec;
    void traverse(TreeNode* root,int curSum,int target){
        if(root==nullptr){
            curVec.push_back(0);
            return;
        }
        curSum+=root->val;
        curVec.push_back(root->val);//前序遍历
        if(root->left==nullptr && root->right==nullptr){//当前是叶子节点         
            if(curSum==target){
                res.push_back(curVec);
            }
        }
        traverse(root->left,curSum,target);//左
        curVec.pop_back();

        traverse(root->right,curSum,target);//右
        curVec.pop_back();
    }
};
//129. 求根节点到叶节点数字之和
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        res=0;
        traverse(root,0);
        return res;
    }
    int res;
    void traverse(TreeNode* root,int cur){
        if(root==nullptr)
            return;
        cur=cur*10+root->val;
        if(root->left==nullptr && root->right==nullptr){
            res+=cur;
            return;
        }
        traverse(root->left,cur);
        traverse(root->right,cur);
    }
};
```

#### 题目

- [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)
- [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)
- [129. 求根节点到叶节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)
- 

### 通用常见递归（返回什么类型的都有）

#### 模板

```c++
//举例
//1.计算二叉树的节点数。其实跟求树高其实类似。
int cntNodes(TreeNode* root){
    if(root==nullptr)// base case
        return 0;
    int left=cntNodes(root->left);//左子树上的节点数
    int right=cntNodes(root->right);//右子树上的节点数
    return left+right+1;//1是指本节点还要占一个节点
}

//2.翻转二叉树（其实跟要构造树、翻转节点、在树中删除、插入某个节点时，一定要用这种递归模板（返回TreeNode*）类似）
void reverse(TreeNode* root){
    if(root==nullptr)//base case
        return;
    reverse(root->left);//左右子树递归
    reverse(root->right);
    //后序遍历
    TreeNode* tmp=root->left;
    root->left=root->right;
    root->right=tmp;
}

//3.二叉树的最近公共祖先
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(root==nullptr)// base case
        return nullptr;
    if(root==p||root==q)//base case 当前节点等于p或者q时，就不往下遍历了。直接返回
        return root;

    TreeNode* left=lowestCommonAncestor(root->left,p,q);//当前节点的左子树的结果
    TreeNode* right=lowestCommonAncestor(root->right,p,q);//当前节点的右子树的结果

    if(left!=nullptr && right!=nullptr)//left和right都不为空，说明p、q分别在左右子树上
        return root;
    if(left==nullptr && right==nullptr)//left和right都为空，说明p、q都不在左右子树上
        return nullptr;
    return left==nullptr?right:left;//left和right一个为空，一个不为空，说明找到了p、q中的一个
}

//4. 相同的树
bool isSame(TreeNode*p,TreeNode*q){
    if(p==nullptr && q==nullptr)
        return true;
    if(p==nullptr || q==nullptr)
        return false;

    return (p->val==q->val && isSame(p->left,q->left) && isSame(p->right,q->right));
}
```

#### 题目

- [236. 二叉树的最近公共祖先（medium）](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

- [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

- [226. 翻转二叉树（easy）](https://leetcode-cn.com/problems/invert-binary-tree/)

- [114. 二叉树展开为链表（medium）](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

- [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)
  - 经典前序遍历的基础上改的
  
- [100. 相同的树](https://leetcode-cn.com/problems/same-tree/)

  

### 双节点类递归（返回void、bool都有可能）

#### 模板

双节点递归。首先你要判断出来，这个题用一个节点来递归能不能完成目的（比如判断出对称）。很显然不能，需要双节点

```c++
//剑指offer第116.填充每个节点的下一个右侧节点指针
class Solution {
public:
    Node* connect(Node* root) {
        if(root==nullptr)
            return nullptr;
        connectTwoNode(root->left,root->right);
        return root;
    }
    void connectTwoNode(Node* node1,Node* node2){
        if(node1==nullptr || node2==nullptr)
            return;
        //前序遍历
        // 将传入的两个节点连接
        node1->next=node2;
        connectTwoNode(node1->left,node1->right);// 连接相同父节点的两个子节点。node1的左和node1的右
        connectTwoNode(node1->right,node2->left);// 连接跨越父节点的两个子节点。node1的右和node2的左
        connectTwoNode(node2->left,node2->right);// 连接相同父节点的两个子节点。node2的左和node2的右
    }
};
```

#### 题目

- [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)
- [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

### 凡是要构造树、合并二叉树、翻转节点、树的镜像、在树中删除、插入某个节点时，一定要用这种递归模板（返回TreeNode*）

#### 模板

```c++
//1.返回TreeNode*的遍历
vector<int> res;//保存遍历结果
TreeNode* traverse(TreeNode* root){
    if(root==nullptr)
        return nullptr;
    //前序遍历 res.push_back(root->val);   (DFS，深度优先)
    root->left=traverse(root->left);
    //中序遍历 res.push_back(root->val);   (DFS，深度优先)
    root->right=traverse(root->right);
    //后序遍历 res.push_back(root->val);   (DFS，深度优先)
    return root;
}
//2.翻转节点
TreeNode* invert(TreeNode* root){
    if(root==nullptr)
        return nullptr;

    TreeNode* tmp=root->left;
    root->left=invert(root->right);
    root->right=invert(tmp);

    return root;
}
//或者是这种写法，一样的
TreeNode* mirror(TreeNode* root){
    if(root==nullptr)
        return nullptr;
    TreeNode* left=mirror(root->left);
    TreeNode* right=mirror(root->right);
    root->right=left;
    root->left=right;
    return root;
}
```

#### 题目

备注：生成树的一般都还要用到左右区间

- [654. 最大二叉树（medium）](https://leetcode-cn.com/problems/maximum-binary-tree/)
- [105. 从前序与中序遍历序列构造二叉树（medium）](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
  - 需要一个左右区间。
- [106. 从中序与后序遍历序列构造二叉树（Medium）](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
  - 需要一个左右区间。
- [226. 翻转二叉树（easy）](https://leetcode-cn.com/problems/invert-binary-tree/)
- [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)
- [450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)
- [701. 二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
- [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

### 如何表示一棵树？序列化

#### 模板

```c++
//将一棵树序列化为string
string serialize(TreeNode* root) {
    serial(root);
    return res;
}
string res;
void serial(TreeNode* root){
    if(root==nullptr){
        res=res+"#,";
        return;
    }
    serial(root->left);
    serial(root->right);
    res=res+ to_string(root->val)+",";
}
```

#### 题目

- [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)
- [652. 寻找重复的子树](https://leetcode-cn.com/problems/find-duplicate-subtrees/)
  - 还需要额外的unordered_map

### 左右区间类\生成树类（已知前序、后序的遍历vector）

#### 模板

结束条件永远都是le>ri或者是le>=ri

```c++
//1.构造二叉树
TreeNode* build(vector<int>& preorder, int preStart, int preEnd, vector<int>& inorder,int inStart, int inEnd){

}

//2.验证二叉搜索树的后续遍历序列
bool verify(vector<int>& postorder,int le,int ri){
}
```

#### 题目

- [105. 从前序与中序遍历序列构造二叉树（medium）](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
  - 需要两个左右区间。
- [106. 从中序与后序遍历序列构造二叉树（Medium）](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
- [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)
  - 这题目有点像利用中序和后续遍历来重建二叉树的思路。需要一个左右区间。但是不必返回`listNode*`类型
- [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

### 罕见的的两个递归函数（一个递归遍历，另一个辅助函数递归完成任务）

备注：按照结题经验来看，很少有双递归函数。如果你的思路是双递归函数，那么尝试着使用后续遍历，在后序递归遍历过程中完成辅助函数该做的事。如果怎么都不行，那就用双递归吧。

双递归：一个递归函数完成遍历，另一个另一个辅助函数递归完成任务(判断)

#### 题目

- [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)
- [572. 另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)
- [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)
- 

### 非递归纯迭代（分情况讨论）

#### 题目

- [剑指 Offer 8-二叉树的下一个节点（分情况讨论，非递归）](https://www.nowcoder.com/questionTerminal/9023a0c988684a53960365b889ceaf5e)
- [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先（迭代）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)
- [117. 填充每个节点的下一个右侧节点指针 II](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)
  - 不太寻常的层次遍历，因为数据类型里有next指针，可以进行特殊优化
- 

## 二叉搜索树

### 升序、降序遍历

#### 模板

```c++
//1.中序递归，升序遍历
void traverse(TreeNode* root) {
    if (root == null) return;
    traverse(root->left);
    // 中序遍历代码位置
    print(root->val);
    traverse(root->right);
}
//2.降序遍历
void traverse(TreeNode* root) {
    if (root == null) return;
    // 先递归遍历右子树
    traverse(root->right);
    // 中序遍历代码位置
    print(root->val);
    // 后递归遍历左子树
    traverse(root->left);
}
```

#### 题目

- [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)
  - 升序递归
- [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)
  - 降序递归
- [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)
  - 降序递归
- [530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)
  - 升序递归
  - 用一个pre记录上一个节点值，从而避免vector来保存遍历数组
- [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)
  - 升序递归
  - 用一个pre节点来记录上一个节点，从而避免vector来保存遍历数组

### 针对二叉搜索树的通用递归框架（利用BST的性质）

#### 模板

```c++
//返回void
void BST(TreeNode* root,int target){
    if(root==nullptr)
        return;
	if(root->val==target)
        //找到目标，做点什么
    if(root->val<target)//当前值小于目标，那么去右子树找
        BST(root->right,target);
    if(root->bal>target)//当前值大于目标，那么去左子树找
        BST(root->left,target);
}

//返回TreeNode*
TreeNode* BST(TreeNode* root,int target){
    if(root==nullptr)
        return nullptr;
	if(root->val==target)
        //找到目标，做点什么
    {
        return ...;
    }
    if(root->val<target)//当前值小于目标，那么去右子树找
        root->right=BST(root->right,target);
    if(root->val>target)//当前值大于目标，那么去左子树找
        root->left=BST(root->left,target);
    return root;
}
```

#### 题目

- [700. 二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

### 二叉搜索树的验证、查、增（改）、删

- [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)
  - 比较复杂。不是常规套路
  
  - 扩充参数列表
  
  - ```c++
    class Solution {
    public:
        bool isValidBST(TreeNode* root) {
            return  isValid(root,nullptr,nullptr);
        }
    
        bool isValid(TreeNode* root,TreeNode* min,TreeNode* max){
            if(root==nullptr)
                return true;
            if(max!=nullptr && root->val >=max->val) return false;//左子树上的值大于节点值了，返回false
            if(min!=nullptr && root->val <=min->val) return false;//右子树上的值小于节点值了，返回false
            return isValid(root->left,min,root) && isValid(root->right,root,max);
            //当前节点root的左子树最小值下界是之前记录的min（可能是nullptr或者是root的父节点），最大值上界是当前节点root。
            //当前节点root的右子树最小值下界是当前节点root，最大值上界是之前记录的max（可能是nullptr或者是root的父节点）。
        }
    };
    ```
- [700. 二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)
  
  - 针对BST的遍历框架
- [701. 二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
  
  - 插入，那肯定使用前文的`凡是要构造树、翻转节点、在树中删除、插入某个节点时，一定要用这种递归模板（返回TreeNode*）`
- [450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)
  
  - 备注：情况有点多，分情况讨论
  - 删除，那肯定使用前文的`凡是要构造树、翻转节点、在树中删除、插入某个节点时，一定要用这种递归模板（返回TreeNode*）`

### 树及二叉搜索树的经验

- 首先试图用最通用的常见递归框架来解，试着用单节点。单节点能完成任务，但是总觉得写不出来时，试着扩充参数列表（例如：int curSum，int& depth，回溯`vector<int>&`）
- 返回值类型能用void就void，void不行就int、bool等
- 像那些填充  每个节点的下一个右侧节点指针、对称的二叉树什么的，才会用到双节点。双节点很少用的到。
- 单递归完成不了时，试着用后序遍历，让递归过程完成辅助递归函数该完成的事。如果还是不行，那就双递归吧，一个递归遍历，另一个递归完成任务。
- 凡是题目已经给出遍历结果的（vector），一定要用左右区间
- 二叉搜索树的验证其实是扩充参数列表的写法；二叉搜索树的搜索是针对BST优化过的遍历；二叉搜索树的插入和删除都要返回`TreeNode*`

## 图

### 图的概念

- 面试笔试很少出现图相关的问题，就算有，大多也是简单的遍历问题，基本上可以完全照搬多叉树的遍历。
- 至于最小生成树，Dijkstra，网络流这些算法问题，他们当然很牛逼，但是，就算法笔试来说，学习的成本高但收益低，没什么性价比，不如多刷几道动态规划，真的。
- 带权重的图叫网络（不管有向无向）
- 使用邻接矩阵找任一顶点的所有“邻接点”（有边直接相连的顶点）
  - 通过for循环，对于无向图，矩阵是对称的，只要扫描一行，所有不是0的顶点，就是有边与它直接连的
  - 对于有向图，矩阵就可能不是对称的，除了扫描一行，还要扫描一列
- 使用邻接矩阵计算任一顶点的“度”。（从该点出发的边数为“出度”，指向该点的边数为“入度”）
  - 对于无向图，对应行（或者列）的非0元素的个数
  - 有向图，对应行非0元素的个数是”出度“；对应列非0元素的个数是”入度“。
- 使用邻接表计算任一顶点的“度”
  - 对于无向图：是的，只要访问该顶点的链表，数一数链表上串了多少边即可
  - 对于有向图：只能方便计算“出度”；需要构造“逆邻接表”（存指向自己的边）来方便计算”入度“

### 图的遍历（DFS+BFS）

每个顶点访问一遍，不能有重复的访问

#### 模板

**当有向图含有环的时候（或者是无向图）才需要** **`visited`** **数组辅助**，如果不含环，连 `visited` 数组都省了，基本就是多叉树的遍历。

- DFS

  - ```c++
    /* 多叉树遍历框架 */
    void traverse(TreeNode* root) {
        if (root == nullptr) return;
    
        for (TreeNode* child : root->children)
            traverse(child);
    }
    
    //当然neighbors是每个节点的邻接表。也可以说是子节点
    //图和多叉树最大的区别是，图是可能包含环的，你从图的某一个节点开始遍历，有可能走了一圈又回到这个节点。
    //所以，如果图包含环，遍历框架就要一个 visited 数组进行辅助：
    
    boolean[] visited;
    /* 图遍历框架：伪代码 */
    void traverse(Node* node, int s) {
        //已经访问过该node了，直接返回
        if (visited[s]==true) return;
        // 经过节点 s
        //对该node进行操作
        //..
        //该节点访问过了，标记为true
        visited[s] = true;
        
        //邻居递归遍历
        for(node* i:node->neighbors){
            traverse(i);
        }
    }
    
    /* 图遍历框架2：C++代码 */
    unordered_map<Node*,bool> mymap;//相当于visited
    void traverse(Node* node){
        //讲道理，node应该没有==nullptr的时候，除非整个图为空
        if(node==nullptr)
            return nullptr;
    
        //已经访问过该node了，直接返回
        if(mymap.count(node)!=0)
            return mymap[node];
    
    	//对该node进行操作
        //...
        //该节点访问过了，标记为true
        mymap[node]=true;
        
        //邻居递归遍历
        for(auto i:node->neighbors){
            traverse(i);
        }
    }
    ```

- BFS

  - 入队时访问
  
  - ```c++
    //伪代码
    void BFS(Vertex V)
    {
        visited[V]=true;
        Enqueue(V,Q);
        while(!IsEmpty(Q))
        {
            V=Dequeue(Q);//父节点出队
            for(V的每个邻接点W){//子节点访问,然后入队
           		if(!visited[W]){
                    visited[W]=true;
                    Enqueue(W,Q);
                }
            }
        }
    }
    
    //c++
    void BFS(){
        unordered_map<Node*,Node*> mymap;
        queue<Node*> q;
        q.push(node);
        while(!q.empty()){
            //int size=q.size();//把分层去了也行，没什么区别。不用区分分层.
            //for(int i=0;i<size;i++){
                Node* cur=q.front();//取出父节点
                q.pop();
                for(auto i:cur->neighbors){//对于父节点的所有邻接点（子节点）
                    if(mymap.count(i)==0){//如果这个邻接点（子节点）没有被访问过
                        ...//访问它
                        q.push(i);//子节点入队
                    }
                }
            //}
        }
    }
    ```

#### 题目

- [797. 所有可能的路径](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

- [133. 克隆图](https://leetcode-cn.com/problems/clone-graph/)

  - 无向图，其实就是双向图。需要visited。

- [1306. 跳跃游戏 III](https://leetcode-cn.com/problems/jump-game-iii/)

  - BFS，需要visited

- [1042. 不邻接植花](https://leetcode-cn.com/problems/flower-planting-with-no-adjacent/)

  - 所有节点遍历。非BFS或者DFS

  - ```c++
    class Solution {
    public:
        vector<int> gardenNoAdj(int n, vector<vector<int>>& paths) {
            vector<vector<int>> graph(n+1);//有 n 个花园，按从 1 到 n 标记。
            for(auto vec:paths){
                graph[vec[0]].push_back(vec[1]);//构建邻接表
                graph[vec[1]].push_back(vec[0]);
            }
            vector<int> res(n+1,0);//N个为0的初始点，初始化全部未染色
            for(int i=1;i<=n;i++){
                unordered_set<int> color{1,2,3,4};//当前节点可以染的颜色的候选集合
                for(auto w:graph[i]){//对于每个节点的邻居
                    color.erase(res[w]);//把已染过色的去除。如果key不存在，会返回0
                }
                res[i]=*(color.begin());//染色
            }
            res.erase(res.begin());//vector里多构造了一个元素
            return res;
        }
    };
    ```

### 拓扑排序（入度、出度）

- AOV(Activity On Vertex)网络
  - 在有向图里边，所有真实的活动是表现在顶点上的，顶点和顶点之间的有向边表示了两个活动之间的先后顺序。
- 拓扑序：如果图中从V到W有一条有向路径，则V一定排在W之前。满足此条件的顶点序列称为一个拓扑序。

- 获得一个拓扑序的过程就是拓扑排序
- AOV如果有合理的拓扑序，则必定是有向无环图（Directed Acyclic Graph,DAG)

#### 模板

```c++
//聪明的拓扑排序。将所有入度为0的顶点，放到另外的一个容器了，比如队列、堆栈、数组
//该算法也可以用来检测一个有向图是不是有向无环图
//入度表（BFS）法
void TopSort()
{
    for(图中的每个顶点V){
        if(Indegree[V]==0) 
            Enqueue(V,Q);//入度为0的顶点统统入队
    }
    while(!IsEmpty(Q))
    {
        V=Dequeue(Q);//从队里拿出来的，必定是入度为0的顶点
        输出V，或者记录V的输出序号;cnt++;//用来记录多少个顶点成功了
        //输出以后，把V从原图里抹掉，方法是将V的邻接点的入度都减1，就假装删掉了V。
        for(V的每个邻接点W)
        {
         	if(--indegree[W]==0)//将V的每个邻接点w的入度都减1，就假装删掉了V
                Enqueue(W,Q);//入度为0的顶点统统入队
        }
    }
    //while跳出来的时候
    if(cnt!=|V|)//如果跳出来的时候，不是所有的顶点都成功了，那就是有回路
        error("图中有回路")
}
//复杂度O(V+E)
```

#### 题目

- [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)
  - 原汁原味拓扑排序
- [210. 课程表 II](https://leetcode-cn.com/problems/course-schedule-ii/)
  - 在上一题基础上，多了一个vector，在出队时push_back到vector
- [997. 找到小镇的法官](https://leetcode-cn.com/problems/find-the-town-judge/)
  - 入度表和出度表

### 最短路径问题（一般是有权图，用不到）

#### 概念

- 在网络中，求两个不同顶点之间的所有路径中，边的权值之和最小的那一条路径。

- 单源最短路径问题：从某固定源点出发，求其到所有其他顶点的最短路径

  - （有向或者无向）无权图

  - （有向或者无向）有权图

- 多源最短路径问题：求任意两顶点间的最短路径

#### 题目

- **无权图的单源**最短路径算法
  - 按照路径长度递增（非递减）的顺序找出到各个顶点的最短路径，使用BFS
- **有权图的单源**最短路径(Dijkstra)
  - [743. 网络延迟时间](https://leetcode-cn.com/problems/network-delay-time/)
- **多源**最短路径算法(Floyd)
  - [743. 网络延迟时间](https://leetcode-cn.com/problems/network-delay-time/)
  - [1334. 阈值距离内邻居最少的城市](https://leetcode-cn.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)

### 最小生成树问题（从没用过）

- prim算法——让一棵小树长大
- Kruskal算法——将森林合并成树

## 并查集（我建议能用深搜dfs的，不用并查集，因为并查集背不下来）

### 并查集概念

- Union-Find 算法，也就是常说的并查集算法，主要是解决图论中**「动态连通性」**问题的
- 动态连通性其实可以抽象成给一幅图连线。比如下面这幅图，总共有 10 个节点，他们互不相连，分别用 0~9 标记：
  - ![image-20210513112136477](http://pichost.yangyadong.site/img/image-20210513112136477.png)
  - ![image-20210513112149028](http://pichost.yangyadong.site/img/image-20210513112149028.png)
- 对于节点i，它的父节点就是parent[i]。根节点的特性是parent[i]==i；
  - 对于并查集中的树节点，就是从0开始的整数。节点的值就代表了唯一的节点
- 并查集与N叉树的区别
  - 换句话说，N叉树中的node*表示一个节点；并查集是parent数组里的int下标来表示某一个节点
  - N叉树的`node* curNode->val`表示节点值；并查集中节点值还是这个int下标
  - N叉树中node里的children数组记录了本node节点的子节点；并查集中parent[i]记录了节点i的父节点。
  - 并查集更像是数，它是N叉的，并且单向，但是指向是从子节点指向父节点；并且并查集中，可能有多棵互不连通的N叉树；而N叉树一定是指一颗树。
- 图与N叉树的区别
  - 备注：图中的节点一般不用`node*`来表示了，一般用`vector<vector<int>> graph(N);`来表示一张图的邻接表，其实也就唯一表示了一张图
    - 做题时一般是题目给出`vector<vector<int>>& edges`来表示边的关系，然后人工构造成图。
  - N叉树中的`node*`表示一个节点；图中的节点也是从0开始的整数，graph的int下标来表示某一个节点；
  - N叉树中的`node* curNode->val`表示节点值；图中的某个节点的值就是那个int下标；
  - N叉树中node里的children数组记录了本node节点的子节点；图中的graph[i]记录了本节点i的邻居；
  - N叉树其实是单向图，因此不需要visited来记录。而graph是双向图，需要visted来记录是否已经访问过
- 并查集与图的区别
  - 图中的节点也是从0开始的整数，graph的int下标来表示某一个节点；并查集是parent数组里的int下标来表示某一个节点
  - 图中的某个节点的值就是那个int下标；并查集中节点值也是int下标
  - 图中的graph[i]记录了本节点i的邻居；并查集中parent[i]记录了本节点i的父节点。
  - 备注：
    - **并查集一般是用来解决图的态连通性问题的!**
- ![image-20210513121122032](http://pichost.yangyadong.site/img/image-20210513121122032.png)

### 经典并查集

#### 并查集通用模板

```c++
class UF {
public:
    // 连通分量个数（连通域的数目）
    int cnt;
    // 存储一棵树。它表达的树是N叉树，并且用了路径压缩之后，该树最大只有3层。树高<=3。节点只能是从0开始的整数
    vector<int> parent;
    // 记录树的“重量”。只有作为root的节点才有资格使用size[root]，普通节点的size没有意义
    vector<int> size;//用于平衡性优化，小树接到大树下面
public:
    //构造并查集
    UF(int n) {
        cnt = n;
        parent = vector<int>(n,0);
        size = vector<int>(n,0);
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }
	
    //将节点 p 和 q 联合
    void unionNode(int p, int q) {//将一个集合（树）的根结点连到另一个集合（树）的根结点上
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ)
            return;

        // 小树接到大树下面，较平衡
        if (size[rootQ] < size[rootP]) {
            parent[rootQ] = rootP;
            size[rootP] += size[rootQ];
        } else {
            parent[rootP] = rootQ;
            size[rootQ] += size[rootP];
        }
        cnt--;//又有两个连通域合并为一个了，因此连通分量数目减一
    }
	
    //判断节点 p 和 q 是否连通
    bool connected(int p, int q) {//两个节点所在树的根节点相同（其实就是都在同一颗树上），即这两个节点连通
        int rootP = find(p);
        int rootQ = find(q);
        return rootP == rootQ;
    }
    
	//返回某个节点 x 所在的树的根节点 
    int find(int x) {//查根节点的时候顺带完成路径压缩
        while (parent[x] != x) {
            // 进行路径压缩（压缩每棵树的高度，使树高始终保持为常数）
            parent[x] = parent[parent[x]];//这个步骤决定了该树最大只有3层
            x = parent[x];
        }
        return x;
    }
    
	//返回当前的连通分量个数
    int count() {
        return cnt;
    }
};
```

- Union-Find 算法的复杂度可以这样分析：
  - 构造函数初始化数据结构需要 O(N) 的时间和空间复杂度；
  - 如果不使用size进行平衡性优化、也不使用路径压缩：那么`find`,`union`,`connected`的时间复杂度都是 O(N)。因为树最差会构建成链表
  - 如果使用size进行平衡性优化，而不使用路径压缩：`find`,`union`,`connected`的时间复杂度都为 O(logN)，因为树高没有限制，树高是O(logN)级别。
  - 当使用了size进行平衡性优化和路径压缩之后：找到某一节点的根节点`find`、连通两个节点`union`、判断两个节点的连通性`connected`、计算连通分量`count`所需的时间复杂度均为 O(1)。因为树高是常数级，最大是3
- **既然有了路径压缩，**`size` **数组的重量平衡还需要吗**？
  - 可以没有size数组重量平衡，但是有的话效率更高。

#### 题目

- [990. 等式方程的可满足性](https://leetcode-cn.com/problems/satisfiability-of-equality-equations/)
  - 一眼看出使用并查集来解决
- [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)
  - 其实跟[130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)非常相似

解题核心在于**适时增加虚拟节点，想办法让元素「分门别类」，建立动态连通关系**。

- [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)
  - 解法1：DFS
    - 解决这个问题的传统方法也不困难，先用 for 循环遍历棋盘的**四边**，用 DFS 算法把那些与边界相连的 `O` 换成一个特殊字符（DFS），比如 `#`；然后再遍历整个棋盘，把剩下的 `O` 换成 `X`，把 `#` 恢复成 `O`。这样就能完成题目的要求，时间复杂度 O(MN)。
  - 解法2：并查集
    - 整个棋盘就是个并查集。你可以把那些不需要被替换的`O` 看成一个拥有独门绝技的门派，它们有一个共同祖师爷叫 `dummy`，这些 `O` 和 `dummy` 互相连通，而那些需要被替换的 `O` 与`dummy`不连通。
    - UF构造的时候，默认都是独立节点，即自己的parent就是自己。然后将边界上的O点与dummy连通。然后再扫描内部所有点，内部的O 与上下左右的 O 连通。如果内部的O与边界O近邻，那么会连通到dummy上。如果不紧邻，那么要么自己为伍，要么与别的O一起连接上了，反正跟dummy一定是不连通。所有的X都是自己为伍，自己就是一棵树。
- 

### 并查集的经验

- 使用 Union-Find 算法，主要是如何把原问题转化成图的动态连通性问题。要么能直接看出连通等价关系，要么就**适时增加虚拟节点，想办法让元素「分门别类」，人工营造出动态连通特性。**
- 另外，将二维数组映射到一维数组，利用方向数组 `d` 来简化代码量，都是在写算法时常用的一些小技巧，如果没见过可以注意一下。
- 很多更复杂的 DFS 算法问题，都可以利用 Union-Find 算法更漂亮的解决。
- 能用DFS的，一般不选择使用并查集。并查集背不下来

## 回溯法(DFS、暴力穷举、找出一条路径或者找出所有路径)

### 概念

- **解决一个回溯问题，实际上就是一个决策树的遍历过程**。
- **核心就是 for 循环里面的递归，在递归调用之前「做选择」，在递归调用之后「撤销选择」**，
- 时间复杂度都不可能低于 O(N!)，因为穷举整棵决策树是无法避免的。**这也是回溯算法的一个特点，不像动态规划存在重叠子问题可以优化，回溯算法就是纯暴力穷举，复杂度一般都很高**。
- **我们定义的** **`backtrack`** **函数其实就像一个指针，在这棵决策树上游走，同时要正确维护每个节点的属性，每当走到树的底层，其「路径」就是一个全排列**。
- **写** **`backtrack`** **函数时，需要维护走过的「路径」和当前可以做的「选择列表」，当触发「结束条件」时，将「路径」记入结果集**。

### 经典回溯（类似于数组回溯模板1）

#### 框架

- 伪代码

  - ```c++
    result = []
    def backtrack(路径, 选择列表):
        if 满足结束条件:
            result.add(路径)
            return;
        for 选择 in 选择列表:
    		// 排除不合法选择
    		if(!isValid(..))
                continue;
            //做选择
        	将该选择从选择列表移除
       	 	路径.add(选择)
                
            // 进入下一行决策 
            backtrack(路径, 选择列表)//在递归之前做出选择，在递归之后撤销刚才的选择
                
            //撤销选择
            路径.remove(选择)
        	将该选择再加入选择列表
    ```

- c++

  - ```c++
    void backtrack(vector<string>& board,int row){
        if(row==n){
            res.push_back(board);
            return;
        }
        for(int j=0;j<n;j++){//for 选择 in 选择列表:
            // 排除不合法选择
            if(!isValid(board,row,j))
                continue;
            //做出选择
            board[row][j]='Q';
            // 进入下一行决策
            backtrack(board,row+1);//在递归之前做出选择，在递归之后撤销刚才的选择
            //撤销选择
            board[row][j]='.';
        }
    }
    ```

- 函数变量里一定要带上遍历的下标。 行、列下标。因为N皇后一行只放一个数，因此只有行下标。解数独就得两个下标。

- 并不想得到所有合法的答案，只想要一个答案

  - ```c++
    bool backtrack(vector<string>& board,int row){
        if(row==n){
            res.push_back(board);
            return true;
        }
        for(int j=0;j<n;j++){//for 选择 in 选择列表:
            // 排除不合法选择
            if(!isValid(board,row,j))
                continue;
            //做出选择
            board[row][j]='Q';
            // 进入下一行决策
            if (backtrack(board, row + 1))//在递归之前做出选择，在递归之后撤销刚才的选择
                return true;
            //撤销选择
            board[row][j]='.';
        }
        return false;
    }
    ```

  - **有的时候，我们并不想得到所有合法的答案，只想要一个答案，怎么办呢**？比如解数独的算法，找所有解法复杂度太高，只要找到一种解法就可以。

  - 这样修改后，只要找到一个答案，for 循环的后续递归穷举都会被阻断。

#### 题目

- [46. 全排列](https://leetcode-cn.com/problems/permutations/)
  - O(n!)
  - 经典回溯模板
- [51. N 皇后](https://leetcode-cn.com/problems/n-queens/)
  - O(n^n)；因为每一行都有N种可能的放置方式，一共N行，需要去遍历查询，即`n*n*n*n*....*n=n^n`
  - 经典回溯模板
- [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

### 二叉树中的回溯

#### 模板

- 与其他的回溯不太一样

- ```c++
  void traverse(TreeNode* root,int curSum,int target,vector<int>& curVec){
          if(root==nullptr){//if 满足结束条件:
              curVec.push_back(0);
              return;
          }
      
          curSum+=root->val;
          curVec.push_back(root->val);
          if(root->left==nullptr && root->right==nullptr){//当前是叶子节点         
              if(curSum==target){
                  res.push_back(curVec);
              }
          }
          traverse(root->left,curSum,target,curVec);//做选择,并进入下一行决策
          curVec.pop_back();//撤销选择
          traverse(root->right,curSum,target,curVec);//做选择,并进入下一行决策
          curVec.pop_back();//撤销选择
      }
  ```

#### 题目

-  [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

  - ```c++
    class Solution {
    public:
        vector<vector<int>> pathSum(TreeNode* root, int target) {
            vector<int> curVec;
            traverse(root,0,target,curVec);
            return res;
        }
        vector<vector<int>> res;
        void traverse(TreeNode* root,int curSum,int target,vector<int>& curVec){
            if(root==nullptr){
                curVec.push_back(0);
                return;
            }
            curSum+=root->val;
            curVec.push_back(root->val);//前序遍历
            if(root->left==nullptr && root->right==nullptr){//当前是叶子节点         
                if(curSum==target){
                    res.push_back(curVec);
                }
            }
            traverse(root->left,curSum,target,curVec);//左
            curVec.pop_back();
            traverse(root->right,curSum,target,curVec);//右
            curVec.pop_back();
        }
    };
    ```

- [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

  - 跟上边那个题一样

- [37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

### 数组中的回溯

#### 模板

- **模板1：递归遍历数组，判断每个下标是否加入（其实就是经典回溯）**

  - 以数组nums中每个下标index角度出发，该下标index对应的值加入or不加入。

  - index就是数组的递归遍历

  - ```c++
    vector<vector<int>> res;
    void dfs(vector<int>& track,int index){
        if(track.size()==k){
            res.push_back(track);
            return;
        }
        if(index==n+1)
            return;
    	
        //相当于把两种情况的For循环拆开了，写成了无For的形式。就两种选择，这个节点加入或者是不加入
        track.push_back(index);//做选择
        dfs(track,index+1);
        track.pop_back();//撤销
        dfs(track,index+1);//做选择
    }
    ```

- **模板2：迭代遍历数组，递归下一个元素是谁（为数组适配）**

  - **推荐！**

  - 因为它跟之前的模板是一致的

  - for循环遍历以每个值为首，然后开始遍历它之后的值是否加入

  - （for从int i=0开始就可以重复使用元素了，从start开始就不能重复使用元素）

  - ```c++
    void dfs(vector<int>& track,int start){
        if(track.size()==k){
            res.push_back(track);
            return;
        }
        // 注意 i 从 start 开始递增。使用start 排除已经选择过的数字，相当于已经continue过了
        for (int i = start; i <= n; i++) {
            // 做选择
            track.push_back(i);
            dfs(track,i+1);//start设置为i+1。尤其注意是 i+1，不是start+1
            // 撤销选择
            track.pop_back();
        }
    }
    ```

#### 建议使用模板2的题目

- [78. 子集](https://leetcode-cn.com/problems/subsets/)
  - 两种模板都行。建议模板2
- [77. 组合](https://leetcode-cn.com/problems/combinations/)
  - 两种模板都行。建议模板2
- [46. 全排列](https://leetcode-cn.com/problems/permutations/)
  - 只能用模板二。
  - 因为模板一只能严格按照数组顺序遍历。并且，在本题下，每个元素都得加入，它没有选择的份。
  - 它特殊在总是for(int i=0；)正常情况应该是for(int i=start)的
- [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)
  - 数组中有重复元素，使用sort+`if(i>0 && nums[i-1]==nums[i] && valid[i-1]==true) continue;`去重
- [剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)
  - 跟[47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)一样
- [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)
  - 模板2。只不过start设置为了i而已
  - 类似[78. 子集](https://leetcode-cn.com/problems/subsets/)
- [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)
  - 数组中有重复元素，利用start去重
- [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)
  - 使用模板1非常方便！因为有非常明确的前后遍历关系。数组也一定是按从左到右遍历。
  - **使用模板2，当成组合问题，也比较简单。**
    - 类似[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)https://leetcode-cn.com/problems/combinations/)
- 切记**用了模板二写了for循环时，for循环里一定不要出现两次dfs**，即**不要把两个模板给杂揉起来，混合使用肯定不对**。
- 经验：模板2几乎万能，建议永远使用模板二
  - 用了start，就一定会去重。比如[1,2]跟[2,1]，用start就可以去重。不用start就不会去重，比如在全排列里，不需要去重（去重反而不对了），那么就不要用start了。
  - 当不需要排列时，即要使用start时：
    - 假如数组中元素可重复使用（比如[1,1,1,1]）：i=start
    - 假如数组中元素不可重复使用：i=start+1
  - 不需要去重时，即排列顺序不同是不同结果时，那么i总是从0开始。不要用start

#### 建议使用模板1的题目

- [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)
- [698. 划分为k个相等的子集](https://leetcode-cn.com/problems/partition-to-k-equal-sum-subsets/)

### 回溯法的经验

- 函数参数一般都有个`vector<int>& track`

- 函数参数把需要遍历的下标带上。可能是行列号、数组下标号等等

- 经常使用递归遍历数组的模板（其实就是数组回溯中的模板1）

  - ```c++
    //迭代遍历数组
    for (int index = 0; index < nums.size(); index++) {
        cout<<nums[index];
    }
    //递归遍历数组
    void traverse(vector<int> nums, int index) {
        if (index == nums.size()) {
            return;
        }
        cout<<nums[index];
        traverse(nums, index + 1);
    }
    ```

- 其实数组模板1跟通用经典回溯（比如N皇后、数独等）是一个思想。反而是数组模板二是专为数组而设计的另类模板，不过很好用。

  - 因此，**做数组的题，一般就用模板二。偶尔也可以使用模板一，即原始的回溯思想。**

- 在数组回溯中，没有意外就都用模板2:

  - 用了start，就一定会(将排列顺序)去重。比如[1,2]跟[2,1]只想保留一个时，用start就可以(将排列顺序)去重。不用start就不会(将排列顺序)去重，比如在全排列里，不需要去重（去重反而不对了），那么就不要用start了。
  - 当需要(将排列顺序)去重时，使用start即可：
    - 假如数组中元素可重复使用（比如[1,1,1,1]）：下一个start=i
    - 假如数组中元素不可重复使用：下一个start=i+1
  - 不需要(将排列顺序)去重时，即排列顺序不同是不同结果时，不要用start。那么i总是从0开始。

- **做这种数组的题：**

  - 默认使用模板二

  - 1.排列顺序是否保留？

    - 不保留，比如[1,2]跟[2,1]只保留一个，那么一定用start。例如：[78. 子集](https://leetcode-cn.com/problems/subsets/)、[77. 组合](https://leetcode-cn.com/problems/combinations/)
    - 保留，那么每轮i都从0开始，不用start。（这种情况下，一定会有个valid数组，以记录是否已经使用了）。例如：[46. 全排列](https://leetcode-cn.com/problems/permutations/)

  - 2.元素是否可以多次使用？

    - 不可以，下一个start=i+1，即`dfs(track,i+1);`。例如：[78. 子集](https://leetcode-cn.com/problems/subsets/)、[77. 组合](https://leetcode-cn.com/problems/combinations/)
    - 可以，下一个start=i，即`dfs(track,i);`。例如：[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

  - 3.数组中是否有重复元素？

    - 没有重复元素。什么都不做。例如：[78. 子集](https://leetcode-cn.com/problems/subsets/)、[77. 组合](https://leetcode-cn.com/problems/combinations/)、[46. 全排列](https://leetcode-cn.com/problems/permutations/)、[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

    - 有重复元素，将原始数组sort。然后根据是否用了start。

      - 1.用了start：  例如：[40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

        - ```c++
          for(int i=start;i<candidates.size();i++){
              if(i>0 && candidates[i]==candidates[i-1] && i!=start)
                  continue;
              ...
          }
          ```

      - 2.没用start：（那么肯定有个valid数组）  例如：[47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)、[剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

        - ```c++
          for(int i=start;i<candidates.size();i++){
          	if(i>0 && candidates[i]==candidates[i-1] && valid[i-1]==true)
              	continue;
              ...
          } 
          ```

- 经常需要人工想一个for循环出来。人工构造一个for，通常比傻傻的全遍历写的更简洁一点。

  - 比如：N皇后中的，以每一行为index。for每个列坐标是否放置。
    - 这就比for 每个row,col坐标是否放置更简洁
  - 比如：[93. 复原 IP 地址](https://leetcode-cn.com/problems/restore-ip-addresses/)。以每个点为index。for长度1到3尝试切割一段长度。
    - 这就比for每个点index来切割更简洁。

## BFS 算法（暴力穷举，找最短路径问题）

### BFS概念

- BFS 的核心思想应该不难理解的，就是把一些问题抽象成图，从一个点开始，向四周开始扩散。一般来说，我们写 BFS 算法都是用「队列」这种数据结构，每次将一个节点周围的所有节点加入队列。
- BFS 相对 DFS 的最主要的区别是：**BFS 找到的路径一定是最短的，但代价就是空间复杂度比 DFS 大很多**，至于为什么，我们后面介绍了框架就很容易看出来了。
- BFS 出现的常见场景好吧，**问题的本质就是让你在一幅「图」中找到从起点 `start` 到终点 `target`的最近距离**这个例子听起来很枯燥，但是 BFS 算法问题其实都是在干这个事儿
- 这个广义的描述可以有各种变体
  -  比如走迷宫，有的格子是围墙不能走，从起点到终点的最短距离是多少
  -  比如说两个单词，要求你通过某些替换，把其中一个变成另一个，每次只能替换一个字符，最少要替换几次
  -  比如说连连看游戏，两个方块消除的条件不仅仅是图案相同，还得保证两个方块之间的最短连线不能多于两个拐点。
- 一般来说在找最短路径的时候使用 BFS，其他时候还是 DFS 使用得多一些（主要是递归代码好写）

### BFS经典框架

#### 框架

- ```c++
  // 计算从起点 start 到终点 target 的最近距离
  int BFS(Node* start, Node* target) {
      Queue<Node*> q; // 核心数据结构
      Set<Node*> visited; // 避免走回头路
  
      q.push(start); // 将起点加入队列
      visited.insert(start);
      int step = 0; // 记录扩散的步数
  
      while (!q.empty()) {
          int sz = q.size();
          /* 将当前队列中的所有节点向四周扩散 */
          for (int i = 0; i < sz; i++) {
              Node* cur = q.front();
              q.pop();
              /* 划重点：这里判断是否到达终点 */
              if (cur is target)
                  return step;
              /* 将 cur 的相邻节点加入队列 */
              for (Node* x : cur->adj())
                  if (x not in visited) {
                      q.push(x);
                      visited.push(x);
                  }
          }
          /* 划重点：更新步数在这里 */
          //树的一层遍历完了；或者说是图的一圈遍历完了
          step++;
      }
  }
  ```

#### 题目

  - [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
  - [752. 打开转盘锁](https://leetcode-cn.com/problems/open-the-lock/)
    - 其实就是暴力穷举。
    - 想办法构造成BFS问题
  - [773. 滑动谜题](https://leetcode-cn.com/problems/sliding-puzzle/)
    - 构造为BFS问题还需要一些技巧。
    - **其中比较有技巧性的点在于，二维数组有「上下左右」的概念，压缩成一维后，如何得到某一个索引上下左右的索引**？

## 数组-二分查找

### 原始二分查找+左右边界二分查找

#### 框架

- 最原始的二分查找，数组中没有重复的，或者是返回重复数字的任意一个下标就行

  - 脑补一下移动规则：

  - ```c++
    int search(vector<int>& nums, int target) {
        int left=0,right=nums.size()-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target)
                return mid;//直接return
            else if(nums[mid]>target)
                right=mid-1;
            else if(nums[mid]<target)
                left=mid+1;
        }
        return -1;
    }
    ```

- 寻找左侧边界的二分搜索

  - 脑补一下移动规则：

    - target比数组里所有数都小时，那么肯定是右边界移动到左边界-1位置，左边界不动
    - target比数组里所有数都大时，那么肯定是左边界移动到右边界+1位置，右边界不动

  - 注意：

    - return -1的情况，一定要按下边这样判断。包含了3中情况：target比nums里的数都大 || target比nums里的数都小 或者是 target这个数不在数组里。
    - 比如12346789。查询5，5不在这里边，一定是用nums[left]!=target来判断出来的。
    - 备注：left>=nums.size()你也可以理解为防止nums[left]越界。left永远不可能<0，因此不用担心这个问题

  - ```c++
    int findleft(vector<int>& nums, int target)
    {
        int left=0,right=nums.size()-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target)
                right=mid-1;//因为要找左边界，找到了也要继续找左半边
            else if(nums[mid]>target)
                right=mid-1;
            else if(nums[mid]<target)
                left=mid+1;
        }
        if(left>=nums.size()|| nums[left]!=target)//target比nums里的数都大 || target比nums里的数都小 或者是 target这个数不在数组里
            return -1;
        return left;
    }
    ```

- 寻找右侧边界的二分搜索

  - 备注：right<0你也可以理解为防止nums[right]越界。right永远不可能>=nums.size()，因此不用担心这个问题

  - ```c++
    int findright(vector<int>& nums, int target)
    {
        int left=0,right=nums.size()-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target)
                left=mid+1;//因为要找右边界，找到了也要继续找右半边
            else if(nums[mid]>target)
                right=mid-1;
            else if(nums[mid]<target)
                left=mid+1;
        }
        if(right<0 || nums[right]!=target)//target比数组里所有的数都大时 || (target比数组里所有的数都小 或者 target这个数不在数组里)
            return -1;
        return right;
    }
    ```

#### 题目

- [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)
  - 最原始的二分查找，数组中没有重复的，或者是返回重复数字的任意一个下标就行
- [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
  - 寻找左边界的二分查找+寻找有边界的二分查找
- [875. 爱吃香蕉的珂珂](https://leetcode-cn.com/problems/koko-eating-bananas/)
  - 相当于是寻找左边界的二分查找，把>=的情况合二为一，即条件成真。并且没有返回-1的情况，要查询的值一定存在
  - 难点在于，确定left和right的值。以及那个bool函数
- [1011. 在 D 天内送达包裹的能力](https://leetcode-cn.com/problems/capacity-to-ship-packages-within-d-days/)
  - 相当于是寻找左边界的二分查找，把>=的情况合二为一，即条件成真。并且没有返回-1的情况，要查询的值一定存在
  - 难点在于，确定left和right的值。以及那个bool函数
- [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)
  - 两个二分查找
- [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)
  - 当做二叉搜索树来做
- [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)
  - 左右边界的二分搜索
- [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)
  - 普通的对称二分，自行判断退出while时返回left还是right
- [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)
  - 普通二分
- 补充题：x的平方根：要求精确到1e-10

### 旋转数组二分（它也属于不对称拉）

#### 框架

- 旋转数组的最小数字

  - ```c++
    int minArray(vector<int>& numbers) {
        int left=0,right=numbers.size()-1;
        while(left<right){//注意：不是left<=right
            int mid=left+(right-left)/2;
            if(numbers[mid]>numbers[right])
                left=mid+1;//此时的mid一定不是要查询的值
            else if(numbers[mid]<numbers[right])
                right=mid;//注意：不是mid-1。为什么？因为比如[4,5,1,2,3]。mid可能就是最小值啊，不能错过它
            else if(numbers[mid]==numbers[right])
                right--;//用于[21222]和[22212]这种无法区分的情况
        }
        return numbers[left];//left、right一样
    }
    ```

  - 为什么是比较right不是left？

    - 因为这是两段递增序列，比较left复杂啊。比如[1,2,3,4,5]和[2,3,4,5,1]的情况。numbers[mid]同样是>numbers[left]，但是最小值却出现在了mid的两个方向，这就不好缩小区间。
    - 比较right就简单，没有这个分歧。numbers[mid]>numbers[right]，mid一定是在左半递增序列上；numbers[mid]<numbers[right]，mid一定是在右半递增序列上或者真个数组就只有一段递增序列

- 

#### 题目

- [剑指 Offer 11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)
  - 最小值有多个时：   [11121]：返回下标0。[12111]：返回下标2
    - 欲想通过这种手段来拆解两段递增序列，就失败了
    - 特别注意！
  - 最小值就一个时： [22212]、[21222]都会返回正确下标
- [154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)
  - 跟上边那个题一样
- 旋转数组的中位数
  - 先找最小值的下标，再进行位移就行。
  - [11121]：返回下标0。[12111]：返回下标2 并不会产生影响。因为这种情况时最小值的个数够多，占了一半以上，因此不管是返回下标0还是后半递增序列的开头，不重要了。
- [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
  - 套用旋转数组的框架，再补充一些if else细节
- [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)
  - 套用旋转数组的框架，再补充一些if else细节

### 不对称拉的二分模板（其实旋转数组的二分也属于不对称二分）[求一个先升序后降序得数组的最大值]

#### 框架

```c++
//补充题：求一个先升序后降序得数组的最大值
//left和right一个拉到mid一个拉到mid+1或者-1
int binarySearchPeak(vector<int>& nums){
    int left=0,right=nums.size()-1;
    while(left<right){//注意：不是left<=right
        int mid=left+(right-left)/2;
        if(nums[mid]>nums[mid+1])
            right=mid;//注意：不是mid-1。mid可能就是要找寻的值，不能错过它
        else if(nums[mid]<nums[mid+1])
            left=mid+1;
        else{
            if (nums[left] > nums[right]) //[2,2,3,2,2,2,2,2,2,2,2,2,2,2,2,2]有大用，防止3被错过
                right--;
            else
                left++; //[2,2,2,2,2,2,2,2,2,2,2,2,2,3,2,2]，防止3被错过
        }
    }
    return nums[left];
}

//使用这种模板经常在退出时区间里剩俩数。需要自己判断一下是left还是right才是想要的。
//还经常需要if(left==mid) break;以防止区间内只剩两个数时，无限死循环
```

#### 题目

- 补充题：求一个先升序后降序得数组的最大值
- [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)
- [1095. 山脉数组中查找目标值](https://leetcode-cn.com/problems/find-in-mountain-array/)

### 递归二分（两个有序数组第k小的数、寻找两个正序数组的中位数。非常规套路，建议直接背下来）

#### 框架

```c++
//在两个有序数组中找到第k小的元素（例如找第一小的元素，k=1，即nums[0]）
//i: nums1的起始位置 j: nums2的起始位置（i，j都是从0开始）
int findKth(vector<int>& nums1, int i, vector<int>& nums2, int j, int k)
{ 
    //若nums1为空（或是说其中数字全被淘汰了）
    //那么很简单，直接在nums2中找第k个元素返回即可。此时nums2起始位置是j，所以是j+k-1
    if(i>=nums1.size()) return  nums2[j+k-1];
    //nums2为空同理
    if(j>=nums2.size()) return nums1[i+k-1];

    //递归出口。找第一小的元素
    if(k == 1)  return std::min(nums1[i], nums2[j]);
    //这两个数组的第K/2小的数字，若不足k/2个数字则赋值整型最大值，以便淘汰另一数组的前k/2个数字
    int midVal1=((i+k/2-1)<nums1.size())?nums1[i+k/2-1]:INT_MAX;
    int midVal2=((j+k/2-1)<nums2.size())?nums2[j+k/2-1]:INT_MAX;

    if(midVal1<midVal2)//淘汰掉nums1中前k/2个数
        return findKth(nums1, i+k/2, nums2,j, k-k/2);
    else
        return findKth(nums1, i, nums2,j+k/2, k-k/2);
}
```

#### 题目

- 补充题17. 两个有序数组第k小的数
  - 递归二分
  - 每次排除k/2个数
- [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)
  - 使用一个小trick，可以避免讨论奇偶：
    - 我们分别找第 (m+n+1)/2个数，和(m+n+2)/2个数，然后求其平均值即可，这对奇偶数均适用。假如 m+n 为奇数的话，那么其实 (m+n+1) / 2 和 (m+n+2) / 2 的值相等，相当于两个相同的数字相加再除以2，还是其本身。
  - 查询第(m + n + 1) / 2小的数和第(m + n + 2) / 2小的数，然后取平均。
  - 即执行两次上边那个题目，然后求平均。

### 二分查找的经验

- 都是左闭右闭的区间
- 如果是**标准的单调排序数组**，考虑**最原始的对称二分查找模板**
  - 即`while(left<=right)`并且大概率在while内部进行return
  - 这种写法下，一般就是left和right每次拉到mid+1或者mid-1。不会拉到mid
  - **自行判断退出时，left和right哪个是想要的（在纸上画一下就知道了，不需要代码判断）**
- **不单调的排序数组（比如旋转数组，先递增又递减数组等等）**，考虑**不对称的二分查找模板**
  - `while(left<right)`，只在while外return。内部不return
  - 自行判断left和right每次拉到mid还是mid+1或者mid-1的位置。这取决于mid位置是否有可能就是正确值。
    - 当mid一定不是正确值时，就可以+1或者-1来跳过它。如果不能保证，那么就不能跳过它，只能拉到mid处。
  - left和right经常一个拉到mid一个拉到mid+1或者-1。
    - 就一定得注意经常需要判断`if(mid==left) break;`以防止区间内只剩两个数时，无限死循环
  - 跳出while时，区间还有两个数没有判断,需要代码来判断返回left还是right。
  - **并且在while外需要代码来判断到底返回left还是right**

- 其实有的时候，对称和不对称的做法都可行。但是我**建议优先考虑对称的写法**
  - **因为原始的对称模板代码边界处理起来简单（不用担心区间内只剩两个数时，无限死循环）。对称模板必可以自行退出while，并且不是返回left就是right，自己在纸上一画就知道了**
  - 不对称的模板思想更简单。但是经常需要人工补充`if(mid==left) break;`以及退出while时再额外判断区间内两个数，需要代码来判断返回left还是right。
- **二分法还可以用于确定一个有范围的整数**
  - 比如：
    - [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)
    - [[287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)
- 递归二分
  - 非常罕见。只用过那两个题。
    - 补充题17. 两个有序数组第k小的数
    - [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)
- 重点：
  - 都是左闭右闭的区间
  - **二分法还可以用于确定一个有范围的整数**
  - **标准的单调排序数组**，考虑**最原始的对称二分查找模板**
    - 最经典
    - 一般的单调数组都用这个方法
  - **不单调的排序数组（比如旋转数组，先递增又递减数组等等）**，考虑**不对称的二分查找模板**
    - 先升序后降序的数组最大值
  - 递归二分非常罕见
    - 两个有序数组的第k小问题
  - **二分查找，乘以个符号，本来处理升序的，就可以处理降序了**
  - 中位数的奇偶trick
    - 我们分别找第 (m+1)/2个数，和(m+2)/2个数，然后求其平均值即可，这对奇偶数均适用。假如 m 为奇数的话，那么其实 (m+1) / 2 和 (m+2) / 2 的值相等，相当于两个相同的数字相加再除以2，还是其本身。

## 数组-左右指针（二分其实就是双指针的一种）

- 这种题目比较简单，一般也是有序数组。
  - 一个left，一个right指针，然后++，--什么的就完事了

## 数组-滑动窗口（最小覆盖子字符串、无重复字符的最长子串、滑动窗口最大值）

### 经典滑动窗口

#### 框架

- 原始框架

  - ```c++
    /* 滑动窗口算法框架 */
    void slidingWindow(string s, string t) {
        unordered_map<char, int> need, window;
        for (char c : t) need[c]++;
    
        int left = 0, right = 0;
        int valid = 0; 
        while (right < s.size()) {
            // c 是将移入窗口的字符
            char c = s[right];
            // 右移窗口
            right++;
            
            //-----进行窗口内数据的一系列更新-------
            ...//！重要！需要自己填！
            //通常是if(need.count(c)==1)//如果是有效字符
             window[c]++;
    		//---------------------------------
                
            /*** debug 输出的位置 ***/
            printf("window: [%d, %d)\n", left, right);
            /********************/
    
            // 判断左侧窗口是否要收缩
            while (window needs shrink) {//！重要！需要自己填！
                //-----大概率在这里记录一些可能需要返回的东西，但是并不是永远都在这里------
                ...//！重要！需要自己填！
                //通常是if(..) res.push_back(..);
                //--------------------------------
                    
                // d 是将移出窗口的字符
                char d = s[left];
                // 左移窗口
                left++;
                
                //-----进行窗口内数据的一系列更新-------
                 window[d]--;
                ...//！重要！需要自己填！  一般来说跟上边是对称的
                //---------------------------------
            }
        }
    }
    ```

- 不用担心left会收过right，因为`while (window needs shrink) `保证了，肯定不会收过头。

- 结束时，一定是right到了字符串结尾，然后[left,right)不满足要查找的内容

- 右移和左移窗口更新操作，它们的操作是完全对称的。

#### 题目

- [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

  - ```c++
    // 判断左侧窗口是否要收缩
    while (valid == need.size()) {
        //-----记录一些可能需要返回的东西------
        if (right - left < len) {
            start = left;
            len = right - left;
        }
        ...
    }
    ```

- [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

  - ```c++
    // 判断左侧窗口是否要收缩
    while(right-left>=s1.size()){//本题移动 left 缩小窗口的时机是窗口大小大于 t.size() 时，应为排列嘛，显然长度应该是一样的。
           //-----记录一些可能需要返回的东西------
        if(valid==need.size())//当发现 valid == need.size() 时，就说明窗口中就是一个合法的排列，所以立即返回 true。
            return true;
        ...
    } 
    ```

- [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

  - 同上题

- [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)、[剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

  -  判断左侧窗口是否要收缩换形式了
  - 并且更新返回值的地方也换了
  - 自己思考一下就能想出来

- [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

  - 滑动窗口+暴力法  O(nk)  使用mutiset
  - 滑动窗口+大顶堆   O(nlogn)
  - 滑动窗口+单调队列  O(n)

- [480. 滑动窗口中位数](https://leetcode-cn.com/problems/sliding-window-median/)

  - 滑动窗口+暴力法  使用mutiset

- [1438. 绝对差不超过限制的最长连续子数组](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

  - 滑动窗口+暴力法  使用mutiset
  - 滑动窗口+两个单调队列
  
- [1498. 满足条件的子序列数目](https://leetcode-cn.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/)

  - 排序+双指针（也可以叫做滑动窗）
  - 这里边的pow避免溢出的思想非常重要
    - 主要就是取模运算的
  
- [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

  - 滑动窗+暴力法
  - 滑动窗+维护历史最大窗口

### 注意事项

- 当`right++` 扩大窗口时，即加入字符时，应该更新哪些数据？

  - 如果一个字符进入窗口，应该增加 `window` 计数器，即 `char c=s[right]; window[c]++;`;

- 什么条件下，窗口应该暂停扩大，开始移动 `left` 缩小窗口？

  - 即判断左侧窗口是否要收缩的条件。这是最核心和重要的

- 当 `left++` 缩小窗口，即移出字符时，应该更新哪些数据？

  - 如果一个字符将移出窗口的时候，应该减少 `window` 计数器。即`char d=s[left]; window[d]--;`;

- 我们要的结果应该在扩大窗口时还是缩小窗口时进行更新？

  - 可能在扩大窗口时、可能在缩小窗口时、可能在在收缩窗口完成后


### 经验

- 右移和左移窗口更新操作，它们的操作是完全对称的。

  - 注意`window[c]++`;和 `window[d]--;`的位置，一般是对称反过来的

- 对于窗口长度是固定的

  - [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

  - [480. 滑动窗口中位数](https://leetcode-cn.com/problems/sliding-window-median/)

  - ```c++
    while (right < s.size()) {
        ...
        while(right-left>=k){//写成这样就可以保证窗口长度固定为k！
            //收缩左边界
        }
        ...
    }
    ```

- 对于窗口只增不减的

  - [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

  - ```c++
    while (right < s.size()) {
        ...
        if(right-left>=k){//写成这样就可以保证窗口只增不减！
            //收缩左边界
        }
        ...
    }
    ```

- 窗口长度不定的

  - [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

    - **维护历史最大窗口**来优化

  - ```c++
    while (right < s.size()) {
        ...
        while(某个条件){//写成这样就可以窗口不定！
            //收缩左边界
        }
        ...
    }
    ```

- 当找窗口的最大、最小、中位数、绝对值的差

  - 要么暴力法，使用mutiset（排序集合）

    - O(kn)

    - ```c++
      //每次erase  left那个数
      int del=nums[left];
      myset.erase(myset.find(del));
      ```

  - 要么**单调队列**

    - O(n)

    - ```c++
      //入队操作
      while (!data.empty() && data.back() < n){//单调队列的 push 方法依然在队尾添加元素，但是要把前面比新元素小的元素都删掉：你可以想象，加入数字的大小代表人的体重，把前面体重不足的都压扁了，直到遇到更大的量级才停住。
          data.pop_back();
      }
      data.push_back(n);
      
      //出队操作
      if (!data.empty() && data.front() == n){//之所以要判断 data.front() == n，是因为我们想删除的队头元素 n 可能已经被「压扁」了，这时候就不用删除了：
          data.pop_front();
      ```

  - 不建议使用优先队列

    - 啥也不是，  O(n logn)
    - 用的话记得考虑把index下边也一起pair存到优先队列里

## 数组-去重算法（单调栈）

#### 使用单调栈

```c++
//push操作
while(!stk.empty() && stk.top()>c){
    stk.pop();
}
stk.push(c);
```

#### 题目

- [316. 去除重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/)

## 单调队列（常用于滑动窗口）

### 普通的单调队列

#### 模板

```c++
//入队操作
while (!data.empty() && data.back() < n){//单调队列的 push 方法依然在队尾添加元素，但是要把前面比新元素小的元素都删掉：你可以想象，加入数字的大小代表人的体重，把前面体重不足的都压扁了，直到遇到更大的量级才停住。
    data.pop_back();
}
data.push_back(n);

//出队操作
if (!data.empty() && data.front() == n){//之所以要判断 data.front() == n，是因为我们想删除的队头元素 n 可能已经被「压扁」了，这时候就不用删除了：
    data.pop_front();
```

#### 题目

- [1438. 绝对差不超过限制的最长连续子数组](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)
- [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

### 经验

- 常用于滑动窗口

## 单调栈（下一个最大、字典序）

### 普通的单调栈

- 你可以想象，加入数字的大小代表人的体重，**把栈下边体重不足的都压扁了**，直到遇到更大的量级才停住。
  - （跟单调队列一个思想，**单调队列是把队列前边体重不足的都压扁了**，直到遇到更大的量级才停住）
- 当然也可以是体重小的把体重大的都压走，直到遇到更小的。

#### 模板

栈里有时候存val，有时候存Index。不同题存的东西不一样

```c++
//入栈
while(!stk.empty() && stk.top()<=cur)//<=还是<，需要自己多判断
    stk.pop();
stk.push(cur);

//出栈
一般来说不出栈（出栈就是普通出栈即可）
```

#### 题目

- [496. 下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)
  - 单调栈里存的是val
- [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)
  - 单调栈里存的是index
- [503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)
  - 循环数组，那么就把数组扩大一倍。
  - 使用取余的操作来假装扩大一倍

### 经验

- **下一个最大、字典序** 等等关键词，都极有可能意味着使用单调栈
- 栈里有时候存val，有时候存Index。不同题存的东西不一样



## 数组-原地修改数组（快慢指针）

### 快慢指针

#### 模板

```c++
int slow=0,fast=0;
while(fast<nums.size()){
    ...
}
```

#### 题目

- [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)
  - 可以理解为slow似乎在延续一个全新的数组一样。
- [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
- [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)
- [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

### 经验

- 快慢指针，能做出来就行。不用拘泥于是不是跟滑动窗的双指针写法一致。
- 可以理解为slow似乎在延续一个全新的数组一样。
- 有时候slow，fast都是从头即0开始。有时候fast从1开始。不用纠结，能做出来就行。

## 数组-twoSum问题（无序数组，不允许排序破坏下标，使用hashmap）

- 对于 TwoSum 问题，一个难点就是给的数组**无序**。对于一个无序的数组，我们似乎什么技巧也没有，只能暴力穷举所有可能。
- **一般情况下，我们会首先把数组排序再考虑双指针技巧**。TwoSum 启发我们，HashMap 或者 HashSet 也可以帮助我们处理无序数组相关的简单问题。

### 不允许排序破坏下标：hashmap

#### 模板

- 使用hashmap

#### 题目

- [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
  - 这个问题返回的是下标，因此不可以排序再双指针

## 数组-nSum问题（排序、左右指针、递归）

### 2Sum(排序+左右指针，复杂度O(N))

#### 模板

- 2Sum

  - 排序+左右指针

  - ```c++
    vector<vector<int>> twoSumTarget(vector<int>& nums, int target){
        sort(nums.begin(),nums.end());
        vector<vector<int>> res;
        int left=0,right=nums.size()-1;
        while(left<right){
            int curLeft=nums[left];
            int curRight=nums[right];
            int sum=curLeft+curRight;
            if(sum<target){
                while(left<right && nums[left]==curLeft)//高效的left++
                    left++;
            }
            else if(sum>target){
                while(left<right && nums[right]==curRight)
                    right--;
            }
            else{
                res.push_back({left,right});//这里放的是下标，有的题让放数值
                
                while(left<right && nums[left]==curLeft)//避重
                    left++;
                while(left<right && nums[right]==curRight)
                    right--;
            }
        }
        return res;
    
    }
    ```

#### 题目

- [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
  - 这个问题返回的是下标，因此不可以排序再双指针。使用hashMap

### nSum（排序、左右指针、递归，复杂度O(N^n-1）

#### 模板

```c++
    /* 注意：调用这个函数之前一定要先给 nums 排序 */
    vector<vector<int>> nSumTarget(
        vector<int>& nums, int n, int start, int target) {

        int sz = nums.size();
        vector<vector<int>> res;
        // 至少是 2 Sum，且数组大小不应该小于 n
        if (n < 2 || sz < n) return res;

        // 2Sum 是 base case
        if (n == 2) {
            // 双指针那一套操作
            int lo = start, hi = sz - 1;
            while (lo < hi) {
                int sum = nums[lo] + nums[hi];
                int left = nums[lo], right = nums[hi];
                if (sum < target) {
                    while (lo < hi && nums[lo] == left) lo++;
                } else if (sum > target) {
                    while (lo < hi && nums[hi] == right) hi--;
                } else {
                    res.push_back({left, right});
                    while (lo < hi && nums[lo] == left) lo++;
                    while (lo < hi && nums[hi] == right) hi--;
                }
            }
        } else {
            // n > 2 时，递归计算 (n-1)Sum 的结果
            // 穷举 nSum 的第一个数
            for (int i = start; i < sz;) {
                int cur=nums[i];
                vector<vector<int>> 
                    sub = nSumTarget(nums, n - 1, i + 1, target - nums[i]);
                for (vector<int>& vec : sub) {//sub为空就不会进入for循环
                    // (n-1)Sum 加上 nums[i] 就是 nSum
                    vec.push_back(nums[i]);
                    res.push_back(vec);
                }
                while (i < sz && nums[i]==cur) i++;// 跳过第一个数字重复的情况，否则会出现重复结果
            }
        }
        return res;
    }
```

#### 题目

- [15. 三数之和](https://leetcode-cn.com/problems/3sum/)
- [18. 四数之和](https://leetcode-cn.com/problems/4sum/)

### 经验

- 只要不是返回下标，那么就排序+双指针
- 要是返回下标，那就hashMap
- 3Sum及以上，就上递归模板
- 复杂度O(N^n-1)
  - 比如：2Sum，O(N)；3Sum，O(N^2)； 4Sum，O(N^3)

## 动态规划

- 存在重叠子问题（备忘录在递归树剪枝，在迭代法中就是dp table）
  - 备忘录 `dp[][]`。「dp table」来优化穷举过程，避免不必要的计算。
- 最优子结构
  - **要符合「最优子结构」，子问题间必须互相独立，才能通过子问题的最值得到原问题的最值**。
  - 比如你想求 `amount = 11` 时的最少硬币数（原问题），如果你知道凑出 `amount = 10` 的最少硬币数（子问题），你只需要把子问题的答案加一（再选一枚面值为 1 的硬币）就是原问题的答案。因为硬币的数量是没有限制的，所以子问题之间没有相互制，是互相独立的。
    - 你只需要知道 `amount = 10`的最少硬币数（子问题）、`amount = 9`的最少硬币数（子问题）、`amount = 6`的最少硬币数（子问题），然后+1取最小值.
- 状态转移方程
  - 只有列出**正确的「状态转移方程」**，才能正确地穷举所有可行解。
  - 写出状态转移方程是最困难的，辅助你思考状态转移方程：
    - **明确 base case -> 明确「状态」-> 明确「选择」 -> 定义 dp 数组/函数的含义**。
- 状态压缩
  - （减少DP表的大小，一般是把一个二维的 DP table 压缩成一维，即把空间复杂度从 O(n^2) 压缩到 O(n)。）

### 原始动态规划

#### 模板

```c++
# 初始化 base case
dp[0][0][...] = base
# 进行状态转移
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 求最值(选择1，选择2...)
```

## 动态规划-子序列类型问题（最长最大子序列、删除字符	使得两个字符串相等）

### 对于一个字符串的题目：dp数组定义为：以array[i]结尾的目标子序列长度、和 等

#### 模板

- 对于一个字符串的题目：dp数组定义为：以array[i]结尾的目标子序列长度、和 等
- 这种定义下，一般就是一维dp数组。`vector<int> dp(nums.size(),0);`
- basecase就是：`dp[0]=xx`
- 比如最长递增子序列
  - <img src="https://gblobscdn.gitbook.com/assets%2F-Mc28-yQjBQGNRby1JtX%2Fsync%2F4490d13fdde822af5c3752f86ea78e2577583546.jpeg?alt=media" alt="img" style="zoom:50%;" />
  - 既然是递增子序列，我们只要找到前面那些结尾比 3 小的子序列，然后把 3 接到最后，就可以形成一个新的递增子序列，而且这个新的子序列长度加一。
- 备注：
  - 为啥比如最长递增子序列需要这种思路呢？**因为这样符合归纳法**，可以找到状态转移的关系。
- **另外，以这种模板的写法，返回值不是dp[dp.size()-1]。而是要遍历dp数组找到最值再返回。**
  - 因为它不是朴素的dp思路。

#### 题目

- [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
  - dp定义：以array[i]结尾的目标子序列长度
- [354. 俄罗斯套娃信封问题](https://leetcode-cn.com/problems/russian-doll-envelopes/)
  - 先对第一维度升序排序，然后对第二维度就转化成了最长递增子序列问题
  - dp定义：以array[i]结尾的目标子序列长度
  - 只不过要多加一个if判断
    - 在所有比第i值小的数里，找最大的。另外，第一维不能相等
- [面试题 08.13. 堆箱子](https://leetcode-cn.com/problems/pile-box-lcci/)
  - 三维问题，把第一维升序排列后，对第二维和第三维进行dp。
  - dp定义：以array[i]结尾的目标子序列和
  - 只不过要多加两个if判断
    - 在所有比第2维和第三维小的数里，找最大的。另外，第一维不能相等
  - 备注：其实不论多少维，只需要对第一维排序。因为剩下的维度都是靠if判断来维系正确
- [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)
  - dp定义：以array[i]结尾的目标子序列和
- [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)
  - dp定义：以s[i]结尾的括号个数
  - 递推方程比较复杂

### 对于两个字符串的题目：dp数组定义为：在子数组`arr1[0..i]`和子数组`arr2[0..j]`中，也就是从开头到第i个数为止的最优答案

#### 模板

- 对于两个字符串的题目：dp数组定义为：在子数组`arr1[0..i]`和子数组`arr2[0..j]`中，也就是从开头到第i个数为止的最优答案

- 涉及两个字符串/数组的子序列

- 这种定义下，一般是二维dp数组，

  - ```c++
    int m=text1.size();
    int n=text2.size();
    vector<vector<int>> dp(m+1,vector<int>(n+1,0));
    ```

- basecase：dp第一行和第一列，即代表空串时的最优解

  - ```c++
    //basecase。一般是这种类似的样式
    for(int i=0;i<m+1;i++)
        dp[i][0]=i;
    for(int j=0;j<n+1;j++)
        dp[0][j]=j;
    ```

- 比如：最长公共子序列、编辑距离

  - <img src="https://labuladong.gitee.io/algo/images/editDistance/4.jpg" alt="img" style="zoom:50%;" />
  - <img src="https://gblobscdn.gitbook.com/assets%2F-Mc28-yQjBQGNRby1JtX%2Fsync%2F4519d891e61e27733dfdddee345772e522dc2aaa.jpg?alt=media" alt="img" style="zoom:50%;" />
    - dp第一行和第一列，即代表空串时的最优解

- 你就往这种矩阵里猜

  - 一般是当前两个字符相等时，`dp[i][j]=d[i-1][j-1]`
  - 当前两个字符不等时，`dp[i][j]=max/min(dp[i][j-1],dp[i-1,j])+1`或者是`dp[i][j]=max/min(dp[i][j-1]+s1[i-1],dp[i-1,j]+s2[j-1]))`等等。自己判断一下就懂。

- 返回值：dp[右下角]

#### 题目

- [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)
  - 两个字符串的子序列问题。必然是这种模板。
  - dp定义为：存储 s1[0..i] 和 s2[0..j] 的最小编辑距离
  - dp第一行和第一列，即代表空串时的最优解
- [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)
  - 两个字符串的子序列问题。必然是这种模板。
  - dp定义为：`dp[i][j] `代表s1[0..i] 和 s2[0..j] 的最长公共子序列
- [718. 最长公共子串](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)
  - 两个子串的问题，即要求连续
  - 子串问题，必定要填充-1了。
  - 当前两个字符，不等就填-1。当前两个字符相等的话，`dp[i][j]=dp[i-1][j-1]==-1?1:dp[i-1][j-1]+1`  //
  - dp意义是：如果满足，那么数值就是长度。如果不满足，就是-1
- [583. 两个字符串的删除操作](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)
  - 两个字符串的子序列问题。必然是这种模板。
  - `dp[i][j] 代表s1[0..i] 和 s2[0..j] 的最少步数`
- [712. 两个字符串的最小ASCII删除和](https://leetcode-cn.com/problems/minimum-ascii-delete-sum-for-two-strings/)
  - 两个字符串的子序列问题。必然是这种模板。
  - `dp[i][j] 代表s1[0..i] 和 s2[0..j] 的最小ascii删除和`

### 对于一个字符串的题目：dp数组定义为：子数组[i...j]上的最优答案

#### 模板

- 对于一个字符串的题目：dp数组定义为：子数组[i...j]上的最优答案

- 它其实跟两个字符的思路挺像的

  - 只是上边两个字符串的题目，都是从s1[0..i] 和 s2[0..j] 。而这种题只有一个字符串，也不再是从0开始了，而是从[i..j]上的最优解

- 只有一个字符串

- 这种定义下，是二维dp数组

  - ```c++
    int size=s.size();
    vector<vector<int>> dp(size,vector<int>(size,0));
    ```

- basecase：

  - 一条斜线

- 比如：最长回文子序列

  - <img src="http://pichost.yangyadong.site/img/image-20210620154929823.png" alt="image-20210620154929823" style="zoom: 80%;" />

- 你就往这种矩阵里猜

  - 一般是当前两个字符相等时，`dp[i][j]=d[i+1][j-1]+xxx`
  - 当前两个字符不等时，`dp[i][j]=max/min(dp[i][j-1],dp[i+1,j])`等等。自己判断一下就懂。

- 返回值：dp[右上角]

#### 题目

- [516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)
  - 斜向遍历
  - dp数组定义为：子数组[i...j]上的最优答案
  - 如果i,j对应的字符相同，那就长度就是[i+1，j-1]上的值+2.
  - 如果i,j对应的字符不同，那肯定就要删掉某一个字符了。那就长度就是[i，j-1] 和 [i+1，j]上的较大值
  - dp[右上角]即返回值
- [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
  - 斜向遍历
  - 子串跟子序列不同，子串要求连续。而子序列则没有这个要求
  - 二维dp数组定义为： 在子串s[i..j]中，如果[i,j]是回文串，那么值为其长度。如果[i,j]不是回文串，那么值为-1
  - 如果当前两个字符相等，并且[i+1,j-1]上也是回文串。那么长度加2
  - 如果当前两个字符相等，但是[i+1,j-1]上不是回文串。那么[i,j]也不是回文串
  - 如果当前两个字符不等，那么，[i,j]一定不是回文串
  - 最后需要遍历数组，找max才能得到返回值

### 子串问题

#### 模板

- **如果要求是连续子串**，而不是子序列。**那么就要填充-1代表false了**
  - 二维数组先全部初始化为0。
  - 当前两个字符，不等就填-1。当前两个字符相等的话，`dp[i][j]=dp[i-1][j-1]==-1?-1或者1:dp[i-1][j-1]+1或者是+2`  //
    - 原因是必须包含第i和j个数
  - **可以理解为必须包含i和j（比较像以i结尾）**
  - dp意义是：如果满足，那么数值就是长度。如果不满足，就是-1
  - 最后遍历二维dp数组寻找返回值

#### 题目

- [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)   一个字符串的i-j
- [718. 最长公共子串](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)。两个字符串的0-i 和0-j
- [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)
  -  它虽然也是最长连续子串。但是没有用这个方法

### 经验

- 一个字符串：
  - 一维dp数组
    - **以i结尾的最优解**（不一定从0开始，可以不包含i-1）
      - 最后遍历dp数组寻找返回值
  - 二维dp数组
    - **[i,j]上的最优解**
      - 有可能最后右上角就是返回值。[516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)
      - 有可能最后需要遍历dp数组寻找返回值。[5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
- 两个字符串：
  - 二维dp数组
    - **0-i和 0-j上的最优解**
      - 右下角就是返回值
    - 记得多申请一行和一列
- **如果要求是连续子串**，而不是子序列。**那么就要填充-1代表false了**
  - 二维数组先全部初始化为0。
  - 当前两个字符，不等就填-1。当前两个字符相等的话，`dp[i][j]=dp[i-1][j-1]==-1?-1或者1:dp[i-1][j-1]+1或者是+2`  //
    - 原因是必须包含第i和j个数
  - dp意义是：如果满足，那么数值就是长度。如果不满足，就是-1
  - 最后遍历二维dp数组寻找返回值
  - 例如：
    - [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)   一个字符串的i-j
    - [718. 最长公共子串](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)。两个字符串的0-i 和0-j
- 简单总结：
  - **以i为结尾**
    - 可以不包含i的前一个（即i-1）。    对应：一个字符串的一维dp数组
      - 举例：[300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)、[53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)
    - 必须包含i的前一个（即i-1），即连续。   对应：子串问题。可能是一个字符串的二维dp，也可能是两个字符串的二维dp
      - 举例：[5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/) 、[718. 最长公共子串](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)
  - 0-i上最优解
    - 对应：两个字符串的普通二维dp数组
      - 举例：[72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)、[1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)
  - i-j上最优解
    - 对应：一个字符串的二维dp数组
      - 举例：[516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

## 动态规划- 背包问题

### 01背包（每个物品只可以取一次）

#### 模板

- 状态：

  - 可选择的物品、背包的容量

- 选择：

  - 是否把第i个物品装入背包

- dp数组定义：

  - `dp[i][w]`的定义如下：对于前i个物品，当前背包的容量为w，这种情况下可以装的最大价值是`dp[i][w]`。

- base case

  - **base case 就是`dp[0][..] = dp[..][0] = 0`**，因为没有物品或者背包没有空间的时候，能装的最大价值就是 0。

- 原始二维dp

  - ```c++
    //N是物品的个数。V是背包的容量。weight和value是长度为N的数组，代表每个物品重量和价值
    int zeroOnePack(int N,int V,vector<int>& weight,vector<int>& value){
        vector<vector<int>> dp(N+1,vector<int>(V+1,0));
        //相当于已经初始化完了base case。第一行第一列为0
        
        for(int i=1;i<N+1;i++){
            for(int j=1;j<V+1;j++){
                if(j<weight[i-1])//如果当前背包的容量不够装当前（即第i-1个）物品
                    dp[i][j]=dp[i-1][j];//只能不装当前（即第i-1个）物品
                else
                    //不装第（i-1）个物品 与 装第(i-1)个物品里取max
                    //装的话是先装，第i-1个必定装进去了
                    dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i-1]]+value[i-1]);
            }
        }
        return dp[N][V];
    } 
    ```

- 原始二维dp+状态压缩节省空间

  - 第i行只与第i-1行状态有关，可以压缩dp数组。`dp[j]=max(dp[j],dp[j-weight[i-1]]+value[i-1])`。必须逆序遍历，因为两行状态是存在一个vector里了，从后往前才不会导致两行状态存放冲突。
  
  - ```c++
    //N是物品的个数。V是背包的容量。weight和value是长度为N的数组，代表每个物品重量和价值
    int zeroOnePack(int N,int V,vector<int>& weight,vector<int>& value){
        vector<int> dp(V+1,0);
        //相当于已经初始化完了base case。第一行第一列为0
        
        for(int i=1;i<N+1;i++){
            for(int j=V;j>=1;j--){
                if(j<weight[i-1])//如果当前背包的容量不够装当前（即第i-1个）物品
                    dp[j]=dp[j];//只能不装当前（即第i-1个）物品
                else
                    //不装第（i-1）个物品 与 装第(i-1)个物品里取max
                    //装的话是先装，第i-1个必定装进去了
                    dp[j]=max(dp[j],dp[j-weight[i-1]]+value[i-1]);
            }
        }
        return dp[V];
    } 
    ```

#### 题目

- [经典01背包题目](https://www.acwing.com/problem/content/2/)

### 划分子集（其实就是01背包，dp元素变为了true false而已）

#### 模板

- 转化为01背包

- 如果有办法在N个数里装满sum/2。那么剩下的数直接装到另一个背包里，就划分好子集了

- 问题转化为了N个数里装满sum/2的背包

- `dp[i][w]`的定义如下：对于前i个物品，当前背包的容量为w，这种情况下是否可以装满背包就是`dp[i][w]`。

- base case:`dp[0][..]=false`，当没有物品可装时，肯定装不满，所以是false。`dp[...][0]=true`，当要装的空间为0时，一定装的满。其实左上角为true为false问题不大，因为没有元素需要它来递推。

- 原始二维dp

  - ```c++
    bool canPartition(vector<int>& nums) {
        int sum=0;
        for(int i:nums)
            sum+=i;
        if(sum%2==1)//如果和为奇数，那么一定没有办法平分为两个背包
            return false;
        int m=nums.size();
        int n=sum/2;//如果有办法在N个数里装满sum/2。那么剩下的数直接装到另一个背包里，就划分好子集了
        vector<vector<bool>> dp(m+1,vector<bool>(n+1,false));
    
        //base case
        //第一列置为true。因为背包容量是0，那肯定有办法装满
        for(int i=1;i<m+1;i++)
            dp[i][0]=true;
    
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                if(j<nums[i-1])//背包装不下第i个数，那么只能选择不装
                    dp[i][j]=dp[i-1][j];
                else//背包装的下第i个数，那么可以选择装或者不装
                    dp[i][j]=dp[i-1][j] || dp[i-1][j-nums[i-1]];
            }
        }
        return dp[m][n];  
    }
    ```

- 原始二维dp+状态压缩

  - ```c++
    bool canPartition(vector<int>& nums) {
        int sum=0;
        for(int i:nums)
            sum+=i;
        if(sum%2==1)//如果和为奇数，那么一定没有办法平分为两个背包
            return false;
        int m=nums.size();
        int n=sum/2;//如果有办法在N个数里装满sum/2。那么剩下的数直接装到另一个背包里，就划分好子集了
        //base case
        vector<bool> dp(n+1,false);
        dp[0]=true;
    
    
        for(int i=1;i<m+1;i++){
            for(int j=n;j>=1;j--){
                if(j<nums[i-1])//背包装不下第i个数，那么只能选择不装
                    dp[j]=dp[j];
                else//背包装的下第i个数，那么可以选择装或者不装
                    dp[j]=dp[j] || dp[j-nums[i-1]];
            }
        }
        return dp[n];  
    }
    ```

#### 题目

- #### [16. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

  - 转化问题为，将这N个数，能装满 sum/2的背包。那么剩下的数装到另一个背包里，就可以等分这个数组。

### 完全背包（每个物品可以取无限次、背包可以不装满 或者 必须装满）

#### 模板

- 与01背包的区别仅在于：

  - `01背包：dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i-1]]+value[i-1]);`

    - 都依赖于第i-1行。因为第i个元素不可以重复取

  -  `完全背包：dp[i][j]=max(dp[i-1][j],dp[i][j-weight[i-1]]+value[i-1]);`

    - 第二项：是`dp[i][j-weight[i-1]]+value[i-1]`。因为第i个元素可以重复取

  - 装的话是先装，第i-1个先装进去，再看余下容量够装多少

  - ```c++
    //N是物品的个数。V是背包的容量。weight和value是长度为N的数组，代表每个物品重量和价值
    int CompletePack(int N,int V,vector<int>& weight,vector<int>& value){
        vector<vector<int>> dp(N+1,vector<int>(V+1,0));
        //相当于已经初始化完了base case。第一行第一列为0
        
        for(int i=1;i<N+1;i++){
            for(int j=1;j<V+1;j++){
                if(j<weight[i-1])//如果当前背包的容量不够装当前（即第i-1个）物品
                    dp[i][j]=dp[i-1][j];//只能不装当前（即第i-1个）物品
                else
                    //不装第（i-1）个物品 与 装第(i-1)个物品里取max
                    //装的话是先装，第i-1个必定装进去了
                    dp[i][j]=max(dp[i-1][j],dp[i][j-weight[i-1]]+value[i-1]);
            }
        }
        return dp[N][V];
    } 
    ```

- 状态压缩：

  -  `完全背包：dp[i][j]=max(dp[i-1][j],dp[i][j-weight[i-1]]+value[i-1]);`

  - 注意了：状态转移方程里，`dp[i][j]`依赖`dp[i][j-weight[i-1]]`。**依赖同一行之前的元素（决定了必须正序遍历）**，并且上一行仅依赖相同位置。因此可以状态压缩，两行状态存放于一个数组里。**正序遍历压缩**

  - ```c++
    //N是物品的个数。V是背包的容量。weight和value是长度为N的数组，代表每个物品重量和价值
    int CompletePack(int N,int V,vector<int>& weight,vector<int>& value){
        vector<int> dp(V+1,0);
        //相当于已经初始化完了base case。第一行第一列为0
        
        for(int i=1;i<N+1;i++){
            for(int j=1;j<V+1;j++){
                if(j<weight[i-1])//如果当前背包的容量不够装当前（即第i-1个）物品
                    dp[j]=dp[j];//只能不装当前（即第i-1个）物品
                else
                    //不装第（i-1）个物品 与 装第(i-1)个物品里取max
                    //装的话是先装，第i-1个必定装进去了
                    dp[j]=max(dp[j],dp[j-weight[i-1]]+value[i-1]);
            }
        }
        return dp[V];
    } 
    ```

- 另一个直接使用01背包模板的技巧

  - V是背包最大容量，Ci是第i件物品的重量（体积）

  - 考虑到第i 种物品最多选⌊V /Ci⌋ 件，于是可以把第i 种物品转化为⌊V /Ci⌋ 件费用及价值均不变的物品，然后求解这个01 背包问题。

  - 这个技巧可以在混合背包里使用

    

#### 题目

- [经典完全背包问题](https://www.acwing.com/problem/content/3/)
  - 背包可以不装满
- [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)
  - 与经典的完全背包问题初始化上有点差别。因为这个要求背包必须装满，初始化需要把第一行设为INT_MAX-1。最后返回时还要判断一下，如果dp里存的就是INT_MAX-1，说明背包没有办法装满。
- 

### 多重背包问题（每个物品可以取的次数是不同的）

- 直接转化成01背包

  - 方案1：直接重建一个weight数组和一个value数组。

    - 把第i商品，重复放入weight数组和value数组 nums[i]次。即将`nums[i]`个元素，平铺开

  - 方案2：用个k来循环，代替重构数组。

  - 时间复杂度都是`O(V*Σnums[i])  `

  - ![image-20210701203254378](http://pichost.yangyadong.site/img/image-20210701203254378.png)

    - F就是dp数组。i是第几种物品，v是当前背包容量。Ci 体积；Wi价值；Mi件数。

  - ```c++
    //N是物品的个数。V是背包的容量。weight和value是长度为N的数组，代表每个物品重量和价值
    //在01背包基础上加了层循环就行
    int MultiplePack(int N,int V,vector<int>& weight,vector<int>& value,vector<int>& nums){
        vector<vector<int>> dp(N+1,vector<int>(V+1,0));
        //相当于已经初始化完了base case。第一行第一列为0
        
        for(int i=1;i<N+1;i++){
            for(int j=1;j<V+1;j++){
                int maxVal=0;
                for(int k=1;k<=nums[i-1];k++){//1件到Mi件
                    if(j<k*weight[i-1]){//如果当前背包的容量不够装当前（即第i-1个）物品
                        //dp[i][j]=dp[i-1][j];//只能不装当前（即第i-1个）物品
                        maxVal=max(maxVal,dp[i-1][j]);
                    }
                    else
                        //不装第（i-1）个物品 与 装第(i-1)个物品里取max
                        //装的话是先装，第i-1个必定装进去了
                        //dp[i][j]=max(dp[i-1][j],dp[i-1][j-k*weight[i-1]]+k*value[i-1]);
                        maxVal=max(maxVal,max(dp[i-1][j],dp[i-1][j-k*weight[i-1]]+k*value[i-1]));
                }
                dp[i][j]=maxVal;
            }
            
        }
        return dp[N][V];
    } 
    ```

- 采用二进制思想转化为01背包

  - O(V ΣlogMi) 。 Mi代表每个类型的物品的件数

  - ```c++
    //-----------------这个与之前的01背包一模一样，不用看-------------
    //N是物品的个数。V是背包的容量。weight和value是长度为N的数组，代表每个物品重量和价值
    int zeroOnePack(int N,int V,vector<int>& weight,vector<int>& value){
        vector<vector<int>> dp(N+1,vector<int>(V+1,0));
        //相当于已经初始化完了base case。第一行第一列为0
        
        for(int i=1;i<N+1;i++){
            for(int j=1;j<V+1;j++){
                if(j<weight[i-1])//如果当前背包的容量不够装当前（即第i-1个）物品
                    dp[i][j]=dp[i-1][j];//只能不装当前（即第i-1个）物品
                else
                    //不装第（i-1）个物品 与 装第(i-1)个物品里取max
                    //装的话是先装，第i-1个必定装进去了
                    dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i-1]]+value[i-1]);
            }
        }
        return dp[N][V];
    } 
    //-----------------------------------------------------
    
    //--------------------使用二进制思想，将多重背包转化为01背包---------------
    //N是物品的个数。V是背包的容量。weight和value是长度为N的数组，代表每个物品重量和价值
    int MultiplePack(int N,int V,vector<int>& weight,vector<int>& value,vector<int>& nums){
        vector<int> tmpWeight;//新数组
        vector<int> tmpValue;//新数组
        for(int i=0;i<N;i++){
            int num=nums[i];//比如nums[i]=13.
            for(int k=1;k<=num;){//那么k是 1,2,4   当k等于8时，num就只剩6了
                tmpWeight.push_back(k*weight[i]);
                tmpValue.push_back(k*value[i]);
                num-=k;//一直在减少
                k*=2;
            }
            if(num>0){//上边只完成了1,2,4。num剩的6也得加进去。1,2,4,6才能够刚好表示1-13。按照朴素的思想，得往新数组里加13个数。而二进制只加4个
                										//0怎么表示？ 在dp里这4个数都不选的时候就表示了
                tmpWeight.push_back(num*weight[i]);
                tmpValue.push_back(num*value[i]);
            }
        }
        //将构建好的新数组，直接调用01背包问题
        return zeroOnePack(tmpWeight.size(),V,tmpWeight,tmpValue);
    } 
    ```

- 单调队列优化

  - 不必掌握

### 混合背包问题（有的物品只能取1次（01背包），有的物品可以取无限次（完全背包） ，有的物品可以取无限次 （多重背包））

#### 模板

- 只有 01 背包 与 完全背包的混合  

  - 01 背包和完全背包中给出的伪代码只有一处不同，  根据物品的类别选用  `01背包：dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i-1]]+value[i-1]);` `完全背包：dp[i][j]=max(dp[i-1][j],dp[i][j-weight[i-1]]+value[i-1]);`即可

- 01 背包 与 完全背包 还有 多重背包的混合

  - 可以理解为完全背包与多重背包的混合，因为其实01背包可以看做多重背包里，只有1件商品的情况

  - 依然使用二进制的方法。只需要在二进制之前，把num处理一下就行

  - ```c++
    //N是物品的个数。V是背包的容量。weight和value是长度为N的数组，代表每个物品重量和价值
    int zeroOnePack(int N,int V,vector<int>& weight,vector<int>& value){
        vector<int> dp(V+1,0);
        //相当于已经初始化完了base case。第一行第一列为0
        
        for(int i=1;i<N+1;i++){
            for(int j=V;j>=1;j--){
                if(j<weight[i-1])//如果当前背包的容量不够装当前（即第i-1个）物品
                    dp[j]=dp[j];//只能不装当前（即第i-1个）物品
                else
                    //不装第（i-1）个物品 与 装第(i-1)个物品里取max
                    //装的话是先装，第i-1个必定装进去了
                    dp[j]=max(dp[j],dp[j-weight[i-1]]+value[i-1]);
            }
        }
        return dp[V];
    } 
    //N是物品的个数。V是背包的容量。weight和value是长度为N的数组，代表每个物品重量和价值
    int HybridPack(int N,int V,vector<int>& weight,vector<int>& value,vector<int>& nums){
        vector<int> tmpWeight;//新数组
        vector<int> tmpValue;//新数组
        for(int i=0;i<N;i++){
            int num=nums[i];
            if(num==-1) //如果是 01背包
                num=1;	//那个数就是1
            else if(num==0) //如果是完全背包
                num=V/weight[i]+1;//个数设为全是第i种物品的时候的数目
            else;//多重背包，不用改
                
            for(int k=1;k<=num;){
                tmpWeight.push_back(k*weight[i]);
                tmpValue.push_back(k*value[i]);
                num-=k;
                k*=2;
            }
            if(num>0){
                tmpWeight.push_back(num*weight[i]);
                tmpValue.push_back(num*value[i]);
            }
        }
        //将构建好的新数组，直接调用01背包问题
        return zeroOnePack(tmpWeight.size(),V,tmpWeight,tmpValue);
    } 
    ```
    
      

### 二维费用背包问题（背包除了重量，还有体积的限制）

#### 模板

- 只需要在01背包基础上，再添一维即可

  - ```c++
    #include<iostream>
    #include<vector>
    #include<algorithm>
    using namespace std;
    
    //N是物品的个数。V是背包的体积容量，M背包的重量容量。weight和value是长度为N的数组，代表每个物品重量和价值
    int zeroOnePack(int N,int V,int M,vector<int>& volumn,vector<int>& weight,vector<int>& value){
        vector<vector<vector<int>>> dp(N+1,vector<vector<int>>(V+1,vector<int>(M+1,0)));
        //相当于已经初始化完了base case。第一行第一列为0
        
        for(int i=1;i<N+1;i++){
            for(int j=1;j<V+1;j++){
                for(int k=1;k<M+1;k++){
                if(j<volumn[i-1] || k<weight[i-1])//如果当前背包的容量不够装当前（即第i-1个）物品
                    dp[i][j][k]=dp[i-1][j][k];//只能不装当前（即第i-1个）物品
                else
                    //不装第（i-1）个物品 与 装第(i-1)个物品里取max
                    //装的话是先装，第i-1个必定装进去了
                    dp[i][j][k]=max(dp[i-1][j][k],dp[i-1][j-volumn[i-1]][k-weight[i-1]]+value[i-1]);
                }
            }
        }
        return dp[N][V][M];
    }  
    ```

### 分组背包问题（先把各种各样物品分成若干组，每组里只能选一件，组内部的物品是互斥的）

### 输出最优路径（进阶：最优路径且字典序最小的那条）

#### 模板

- 记录下每个状态的最优值是由状态转移方程的哪一项推出来的，换句话说，记录下它是由哪一个策略推出来的。便可根据这条策略找到上一个状态，从上一个状态接着向前推即可。

- 以01 背包为例，再用一个数组record[i, v]，设record[i, v] = 1 表示推出dp[i, v] 的值时是采用了方程的前一项；record[i,v] = 2 表示采用了方程的后一项。

  - ```c++
    //记录路径
    void zeroOnePack(int N,int V,vector<int>& weight,vector<int>& value){
        vector<vector<int>> dp(N+1,vector<int>(V+1,0));
        vector<vector<int>> record(N+1,vector<int>(V+1,0));
        for(int i=1;i<N+1;i++){
            for(int j=1;j<V+1;j++){
                if(j<weight[i-1]){//如果当前背包的容量不够装当前物品
                    dp[i][j]=dp[i-1][j];//只能不装当前物品
                    record[i][j]=1;
                }
                else{
                    dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i-1]]+value[i-1]);
                    if(dp[i-1][j]>dp[i-1][j-weight[i-1]]+value[i-1])
                        record[i][j]=1;//选择的前者
                    else
                        record[i][j]=2;//选择的后者
                }
            }
        }
        
        vector<int> res;
        int i=N,j=V;
        while(i>=0){
            if(record[i][j]==2){
                res.push_back(i);
                j=j-weight[i-1];
            }
            
            i--;
        }
        
        reverse(res.begin(),res.end());
        for(int i:res)
            cout<<N+1-i<<" ";
        
        // return dp[N][V];
    } 
    ```

- 如果要求字典序最小的方案

  - 不能用之前的是因为，最优方案其实有好几个。我用了较大字典序的方法把较小字典序的方案给覆盖了

  - 下别的写法自己悟一下...背包九讲上建议这样做，我是没太理解，但是这样确实有效

    - ，先把物品编号做x  ←N + 1 - x 的变换，在输出方案时再变换回来。

  - ```c++
    //先把数据reverse了，最后输出的时候，把序号人工修改回来..
    void zeroOnePack(int N,int V,vector<int>& weight,vector<int>& value){
        reverse(weight.begin(),weight.end());
        reverse(value.begin(),value.end());
        vector<vector<int>> dp(N+1,vector<int>(V+1,0));
        vector<vector<int>> record(N+1,vector<int>(V+1,0));
        for(int i=1;i<N+1;i++){
            for(int j=1;j<V+1;j++){
                if(j<weight[i-1]){//如果当前背包的容量不够装当前物品
                    dp[i][j]=dp[i-1][j];//只能不装当前物品
                    record[i][j]=1;
                }
                else{
                    dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i-1]]+value[i-1]);
                    if(dp[i-1][j]>dp[i-1][j-weight[i-1]]+value[i-1])
                        record[i][j]=1;//选择的前者
                    else
                        record[i][j]=2;//选择的后者
                }
            }
        }
        
        vector<int> res;
        int i=N,j=V;
        while(i>=0){
            if(record[i][j]==2){
                res.push_back(i);
                j=j-weight[i-1];
            }
            
            i--;
        }
        
        for(int i:res)
            cout<<N+1-i<<" ";
        
        // return dp[N][V];
    } 
    ```

### 背包问题求方案数

### 有依赖的背包问题（选一个物品，必须得把其他的某些物品也选了）

### 经验

- 注意下述所有的第i个数，其实是指第i-1个数。为了方便描述，第i个数，是指`dp[i]`，在weight和value里是`i-1`。

- 01背包：
  - `dp[i][j]=max(dp[i-1][j],dp[i-1][j-weight[i-1]]+value[i-1]);`
    - 因为物品不可重复使用，所以第i个数装进背包后，就都只能使用第i-1的状态。`dp[i-1][j-weight[i-1]]`
- 完全背包：
  - `dp[i][j]=max(dp[i-1][j],dp[i][j-weight[i-1]]+value[i-1]);`
    - 物品可以重复使用，所以第i个装进背包后，还可以装第i个数。`dp[i][j-weight[i-1]]`
- 如果背包要求必须装满
  - 那么第一行为无穷小。第一列全为0
  - 因为第一列代表：当前背包容量为0时的，最大价值，那肯定为0，什么都不装就算装满了。
  - 第一行：什么都不允许装，而背包的容量在增大，肯定没有装满，那么就不能是0
- 如果背包不要求被装满，只希望价格尽量大  ：
  - 那么第一行第一列全为0。
- 

## 排序

### 快排

#### 模板

- 快排的思想：

  - 比如选择最左侧下标l为哨兵，那么就从从右侧寻找第一个严格小于nums[l]的数nums[j]，然后从左侧寻找第一个严格大于nums[l]的数nums[i]。然后把这两个数交换。然后两个指针继续向内收缩直至相遇。然后把哨兵跟nums[i]或者说是nums[j]（相遇了，ij相等）交换。
    - 意味着此时哨兵的左侧都是小于哨兵的数，哨兵的右侧都是大于哨兵的数
  - 然后继续递归区间[l,i-1]和区间[i+1,r]。
  - 备注：
    - 固定选取**l**作为哨兵，一定要先从右侧寻找（即j--）
    - 如果固定选取**r**作为哨兵，一定要先从左侧寻找（即i++)
    - 底下的写法全部都是按照**l**作为哨兵

- 原始快排

  - 时间复杂度：`O(n*logn)`。

  - 空间复杂度：快速排序的递归深度最好（平均）为`O(logN)` ，最差情况（即输入数组完全倒序）为 `O(N)`。

  - ```c++
    void quickSort(vector<int>&arr,int l,int r){
        if(l>=r)
            return;
        int i=l,j=r;
        while(i<j){
            while(i<j && arr[j]>=arr[l]) j--;
            while(i<j && arr[i]<=arr[l]) i++;
            swap(arr[i],arr[j]);
        }
        swap(arr[i],arr[l]);
        quickSort(arr,l,i-1);
        quickSort(arr,i+1,r);
    }
    ```

- 只需要第K个数  或者是 前k个数（前k个数顺序任意）

  - 时间复杂度 `O(N)` 

  - 空间复杂度`O(logN) `。平均递归深度为`O(logN`)。

  - ```c++
    int k;
    void quickSort(vector<int>& nums,int l,int r){
        if(l>=r)
            return;
        int i=l,j=r;
        while(i<j){
            while(i<j && nums[j]<=nums[l]) j--;
            while(i<j && nums[i]>=nums[l]) i++;
            swap(nums[i],nums[j]);
        }
        swap(nums[i],nums[l]);
        if(i>k-1)     //如果i够大，那么只排左侧 （右侧顺序不重要了。因为只要前k个数）
            quickSort(nums,l,i-1);
        else if(i<k-1)  //i不够大，那么只排右侧。（左侧顺序不重要了。因为前k个数顺序随意，如果要求前k个数有序，那么也必须得排左侧）
            quickSort(nums,i+1,r);
        else        //i刚好等于k-1
            return;
    }
    ```

- 快排+trick（三数选取优化基准数、随机化选取哨兵）

  - 有时案例比较特殊，默认快排过不了。

  - ```c++
        void quickSort(vector<int>& nums,int l,int r){
            if(l>=r)
                return;
            int i=l,j=r;
           
         //1.增加三数选取优化基准数的选择。相当于人工先把(l、mid、r)这三个位置排好。这3数排成升序降序要跟快排序一致。
            int m=l+(r-l)/2;
            if(nums[l]>nums[r]) swap(nums[l],nums[r]);
            if(nums[m]>nums[r]) swap(nums[m],nums[r]);//前两行保证arr[r]始终为这三个数中最大的一个
            if(nums[m]>nums[l]) swap(nums[m],nums[l]);//最后这行代码保证arr[l]为中间元素，这样下面代码不必再做修改
         /*2.随机化选取哨兵。随机选择一个下标，然后跟l交换。
            int idx=rand()%(r-l+1)+l;
            swap(nums[l],nums[idx]);*/
            while(i<j){
                while(i<j && nums[j]>=nums[l]) j--;
                while(i<j && nums[i]<=nums[l]) i++;
                swap(nums[i],nums[j]);
            }
            swap(nums[i],nums[l]);
            quickSort(nums,l,i-1);
            quickSort(nums,i+1,r);
        }
    
    };
    ```


#### 题目

- [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)
  - 前k个数（前k个数顺序任意）。单侧排序改进快排 O(N)
- [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)
  - 只需要第K个数。单侧排序改进快排 O(N)
- [912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)
  - 整个数组排序。三数选取优化基准数/随机化哨兵

