## STL对应关系

### 1.关联容器：

- map：内部自建一颗**红黑树**(一种非严格意义上的平衡二叉树)，这颗树具有对数据自动排序的功能，所以在map内部所有的**数据都是有序的**。map中**key值是唯一**的。
  - multimap：可以插入多个相同的key，对应不同value。key不唯一。（多重映射）
- unordered_map：基于**hash表**（存Key和value)。（hash映射）
- set:底层使用**红黑树**实现，set中的**元素都是唯一**的，而且**默认**情况下会对元素进行**升序排列**。所以在set中，不能直接改变元素值，因为那样会打乱原本正确的顺序，要改变元素值必须先删除旧元素，再插入新元素。
  - multiset：可以插入多个相同的元素，元素不唯一。（多重集合）
- unordered_set: 基于**hash表**（只存key)。（hash集合）

### 2.顺序容器：

- array：就是对内置数组的封装
- vector：动态数组，支持随机访问，在中间或头部插入或删除元素慢（不能前插？）
- deque：双端队列，支持随机访问，在两端插入或删除元素快,在中间插入或删除元素慢
- forward_list：单链表，不支持随机访问
- list：双向链表，不支持随机访问，在任意位置插入和删除元素都很快

特点：

- 元素线性排列，可以随时在指定位置插入元素和删除元素。
- 必须符合Assignable这一概念（即具有公有的拷贝构造函数并可以用“=”赋值）。
- 有两个容器比较特殊：
  - array对象的大小固定，forward_list有特殊的添加和删除操作。

### 3.容器适配器：

都不支持迭代器，因为它们不允许对任意元素进行访问

- stack：栈，可以用任何一种顺序容器做基础容器。
- queue：队列，只可以用前插顺序容器做基础容器（双端队列deque、list）。
- priority_queue：优先队列（堆），基础容器必须是支持随机访问的顺序容器（vector、deque、array)

### 4.总结

内部实现
\1. vector: 内部实现是数组，一段连续的内存。
\2. list， 内部实现是双链表
\3. deque 内部实现是内存块的链表。
\4. string： 连续的内存
\5. set，map： 红黑树(平衡二叉树的一种)
\6. hash_map, hash_set 用哈希表(散列表)来实现。
\7. stack: 用vector或者是deque来实现
\8. queue,用deque实现

## 三、树

### 1.顺序查找

```c++
//无哨兵
typedef int  ElementType ;

struct LNode {
	ElementType Element[100];//最多容纳100个元素,元素从1的位置开始存放
	int Length;//代表最后一个元素所在的下标
};

typedef struct LNode* List;

int SequentialSearch(List Tb1,ElementType K)
{//在表Tb1中查找关键字为K的数据元素
    int i;
    for(i=Tb1->Length;i>0 && Tb1->Element[i]!=K;i--);
    return i;
}
//从length位置，由大向小搜寻，i=0是我的数组边界，每一轮都要比较i>0吗，也就是有没有到边界
```

```c++
//有哨兵实现
typedef int  ElementType ;

struct LNode {
	ElementType Element[100];//元素从1的位置开始存放
	int Length;//代表最后一个元素所在的下标
};

typedef struct LNode* List;

int SequentialSearch(List Tb1, ElementType K)
{//在表Tb1中查找关键字为K的数据元素
	int i;
	Tb1->Element[0] = K;//我要查找的值是几，就把0位置（哨兵位置）设成这个值
	for (i = Tb1->Length; Tb1->Element[i] != K; i--);//少了i>0这个比较
	return i;
}
//有哨兵实现,将哨兵处设为目标值，也是从length位置，由大向小搜寻，省去了每轮与i>0的比较。循环结束的条件只有数组元素等于k,等于K有两种情况，一种是在0-length之间有这个值，那我就退出循环。另一种是在哨兵处等于K，即return i=0，说明没找到。
```

### 2.二分查找

```c++
typedef int  ElementType ;

struct LNode {
	ElementType Element[100];//元素从1的位置开始存放
	int Length;//代表最后一个元素所在的下标
};

typedef struct LNode* List;

int BinarySearch(List Tb1,ElementType K)
{
    //在表Tb1中查找关键字为K的数据元素
    int left,right,mid,NoFound=-1;
    
    left=1;//初始化左边界
    right=Tb1->Length;//初始化右边界
    while(left<=right)
    {
        mid=(left+right)/2;
        if(Tb1->Element[mid] >K) right=mid-1;
        else if(Tb1->Element[mid] <K) left=mid+1;
        else return mid;//查找成功，返回下标
    }
    return NoFound;
}
```

### 3.树的表示

- 子树是不相交的

- 除了根节点外，每个结点有且仅有一个父节点；

- 一颗N个结点的树有N-1条边（因为每一个结点都有往上的一条边，只有根节点没有）

  ![image.png](https://i.loli.net/2020/02/17/ziwds5oAb2Quc3O.png)

  **完全二叉树，就是完美二叉树缺掉一些连续高序号的。**

![image.png](https://i.loli.net/2020/02/17/IKAOwCqSrayzx4E.png)

![image.png](https://i.loli.net/2020/02/21/2y6Sg8eahtndOZ3.png)

- 树的遍历方法：
  - 先序——根、左子树、右子树
  - 中序——左子树、根、右子树
  - 后序——左子树、右子树、根
  - 层次遍历——从上到下、从左到右

- 二叉树的链表存储

```c++
typedef struct TreeNode* BinTree;
typedef BinTree Position;//这句话目前没用
struct TreeNode{
    ElementType Data;
    BinTree Left;
    BinTree Right;
}
```

### 4.二叉树的遍历

#### 1.先序遍历(DFS，深度优先)

**遍历过程：**

1. 访问根结点
2. 先序遍历其左子树
3. 先序遍历其右子树

```c++
void PreOrderTraversal(BinTree BT)
{
    if(BT){//如果指针非空，也就是树非空
		cout<<BT->Data;//访问数据
        PreOrderTraversal(BT->Left);
        PreOrderTraversal(BT->Right);
    }
}
```

![image.png](https://i.loli.net/2020/02/21/vWb8u1Ua62Ylz95.png)

#### 2.中序遍历(DFS，深度优先)

**遍历过程：**

1. 先序遍历其左子树
2. 访问根结点
3. 先序遍历其右子树

```c++
void PreOrderTraversal(BinTree BT)
{
    if(BT==nullptr) return;
    PreOrderTraversal(BT->Left);
   	cout<<BT->Data;//访问数据
    PreOrderTraversal(BT->Right);
    
}//对于每个结点来说，肯定是他的左侧子树全部cout后，它才能cout
```

![image.png](https://i.loli.net/2020/02/21/J6MOKb43E95zThU.png)

#### 3.后序遍历(DFS，深度优先)

**遍历过程：**

1. 先序遍历其左子树
2. 先序遍历其右子树
3. 访问根结点

```c++
void PreOrderTraversal(BinTree BT)
{
    if(BT){//如果指针非空，也就是树非空
        PreOrderTraversal(BT->Left);
        PreOrderTraversal(BT->Right);
        cout<<BT->Data;//访问数据
    }
}//对于每个结点来说，肯定是他的左侧子树 和 右侧子树全部cout后，它才能cout
```

![image.png](https://i.loli.net/2020/02/21/qUxhpkE9aci1KXm.png)

#### **4.三种遍历方式可以等价于第几次遇到它**

**当成完全二叉树来判断，这是第几次经过这个结点**

![](https://i.loli.net/2020/02/21/ltehKqA3gC85xSs.png)

#### 5.非递归遍历

使用堆栈。中序非递归：

1. 遇到一个结点，将它入栈，遍历他的左子树
2. 当左子树遍历结束后，从栈顶弹出这个结点并访问它
3. 然后按其右指针再去中序遍历该结点的右子树

```c++
//中序非递归
void InOrderTraversal(BinTree BT)
{
    BinTree T=BT;
    Stack S=CreatStack(MaxSize);//创建并初始化堆栈S
    while(T || !IsEmpty(S))//树不空或者堆栈不空
    {
        while(T)//一直向左并将沿途结点压入堆栈
        {
            Push(S,T);
            T=T->Left;
        }
        
        if(!IsEmpty(S)){//这句if判断可以去掉       
            T=Pop(S);//结点弹出堆栈
            cout<<T->Data;//输出结点
            T=T->Right;//转向右子树
        }
    }
}
```

```c++
//改为先序非递归

void InOrderTraversal(BinTree BT)
{
    BinTree T=BT;
    Stack S=CreatStack(MaxSize);//创建并初始化堆栈S
    while(T || !IsEmpty(S))//树不空或者堆栈不空
    {
        while(T)//一直向左并将沿途结点压入堆栈
        {
            Push(S,T);
            cout<<T->Data;//输出结点
            T=T->Left;
        }
        
        if(!IsEmpty(S)){//这句if判断可以去掉
            T=Pop(S);//结点弹出堆栈
            T=T->Right;//转向右子树
        }
    }
}
```

```c++
//后序非递归
//即先序遍历为：根-左-右 左右子树顺序倒置后得到：根-右-左，逆序输出得到左-右-根。
//使用两个栈储存，一个保存左右颠倒的先序遍历，一个保存结果，然后将结果逆序输出。
void InOrderTraversal(BinTree BT)
{
    BinTree T=BT;
    Stack S=CreatStack(MaxSize);//创建并初始化堆栈S
    Stack S2=CreatStack(MaxSize);//保存结果用于逆序输出的堆栈
    while(T || !IsEmpty(S))//树不空或者堆栈不空
    {
        while(T)//一直向左并将沿途结点压入堆栈
        {
            Push(S,T);
            Push(S2,T); 
            T=T->Right;
        }
        
        if(!IsEmpty(S)){//这句if判断可以去掉
            T=Pop(S);//结点弹出堆栈
            T=T->Left;
    }
    while(!IsEmpty(S2))
    {
        T=Pop(S2);
        cout<<T->Data;
    }
}
```

#### 6.层序遍历（BFS，广度优先）

队列实现：从根节点开始，首先将根节点入队，然后开始执行循环：结点出队、访问该结点、其左右儿子入队。

```c++
void LevelOrderTraversal(BinTree BT)
{
    BinTree T;
    if(!BT) return;//若是空树则直接返回
    Queue Q=CreatQueue(MaxSize);//创建并初始化队列
    AddQ(Q,BT);//将根节点入队
    while(!IsEmptyQ(Q))
    {
        T=DeleteQ(Q);//出队
        cout<<T->Data;
        if(T->Left) AddQ(Q,T->Left);//左儿子入队
        if(T->Right) AddQ(Q,T->Right);//右儿子入队
    }
}
```

### 5.遍历的应用

#### 1.输出叶子节点

```c++
void PreOrderPrintLeaves(BinTree BT)
{
    if(BT){
        if(!BT->Left && !BT->Right)//之前的先序递归遍历基础上，加了个左右儿子为空的条件判断，就可以只输出叶子节点了
            cout<<BT->Data;
        PreOrderPrintLeaves(BT->Left);
        PreOrderPrintLeaves(BT->Right);
        
    }
}
```

#### 2.求二叉树的高度

整个树的高度=max(左子树高度，右子树高度)+1

```c++
int PostOrderGetHeight(BinTree BT)
{
    int HL,HR,MaxH;
    if(BT){
        HL=PostOrderGetHeight(BT->Left);//求左子树的高度
        HR=PostOrderGetHeight(BT->Right);//求右子树的高度
        MaxH=(HL>HR)?HL:HR;//取左右子树较大的深度
        return(MaxH+1);//返回树的深度
    }
    else return 0;//空树深度为0
}
```

#### 3.二元运算表达式树及其遍历

#### 4.由两种遍历序列确定二叉树

中序遍历+前序或者后序遍历，就可以唯一确定二叉树。

必须要有中序遍历：因为假如不给中序，只给前序和后序。

前序：根左右，中序：左根右，后序：左右根。

![image.png](https://i.loli.net/2020/02/23/w1EPiOabn8SCQLZ.png)

**利用先序和中序遍历序列来确定一颗二叉树**

- 根据先序遍历序列第一个节点确定根节点
- 根据**根节点**在**中序遍历序列**中分割出**左右两个子序列**
- 对左子树和右子树分别递归使用相同的方法继续分解

###  6.二叉搜索树

#### 1.二叉搜索树的概念

二叉搜索树（BST，Binary Search Tree），也称为二叉排序树或二叉查找树。

**性质：**

1. 非空左子树的所有键值小于其根节点的键值。
2. 非空右子树的所有键值大于其根节点的键值。
3. 左、右子树都是二叉搜索树。

#### 2.二叉搜索树的查找

**递归实现**

```c++
//递归实现
Position Find(ElementType X,BinTree BST)
{
    if(!BST) return NULL;//查找失败
    if(X>BST->Data)
        return Find(X,BST->Right);//在右子树中继续查找
    else if(X<BST->Data)
        return Find(X,BST->Left);//在左子树中继续查找
    else
        return BST;//查找成功，返回结点地址
}
```

**循环实现**

```c++
//循环实现
//查找效率取决于树的高度
Position IterFind(ElementType X,BinTree BST)
{
    while(BST)
    {
        if(X>BST->Data)
            BST=BST->Right;
        else if(X<BST->Data)
            BST=BST->Left;
        else
            return BST;
    }
    return NULL;
}
```

**查找最大和最小元素**

```c++
//递归实现，查找最小元素
Position FindMin(BinTree BST)
{
    if(BST==NULL) return NULL;
    else if(BST->Left==NULL) return BST;
    else 
        return FindMin(BST->Left);
}
```

```c++
//循环实现，查找最小元素
Positon FindMin(BinTree BST)
{
    if(BST==NULL) return NULL;
    while(BST->Left) BST=BST->Left;
    return BST;
}
```

```c++
//递归实现，查找最大元素
Position FindMax(BinTree BST)
{
    if(BST==NULL) return NULL;
    else if(BST->Right==NULL) return BST;
    else 
        return FindMax(BST->Right);
}
```

```c++
//循环实现，查找最大元素
Position FindMax(BinTree BST)
{
    if(BST==NULL) return NULL;
    while(BST->Right) BST=BST->Right;
    return BST;
}
```

#### 3.二叉搜索树的插入

```c++
//递归实现
BinTree Insert(ElementType X,BinTree BST)
{
    if(BST==NULL)
    {//若树为空，生成并返回一个结点的二叉搜索树
        BST=malloc(sizeof(Struct TreeNode));
        BST->Data=X;
        BST->Left=BST->Right=NULL;
    }
    else//开始找要插入元素的位置
        if(X<BST->Data)
            BST->Left=Insert(X,BST->Left);//递归插入左子树
    
    else if(X>BST->Data)
        BST->Right=Insert(X,BST->Right);//递归插入右子树
    else;//else就是X已经存在，不用插入了，什么都不做
    return BST;
    
}
```

#### 4.二叉搜索树的删除

- 如果要删除的是叶结点：
  - 直接删除，并再修改其父节点指针置为NULL
- 如果要删除的结点只有一个孩子结点：
  - 将其（准备删除的结点）父节点的指针指向要删除结点的孩子结点
- 如果要删除的结点有左右两棵子树：
  - 用另一结点代替被删除结点：**右子树的最小元素**或者**左子树的最大元素**（因为这种结点，只有1个儿子或者没有儿子）。比如：用右子树最小元素数值去覆盖本该删除的结点处的数值，然后将右子树最小元素结点删除就好（右子树最小元素结点一定只有1个儿子或者没有儿子）。

```c++
BinTree Delete(ElementType X,BinTree BST)
{
 	Position Tmp;
    if(BST==NULL) cout<<"要删除的元素未找到"<<endl;
    else if(X<BST->Data) BST->Left=Delete(X,BST->Left);//左子树递归删除
    else if(X>BST->Data) BST->Right=Delete(X,BST->Right);//右子树递归删除
    else //找到要删除的结点
        if(BST->Left&&BST->Right){//要删除的结点有左右两颗子树	
            Tmp=FindMin(BST->Right);//右子树中找到最小元素。也可以用左子树的最大元素
            BST->Data=Tmp->Data;//元素覆盖
            BST->Right=Delete(Tmp->Data,BST->Right);//去把右子树最小元素位置给删了
        }
    else{
        Tmp=BST;
        if(BST->Left==NULL)//只有右儿子或者无子结点
            BST=BST->Right;//将其（准备删除的结点）父节点的指针指向要删除结点的孩子结点
        else if(BST->Right==NULL)//只有左儿子或者无子节点
            BST=BST->Left;
        
        free(Tmp);//将这个结点删除
    }
    
    return BST;
}
```

### 7.平衡二叉树

#### 1.平衡二叉树的定义

![image.png](https://i.loli.net/2020/02/24/UMLStGDRIdgynJP.png)

普通的二叉树形状各异。同样是12个结点，不同的插入顺序会形成不同的树结构，导致查找效率不一样。所以要设计查找顺序，使得效率最高。

两个指标：左右子树结点数差不多、左右子树高度差不多

平衡因子BF：左子树高度-右子树高度

**平衡二叉树（Balanced Binary Tree),AVL树：**（可以使树的高度低一些）

**定义为：空树，或者任一结点左右子树高度差的绝对值不超过1，即|BF(T)|<=1**

![image.png](https://i.loli.net/2020/02/24/1x9vFiBpo2Jq7wL.png)

**给定结点数为n的AVL树的最大高度为O(log2n)**

#### 2.平衡二叉树的调整

有可能一插入某个结点，这个树就不平衡了

**RR旋转**（右单旋）

破坏者处于被破坏者的RR位置时，采用RR旋转（右单旋）

![image.png](https://i.loli.net/2020/02/24/gALohrjCtIn23U5.png)

![image.png](https://i.loli.net/2020/02/24/4tpi8QlGeAoSLBc.png)

![image-20200224170347460](C:\Users\杨亚东\AppData\Roaming\Typora\typora-user-images\image-20200224170347460.png)

**LL旋转**

![image.png](https://i.loli.net/2020/02/24/7G8kzCqTLevXxI3.png)

**LR旋转**

![image.png](https://i.loli.net/2020/02/24/6kSdcJEY9jlq4Ou.png)

**RL旋转**

![image.png](https://i.loli.net/2020/02/24/5RkyE98r2PpInhg.png)

### 8.判断两个树是否相同

```c++

```

### 9.leetcode

#### 1.前序遍历

```c++
//递归
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        helper(root,res);
        return res;
    }
private:
    void helper(TreeNode* root,vector<int>&res)
    {
        if(root==nullptr) return;
        res.push_back(root->val);
        helper(root->left,res);//加个if(root->left)也行
        helper(root->right,res);//加个if(root->right)也行
    }
};
```

```c++
//迭代，与中序通用
class Solution {
public:
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
};

//迭代：非通用版本。以出栈顺序作为遍历顺序
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        if(root==nullptr) return res;

        stack<TreeNode*> s;
        s.push(root);

        while(!s.empty())
        {
            TreeNode* tempnode=s.top();
            s.pop();
            res.push_back(tempnode->val);

            if(tempnode->right!=nullptr) s.push(tempnode->right);//先Push右，后push左
            if(tempnode->left!=nullptr) s.push(tempnode->left);//所以出栈时，先左后右
        }
        return res;
    }
};
```

#### 2.中序遍历

```c++
//递归
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        helper(root,res);
        return res;
    }
private:
    void helper(TreeNode* root,vector<int>& res)
    {
        if(root==nullptr) return;
        helper(root->left,res);//if(root->left)   helper(root->left,res);也行
        res.push_back(root->val);
        helper(root->right,res);// if(root->right)  helper(root->right,res);也行
    }
};
```

```c++
//迭代
class Solution {
public:
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
};
```

#### 3.后序遍历

```c++
//递归
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        helper(root,res);
        return res;
    }
private:
    void helper(TreeNode* root,vector<int>& res)
    {
        if(root==nullptr) return;
        helper(root->left,res);
        helper(root->right,res);
        res.push_back(root->val);
    }
};
```

```c++
//迭代
class Solution {
public:
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
};
```

#### 4.层序遍历

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if(root==nullptr) return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            int size=q.size();//这就是这一层的结点个数
            vector<int> array;
            for(int i=0;i<size;i++)//一个for循环就是一层
            {
                TreeNode* temp=q.front();
                q.pop();
                array.push_back(temp->val);
                if(temp->left) q.push(temp->left);
                if(temp->right) q.push(temp->right);                
            }
            res.push_back(array);
        }
        return res;
    }
};
```

#### 5.自顶向下

自顶向下” 意味着在每个递归层级，我们将首先访问节点来计算一些值，并在递归调用函数时将这些值传递到子节点。 所以 “自顶向下” 的解决方案可以被认为是一种**前序遍历**。

```c++
//递归函数：top_down(root, params)
1. return specific value for null node
2. update the answer if needed                      // anwer <-- params
3. left_ans = top_down(root.left, left_params)      // left_params <-- root.val, params
4. right_ans = top_down(root.right, right_params)   // right_params <-- root.val, params
5. return the answer if needed                      // answer <-- left_ans, right_ans
```

```C++
//自顶向下的思想求树高
class Solution {
public:
    int maxDepth(TreeNode* root) {
        max_depth(root,1);
        return answer;
    }
private:
    int answer=0;
    void max_depth(TreeNode* root,int depth)//depth代表当前层的高度，如果是叶结点，就要比较返回了
    {
        if(root==nullptr) return;
        if(root->left==nullptr && root->right==nullptr)
            answer=max(answer,depth);
        max_depth(root->left,depth+1);
        max_depth(root->right,depth+1);
    }
};
```

#### 6.自底向上

“自底向上” 是另一种递归方法。 在每个递归层次上，我们首先对所有子节点递归地调用函数，然后根据返回值和根节点本身的值得到答案。 这个过程可以看作是**后序遍历**的一种。 

```c++
//递归函数 bottom_up(root)
1. return specific value for null node
2. left_ans = bottom_up(root.left)          // call function recursively for left child
3. right_ans = bottom_up(root.right)        // call function recursively for right child
4. return answers                           // answer <-- left_ans, right_ans, root.val
```

```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==nullptr) return 0;
        int left_depth=maxDepth(root->left);
        int right_depth=maxDepth(root->right);
        return max(left_depth,right_depth)+1;
    }
};
```

#### 7.总结

了解递归并利用递归解决问题并不容易。

当遇到树问题时，请先思考一下两个问题：

1. 你能**确定一些参数**，**从该节点自身解决出发寻找答案**吗？
2. 你可以使用这些参数和节点本身的值来**决定什么应该是传递给它子节点的参数**吗？

如果答案都是肯定的，那么请尝试使用 “`自顶向下`” 的递归来解决此问题。

或者你可以这样思考：对于树中的任意一个节点，如果你知道它子节点的答案，你能计算出该节点的答案吗？ 如果答案是肯定的，那么 “`自底向上`” 的递归可能是一个不错的解决方法。

#### 8.对称二叉树的判断

```c++
//递归
//自底向上是不行的。因为子树的对称性跟母树的对称性没有关系，母树对称，它的两个字数可能不对称。
//所以应该考虑自顶向下
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr) return true;
        return ismirror(root->left,root->right);
    }
private:
    bool ismirror(TreeNode* tr1,TreeNode* tr2)
    {
        if(tr1==nullptr && tr2==nullptr) return true;
        if(tr1==nullptr || tr2==nullptr) return false;
        
        if(tr1->val!=tr2->val) return false;
        return ismirror(tr1->left,tr2->right) && ismirror(tr1->right,tr2->left);
    }
};
```

```c++
//迭代。不要把当前的left、right作为判断条件来return。条件判断都应该尽可能写成当前的结点的。
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr) return true;
        queue<TreeNode*> q;
        q.push(root->left);
        q.push(root->right);
        while(!q.empty())
        {
          	TreeNode* tr1=q.front();
            q.pop();
            TreeNode* tr2=q.front();
            q.pop();
            if(tr1==nullptr&&tr2==nullptr) continue;
            if(tr1==nullptr || tr2==nullptr) return false;
            if(tr1->val!=tr2->val) return false;
            q.push(tr1->left);
            q.push(tr2->right);
            q.push(tr1->right);
            q.push(tr2->left);                               
        }
        return true;
    }
};
```

#### 9.路径总和

```c++
//递归。自上而下的思想。注意，一定要用root->left==nullptr && root->right==nullptr这个条件判断。想单凭root==nullptr条件下，去判断sum==root->val是不行的，因为有可能 【1,2】。这种结点，问有sum=1的结点吗。单凭root==nullptr，去判断sum==root->val，就会返回ture，从而出现解题错误
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if(root==nullptr) return false;
        if(root->left==nullptr && root->right==nullptr) return sum==root->val;
        return hasPathSum(root->left,sum-root->val)|| hasPathSum(root->right,sum-root->val);
    }
            
};
```

```c++
//迭代。在前序遍历基础上，加了个stack存储减去当前节点后还剩的sum
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if(root==nullptr) return false;
        TreeNode*T=root;
        stack<int> stacksum;
        stack<TreeNode*> s;
        int nowsum=sum;
        while(T||!s.empty())
        {
            
            while(T)
            {
                s.push(T);
                nowsum-=T->val;
                stacksum.push(nowsum);
                if(T->left==nullptr && T->right==nullptr && nowsum==0)
                    return true;
                T=T->left;
            }
            
            T=s.top();
            s.pop();
            nowsum=stacksum.top();
            stacksum.pop();        
            T=T->right;
        }
        return false;   
    }
};
```

#### 10.从前序和中序遍历序列构造二叉树

```c++
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        mypreorder=preorder;
        myinorder=inorder;
        if(preorder.size()==0 || inorder.size()==0) return nullptr;
        TreeNode* res=helper(0,preorder.size()-1,0,inorder.size()-1);
        return res;
    }
private:
    vector<int> mypreorder;//存下来，省的老往里边传参
    vector<int> myinorder;
    TreeNode* helper(int p_s,int p_e,int i_s,int i_e)
    {//p_s:preorder的start位置。 p_e:preorder的end位置。左右都是闭区间
        //构建根节点
        TreeNode* root=new TreeNode(mypreorder[p_s]);
        root->left=nullptr;root->right=nullptr;

        //在中序序列中寻找根节点
        int i=i_s;//i代表中序序列中，查找到的根节点下标
        while(i<=i_e && myinorder[i]!=mypreorder[p_s]) i++;
        int left_l=i-i_s;
        int right_l=i_e-i;

        if(left_l>0)
        {
            root->left=helper(p_s+1,p_s+left_l-1,i_s,i-1);//不会出现i=i_s的情况，因为有left_1>0卡着，所以可以放心大胆的写i-1
        }
        if(right_l>0)
        {
            root->right=helper(p_s+left_l+1,p_e,i+1,i_e);
        }
        return root;
    }
};
```

#### 11.从中序和后序序列构造二叉树

```C++
//也是自上向下递归。与上一题的区别就是母结点每次都是postorder的最后一个元素
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if(inorder.size()==0 || postorder.size()==0) return nullptr;
        myinorder=inorder;mypostorder=postorder;
        
        TreeNode* res=helper(0,inorder.size()-1,0,postorder.size()-1);
        return res;
    }
private:
    vector<int> myinorder;
    vector<int> mypostorder;
    TreeNode* helper(int in_s,int in_e,int po_s,int po_e)
    {
        TreeNode* root=new TreeNode(mypostorder[po_e]);
        root->left=nullptr;root->right=nullptr;
        
        int i=in_s;
        while(i<=in_e && myinorder[i]!=mypostorder[po_e]) i++;
        
        int left_len=i-in_s;
        int right_len=in_e-i;
        
        if(left_len>0) root->left=helper(in_s,i-1,po_s,po_s+left_len-1);
        if(right_len>0) root->right=helper(i+1,in_e,po_s+left_len,po_e-1);
        return root;
    }
};
```

#### 12.填充每个节点的下一个右侧节点指针

```c++
//迭代，层序遍历
class Solution {
public:
    Node* connect(Node* root) {
        if(root==nullptr) return nullptr;
        queue<Node*> q;
        q.push(root);
        Node* last=root;
        Node* temp=nullptr;
        while(!q.empty())
        {
            int size=q.size();
            for(int i=0;i<size;i++)
            {
                temp=q.front();
                last->next=temp;
                if(i==0) last->next=nullptr;
                last=temp;
                q.pop();
                if(temp->left) q.push(temp->left);
                if(temp->right) q.push(temp->right);
            }
            
        }
        last->next=nullptr;
        return root;
        
    }
};
```

#### 13.二叉树的最近公共祖先

```c++
不会
```

#### 14.验证二叉搜索树

```c++
//中序遍历，迭代，一有false就return了
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        TreeNode* T=root;
        stack<TreeNode*> s;
        long last=long(INT_MIN)-1;
        while(T || !s.empty())
        {
            while(T)
            {
                s.push(T);
                T=T->left;
            }
            T=s.top();
            s.pop();
            if(last>=T->val) return false;
            last=T->val;
            T=T->right;
        }
        
        return true;
    }
};

//递归，效率不好，只能遍历整个树才停下来
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        res=true;
        last=double(INT_MIN)-1;
        helper(root);
        return res;
    }
private:
    bool res;
    double last;
    void helper(TreeNode* root)
    {
        if(root==nullptr) return;
        helper(root->left);
        if(last>=root->val) res=false;
        last=root->val;
        helper(root->right);
    }
};
```

#### 15.二叉搜索树迭代器

```c++
//就是个中序遍历
class BSTIterator {
public:
    BSTIterator(TreeNode* root) {
        T=root;
    }
    
    /** @return the next smallest number */
    int next() {
        int res=0;
        while(T||!s.empty())
        {
            while(T)
            {
                s.push(T);
                T=T->left;
            }
            T=s.top();
            res=T->val;
            s.pop();
            T=T->right;
            break;
        }
        return res;//
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return (T!=nullptr||!s.empty());
    }
private:
    TreeNode* T;
    stack<TreeNode*> s;
};
```

#### 16.二叉搜索树的搜索操作

```c++
//递归搜索
class Solution {
public:
     TreeNode* searchBST(TreeNode* root, int val) {
     	if(root==nullptr) return nullptr;//查找失败     
     	if(val<root->val) return searchBST(root->left,val);//在左子树中继续查找
     	else if(val>root->val) return searchBST(root->right,val);//在右子树中继续查找
     	else return root;//查找成功，返回节点位置
    }
};
```

```c++
//迭代搜索
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root)
        {
            if(val<root->val) root=root->left;
            else if(val>root->val) root=root->right;
            else return root;
        }
        return nullptr;
    }
};
```

#### 17.二叉搜索树的插入操作

```c++
//递归
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        
        if(root==nullptr)//空节点处插入
        {
            TreeNode* temp=new TreeNode(val);
            return temp;
        }
        //开始找要插入元素的位置
        if(val<root->val)
            root->left=insertIntoBST(root->left,val);//递归插入左子树
            else if(val>root->val)
                root->right=insertIntoBST(root->right,val);//递归插入右子树
                else ;//else就是X已经存在，不用插入了，什么都不做
        return root;
    }
};
```

#### 18.二叉搜索树的删除操作

```c++
//容易理解的版本
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(root==nullptr) return nullptr;
        if(key<root->val) root->left=deleteNode(root->left,key);//左子树递归删除
        else if(key>root->val) root->right=deleteNode(root->right,key);//右子树递归删除
        else//找到要删除节点了 
        {
            if(root->left && root->right) //要删除的节点有左右两棵子树
            {
                int temp=findmin(root->right);//在右子树找到最小值
                root->val=temp;
                root->right=deleteNode(root->right,temp);//右子树中删除这个节点
                return root;
            }
            else if(root->right!=nullptr)//只有右子树，左子树为空
            {
                TreeNode* temp=root;
                root=root->right;
                delete temp;
                return root;
            }
            else if(root->left!=nullptr)//只有左子树，右子树为空
            {
                TreeNode* temp=root;
                root=root->left;
                delete temp;
                return root;
            }
            else //叶子节点
            {
                delete root;
                root=nullptr;
                return root;
            }

        }
            return root;
    }
private:
    int findmin(TreeNode* root)
    {
        if(root==nullptr) return 0;
        while(root->left)//一直向左找
        {
            root=root->left;
        }
        return root->val;

    }
};
```

```c++
//精简版
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(root==nullptr) return nullptr;
        if(key<root->val) root->left=deleteNode(root->left,key);//左子树递归删除
        else if(key>root->val) root->right=deleteNode(root->right,key);//右子树递归删除
        else//找到要删除节点了 
        {
            TreeNode* temp_node=nullptr;
            if(root->left && root->right) //要删除的节点有左右两棵子树
            {
                int temp=findmin(root->right);
                root->val=temp;
                root->right=deleteNode(root->right,temp);
            }
            else if(root->right!=nullptr)
            {
                temp_node=root;
                root=root->right;
            }
            else if(root->left!=nullptr)
            {
                temp_node=root;
                root=root->left;
            }
            else //叶子节点
            {
                temp_node=root;
                root=nullptr;
            }
            delete temp_node;

        }
            return root;
    }
private:
    int findmin(TreeNode* root)
    {
        if(root==nullptr) return 0;
        while(root->left)
        {
            root=root->left;
        }
        return root->val;

    }
};
```

#### 19.平衡二叉树的判断

```c++
//层次遍历，每个结点判断是否满足条件
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if(root==nullptr) return true;
        queue<TreeNode*>q;
        q.push(root);
        while(!q.empty())
        {
            TreeNode* temp=q.front();
            q.pop();
            if(abs(height(temp->left)-height(temp->right))>1) return false;
            if(temp->left) q.push(temp->left);
            if(temp->right) q.push(temp->right);
            
        }
        return true;
        
    }
private:
    int height(TreeNode* root)
    {
        if(root==nullptr) return 0;
        return max(height(root->left),height(root->right))+1;
    }
};
```

#### 20.将有序数组转换为二叉搜索树

```c++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        if(nums.size()==0) return nullptr;
        mynums=nums;
        
        return helper(0,nums.size()-1);
    }
private:
    vector<int> mynums;
    TreeNode* helper(int start,int end)
    {
        if(start>end) return nullptr;
        int mid=(start+end)/2;//每次构造mid节点
        TreeNode* root=new TreeNode(mynums[mid]);
        root->left=helper(start,mid-1);
        root->right=helper(mid+1,end);
        return root;
    }
};
```

#### 21.最大层内元素和（被归为graph标签）

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
//简简单单一个层序遍历
class Solution {
public:
    int maxLevelSum(TreeNode* root) {
        if(root==nullptr) return 0;
        queue<TreeNode*> q;
        q.push(root);
        double max_sum=DBL_MIN;//记录最大sum
        int index=0;//记录当前是第几层
        int res=0;//记录最大和的层数
        while(!q.empty())
        {
            index++;
            int size=q.size();//用于区分每一层
            int sum=0;//记录本层sum
            for(int i=0;i<size;i++)//一个for就是一层
            {
                TreeNode* temp=q.front();
                q.pop();
                sum+=temp->val;
                if(temp->left!=nullptr) q.push(temp->left);
                if(temp->right!=nullptr) q.push(temp->right);               
            }
            if(sum>max_sum)//
            {
                max_sum=sum;
                res=index;
            }
        }
        return res;
    }
};
```



## 四、堆

### 1.堆的概念

优先队列（Priority Queue）：特殊的”队列“，取出元素的顺序是依照元素的优先权（关键字）大小，而不是进入队列的先后顺序。

堆的两个特性：

- 结构性：用数组表示的完全二叉树。
- 有序性：任一结点的关键字是其子树所有结点的最大值（或最小值）。
  - 最大堆：MaxHeap，也称大顶堆：最大值
  - 最小堆：MinHeap，也成小顶堆：最小值

最大堆使用完全二叉树进行存储，它的任何一个结点值都比它的左右子树任何一个结点值大。也就是每个结点是以它为根的子树里最大的。从根节点到任意节点路径上结点序列是有序的，从大到小。（注意，仅是每一个路径。）

![image.png](https://i.loli.net/2020/03/06/CbX8wUk3nGce5BF.png)

### 2.堆的插入

```c++
//最大堆的创建
typedef struct HeapStruct* MaxHeap;
struct HeapStruct{
    ElementType* Elements;//存储堆元素的数组
    int Size;//堆的当前元素个数
    int Capacity;//堆的最大容量
}
```

```c++
//创建一个最大堆
MaxHeap Create(int MaxSize)
{
    MaxHeap H=malloc(sizeof(struct HeapStruct));
    H->Elements=malloc((MaxSize+1) * sizeof(ElementType));//+1是+了一个哨兵
    H->Size=0;
    H->Capacity=MaxSize;//可以再存MaxSize个数
    H->Elements[0]=MaxData;//定义“哨兵”为大于堆中所有可能元素的值，便于以后更快操作。
    return H;
}
```

```c++
//插入算法:将 新增结点 插入到 从其父节点到根节点的有序序列 中
//有哨兵的好处：省去了while循环里判别i>1
void Insert(MaxHeap H,ElementType item)
{
    //将元素item插入最大堆H，其中H->Elements[0]已经定义为哨
    if(IsFull(H))
    {
        cout<<"最大堆已满"<<endl;
        return;
    }
   int i=++ H->Size;//i指向插入之后，堆中最后一个元素的位置。假装已经把item插入到这个位置了
    while(H->Elements[i/2]<item)// i/2的位置就是i结点的父结点。如果item比父结点的值大的话
    {
        H->Elements[i]=H->Elements[i/2];//交换元素值
        i/=2；//继续往下
    }
    H->Elements[i]=item;//这是真实的Item插入，这样做的目的是省去了上边的swap，更高效。
}
```

### 3.堆的删除

```c++
//删除算法：
//最大堆的删除：取出根节点（最大值）元素，同时删除堆的一个结点。
ElementType DeleteMax(MaxHeap H)
{
    //从最大堆H中取出键值为最大的元素，并删除一个结点
    int Parent,Child;
    ElementType MaxItem,temp;
    if(IsEmpty(H)){
        cout<<"最大堆已为空"<<endl;
        return;
    }
    MaxItem=H->Elements[1];//取出根节点最大值
    //用最大堆中最后一个元素从根节点开始向上过滤下层结点
    temp=H->Elements[H->Size--];//完全二叉树的最后一个元素，Size少一个
    for(Parent=1;Parent*2<=H->Size;Parent=Child)
    {
        Child=Parent*2;
        if((Child!=H->Size) && (H->Elements[Child] < H->Elements[Child+1]))
            Child++;//Child指向左右子结点的较大者。
        if(temp>=H->Elements[Child]) break;
        else
            H->Elements[Parent]=H->Elements[Child];
    }
    H->Elements[Parent]=temp;
    return MaxItem;
}
```

### 4.堆的建立

建立最大堆：将已经存在的N个元素按最大堆的要求存放在一个一维数组中

```c++

```

### 5.leetcode

#### 1.数据流中的第K大元素

```c++
class KthLargest {
public:
    KthLargest(int k, vector<int>& nums) {
        this->k=k;
        for(int i:nums)
        {
            add(i);
        }
    }
    
    int add(int val) {
        pri_que.push(val);
        if(pri_que.size()>k)//队列是从小到大排序的，所以一直只保留了较大的几个数
        {
            pri_que.pop();
        }
        return pri_que.top();
    }
private:
    priority_queue<int, vector<int>, greater<int>> pri_que;//小顶堆，即最小堆，从顶到尾递增
    int k;
};
```

#### 2.最后一块石头的重量

```c++
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        priority_queue<int> pq;//大顶堆
        int first=0,second=0;
        for(int i:stones) pq.push(i);//入堆
        while(!pq.empty())
        {
            first=pq.top();
            pq.pop();
            if(pq.empty()) break;
            second=pq.top();
            pq.pop();
            if(first!=second){
                pq.push(abs(first-second));
            }
            else first=0;

        }
        return first;
    }
};
```

## 五、哈夫曼树

### 1.哈夫曼树的定义

带权路径长度（WPL）：设二叉树有n个叶子结点，每个叶子结点带有权值wk，从根节点到每个叶子结点的长度为lk，则每个叶子结点的带权路径长度之和就是：WPL=∑wk*lk

**最优二叉树**或者**哈夫曼树**：WPL最小的二叉树。

### 2.哈夫曼树的构造

权值从小到大排序，排完序之后把权值（频率）最小的两个结点并在一起形成一颗二叉树。这个二叉树的权值就是并在一起的权值的和。

例子：

权值：1 2 3 4 5

![image.png](https://i.loli.net/2020/03/10/wPcEkKz4LbnNDR7.png)

其实红箭头那里是没有值的，都是类似if判断的东西。

```c++
typedef struct TreeNode* HuffmanTree;
struct TreeNode{
    int Weight;
    HuffmanTree Left,Right;
}
HuffmanTree Huffman(MinHeap H)//输入一个小顶堆
{
    //假设H->Size个权值已经存在H->Elements[]->Weight里
    HuffmanTree T;
    BuildMinHeap(H);//将H->Elements[]按权值调整为最小堆
    for(int i=1;i<H->Size;i++)//做H->Size-1次合并
    {
        T=malloc(sizeof(struct TreeNode));//建立新节点
        T->Left=DeleteMin(H);//从最小堆中拿一个结点出来，作为新T的左子结点
        T->Right=DeleteMin(H);//从最小堆中删除一个结点，作为新T的右子结点
        T->Weight=T->Left->Weight+T->Right->Weight;
        Insert(H,T);//将新T插入最小堆
    }
    T=DeleteMin(H);
    return T;
}
```

### 3.哈夫曼树的特点

- 没有度为1的结点；
- n个叶子结点的哈夫曼树共有2n-1个结点；
  - 因为n2=n0-1;叶结点n0=n个，n2=n-1，又没有n1，所以一共2n-1
- 哈夫曼树的任意非叶结点的左右子树交换后仍是哈夫曼树。
- 同一组权值，可能产生不同构的两颗哈夫曼树，但是它们的WPL一样
  - 例如：
  - ![image.png](https://i.loli.net/2020/03/10/w9Wb2mDFaRTvqLO.png)

### 4.哈夫曼编码

![image.png](https://i.loli.net/2020/03/10/M8JAg2eQjl3ODqy.png)

![image.png](https://i.loli.net/2020/03/10/aLPEkrmCoc1Y3XS.png)

## 六、集合

### 1.集合的概念

![image.png](https://i.loli.net/2020/03/10/Et2xS3YyCL6G9fj.png)

数组存储形式：

```c++
typedef struct{
    ElementType Data;
    int Parent;
}SetType;
```

![image.png](https://i.loli.net/2020/03/10/iRgJwfd6kbWVNXG.png)

Parent指的是该结点的父亲在数组中的下标。当这个结点是根节点的时候，parent=-1;

### 2.集合运算

#### 1.查找操作

```c++
//查找操作
//给你一个元素X，查询它属于哪个set（用根节点表示）
int Find(SetType S[],ElementType X)//SetType是一个结构体，S是以结构体为元素的数组
{
    //在数组S中查找值为X的元素所属的集合
    //MaxSize是全局变量，为数组S的最大长度
    int i;
    for(i=0;i<MaxSize && S[i].Data！=X;i++)//找到后，i就锁在那儿了
	if(i>=MaxSize) return -1;//所有元素中并未包含X
    for(;S[i].Parent>=0;i=S[i].Parent);//以当前节点，往上迭代找父节点，因为属于哪个set是由根节点的下标所代表的。
    return i;//找到X所属集合，返回树根节点在数组S中的下标
}    
```

#### 2.并运算

- 分别找到X1和X2两个元素所在集合树的根节点；
- 如果它们不同跟，则将其中一个根节点的父节点指针设置成另一个根节点的数组下标

```c++
//原始Union，这样并的话，树会越并越高，使得Find效率下降（主要是往上迭代找parent的时候会非常慢）
void Union(SetType S[],ElementType X1,ElementType x2)
{
    int Root1,Root2;
    Root1=Find(S,x1);
    Root2=Find(S,x2);
    if(Root1!=Root2) S[Root2].Parent=Root1;
}
```

```c++
//为了改善合并以后的查找性能，可以采用小的集合合并到相对大的集合中。
//将根节点的Parent分量，不再设置成统一的-1.而是负多少，绝对值代表这棵树的结点数
void Union(SetType S[],ElementType X1,ElementType x2)
{
    int Root1,Root2;
    Root1=Find(S,x1);
    Root2=Find(S,x2);
    if(Root1!=Root2) 
    {
        if(abs(S[Root1].Parent)>abs(S[Root2].Parent))
        {//root1的结点比较多，root2挂载在root1上
            S[Root1].Parent=Root2;
        }
        else
        {//root2的结点比较多，root1挂载到root2上
            S[Root2].Parent=Root1;
        }
    }
}
```

### 3.并查集的构造

简单的数组构造，比上边的结构体构造写起来简单

```c++
//Find函数，查找x结点的根。（或者说查找它所在的集合）
int Find(int x)
{
    int fx=x;
    while(parent[fx]!=fx){//查找根
        fx=parent[fx];
    }//执行完while循环后，fx就是x的根节点下标
    
    while(parent[x]!=fx)//路径压缩，就是一个将所有结点与其根进行直接连接的过程。这个过程不仅仅将x结点与其根连接,而且将x到根结点路径上所有的结点都与根进行连接(实际上它们也应该连接上,仔细想想,不是么?)这样找一个结点,可以压缩这个结点到根的路径上的所有结点的路径,相当于找到路径上所有结点的根了.
    {
        int temp=parent[x];//保存x的上一级结点
        parent[x]=fx;//x直接挂在到根上
        x=temp;//准备下一轮把x的上一级结点也挂在到根上
    }
    return fx;
}
```

```c++
//Union函数：
//Union函数是在Find函数的基础上使用的,目的是将集合合并,实际上就是将一个集合的根结点连到另一个集合根结点上即可.
void Union(int x,int y)//合并集合
{
    int root_x=Find(x);
    int root_y=Find(y);
    if(root_x!=root_y)
        parent[root_x]=root_y;
}
```

```c++
void init()     //初始化,每个节点自成一个集合;
{
    for(int i = 1; i <= n; i++)
        pre[i] = i;
}
```



### 3.leetcode

#### 求两个数组的交集

```c++
//求两个数组的交集
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        set<int> s1(nums1.begin(),nums1.end());
        set<int> s2(nums2.begin(),nums2.end());      		       			set_intersection(s1.begin(),s1.end(),s2.begin(),s2.end(),insert_iterator<vector<int>>(res,res.begin()));
        return res;
    }
};
```



## 七、图

### 1.图的定义

![image.png](https://i.loli.net/2020/03/10/yRuDd9UVtHGQmxg.png)

类型名称：图（Graph）

数据对象集：G（V,E）由一个非空的有限顶点集合V和一个有限边集合E组成。（注意，一个图可以一条边都没有，但至少要有一个顶点）

**带权重的图叫网络**

![image.png](https://i.loli.net/2020/03/11/exfYnDvVhgJzj31.png)

### 2.图的表示

#### 1.邻接矩阵表示法

对于无向图：

![image.png](https://i.loli.net/2020/03/11/YxGjZR4XpWBwPfg.png)

**对于无向图，由于是对称矩阵，可以只存下三角（粉色区域）， 省一半空间。换用一维数组存储**

![image.png](https://i.loli.net/2020/03/11/hnstrm1HwBeXcAp.png)

网络里边，如果没有边，不可以简单的用0为权值表示？如何表示？（0或者是无穷大）

**邻接矩阵表示法：好处**

- 直观、简单、好理解
- 方便检查任意一对顶点间是否存在边
- 方便找任一顶点的所有“邻接点”（有边直接相连的顶点）
  - 通过for循环，对于无向图，矩阵是对称的，只要扫描一行，所有不是0的顶点，就是有边与它直接连的
  - 对于有向图，矩阵就可能不是对称的，除了扫描一行，还要扫描一列
- 方便计算任一顶点的“度”。（从该点出发的边数为“出度”，指向该点的边数为“入度”）
  - 对于无向图，对应行（或者列）的非0元素的个数
  - 有向图，对应行非0元素的个数是”出度“；对应列非0元素的个数是”入度“。

**邻接矩阵表示法：坏处**

- 浪费空间：对于稀疏图（点很多而边很少）有大量无效元素
  - 对于稠密图（特别是完全图）还是很合算的
- 浪费时间：比如想统计稀疏图里边一共有多少条斌，时间上会有大量浪费

#### 2.邻接表表示法

![image.png](https://i.loli.net/2020/03/11/PTJdKwAx3EULZnD.png)

**邻接表**

- 方便找任一顶点的所有“邻接点”
- 节约稀疏图的空间
  - 需要N（=9）个头指针+2E（2*17=34）个结点（N个顶点，E条边。每个结点至少2个域，1/0、next，如果考虑权重的话，可能还得再加一个域）
  - 2E个结点，是因为无向图，每个边被存了两次，E是边数
- 方便计算任一顶点的“度”
  - 对于无向图：是的，只要访问该顶点的链表，数一数链表上串了多少边即可
  - 对于有向图：只能方便计算“出度”；需要构造“逆邻接表”（存指向自己的边）来方便计算”入度“
- 并不方便检查任意一对顶点间是否存在边（或者说不如矩阵方便）

### 3.图的遍历

每个顶点访问一遍，不能有重复的访问

#### 1.DFS：深度优先搜索

depth first search//每次把它视力范围内的点亮，再返回

```c++
//递归
void DFS(Vertex V)
{
    visited[V]=true;
    for(V的每个邻接点 W)
        if(!visited[W])
            DFS(W);
}
```

```c++
//布尔型数组Visited[]初始化成false。栈实现
//如果用栈实现的话，应该以出栈顺序作为遍历顺序。这跟二叉树不一样。二叉树以出栈顺序是中序排列。而这里以出栈顺序应该是和二叉树中的先序是一致的。
//换句话说，这与树的前序遍历的非通用版本是一致的
void DFS(Vertex v)
{    
    Stack  sta = MakeStack(MAX_SIZE);
    Push(sta, v);
    while (!Empty(sta))
    {
        Vertex w = Pop(sta);
        visited[w]=true;
        for each u adjacent to w
        {            
            if (!Visited[u]) 
            {
                Push(sta, u);           
            }
        }
    }
}
```

若有N个顶点、E条边，时间复杂度

- 用邻接表存储图，有O（N+2E）；//就链表走一圈，每个结点一次，每条边一次
- 用邻接矩阵存储图，有O（N^2）；//每个结点要访问一整行才能只能邻接点，N*N

#### 2.BFS：广度优先搜索

Breadth First Search

```c++
//入队时访问
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
```

若有N个顶点、E条边，时间复杂度

- 用邻接表存储图，有O（N+2E）；//每个顶点入队一次，每一个邻接点即每个边被访问了一次
- 用邻接矩阵存储图，有O（N^2）；//点亮入队时，每个结点要访问一整行才能只能邻接点，N*N

#### 3.为什么需要两种不同的遍历

两种遍历各有特点

#### 4.图不连通怎么办

- **连通**：如果从V到W存在一条（无向）路径，则称v和w是连通的。

- **路径**：V到W的路径是一系列顶点{V,v1,v2,....,vn,W}的集合，其中任一对相邻的顶点间都有图中的边。**路径的长度**是路径中的边数（如果带权，则是所有边的权重和）。如果v到w之间的所有顶点都不同，则称**简单路径**。（比如V,v1,v2,v1,W，这就是不简单的路径，因为v1重复了两次，或者说存在回路）

- **回路**：起点等于终点的路径。

- **连通图**：图中任意两顶点均联通

- **连通分量**：**无向图**的**极大**连通子图
  
  - ![image.png](https://i.loli.net/2020/03/11/XRS9VxqLuNsahZK.png)
  - 极大的含义：
  -  极大顶点数：我在这个连通分量里，再加一个顶点就不连通了（连通分量包含的是可以让它连通的所有顶点，全都包在里边） 比如：ABCD，再加一个顶点E，它就不联通了
  - 极大边数：连通分量子图里边，所有的顶点相连的所有的边都要包在里头。比如：右上的ABCD，虽然顶点都在，BC边没有包在里边，不算联通分量
  
- 对于有向图：

- 强联通：有向图中顶点V和W之间存在双向路径（不一定是同一条），则称V和W是强连通的。

- 强连通图：有向图中任意两顶点均强联通

- 强联通分量：有向图的极大强联通子图

- ![image.png](https://i.loli.net/2020/03/11/xisPTnzmX42WloH.png)

- 比如：上图是一个非强连通图，但是有两个强联通分量

- 弱联通：非强联通，但是抹掉方向成为无向图后是连通的

- 每调用一次DFS(V)或者BFS(V)，就是把V所在的连通分量遍历了一遍。只是顺序不同

- 对于不连通的图，怎么全遍历呢？

- ```c++
  void ListComponents(Graph G)
  {
      for(each V in G)//对于每个结点，如果它没有被访问过，就去把它所在的连通子图全部访问一遍
          if(!visited[V])
          {
              DFS(V);//or BFS(V)
          }
  }
  ```

#### 5.例题：拯救007

```c++
void Save007(Graph G)
{
	for(each V in G)
    {
        if(!visited[V]&&FirstJump[V]){//把第一次跳跃够得着的结点，挨着调用DFS
            answer=DFS(V);
            if(answer==YES) break;
        }
    }
    if(answer=="YES") output("YES");
    else output("NO");
}
```

```c++
//经典DFS
void DFS(Vertex V)
{
    visited[V]=true;//先踩到鳄鱼头上
    for(V的每个邻接点W)//它等价于这个鳄鱼还没有踩过并且我跳的到 （each W in G) ,Jump(V,W)
        if(!visited[W])//如果这个鳄鱼还没有被踩过的话
            DFS[W];//一脚踩上去
}
//改进DFS
int DFS(Vertex V)
{
    visited[V]=true;//一脚踩上去
    if(IsSafe(V)) answer=YES;//从这个地方能否上岸
    else//这条鳄鱼不行，看看其他的鳄鱼
    {
        for(each W in G)//这相当于v的所有邻接点
            if(!visited[W] && Jump(V,W))
            {
                answer=DFS(W);
                if(answer==YES) break;
            }
    }
    return answer;
}
```

#### 6.例题：六度空间

```c++
void SDS()
{
	for(each V in G)
    {
        count=BFS(V);//累计访问总结点数
        Output(count/N);
    }
}
```

```c++
//没记录层数的BFS
int BFS(Vertex V)//遍历过程中，记录下来累计总结点数
{
    visited[V] = true;
    count=1;
    Enqueue(V,Q);
    while(!IsEmpty(Q)){
        V=Dequeue(Q);
        for(V的每个邻接点W){
        	if(!visited[W]){
                visited[W]=true;
                ENqueue(W,Q);
                count++;
            }
        }
    }
    return  count;
}
```

```c++
//带有记录层数的BFS
int BFS(Vertex V)
{
    visited[V]=true;count=1;
    level=0;//当前顶点所在的层数
    last=V;//当前这一层访问的最后一个结点是谁
    Enqueue(V,Q);
    while(!IsEmpty(Q))
    {
        V=Dequeue(Q);
        for(V的每个邻接点W)
        {
            if(!visited[W])
            {
                visited[W]=true;
                Enqueue(W,Q);
                count++;
                tail=W;
            }
        }
        if(V==last){//如果弹出来的V是上一层的最后一个，说明该层遍历要结束了。层数+1
            level++;last=tail;
        }
        if(level==6) break;      
    }
    return count;
}
```

### 4.如何建立图

#### 1.邻接矩阵建立图

```c++
//邻接矩阵表示的图节点结构
struct GNode{
    int Nv;//顶点数
    int Ne;//边数
    WeightType G[MaxVertexNum][MaxVertexNum];
    //DataType Data[MaxVertexNUM][MaxvertexNum];//可选项，存顶点的数据
};
typedef struct GNode* PtrToGNode;
typedef PtrToGNode MGraph;//以邻接矩阵存储的图类型
```

```c++
//初始化一个有VertexNum个顶点但没有边的图
typedef int Vertex;//用顶点下标表示顶点，为整型。
MGraph CreateGraph(int VertexNum)
{
    MGraph Graph;
    Graph=(MGraph)malloc(sizeof(struct GNode));//申请内存空间
    Graph->Nv=VertexNum;//VertexNum个顶点
    Graph->Ne=0;//0条边
    
    //注意：这里默认顶点编号从0开始，到（Graph->Nv-1）
    for(Vertex V=0;V<Graph->Nv;V++)//两个for循环将邻接矩阵全部置为0，即没有边
        for(Vertex W=0;W<Graph->Nv;W++)
            Graph->G[V][W]=0;//或infinity
    return Graph;
}
```

```c++
//向MGraph中插入边
struct ENode{
    Vertex V1,V2;//有向边<v1,v2>
    WeightType Weight;//有权图需要定义权重
};
typedef struct ENode* PtrToENode;
typedef PtrToENode Edge;
void InsertEdge(MGraph Graph,Edge E)
{
    //插入边<V1,V2>。对于邻接矩阵，只需要把该边权重赋值给二维数组对应的位置即可。
    Graph->G[E->V1][E->V2]=E->Weight;
    //若是无向图，还要插入边<V2,V1>
    Graph->G[E->V2][E->V1]=E->Weight;
}
```

```c++
//完整地建立一个MGraph
	/*输入格式
	Nv Ne
    V1 V2 Weight
    .....*/

MGraph BuildGraph()
{
    int Nv;
    scanf("%d",&Nv);//先把顶点数读进来，有了顶点数就可以初始化一个图了。即Nv*Nv的二维数组
    MGraph Graph=CreatGraph(Nv);//初始化成有顶点，但是一条边都没有的图
    scanf("%d",&(Graph->Ne));//边数读进来，直接保存到结构体内
    if(Graph->Ne!=0)//边数不为0
    {
        Edge E=(Edge)malloc(sizeof(struct ENode));//临时的边，用来存储scanf进来的数据
        for(int i=0;i<Graph->Ne;i++)
        {
            scanf("%d %d %d",&E->V1,&E->V2,&E->Weight);//按照输入样式，依次读入数据，存入E中
            InsertEdge(Graph,E);//把这个边插入图中
        }
    }
    //可选项，如果顶点还存数据的话即GNode里边的DataType Data[MaxVertexNUM][MaxvertexNum];
    /*for(Vertex V;V<Graph->Nv;V++)
    	scanf("%c",&(Graph->Data[V]));*/
    return Graph;
}
```

```c++
//简洁版的整体图,不封装的，不使用结构体的
//完整地建立一个MGraph
	/*输入格式
	Nv Ne
    V1 V2 Weight
    .....*/

int G[MAXN][MAXN],Nv,Ne;//全局变量
void BuildGraph()
{
    scanf("%d",&Nv);//读入顶点数
    //int G[Nv][Nv];//已经在全局变量声明过G了
    //CreateGraph
    for(int i=0;i<Nv;i++)
        for(int j=0;j<Nv;j++)
            G[i][j]=0;//或者INFINITY
       
    scanf("%d",&Ne);//读入边数
    int v1,v2,w;//临时的顶点坐标v1 v2，权重w
    for(int i=0;i<Ne;i++)//读入边数次
    {
        scanf("%d %d %d",&v1,&v2,&w);
        //InsertEdge
        G[v1][v2]=w;//将矩阵对应位置置为权值
        G[v2][v1]=w;//无向图
    }
}
```

#### 2.邻接表建立图

```c++
//以邻接表方式存储的图类型

struct AdjVNode{//相当于链表里的每个结点
    Vertex Adjv;//邻接点下标
    WeightType Weight;//边权重。从母结点，指向我这个结点的边的权重
    PtrToAdjVNode Next;//指向下一个结点
}
Struct AdjVNode* PtrToAdjVNode;

typedef struct Vnode{//这相当于每一条链
    PtrToAdjVNode FirstEdge;//这是链表（或者说指针）
    //DataType Data;//这是可选的Data域
}AdjList[MaxVertexNum];//邻接表本来应该是指针数组，这里换成了结构体数组。每个数组元素就是个链表（这里是结构体）

struct GNode{//它就包含了一个图的全部信息
    int Nv;//顶点数
    int Ne;//边数
    AdjList G;//邻接表。它本来应该是一个指针数组，但是这是换成了结构体数组。结构体里边除了指针，还有个data域
}
typedef struct GNode* LGraph;//LGraph与GNode基本一样，就只是换成了指针
```

```c++
//LGraph初始化
//初始化一个有VertexNum个顶点但没有边的图
typedef int Vertex;//用顶点下标表示顶点，为整型
LGraph CreateGraph(int VertexNum)
{
    LGraph Graph=(LGraph)malloc(sizeof(struct GNode));
    Graph->Nv=VertexNum;
    Graph->Ve=0;
    //从0到Nv-1
    for(Vertex V=0;V<Graph->Nv;V++)
        Graph->G[V].FirstEdge=NULL;
    return Graph;
}
```

```c++
//向LGraph中插入边
/*struct ENode{
    Vertex V1,V2;//有向边<v1,v2>
    WeightType Weight;//有权图需要定义权重
};
typedef ENode* Edge;//边的定义*/

void InsetEdge(LGraph Graph,Edge E)
{
    PtrToAdjVNode NewNode;
    //插入边<V1,V2>
    //为V2建立新的邻接点
    NewNode=(PtrToAdjVNode) malloc(sizeof(struct AdjVNode));//链表结点
    NewNode->AdjV=E->V2;//新节点的邻接点下标
    NewNode->Weight=E->Weight;
    //将V2插入V1的表头
    NewNode->Next=Graph->G[E->V1].FirstEdge;//我新建的节点指向母结点的第一个节点
    Graph->G[E->V1].FirstEdge=NewNode;//新建的节点插入表头
    
    //如果是无向图，还要插入边<v2,v1>
    //为V1建立新的邻接点
    NewNode=(PtrToAdjVNode)malloc(sizeof(struct AdjVNode));
    NewNode->AdjV=E->V1;
    NewNode->Weight=E->Weight;
    //将V1插入V2的表头
    NewNode->Next=Graph->G[E->V2].FirstEdge;
    Graph->G[E->V2].FirstEdge=NewNode;
}
```

```c++
//简洁版的整体图,不封装的，不使用结构体的
//完整地建立一个MGraph
	/*输入格式
	Nv 
    V1 V2 Weight
    .....*/
struct AdjVNode{//相当于链表里的每个结点
    Vertex Adjv;//邻接点下标
    WeightType Weight;//边权重。从母结点，指向我这个结点的边的权重
    AdjVNode* Next;//指向下一个结点
}

typedef struct Vnode{//这相当于每一条链
    AdjVNode* FirstEdge;//这是链表（或者说指针）
    //DataType Data;//这是可选的Data域
}

int Nv,Ne;//顶点数，边数
struct Vnode G[MAXN];//邻接表本来应该是指针数组，这里换成了结构体数组。每个数组元素就是个链表（这里是结构体）
void BuildGraph()
{
    scanf("%d",&Nv);//读入顶点数
    //CreateGraph
    for(int i=0;i<Nv;i++)
    {
        G[i].FirstEdge=NULL;
    }
       
    scanf("%d",&Ne);//读入边数
    int v1,v2,w;//临时的顶点坐标v1 v2，权重w
    for(int i=0;i<Ne;i++)//读入边数次
    {
        scanf("%d %d %d",&v1,&v2,&w);
        //InsertEdge插入边<V1,V2>
        AdjVNode* tempnode=(AdjVNode*)malloc(sizeof(struct AdjVNode));//为结点分配内存
        tempnode->Adjv=v2;//邻接点
        tempnode->Weight=w;//母结点指向该节点的边的权值
        tempnode->Next=G[v1].FirstEdge;//我新建的节点指向母结点的第一个节点
        G[v1].FirstEdge=tempnode;//新建的节点插入表头
        
        //如果是无向图，还要重复插入边<v2,v1>
        tempnode=(AdjVNode*)malloc(sizeof(struct AdjVNode));
        tempnode->Adjv=v1;
        tempnode->Weight=w;
        tempnode->Next=G[v2].FirstEdge;
        G[v2].FirstEdge=tempnode;
    }
}
```

## 八、最短路径问题

### 1.概述

在网络中，求两个不同顶点之间的所有路径中，边的权值之和最小的那一条路径。

- 这条路径就是两点之间的最短路径
- 第一的顶点为源点
- 最后一个顶点为源点

单源最短路径问题：从某固定源点出发，求其到所有其他顶点的最短路径

- （有向或者无向）无权图
- （有向或者无向）有权图

多源最短路径问题：求任意两顶点间的最短路径

### 2.无权图的单源最短路径算法

#### 按照路径长度递增（非递减）的顺序找出到各个顶点的最短路径

![image.png](https://i.loli.net/2020/03/14/2vw8QRqX1C7gKIc.png)

```c++
//原始的BFS
void BFS(Vertex S)
{
    visited[S]=true;
    Enqueue(S,Q);
    while(!IsEmpty(Q))
    {
        V=Dequeue(Q);
        for(V的每个邻接点W){
            if(!visited[W]){
                visited[W]=true;
                Enqueue(W,Q);
            }
        }
    }
}
```

```c++
//dist[W]=S到W的最短距离，源点S到各个节点的最短距离
//dist[S]=0
//path[W]=S到W的路上经过的，W的前一个顶点
//S、W说白了就是数组下标,0,1,2...
void Unweighted(Vertex S)
{
    Enqueue(S,Q);
    while(!IsEmpty(Q)){
        V=Dequeue(Q);
        for(V的每个邻接点W){
            if(dist[W]==-1){//没有被访问过
                dist[W]=dist[V]+1;//父结点到源点的最短距离+1
                path[W]=V;//用于从该节点往上溯源
                Enqueue(W,Q);
            }
        }           
    }
}
```

#### 示例

![image.png](https://i.loli.net/2020/03/14/sQHkB4bxoJS9AEW.png)

假设V3是源点，那么经过程序Unweighted(3)之后。得到两个数组。

dist记录的是，从V3到各个顶点（下标1-7代表v1-v7）的最小距离

path记录的是，最小路径的轨迹。比如我想问从源点（V3）到顶点V7的最小距离和最短路径。那么 最小距离即dist[7]=3。最短路径是7<-path[7]=4<-path[4]=1<-path[1]=3<-path[3]=-1;即V3->V1->V4->V7。

### 3.有权图的单源最短路径(Dijkstra)

按照递增的顺序找出到各个顶点的最短路

Dijkstra算法

- 令 S={源点s + 已经确定了最短路径的顶点Vi}
- 对任一未收录的顶点v，定义dist[v]为s到v的最短路径长度，但该路径仅经过s中的顶点。即路径{s->(Vi∈S)->v}的最小长度。（这个最小长度在一直更新，随着S的扩大，最后变成最终的结果）
- 若路径是按照长度递增（非递减）的顺序生成的，则
  - 真正的最短路必须只经过s中的顶点
  - 每次从未收录的顶点中选一个dist最小的收录（贪心）
  - 增加一个v进入S，可能影响另外一个顶点w的dist值（新增的v只有可能影响到v的邻接点，不可能影响到邻接点以外的点）。即只要把v收进集合S的时候，检查一下v的邻接点们，看看它们会不会有谁会得到一个更小的路径（dist）值。
    - dist[w]=min{dist[w],dist[v]+<v,w>的权重}。w指的是，新加进来的的v的所有邻接点（不在集合S中的）

```c++
//不能解决有负边的情况
//收录就意味着，这个点的最短路径已经完全确定了
void Dijkstra(Vertex s)
{
    dist[all vertex]=infinty
    path[all vertex]=-1
    dist[源点]=0
    while(1){
        V=未收录顶点中dist最小者；//要知道第一次进来时，除了源点s，谁也还没收录
        if(这样的V不存在)
            break;
        collected[V]=true;//将V收入集合S
        //收进来以后，看看V会不会影响到它的邻接点们的dist值
        for(V的每个邻接点W)
        {
            if(collected[W]==false){//未收录的邻接点
                if(dist[V]+E<V,W> < dist[W]){
                    dist[W]=dist[V]+E<V,W>;
                    path[W]=V;
                }
            }
        }
    }
}
```

https://www.bilibili.com/video/av18586085?p=83没懂的话看一下例子就懂了。

### 4.多源最短路径算法(Floyd)

#### 方法1：单源最短路算法调用|V|遍

#### 方法2：Floyd算法

```c++
//G[i][j]初始化时，默认应该全部是个很大的值，但不该是无穷大。看题干。（否则，在D[i][k]+D[k][j]时可能会爆掉）
void Floyd()//D[i][j]最终将保存i->j所有路径的最小长度
{
    for(i=0;i<N;i++)
        for(j=0;j<N;j++)
        {
            D[i][j]=G[i][j];//初始化成邻接矩阵，D[i][j]最终将保存i->j的最小长度
            path[i][j]=-1;//记录结点。是指i->k,k->j合并起来最短
        }
    for(k=0;k<N;k++)//k轮更新迭代，其实也是结点
        for(i=0;i<N;i++)
            for(j=0;j<N;j++)
            {
                if(D[i][k]+D[k][j]<D[i][j])
                {
                    D[i][j]=D[i][k]+D[k][j];
                    path[i][j]=k;//i到j的最短路径，是由i->k，k->j的最短路径加起来
                }
            }
}
```

## 九、最小生成树问题

### 1.定义

- 是一棵树
  - 无回路
  - |V|个顶点一定有|V|-1条边（比如7个顶点，那么6条边就可以全连通）
- 是生成树
  - 包含全部顶点
  - |V|-1条边都在图里
  - 向生成树中任加一条边都一定构成回路
- 边的权重和最小
- 最小生成树存在<—>图连通 互为充要条件

![image.png](https://i.loli.net/2020/03/14/dAVHzn2B8JTZWxk.png)

### 2.贪心算法

- 什么是“贪“：每一步都要最好的
- 什么是“好”：权重最小的边
- 需要约束：
  - 只能用图里边有的边
    - 只能正好用掉|V|-1条边（比如7个顶点，那么6条边就可以全连通）
  - 不能有回路

#### prim算法——让一棵小树长大

对于稠密图比较合算，即边很多

```c++
//回顾Dijkstra算法
//不能解决有负边的情况
void Dijkstra(Vertex s)
{
    while(1){
        //dist最小，指的是到源点最小
        V=未收录顶点中dist最小者；//要知道第一次进来时，除了源点s，谁也还没收录
        if(这样的V不存在)
            break;
        collected[V]=true;//将V收入集合S
        //收进来以后，看看V会不会影响到它的邻接点们的dist值
        for(V的每个邻接点W)
        {
            if(collected[W]==false){//未收录的邻接点
                if(dist[V]+E<V,W> < dist[W]){//两个if其实可以合并
                    dist[W]=dist[V]+E<V,W>;
                    path[W]=V;//记录父节点
                }
            }
        }
    }
}
```

```c++
//dist[V]初始化，与源节点直接相连的，就初始化为权重值。不直接相连的，初始化为正无穷。源点初始化为0
//parent[s]初始为-1，源点邻接点初始化为源点 。其他点呢？（正无穷感觉可以，或者-1？可以知道哪些点连通，哪些点不连通？）
void Prim()
{
    MST={s};//一步步收集的是顶点
    while(1){
        //dist指的是，顶点V到已经生成的树的最小距离
        V=未收录顶点中dist最小者;//直接遍历dist数组就行
        if(这样的V不存在) break;
        dist[V]=0;//将V收录进MST
        for(V的每个邻接点W)
        {
            if(dist[W]!=0 && E(V,W)<dist[W])//W未被收录 && V到W的距离小于原始的dist
            {
                dist[W]=E(V,W);//更新w的dist
                parent[W]=W;//更新父节点
            }
        }
    }
    //退出循环有两种情况
    //1.顶点全部收录入MST中，共|v|个顶点
    //2.还有未收录顶点，他们的dist都是无穷大，即说明这个图不连通，那么最小生成树不存在
    if(MST中收的顶点不到|V|个)
    {
        error("生成树不存在");
    }
}
//复杂度O(V^2)
```

#### Kruskal算法——将森林合并成树

稀疏图比较合算，即边数和顶点数差不多数量级的时候。

每次找权重最小边，将它收进去。然后不能构成回路，只能有|V|-1条边。

它与prim不一样，prim是一步步收集顶点。而Kruskal是一步步收集边。

```c++
void Kruskal(Graph G)
{
    MST={};//空集开始，一步步收集边
    while(MST中不足|V|-1条边&&E（原始图的边集合）中还有边){
        从所有边中取一条权重最小的边E(V,W);//小顶堆
        将E(V,W)从E中删除;
        if(E(V,W)不在MST中构成回路)//并查集
            将E(V,W)加入MST；
            else
                彻底无视E(V,W);
    }
    //打破while的情况
    //1.MST够|V|-1条边了
    //2.MST中不够|V|-1条边，意味着还没收集满呢，原图里边的边都被删光了，没有边了。即原图不连通
    if(MST中不足|V|-1条边)
        error("生成树不存在");
}
```

复杂度O(E*logE)

### 3.拓扑排序（入度表法）

AOV(Activity On Vertex)网络

在有向图里边，所有真实的活动是表现在顶点上的，顶点和顶点之间的有向边表示了两个活动之间的先后顺序。

![image.png](https://i.loli.net/2020/03/15/J3FrUSQRnCPDZvu.png)

- 拓扑序：如果图中从V到W有一条有向路径，则V一定排在W之前。满足此条件的顶点序列称为一个拓扑序。
- 获得一个拓扑序的过程就是拓扑排序
- AOV如果有合理的拓扑序，则必定是有向无环图（Directed Acyclic Graph,DAG)

```c++
//笨的拓扑排序。一个个输出的顺序，就是拓扑排好的序
void TopSort()
{
    for(cnt=0;cnt<|V|;cnt++){//把所有顶点，一个一个输出
        V=未输出的入度未0的顶点；
        //如果for循环还没结束，而我已经找不到这样的V了，说明有回路
        if(这样的V不存在)
        {
           	error("图中有回路");
            break;
        }    
        输出V，或者记录V的输出序号；
       //输出以后，把V从原图里抹掉，方法是将V的邻接点的入度都减1，就假装删掉了V     
        for(V的每个邻接点W)
        {
            indegree[w]--;//将V的每个邻接点W的入度减一。因为假装V被删掉了，所有它的邻接点们入度都减1
        }
    }
}
//复杂度 O(V^2)
```

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
        输出V，或者记录V的输出序号;cnt++;//用来记录输出了多少次
        //输出以后，把V从原图里抹掉，方法是将V的邻接点的入度都减1，就假装删掉了V。
        for(V的每个邻接点W)
        {
         	if(--indegree[W]==0)
                Enqueue(W,Q);
        }
    }
    //while跳出来的时候
    if(cnt!=|V|)//如果跳出来的时候，不是所有的顶点都被输出了，那就是有回路
        error("图中有回路")
}
//复杂度O(V+E)

```

### 4.关键路径(拓扑问题的应用)

AOE(Activity On Edge)网络

一般用于安排项目的工序。工序与工序之间有先后的依赖关系

![image.png](https://i.loli.net/2020/03/15/odfZACYLzt36H1W.png)

![image.png](https://i.loli.net/2020/03/15/agPLhNFHT85UzVK.png)

### 5.例题：旅游规划

![image.png](https://i.loli.net/2020/03/15/H7orRuMJjIfQxyw.png)

```c++
void Dijkstra(Vertex s)
{
    while(1)
    {
        V=未收录顶点中dist最小者;
        if(这样的V不存在)
            break;
        collected[V]=true;
        for(V的每个邻接点W)
        {
            if(collected[W]==false)//W没被收录
            {
                if(dist[V]+E(V,W)<dist[W])//这个顶点收录以后，影响了它的邻接点(即顶点W有了更短路径)
                {
                    dist[W]=dist[V]+E(V,W);//更新源点到w的路径长度
                    path[W]=V;//更新路径标记
                    cost[W]=cost[V]+C(V,W);//更新源点到w的花费
                }
                //如果路径长度相同，但是花费更少
           			 else if((dist[V]+E(V,W)==dist[W]) && (cost[V]+C(V,W)<cost[W]))
                     {
                         path[W]=V;//更新路径
                         cost[W]=cost[V]+C(V,W);//更新费用                  
                    }
                         
             }
        }
                
    }
        
}
```

### 6.其他类似问题

- 求数最短路径有多少条
  - count[s]=1;s到s有一条路径，所以初始化为1
  - //count[V]代表从源点到V点的最短路径有多少条
  - 如果找到更短路：即dist[V]+E(V,W)<dist[W]时
    - count[W]=count[V];//因为从S到V有多少条最短路，又从V到W只有一条路，所以源点到W就有多少条最短路。（W指的是V的邻接点）
  - 如果找到等长最短路：即dist[V]+E(V,W)==dist[W]
    - count[W]+=count[V];//路径数相加。因为此时的V跟之前的V可不一样了。这个V的所有最短路径数目都要继承
- 求边数最少的最短路
  - 之前的旅游规划中，已经解决了，路径最短 && 花费最少，这里只要把所有的花费定义为1，即可解决 路径最短&&边数最少
  - count[s]=0;//count[s] s到s是没有边的，所以初始化为0。
  - //count[V]代表从源点到V点的最短边的数量
  - 如果找到更短路径，即dist[V]+E(V,W)<dist[W]时
    - count[W]=count[V]+1;//1可以理解为一个边的特殊的权重
  - 如果找到等长最短路：即dist[V]+E(V,W)==dist[W]
    - count[W]=count[V]+1;

## 十、图leetcode题目

### 1.课程表（拓扑排序）

```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> degrees(numCourses);//记录所有顶点的入度
        vector<vector<int>>adjacents(numCourses);//邻接表
        queue<int> q;//0入度的顶点入队
        int num=0;//用来记录输出了多少次
        //构建入度数组 和 邻接表
        for(int i=0;i<prerequisites.size();i++)
        {
            degrees[prerequisites[i][0]]++;//0号位置需要前修课程，入度++
            adjacents[prerequisites[i][1]].push_back(prerequisites[i][0]);//构造邻接表。
        }

        for(int i=0;i<numCourses;i++)//对于图中每个顶点，入度为0的统统入队
        {
            if(degrees[i]==0){
                q.push(i);//入队
            }
        }
        
        while(!q.empty())
        {
            int temp=q.front();//从队里拿出来的，必定是入度为0的点
            q.pop();
            num++;//输出次数+1
            //输出以后，把V从原图里抹掉，方法是将V的邻接点的入度都减1，就假装删掉了V。
            for(int i:adjacents[temp])//对于刚刚输出的V的所有邻接点
            {
                if(--degrees[i]==0)//入度减1
                    q.push(i);
            }
        }
        if(num!=numCourses)
            return false;
        return true;
    }
};
```

### 2.克隆图（图的遍历）

```c++
//DFS（递归），采用Hash_map来实现记录是否访问过的功能
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if(node==nullptr) return nullptr;
        DFS(node);
        return mp[node];
    }
private:
    unordered_map<Node*,Node*> mp;//相当于visited的作用
    void DFS(Node* v)
    {
        mp[v]=new Node(v->val);//先把该结点创建了
        for(const auto& w:v->neighbors)//对于它的所有邻接点
        {
            if(mp.count(w)==0)//如果没有访问过，DFS一下
            {
                DFS(w);
            }
            mp[v]->neighbors.push_back(mp[w]);//记录邻接表
        }
    }
};
```

```c++
//DFS（堆栈，非递归）类似先序遍历。先序是指，map中建立好了，并且 neighbors也都存好了
//这个跟BFS的队列基本上一样的写法，只是一个用了队列，一个用了堆栈
//但是树里边的DFS和BFS写法就差别比较大，图里边好像比较类似
//入栈时构造
//讲道理应该以出栈顺序算作遍历顺序
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if(node==nullptr) return nullptr;
        unordered_map<Node*,Node*> mp;//相当于visited的作用
        stack<Node*> st;//堆栈
        mp[node]=new Node(node->val);//为源点复制一个结点，保存到map中
        st.push(node);//源点入队
        while(!st.empty())
        {
            Node* v=st.top();//取出父节点
            st.pop();//父节点出栈
            for(const auto&w:v->neighbors)//对于父节点的所有邻接点（子节点）
            {
                if(mp.count(w)==0)//如果这个邻接点（子节点）没有被访问过
                {             
                    mp[w]=new Node(w->val);//复制结点
                    st.push(w);//子节点入队
                }
                mp[v]->neighbors.push_back(mp[w]);//填充邻接表
            }
        }
        return mp[node];//返回原来的源点对应的克隆源点
    }
};

//这个题还想这样套DFS是不行的。因为有个neighbors变量要存储，所以每次while取出时，子节点必须得建立好，而不是仅仅Push进栈。所以不能按底下这样写。应该按上边的入栈时构造。
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if(node==nullptr) return nullptr;
        unordered_map<Node*,Node*> mp;
        stack<Node*> st;//堆栈
        st.push(node);//源点入队
        while(!st.empty())
        {
            Node* v=st.top();//取出父节点
            st.pop();//父节点出栈
            mp[v]=new Node[v->val];//拷贝
            for(const auto&w:v->neighbors)
            {
                if(mp.count(w)==0)
                {
                    st.push(w);//子节点入队
                }
                mp[v]->neighbors.push_back(mp[w]);//这句话有大问题。这样写，neighbors无法填充
            }
        }
        return mp[node];
    }
};
```



```c++
//BFS（队列。BFS只能非递归，没有递归法），采用Hash_map来实现记录是否访问过的功能
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if(node==nullptr) return nullptr;
        unordered_map<Node*,Node*> mp;//相当于visited的作用
        queue<Node*> que;
        Node* new_node=new Node(node->val);//为源点复制一个结点
        mp[node]=new_node;//保存到map中
        que.push(node);//源点入队
        while(!que.empty())
        {
            Node* V=que.front();//取出父节点
            que.pop();//父节点出队
            for(const auto&w:V->neighbors)//对于父节点的所有邻接点（子节点）
            {
                if(mp.count(w)==0)//如果这个邻接点（子节点）没有被访问过
                {                
                    mp[w]=new Node(w->val);//访问它
                    que.push(w);//子节点入队
                }
                mp[V]->neighbors.push_back(mp[w]);//填充邻接表
            }
        }
        return mp[node];//返回原来的源点对应的克隆源点

    }
};
```

### 3.冗余连接（并查集，用于判断无向图中某条边会不会构成回路）

```c++
//树指的是一个连通且无环的无向图（无根树）
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        for(int i=0;i<1001;i++)
            parent[i]=i;//并查集初始化

        for(const auto&edge:edges)//一个边一个边的取出
        {
            if(!union_root(edge[0],edge[1]))//判断这俩顶点是否已经属于同一个连通集，也就是说这俩顶点之间有没有通路。如果之前已经连通的话，那么当前这条边就是多余的。如果之前没有连通，那么这个边的引入，就要让它们连通起来。
                return edge;
        }
        return {};//没有冗余的边，返回空数组
    }
private:
    int parent[1001];//只知道最多1000条边，只能按最坏处理，最坏1001个顶点
    int Find(int x)
    {
        int fx=x;
        while(parent[fx]!=fx)//寻找母结点
        {
            fx=parent[fx];
        }
        while(parent[x]!=fx)//路径压缩
        {
            int temp=parent[x];//将x到它的最顶点的路上的所有顶点，全部直接挂载到顶点上
            parent[x]=fx;
            x=temp;
        }
        return fx;//以最上层顶点来标识属于哪个连通集
    }

    bool union_root(int x,int y)
    {
        int root_x=Find(x);//取出顶层结点
        int root_y=Find(y);
        if(root_x==root_y) return false;//意味着这两个点之前已经有通路了，当前进来的这个边就是多余的边
        parent[root_x]=root_y;//进行联合
        return true;//之前没连通，现在连通上了
    }
};
```

### 4.除法求值（双向图+图的遍历）/(带权重的并查集)

```c++
//网上的版本，先建立双向图，用的BFS，没用并查集
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        int n=equations.size();//多少条边
        for(int i=0;i<n;i++)
        {
            //建立双向图
            ump[equations[i][0]].push_back({equations[i][1],values[i]});
            ump[equations[i][1]].push_back({equations[i][0],1.0/values[i]});
        }
        vector<double> res;
        for(auto& vec:queries)
        {
            res.push_back(helper(vec[0],vec[1]));
        }
        return res;
    }
private:
    unordered_map<string,vector<pair<string,double>>> ump;//相当于邻接表。第一个string是顶点，后边的vector里的string和double代表 指向哪个顶点以及权重
    double helper(string& str1,string& str2)
    {
        auto it1=ump.find(str1);
        auto it2=ump.find(str2);
        if(it1==ump.end() || it2==ump.end()) return -1.0;
        if(str1==str2)  return 1.0;
        
        //BFS遍历
        queue<pair<string,double>> q;
        q.push({str1,1.0});
        unordered_map<string,bool> visited;
        double res=-1.0;
        while(!q.empty())
        {
            auto v=q.front();
            q.pop();
            visited[v.first]=true;
            if(v.first==str2)
            {
                res=v.second;
                break;
            }
            for(const auto& w:ump[v.first])
            {
                if(visited.count(w.first)==0){
                    visited[w.first]=true;
                    q.push({w.first,w.second*v.second});}

            }
            
        }
        return res;
    }
};
```

```c++
//自己手写的，重新实现上边的代码（先构建双向图，邻接表表示法。）然后BFS,没用并查集
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        for(int i=0;i<equations.size();i++)//建立双向图
        {
            ump[equations[i][0]].push_back({equations[i][1],values[i]});
            ump[equations[i][1]].push_back({equations[i][0],1.0/values[i]});
        }
        vector<double> res;
        for(const auto& vec:queries)
        {
            res.push_back(helper(vec[0],vec[1]));
        }
        return res;
    }
private:
    unordered_map<string,vector<pair<string,double>>> ump;//相当于邻接表。第一个string是顶点，后边的vector里的string和double代表 指向哪个顶点以及权重
    double helper(const string&str1,const string&str2)
    {
        if(ump.count(str1)==0 || ump.count(str2)==0) return -1.0;//如果有没在图里的点，直接返回-1
        if(str1==str2) return 1.0;//两个相等，返回1.0
        double res=-1.0;//如果是两个不连通的点之间的查询，就会默认这个值。也就是while循环完了，res的值不会被修改。
        set<string> vis;//visited的作用，一旦访问过了，就加入set中
        queue<pair<string,double>> q;//一定要保存pair，pair里的double是记录从源点一路过来的累乘
        q.push({str1,1.0});
        while(!q.empty())
        {
            auto v=q.front();
            q.pop();
            vis.insert(v.first);//标识已经访问过了
            for(const auto&w:ump[v.first] )
            {
                if(w.first==str2)
                {
                    res=v.second*w.second;//找到目标点了，更新res后break
                    break;
                }

                if(vis.count(w.first)==0)//不是目标点，也没有访问过
                {
                    q.push({w.first,v.second*w.second});
                }
				//已经访问过的点什么都不做
            }
        }
        return res;

    }
};
```

```c++
//并查集法
//上边的邻接表法，记录的unordered_map，是unorder_map<本节点，pair<下个结点,本节点指向下结点的权重>>
//而并查集里的两个Map，unordered_map<本节点,pair<父节点,父节点指向本节点的权重>>。底下的实现中，将pair拆成了两个unordered_map而已，实际一样。
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        for(int i=0;i<equations.size();i++)
        {
            union_root(equations[i][0],equations[i][1],values[i]);
        }
        vector<double> res;
        for(const auto& vec:queries)
        {
            res.push_back(helper(vec[0],vec[1]));
        }
        return res;
    }
private:
    unordered_map<string, string> parent;//本节点，父节点（find过程会直接与顶点相连起来）
    unordered_map<string, double> val;//本节点，父节点指向本节点的权重
    
    string Find(const string& x)//这个递归find好好学，只能用递归，先往上递归找到集合顶点，然后再层层向下，这样才能正确更新val数组
    {
        if(parent[x]==x) return x;
        else{
            string f=Find(parent[x]);
            val[x]=val[x]*val[parent[x]];
            parent[x]=f;
            return f;//往下递归return时，其实一直都是return的头结点
        }
    }

    void union_root(const string& str1,const string& str2,double v)
    {//不用担心出现回环，题目保证一定有意义
        if(parent.count(str1)==0) {parent[str1]=str1;val[str1]=1;}//如果以前没见过，先初始化
        if(parent.count(str2)==0){parent[str2]=str2;val[str2]=1;}
        string f_str1=Find(str1);
        string f_str2=Find(str2);
        parent[f_str2]=f_str1;//两个根连接起来
        val[f_str2]=val[str1]*1.0/val[str2]*v;//更新的是根的权重，不是str1或者str2
    }

    double helper(const string& str1,const string& str2)
    {//这个函数用于计算
        if(parent.count(str1)==0 || parent.count(str2)==0) return -1.0;//要先判断是否在列表
        if(str1==str2) return 1.0;//两个相同
        string f_str1=Find(str1);
        string f_str2=Find(str2);
        if(f_str1!=f_str2) return -1.0;//两个点不在一个并查集，没有通路，返回-1
        return (1.0/val[str1]*val[str2]);//相当于直接与顶点相连的两个点，两条边的权重

    }
    
};
```

### 5.最小高度树（拓扑排序的变式）

```c++
//最终输出的点只可能是1个或者2个
//入度为1的点就是叶子，可以删去，入度大于1的点应该更有可能为树根
//为什么本题不能一个一个的删除入度为1的点，而应在一个循环中，一次性删除入度为1的点，使得以同样的速度缩小图，直至剩下<=2个点为止？
//因为不这样做的话，最后剩了两个点，你根本不知道到底是只取一个点还是两个点都对。
//而一次性删除的话，它剩几个，就是几个答案。有时候队列只剩一个，有时候剩两个。
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if(n==1) return {0};//提交时修改的
        vector<int> indegrees(n);//入度表
        vector<vector<int>> graph(n);//图的邻接表
        for(const auto&vec:edges)
        {
            graph[vec[0]].push_back(vec[1]);//构建临建表
            indegrees[vec[0]]++;//入度加一
            graph[vec[1]].push_back(vec[0]);//无向图，另一边也要
            indegrees[vec[1]]++;
        }
        queue<int> q;//准备存入入度为1的点
        
        for(int i=0;i<n;i++)//将入度为1的点一次性全压入队
        {
            if(indegrees[i]==1)
                q.push(i);
        }

        while(n>2)//剩余的点，不能用q.size()>2，因为有可能三个点，上来就只进去2个点。应该用n来判断
        {
            int size=q.size();
            n=n-size;//剩余的点（还有度的点，或者说还没被删除的点）
            for(int i=0;i<size;i++)//本队列中所有入度为1的点处理完了，再出去判断while
            {
                int temp=q.front();
                q.pop();
                indegrees[temp]=0;//因为取出来的都是入度为1的点，它-1，就是0
                for(const auto& i:graph[temp])
                {
                    indegrees[i]--;//邻接点们入度-1
                    if(indegrees[i]==1)//入度为1时入队，但它不会在本轮for循环中处理
                        q.push(i);
                }
            }

        }
        
        vector<int> res;//可能剩余1个或者两个
        while(!q.empty())
        {
            res.push_back(q.front());
            q.pop();
        }
        return res;
    }
    
};
```

### 6.找到小镇的法官(入度表、出度表)

```c++
//很显然，用入度表和出度表。法官满足这样的条件：出度为0，入度为N-1。还说仅有一个法官，其实能满足这个条件的，必定只有一个。所以一旦找到一个，就可以break了
class Solution {
public:
    int findJudge(int N, vector<vector<int>>& trust) {
        vector<int> indegrees(N+1);//因为例题给的下标从1开始
        vector<int> outdegrees(N+1);
        int res=-1;
        for(const auto& vec:trust)
        {//构造出度表、入度表
            outdegrees[vec[0]]++;
            indegrees[vec[1]]++;
        }
        
        for(int i=1;i<N+1;i++)
        {//遍历查询满足条件的点
            if(outdegrees[i]==0&&indegrees[i]==N-1)
                
                {
                    res=i;
                    break;
                }
        }
        return res;
    }
};
```

### 7.不邻接植花（邻接表遍历）

```c++
//最简单的，构造一个邻接表，然后挨个顶点遍历。将它的邻接点染过的颜色去除，最后剩的就是它可以染的色。
class Solution {
public:
    vector<int> gardenNoAdj(int N, vector<vector<int>>& paths) {
        vector<vector<int>> graph(N);
        for(const auto& vec:paths)
        {//建立邻接表
            graph[vec[0]-1].push_back(vec[1]-1);//花园是按 1开始记下标的
            graph[vec[1]-1].push_back(vec[0]-1);
        }
        vector<int> res(N,0);//N个为0个初始点，初始化全部未染色
        for(int i=0;i<N;i++)
        {
            set<int> color{1,2,3,4};//当前节点可以染的颜色的候选集合
            for(const auto& w:graph[i])
            {
                color.erase(res[w]);//把已染过色的去除
            }
            res[i]=*(color.begin());//染色
        }
        return res;

    }
};
```

### 8.统计参与通信的服务器（跟图没啥关系）

```c++
//暴力法，扫描整个矩阵。一碰到==1的，就去扫它这一行和这一列，如果还有==1的，就count++;
class Solution {
public:
    int countServers(vector<vector<int>>& grid) {
        int count=0;
        for(int j=0;j<grid[0].size();j++)
        {
            for(int i=0;i<grid.size();i++)
            {
                if(grid[i][j]==1)
                {
                    if(helper(grid,i,j)==true)//如果本行或本列还有其他==1的元素
                        count++;
                }
            }
        }
        return count;
    }
private:
    bool helper(vector<vector<int>>& grid,int i,int j)
    {
        for(int jj=0;jj<grid[0].size();jj++)
        {
            if(grid[i][jj]==1 && jj!=j)
                return true;
        }
        for(int ii=0;ii<grid.size();ii++)
        {
            if(grid[ii][j]==1 && ii!=i)
                return true;
        }
        return false;
    }
};
O(MN*(M+N))
```

```c++
//先扫描一遍矩阵，记录每一行和每一列的服务器数。第二遍扫描，如果这个服务器对应的行或列上，还有其他的服务器，那么count++
class Solution {
public:
    int countServers(vector<vector<int>>& grid) {
        int count=0;//可以通信的服务器个数
        int m=grid.size();
        int n=grid[0].size();
        vector<int> count_m(m,0);//记录每一行的服务器数目，共m行
        vector<int> count_n(n,0);//记录每一列的服务器数目，共n列
        for(int j=0;j<n;j++)
            for(int i=0;i<m;i++)
            {
                if(grid[i][j]==1)
                    {count_m[i]++;
                    count_n[j]++;}
            }
        for(int j=0;j<n;j++)
            for(int i=0;i<m;i++)
            {
                if(grid[i][j]==1 && (count_m[i]>1||count_n[j]>1))//如果这个服务器，所在的行或列上还有其他的服务器
                    count++;
            }
        return count;
    }
};
//O(2*MN)
```

### 9.跳跃游戏三（BFS DFS，不用构建图）

```c++
//简简单单BFS。一定要加上visited记录是否访问过
class Solution {
public:
    bool canReach(vector<int>& arr, int start) {
        if(arr.size()==0) return false;
        set<int> visited;
        queue<int> q;
        q.push(start);
        visited.insert(start);
        while(!q.empty())
        {
            int v=q.front();
            q.pop();
            if(arr[v]==0) return true;
            if(v-arr[v]>=0 && visited.count(v-arr[v])==0)
                {q.push(v-arr[v]);visited.insert(v-arr[v]);}
            if(v+arr[v]<arr.size() && visited.count(v+arr[v])==0)
                {q.push(v+arr[v]);visited.insert(v+arr[v]);}
        }
        return false;   
    }
};
```

```c++
//DFS，出栈时访问
class Solution {
public:
    bool canReach(vector<int>& arr, int start) {
        if(arr.size()==0) return false;
        set<int> visited;
        stack<int> q;
        q.push(start);
        while(!q.empty())
        {
            int v=q.top();
            q.pop();
            visited.insert(v);//出栈时访问
            if(arr[v]==0) return true;
            if(v-arr[v]>=0 && visited.count(v-arr[v])==0)
                {q.push(v-arr[v]);}
            if(v+arr[v]<arr.size() && visited.count(v+arr[v])==0)
                {q.push(v+arr[v]);}
        }
        return false;   
    }
};
```

### 10.验证二叉树（DFS/BFS）

```c++
//很重要的一点，运用图的深搜或者广搜时，对于一个二叉树来说，它的visited矩阵是不应该重复的。换句话说，二叉树的遍历，对于某个邻接点if(已经visited)，那么它一定不是个二叉树
//因为满足无环连通图，即为树。
//若要满足无环图，遍历过程中不能出现重复结点。若要满足连通图，遍历完成后经过的顶点数需为n个。
//并且为什么不会出现N叉树？因为两个子节点数组可以看作一个邻接表，每个结点的邻居数可能为2、1、0，所以，如果它是树，一定是二叉树。
class Solution {
public:
    bool validateBinaryTreeNodes(int n, vector<int>& leftChild, vector<int>& rightChild) {
        vector<int> indegrees(n,0);//存入度，为了找树根
        for(int i=0;i<n;i++)
        {
            if(leftChild[i]!=-1) 
            {
                indegrees[leftChild[i]]++;
            }
            if(rightChild[i]!=-1)
            {
                indegrees[rightChild[i]]++;
            }
        }

        int root=-1;//树根下标
        for(int i=0;i<n;i++)
        {
            if(indegrees[i]==0){//把第一个入度为0的点作为树根
                root=i;
                break;
            }
        }
        if(root==-1) return false;//没有入度为0的点
        unordered_set<int>visited;
        queue<int> q;
        q.push(root);
        visited.insert(root);
        while(!q.empty())//BFS
        {
            int v=q.front();
            q.pop();
            if(leftChild[v]!=-1)
            {
                if(visited.count(leftChild[v])!=0) return false;//这里与图的遍历不一样。不再是绕开visited过的点，而是一旦有visited过的点，直接false。因为二叉树的遍历不会有重复
                q.push(leftChild[v]);
                visited.insert(leftChild[v]);
            }
            if(rightChild[v]!=-1)
            {
                if(visited.count(rightChild[v])!=0) return false;
                q.push(rightChild[v]);
                visited.insert(rightChild[v]);
            }
        }
        return visited.size()==n;//不等于n，说明是多棵树

    }
};
```

```c++
//将BFS换成了DFS。先序遍历的非通用版本。出栈时访问
class Solution {
public:
    bool validateBinaryTreeNodes(int n, vector<int>& leftChild, vector<int>& rightChild) {
        vector<int> indegrees(n,0);//存入度，为了找树根
        for(int i=0;i<n;i++)
        {
            if(leftChild[i]!=-1) 
            {
                indegrees[leftChild[i]]++;
            }
            if(rightChild[i]!=-1)
            {
                indegrees[rightChild[i]]++;
            }
        }

        int root=-1;//树根下标
        for(int i=0;i<n;i++)
        {
            if(indegrees[i]==0){//把第一个入度为0的点作为树根
                root=i;
                break;
            }
        }
        if(root==-1) return false;//没有入度为0的点
        unordered_set<int>visited;
        stack<int> st;
        st.push(root);

        while(!st.empty())//DFS
        {
            int v=st.top();
            st.pop();
            visited.insert(v);
            if(leftChild[v]!=-1)
            {
                if(visited.count(leftChild[v])!=0) return false;
                st.push(leftChild[v]);
            }
            if(rightChild[v]!=-1)
            {
                if(visited.count(rightChild[v])!=0) return false;
                st.push(rightChild[v]);
            }
        }
        return visited.size()==n;

    }
};
```

```c++
//并查集。修改了union函数（1.加上这条边，构成回路，那这幅图不是树 2.如果子节点（left或者right)之前没有被指向，或者说他们的parent是本身，那么指向上，也就是传统的union上 3.如果我的这个结点的left或者right，在我指向之前，已经有指向了，那么这幅图肯定不是树。）并且find函数不能进行路径压缩！（原始结构要保留，毕竟这是树）
class Solution {
public:
    bool validateBinaryTreeNodes(int n, vector<int>& leftChild, vector<int>& rightChild) {
            vector<int>parent(n);
            for(int i=0;i<n;i++) parent[i]=i;
            for(int i=0;i<n;i++)
            {
                //这条边是从i->left的边
                int left=leftChild[i];
                if(left!=-1){//不等于-1的子结点才处理
                    if(union_root(i,left,parent)==false)
                        return false;
                }

                //这条是i->right的边
                int right=rightChild[i];
                if(right!=-1){
                    if(union_root(i,right,parent)==false)
                        return false;
                }
            }
            int cnt=0;//统计连通块的个数
            for(int i=0;i<n;i++)
            {
                if(parent[i]==i) cnt++;
            }
            return cnt==1;

    }
private:
    int find(int x,vector<int>& parent)
    {
        while(x!=parent[x])
            x=parent[x];
        return x;
    }
    bool union_root(int x,int y,vector<int>&parent)
    {
        int root_x=find(x,parent);
        int root_y=find(y,parent);
        if(root_x==root_y) return false;//之前已经有通路了，再加上这条边将会构成回路。也就是给的图是有回路的，那肯定不是树
        else if(root_y==y) {parent[root_y]=root_x;return true;}//这个子节点，还没有任何指向。指向完成。
        else//箭头指向的结点（left位置）已经有别的结点指向了（别的root也指向了这个left）那肯定不是二叉树了
         return false;
    }
};
```

### 11.网络延迟时间（最短路径问题）

本质是求源点到各节点的最短路径中最长的那条路径长度。

**最短路算法的分类：**

- 单源最短路
  - 所有边权都是正数
    - 朴素的Dijkstra算法 O(n^2) 适合稠密图
    - 堆优化版的Dijkstra算法 O(mlog n)（m是图中节点的个数）适合稀疏图
- 存在负权边
  - Bellman-Ford O(nm)
  - spfa 一般O(m),最坏O(nm)
- 多源汇最短路 Floyd算法 O(n^3)

#### 朴素Dijkstra

```c++
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<vector<int>> graph(N+1);//邻接表。也可以用邻接矩阵。
        vector<vector<int>> weight(N+1);//对应的权重
        for(const auto& vec:times)//初始化图
        {
            graph[vec[0]].push_back(vec[1]);
            weight[vec[0]].push_back(vec[2]);
        }
        unordered_set<int> collected;//是否已经得到最优解
        vector<int> dist(N+1,INT_MAX);//每个点距离起源点的最短距离
        dist[K]=0;//初始化源点
        vector<int> parent(N+1,-1);//记录路径（这个题里没有用）
        while(1)
        {
            int min=INT_MAX;
            int V=-1;//准备记录未收录顶点中，dist最小的点
            for(int i=1;i<N+1;i++)//遍历dist寻找最小的dist
            {
                if(dist[i]<min && collected.count(i)==0) 
                {
                    min=dist[i];
                    V=i;
                }
            }

            if(V==-1) break;//如果这样的点不存在
            collected.insert(V);//将V收入集合

            //收进来以后，看看V会不会影响到它的邻接点们的dist值
            for(int i=0;i<graph[V].size();i++)//这里是从0开始的，因为邻接点都是push_back进去的，下标从0开始
            {
                int w=graph[V][i];//取出邻接点
                if(collected.count(w)==0)//未收录的邻接点
                {
                    if(dist[V]+weight[V][i]<dist[w])//如果V收录后，w有了更短路径，那么更新
                    {
                        dist[w]=dist[V]+weight[V][i];
                        parent[w]=V;
                    }
                }
            }
        }
        if(collected.size()!=N) return -1;//说明图不连通
        int res=0;//遍历dist，找出最大的dist
        for(int i=1;i<N+1;i++)
        {
            if(res<dist[i])
                res=dist[i];
        }
        return res;
    }
};
```

#### 堆优化的Dijkstra

```c++
//采用priority_queue维护一个小顶堆，
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<vector<int>> graph(N+1);//邻接表
        vector<vector<int>> weight(N+1);//对应的权重
        for(const auto& vec:times)//初始化图
        {
            graph[vec[0]].push_back(vec[1]);
            weight[vec[0]].push_back(vec[2]);
        }
        unordered_set<int> collected;//是否已经得到最优解
        vector<int> dist(N+1,INT_MAX);//每个点距离起源点的最短距离
        dist[K]=0;//初始化源点
        vector<int> parent(N+1,-1);//记录路径（这个题里没有用）
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> pri_que;//小顶堆，用来快速取出dist中最小的dist。pair第一个元素是dist值，第二个是所属的点
        //优先队列会按照pair的第一个元素进行排序
        pri_que.push({0,K});
        while(1)
        {
            if(pri_que.size()==0) break;
            int min=pri_que.top().first;
            int V=pri_que.top().second;
            pri_que.pop();
            if(collected.count(V)!=0)//如果已经收集过了，说明是堆里冗余的
                continue;
            //if(min==INT_MAX)//如果这样的点不存在。//不需要这个break条件了
                //break;

            collected.insert(V);//将V收入集合

            //收进来以后，看看V会不会影响到它的邻接点们的dist值
            for(int i=0;i<graph[V].size();i++)//这里是从0开始的，因为邻接点都是push_back进去的，下标从0开始
            {
                int w=graph[V][i];//取出邻接点
                if(collected.count(w)==0)//未收录的邻接点
                {
                    if(dist[V]+weight[V][i]<dist[w])//如果V收录后，w有了更短路径，那么更新
                    {
                        dist[w]=dist[V]+weight[V][i];
                        parent[w]=V;
                        pri_que.push({dist[w],w});
                    }
                }
            }
        }
        if(collected.size()!=N) return -1;//说明图不连通
        int res=0;//遍历dist，找出最大的dist
        for(int i=1;i<N+1;i++)
        {
            if(res<dist[i])
                res=dist[i];
        }
        return res;
    }
};
```

#### Floyd

```c++
#define MAX_W (100*100+1)//这个很重要，这是题干上给的，用max_w初始化图，而不是INT_MAX。因为底下的graph[i][k]+graph[k][j]<graph[i][j]时，那个相加操作，会爆掉Int范围。或者是采用底下的double转换
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<vector<int>> graph(N+1,vector<int>(N+1,MAX_W));//邻接矩阵
        vector<vector<int>> path(N+1,vector<int>(N+1,-1));//本题没用到
        for(int i=1;i<N+1;i++) graph[i][i]=0;//初始化矩阵
        for(const auto& vec:times)//构建邻接矩阵
        {
            graph[vec[0]][vec[1]]=vec[2];
        }
        
        //Folyd算法
        for(int k=1;k<N+1;k++)//这个小K，要跟题干的大K区分开
            for(int i=1;i<N+1;i++)
                for(int j=1;j<N+1;j++)
                    {
                        if(graph[i][k]+graph[k][j]<graph[i][j])
                            {//由于设置了合适的MAX_W，所以int不会爆掉
                                graph[i][j]=graph[i][k]+graph[k][j];
                                path[i][j]=k;
                            }
                    }
        
        int res=-1;
        for(int j=1;j<N+1;j++)
        {
            if(graph[K][j]==MAX_W) return -1;
            if(graph[K][j]>res) res=graph[K][j];
        }
        return res;

    }
};
```

```c++
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        vector<vector<int>> graph(N+1,vector<int>(N+1,INT_MAX));//邻接矩阵
        vector<vector<int>> path(N+1,vector<int>(N+1,-1));//本题没用到
        for(int i=1;i<N+1;i++) graph[i][i]=0;//初始化矩阵
        for(const auto& vec:times)//构建邻接矩阵
        {
            graph[vec[0]][vec[1]]=vec[2];
        }
        
        //Folyd算法
        for(int k=1;k<N+1;k++)//这个小K，要跟题干的大K区分开
            for(int i=1;i<N+1;i++)
                for(int j=1;j<N+1;j++)
                    {
                       if(double(graph[i][k])+double(graph[k][j])<double(graph[i][j]))
                          {//这个double转换很重要，否则相加操作会爆掉int上限
                              graph[i][j]=graph[i][k]+graph[k][j];
                              path[i][j]=k;//记录K结点，本题并用不到
                          }
                    }
        
        int res=-1;
        for(int j=1;j<N+1;j++)//查询从大K出发，到各个点的路径长度。如果有INT_MAX说明是达不到的点。
        {
            if(graph[K][j]==INT_MAX) return -1;
            if(graph[K][j]>res) res=graph[K][j];//保留从K出发，到各个节点的最短路径中的最长的长度。
        }
        return res;

    }
};
```

### 12.找到最终的安全状态（拓扑排序，根据出度）/(DFS)

```c++
//传统的拓扑排序，是构建邻接表和入度表
//本题用逆邻接表和出度表刚好合适，满足条件
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int N=graph.size();//结点个数
        vector<vector<int>> reversegraph(N);//逆邻接表
        vector<int> outdegrees(N,0);//出度表
        for(int i=0;i<N;i++)
            for(const auto& e:graph[i])
            {
                outdegrees[i]++;//构建出度表
                reversegraph[e].push_back(i);//构建逆邻接表
            }
        queue<int> q;//队列，存储所有出度为0的点
        vector<int> res;//存结果

        for(int i=0;i<N;i++)//把所有出度为0的点入队
        {
            if(outdegrees[i]==0)
                q.push(i);
        }

        while(!q.empty())
        {
            int v=q.front();//出队
            q.pop();
            res.push_back(v);//记录
            for(const auto& w:reversegraph[v])//所有逆邻接点出度减一
            {
                outdegrees[w]--;
                if(outdegrees[w]==0)//新的点出度是0了，那就入队
                    q.push(w);
            }
        }
        sort(res.begin(),res.end());//由于题目的答案是这样排序的，就需要排序。默认升序
        //sort(res.rbegin(),res.rend());//这样就是降序
        return res;

    }
};
```

```c++
//DFS,递归，很难想，暂时学不会
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        if(graph.size()==0) return vector<int>{};
        //dfs
        int N=graph.size();
        vector<int> color(N,0);
         // 0未访问
        // 1由本次发起dfs的结点访问过，说明有环，在该环上的所有顶点都是不安全的
        // 2表示非本轮dfs访问的结点，安全
        vector<int> res;
        for(int i=0;i<N;i++)
        {
            if(dfs(i,graph,color))
                res.push_back(i);
        }
        return res;
    }
private
    //vector<int>& color很关键，因为第一个结点的DFS，就已经可以确定很多个结点可以或者不可以了
    //color数组的改变，只在每次DFS不同的源点的时候（换句话说，一个源点所联通的所有结点，在这个结点的一次整个DFS过程中，就已经确定了答案，可以或者不可以。）
    bool dfs(int i,vector<vector<int>>& graph,vector<int>& color)
    {
        if(color[i]==1) return false;
        if(color[i]==2) return true;
        //底下是color==0的情况
        color[i]=1;
        for(int j=0;j<graph[i].size();j++)
        {
            if(!dfs(graph[i][j],graph,color)) return false;
        }
        color[i]=2;
        return true;
    }
};
```

### 13.阈值距离内邻居最少的城市（多源最短路问题Floyd）

```c++
#define MAXMAX (100*1e4+1)//怕graph[i][k]+graph[k][j]<graph[i][j]这一步求和Int溢出，不能用INT_MAX
class Solution {
public:
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        vector<vector<int>> graph(n,vector<int>(n,MAXMAX));//邻接矩阵
        for(int i=0;i<n;i++) graph[i][i]=0;//邻接矩阵初始化
        for(const auto& vec:edges)//构造邻接矩阵
        {
            graph[vec[0]][vec[1]]=vec[2];
            graph[vec[1]][vec[0]]=vec[2];
        }
		//Floyd更新邻接矩阵
        for(int k=0;k<n;k++)
        for(int i=0;i<n;i++)
        for(int j=0;j<n;j++)
        {
            if(graph[i][k]+graph[k][j]<graph[i][j])
                graph[i][j]=graph[i][k]+graph[k][j];
        }
        //遍历每个结点，数一数小于阈值的可抵达城市的数目
        int min_num=INT_MAX;//最小城市数目
        int res=0;//记录对应的结点
        for(int i=0;i<n;i++)
        {
            int count=0;
            for(int j=0;j<n;j++)
            {
                if(graph[i][j]<=distanceThreshold)
                    {
                        count++;
                        if(i>0 && count>min_num)//这一步主要是为了加快运算，别傻傻的循环完
                            break;
                    } 
            }
            if(count<=min_num)//
                {
                    min_num=count;
                    res=i;
                }
        }
        return res;
    }
};
```

## 十一、排序

### 1.概论

```c++
void X_sort(ElementType A[],int N)//A待排数组，N代表数据规模
```

- 简单起见，默认整数排序，而且是从小到大。
- N是正整数。
- 只讨论基于比较的排序（即小于 等于 大于 这三个符号要有定义）
- 只讨论内部排序（内存足够大，所有排序都在内存里一次性完成）。外部排序不考虑（比如内存只要2G，要对1TB数据进行排序）
- 稳定性：任意两个相等的数据，排序前后的相对位置不发生改变。（比如两个小明，排序之前,小明1在小明2的前边，那么排序后,小明1依然在小明2的前边，这就叫稳定的）
- 没有一种排序是任何情况下都是表现最好的（与数据的特征有关，不同的数据特征下，最好的排序算法不同）

### 2.简单排序

#### 冒泡排序

一趟冒泡排序后，最大的泡泡一定到了最后边。下一次排序只需要对前n-1个数重复，每一次都减1。

```c++
//对于数组和链表都可以
//直观理解就是，每次都把最大的值排到队尾
void Bubble_Sort(ElementType A[],int N)
{
    for(int P=N-1;P>=0;P--){
        int flag=0;//用于万一在P=中间某个值的时候，就已经运气好，排序完成了
        for(int i=0;i<P;i++){//一趟冒泡
            if(A[i]>A[i+1])//只有严格大于才做交换，相等是不交换的，保证了排序的稳定性
            {
                Swap(A[i],A[i+1]);//前后交换
                flag=1;//标识发生了交换
            }
        }
        if(flag==0) break;//P=中间某个值的时候，就已经排序好了，可以直接停，不用傻傻的等P到0，一圈又一圈多余循环
    }
}
//最好O(N)
//最差复杂度O(N^2)
```

#### 插入排序

```c++
//每一轮排好数组下标前P个数（包含P位置）
//也就是说前P-1个可以理解为手里已经排好的牌
void Insertion_sort(ElementType A[],int N)
{
    for(P=1;P<N;P++)
    {//前P-1张都在我手里（A数组前P-1个位置就是我手里的牌，遍历到P=N-1就都排好了），且排好序了
        Tmp=A[P];//摸下一张牌
        for(i=P;i>0&&A[i-1]>Tmp;i--)//如果A[i-1]<tmp，第i-1个位置已经比我小了，更往前的(0到i-2)又都是已经排好序的，我就插到i位置就行
        {
                A[i]=A[i-1];//移出空位,因为A[P]的值是记录在tmp里边的，放心大胆的移动，不怕覆盖
        }
        A[i]=Tmp;//新牌落位
    }
}
//最好O(N)
//最差复杂度O(N^2)
//例子：
         0   1  2  3  4  5
原始     34| 8  64 51 32 21
P=1结束后 8  34| 64 51 32 21
P=2结束后 8  34  64|51 32 21
P=3结束后 8  34  51 64|32 21
P=4结束后 8  32  34 51 64|21
P=5结束后 8  21  32 34 51 64|
```

#### 逆序对

- 对于下标i<j，如果A[i]>A[j]，则称(i,j)是一对逆序对（inversion)
- 交换2个相邻元素正好消去1个逆序对
- 所以插入排序的实际复杂度：T(N,I)=O(N+I)   注意：O(N^2)是最差的情况
  - 意味着**如果序列基本有序（大部分顺序是正确的）**，则**插入排序**简单且高效
  - I是逆序对的个数
  - 起码要对所有元素扫描一遍，所以是至少是O(N)，另外操作的次数是与逆序对的个数成正比的。所以是O(N+I)
  - 举个例子，如果顺序是刚好全反的，那么I=N*(N-1)/2，即I是N^2量级的，那么O(N+I)=O(N^2)，与上边的结论是一致的。
- 定理：任意N个不同元素组成的序列，平均具有N*(N-1)/4个逆序对。也就是说I是O(N^2)的
- 定理：任何**仅以交换相邻两元素**（比如冒泡、插入）来排序的算法，平均时间复杂度Ω(N^2)。 Ω是下界的意思，就是说，最好最好，也就是N^2数量级的复杂度了。
- 意味着：想提高算法效率：
  - 每次消去不止1个逆序对
  - 每次交换相隔较远的2个元素

### 3.希尔排序

英文：shell sort

利用了插入排序的简单，又克服了插入排序每次只能交换相邻元素的缺点。

![image.png](https://i.loli.net/2020/03/23/FebW4I6nsOCZv35.png)

即：比如5-间隔排序后，再3-间隔排序，3-间隔排序不会浪费掉5-间隔排序的努力。3-间隔排序后，序列将变成 5-间隔有序&& 3-间隔有序。

#### 原始希尔排序

![image.png](https://i.loli.net/2020/03/24/RnoQCe4AbZk5VlO.png)

```c++
//原始希尔排序
//第二个for循环一定是P++而不是P+=D
/*因为：比如这时D=3
每一轮将把以下位置变成有序的：0 3，1 4, 2 5, 0 3 6, 1 4 7, 2 5 8, 0 3 6 9, 1 4 7 10 ....
因此每次都是P++*/
void Shell_sort(ElementType A[],int N)
{
    for(D=N/2;D>0;D/=2)//希尔增量序列，从N/2，到1为止，每次除以2
    {
        //插入排序，把原来的插入排序的所有的1，都换成D就可以了
        //底下这句一定是P++，而不是P+=D。
        for(P=D;P<N;P++)//原来是从下标1开始摸牌，现在从D开始摸
        {
            tmp=A[P];
            for(i=P;i>=D && A[i-D]>tmp;i-=D)
                A[i]=A[i-D];
            A[i]=tmp;
        }
    }
}
```

当增量元素不互质时，小增量可能根本不起作用（比如 8 4 2 1间隔， 4 2可能完全不起作用)，导致前边的增量排序都浪费了。还是得靠1间隔来完成大量工作。

注：互质，即两个数，没有公因子。

#### 其他增量序列

![image.png](https://i.loli.net/2020/03/24/dkPbjiJLVBIHOoC.png)

如果我的排序数量是几万数量级的，那么用shell排序+Sedgewick增量序列，可能有不错的效果。

### 4.选择排序

```c++
//每次拿出最小的，从数组头依次往后放
void Seleciotn_Sort(ElementType A[],int N)
{
    //每轮找到最小的，从数组头依次往后放。
    //第一轮A[0]排成正确的，下一轮A[0]、A[1]有序，然后是A[0]、A[1]、A[2]有序...
    for(i=0;i<N;i++)
    {
        //从A[i]到A[N-1]中找最小元，并将其位置赋给MinPosition
        MinPosition=ScanForMin(A,i,N-1);
        //将未排序部分的最小元minposition换到有序部分的最后位置i
        Swap(A[i],A[MinPosition]);
    }
}
//复杂度：T=θ(N^2)
//制约因素在于scanformin这一步，由此引出堆排序
```

### 5.堆排序

```c++
//傻傻的堆排序
//跟选择排序一个思路，每次拿出最小的，从数组头依次往后放
void Heap_Sort(ElementType A[],int N)
{
    BuilHeap(A);//把数组转为最小堆
    for(i=0;i<N;i++)
        TempA[i]=DeleteMin(A);//每次弹出顶部元素，存入临时数组TempA
    for(i=0;i<N;i++)
        A[i]=TempA[i];//从临时数组拷贝回去
}
//builheap O(N)+ DeleteMin(A) N*O(logN) + 拷贝O(N)
//整体时间复杂度：O(NlogN)
//空间复杂度：需要额外的O(N)
```

```c++
//比较聪明的堆排序
似乎用不上
```

### 6.归并排序

一般不会用于内排序，也就是在内存里完成的排序，因为它需要一个等长的额外空间。一般只用于外排序

**有序子列的归并**

```c++
//左右子列合并
//A数组中，[L,R-1]是有序左子列，[R,RightEnd]是有序右子列
//A的整个下标是从[L,RightEnd];
//L=左子列起始位置  R=右子列起始位置  RightEnd=右子列终点位置
//原始待排数组A，临时数组TmpA
void Merge(ElementType A[],ElementType TmpA[],int L,int R,int RightEnd)
{
    LeftEnd=R-1;//左子列终点位置，假设左右两列尾头挨着。所以左边的终点就是右边起点-1。
    Tmp=L;//存放结果的数组的初始位置，tmp相当于C pointer
    NumElements=RightEnd-L+1;//两个序列的长度和
    while(L<=LeftEnd && R<=RightEnd) 
    {
        if(A[L]<=A[R]) TmpA[Tmp++]=A[L++];//左子列对应元素更小，那么存左边的数进去，并且L++
        else TmpA[Tmp++]=A[R++];//右子列对应元素更小，存右边的数进去，并且R++
    }
    //跳出的条件是，左右子序列，至少有一个为空了
    while(L<=LeftEnd) TmpA[Tmp++]=A[L++];//如果左边还有剩下的，直接复制左子列剩下的
    while(R<=RightEnd) TmpA[Tmp++]=A[R++];//如果右边还有剩下的， 直接复制右子列剩下的
    for(i=0;i<NumElements;i++,RightEnd--)
        A[RightEnd]=TmpA[RightEnd];//把tmpA的内容导回A
}
```

#### 递归算法

![image.png](https://i.loli.net/2020/03/24/FxdOBmq5nsKczui.png)

```c++
//L:数组A的起始下标  RightEnd:数组A的结尾
void MSort(ElementType A[],ElementType TmpA[],int L,int RightENd)
{
    int Center;
    if(L<RightEnd)//一直递归到L=RightEnd，这时候，序列长度为1，直接返回就行了
    {
        Center=(L+RightEnd)/2;//一分为二
        Msort(A,TmpA,L,Center);//前半段排序（排成有序的）
        Msort(A,TmpA,Center+1,RightEnd);//后半段排序（排成有序的）
        Merge(A,TmpA,L,Center+1,RightEnd);//左右子列合并
    }
}
//T(N)=O(NlogN)
/*比如 0 1 2 3 |4 5 6 7  共8个位置
实际真正的过程是，递归到最深处
先把 0和1位置merge，然后是2和3位置merge，然后 0 1和2 3 merge，然后是4和5位置merge，6和7 merge，4 5和6 7 merge， 最后是0 1 2 3和4 5 6 7一起merge
merge就是有序子列归并*/
```

```c++
//统一函数接口
void Merge_sort(ElementType A[],int N)
{
    ElementType *TmpA;
    TmpA=malloc(N*sizeof(ElementType));
    if(TmpA!=NULL)//给TmpA的内存申请到了
    {
        Msort(A,TmpA,0,N-1);
        free(TmpA);
    }
    else Error("空间不足");
}
//不要在merge中声明临时数组来保存tmpA然后倒回A中，因为这样的话，会malloc free临时数组非常非常多次，很不合算
```

#### 非递归算法

非递归的思路：

![image.png](https://i.loli.net/2020/03/24/6DcvuOZiSBGs4re.png)

```c++
//length =当前有序子列的长度,1,2,4,8....
//一趟归并，结果存在TmpA中
void Merge_pass(ElementType A[],ElementType TmpA[],int N,int length)
{
    //这个i<=N-2*length，是为了最后多出来的那个长度不一致的序列
    for(i=0;i<=N-2*length;i+=2*length)
        Merge1(A,TmpA,i,i+length,i+2*length-1);//从左至右，一对儿一对二的调用merge。merge1与原始merge的差别就在，merge1最后是将有序的内容放在TmpA中的。
    if(i+length<N)//说明还剩了，一个完整子列和一个残缺子列,将它俩归并
        Merge1(A,TmpA,i,i+length,N-1);
    	else//只剩了一个残缺子列，把剩下的部分直接倒入tmpA中就行了
            for(j=i;j<N;j++) TmpA[j]=A[j];
}
/*比如 0 1 2 3 |4 5 6 7  共8个位置
实际真正的过程与递归还是有点差异的
先把 0和1位置merge，然后是2和3位置merge，然后是4和5位置merge，6和7 merge，然后是0 1和 2 3merge，然后是4 5和6 7 merge， 最后是0 1 2 3和4 5 6 7一起merge
merge就是有序子列归并*/
```

```c++
//统一接口
//思路，TmpA和A来回倒
void Merge_sort(ElementType A[],int N)
{
    int length=1;//初始化子序列长度
    ElementType *TmpA;
    TmpA=malloc(N*sizeof(ElementType));
    if(TmpA!=NULL){//申请内存成功
        while(length<N)//每个while里一定是两次merge_pass，就算第一次已经有序了，我再多一次也是只一次拷贝而已，遮掩就保证了，结果一定是存在A里边的。
        {
            Merge_pass(A,TmpA,N,length);//TmpA中将存放结果
            length*=2;
            Merge_pass(TmpA,A,N,length);//A中将存放结果
            length*=2;
        }
        
        free(TmpA);
    }
    else Error("空间不足");
}
```



### 7.快速排序

### 8.表排序

### 9.桶排序 

基数排序是桶排序的升级版

## 十二、散列表

### 1.引子

**已知的几种查找方法：**

- 顺序查找： O(N)
- 二分查找（只能静态查找） O(log2N)
- 树可以实现动态查找（可以插入删除等）
  - 二叉搜索树  O(h)  h为二叉查找树的高度
  - 平衡二叉树O(log2N)

查找的本质：已知对象找位置

**两种提高查找效率的思路：**

- 有序安排对象：全序（二分查找）、半序（比如二叉搜索树，任何一个结点都比左边所有结点大，都比右边所有结点小）
- 直接“算出”对象的位置：散列

**散列查找法的两项基本工作：**

- 计算位置：构造散列函数确定关键词存储位置；
- 解决冲突：应用某种策略解决多个关键词位置相同的问题

**时间复杂度**几乎是常量：O(1)，即查找时间与问题规模无关

### 2.什么是散列表

```c++
//操作集
//创建一个长度为TableSize的符号表
SymbolTable InitializeTable(int TableSize);
//查找特定的名字Name是否在符号表Table中
Bool IsIn(SymbolTable Table,NameType Name);
//获取Table中指定名字Name对应的属性
AttributeType Find(SymbolTable Table,NameType Name);
//将Table中指定名字Name的属性修改为Attr
SymbolTable Modefy(SymbolTable Table,NameType Name,AttributeType Attr);
//向Table中插入一个新名字Name及其属性Attr
SymbolTable Insert(SymbolTable Table,NameType Name,AttributeType Attr);
//从Table中删除一个名字Name及其属性
SymbolTable Delete(SymbolTable Table,NameType name);
```

装填因子（Loading Factor）：设散列表空间大小为m，填入表中元素个数是n，则称α=n/m为散列表的装填因子。

**散列(Hashing)的基本思想是：**

1. 以关键字key为自变量，通过一个确定的函数h（散列函数），计算出对应的函数值h(key)，作为数据对象的存储地址。
2. 可能不同的关键字会映射到同一个散列地址上，即h(keyi)=h(keyj)  （当keyi≠keyj），成为冲突（collision)，需要某种解决冲突的策略

### 3.散列函数的构造

**一个“好”的散列函数一般应该考虑下列两个因素：**

1. 计算简单，以便提高转换速度
2. 关键词对应的地址空间分布均匀，以尽量减少冲突

#### 数字关键词的散列函数构造

**1.直接定址法**

取关键词的某个线性函数值为散列地址，即h(key)=a*key+b （a,b为常数)

例如;h(key)=1*key-1990

![image.png](https://i.loli.net/2020/03/25/OiI8y5sFj9hnW63.png)

**2.除留余数法**

散列函数为：h(key)=key mod p

例：h(key)=key % 17  。一般p取为表的大小（也可以不是），并且一般也要取为素数

![image.png](https://i.loli.net/2020/03/25/ueDSdZMy4BOg9Yx.png)

**3.数字分析法**

分析数字关键字在各位上的变化情况，取比较随机的位作为散列地址

比如：取11位手机号码key的后4位作为地址：后4位比前7位变化更多，更具有随机性

​		h(key)=atoi(key+7)

注：key是个11位字符串，指向首地址，+7后将指向后4个字符。atoi将字符串变为数字。

**4.折叠法**

把关键词分割成位数相同的几个部分，然后叠加：（比如下边是从后往前，3位3位分割，累加求和，再只取后3位。

这种方法是为了让结果  地址h(key)受更多的位数影响。

![image.png](https://i.loli.net/2020/03/25/PbzqBVY5JGQ4igo.png)

**5.平方取中法**

比如：把数字取平方，再只取中间的3位。这是为了让结果  地址h(key)受更多的位数影响

![image.png](https://i.loli.net/2020/03/25/lnKEzO3FAdwq1pR.png)

#### 字符关键词的散列函数构造

**1.简单的散列函数——ASCII码加和法**

h(key)=(∑key[i]) mod TableSize     // 冲突严重

key[i]是ASCII码，0-127中的字符部分（可能是把a-z 代表0-26?)

**2.简单的改进——前3个字符移位法**

把前3个字符看做27进制里的百位数、十位数、个位数

`h(key)=(key[0]*27^2+key[1]*27+key[2]) mod TableSize`  //26个字母+空格

缺点：1.冲突依然严重，如果字符串前3位相同，就无法区分

​			2. 空间浪费，这可以表示26^3种字符，但是实际上前三位可能出现的大概只有3000种。

**3.好的散列函数——移位法**

涉及关键词所有n个字符，并且分布得很好：

h(key)=(∑  key[n-i-1]*32^i)  mod TableSize

```c++
Index Hash(const char*Key,int TableSize)
{
    unsigned int h=0;//散列函数值，初始化为0
    while(*key!='/0')
        h=(h<<5)+*key++;//左移5位，就是乘32
    return h%TableSize;
}
```

### 4.冲突处理方法

**两种处理冲突的思路：**

- 换个位置：开放地址法
  - 这种方案里，数组空间一定得比要存的数多，也就是填充因子<1。
  - mod的数一般是tablesize（也可以不是），如果tabelszie不是素数，还要找一个最近的素数。
- 同一位置的冲突对象组织在一起：链地址法
  - 这种方案里，数据可以存储在链表开辟的空间里，所以不一定数组空间比要存的数多。

#### 4.1 开放地址法

一旦产生了冲突（该地址已有其它元素），就按某种规则去寻找另一空地址。

![image.png](https://i.loli.net/2020/03/27/lpJda2PBcMo1EUY.png)

#### 4.1.1线性探测法

- 线性探测的优点：
  - 只要有空位，一定可以存进去
- 线性探测的缺点：
  - 会产生聚集现象，即某个位置冲突会越来越多。

**注意：底下这个例子是mod 11，而不是mod Tablesize**=13

![image.png](https://i.loli.net/2020/03/27/igKLYACfskUw92z.png)

![image.png](https://i.loli.net/2020/03/27/m8TXkloPEK5YLfr.png)

成功平均查找长度（ASLs）：在散列表的每个元素，算算找每个元素需要多少步，然后平均一下

不成功平均查找长度（ASLu）：不在散列表的元素，我需要比较多少次，才能确定它不在。

![image.png](https://i.loli.net/2020/03/28/hnjID5pCJxamLtX.png)

因为这个题目是mod 11的，而不是mod Tablesize

11类是指，不在散列表的元素，且mod完余数分别是0-10的。

比如：key=22，它不在表，要断定它不在表，我需要从mod(22)=0开始，往后找，0位置一看是11，不是,1位置一看是30，不是,2位置空了，所以它肯定不在表。所以不在散列表，且mod完余数是0的，需要比较3次，才能确认它不在表。再比如，mod()完==1的，且不等于30的，即不在表的，需要比较2次，才能确认它不在表。

一定不是分成tablesize=13类，因为正常mod完，就没有等于11 12 的。

#### 4.1.2平方探测法

![image.png](https://i.loli.net/2020/03/28/O15iVa8wQ92HEgs.png)

碰撞1次：+1，碰撞2次：-1, 3次：+4……

- 平方探测的优点：
  - 一定程度上缓解了线性探测的聚集现象。
- 平方探测的缺点：
  - 有可能出现，表里明明还有空间，但是我碰撞来碰撞去，就是一直在碰撞，跳不到表里的空位上。
- 有定理显示：如果散列表长度TableSize是某个4k+3（k是正整数）形式的素数时，平方探测法就可以探查到整个散列表空间。

#### 4.1.3双散列探测法

![image.png](https://i.loli.net/2020/03/28/JDbKpPC3GnTUqcH.png)

**4.1.4再散列（Rehashing)**

- 当散列表元素太多（即装填因子α太大）时，查找效率会下降。
  - 实用最大装填因子一般取0.5<=α<=0.85
- 当装填因子过大时，解决的方法是加倍扩大散列表，这个过程叫做“再散列”（Rehashing)

#### 4.2分离链接法

分离链接法：将相应位置上冲突的所有关键词存储在同一个单链表中。	

### 5.leetcode

#### 1.设计哈希集合（即只存Key，没有value)

```c++
struct Node{
    int val;
    Node* next;
    Node(int val):val(val),next(nullptr){}
};//准备拉链的结构体
const int len=100;//数组长度
class MyHashSet {
public:
    /** Initialize your data structure here. */
    MyHashSet() {
        arr=vector<Node*>(len,nullptr);//初始化数组，否则vector不能按下标访问
    }
    
    void add(int key) {
        int ha_val=key%len;//计算哈希值
        Node* root=arr[ha_val];//取出链表头地址
        if(root==nullptr)//该位置还没有过任何节点
        {
            Node* node=new Node(key);
            arr[ha_val]=node;
            return;
        }
        else{//如果已经存过值了，需要拉链了
            while(root)
            {
                if(root->val==key) return;//已经存过这个key了
                if(root->next==nullptr){//一直到链尾
                    Node* node=new Node(key);//建立新节点
                    root->next=node;//插到链尾后边
                    return;
                }
                root=root->next;
            }
        }
     }
    
    void remove(int key) {
        int ha_val=key%len;
        Node* root=arr[ha_val];
        Node* pre=root;
        while(root)
        {
            if(root->val==key)
            {
                if(pre==root)//如果要删除是头结点
                    {
                        arr[ha_val]=root->next;
                    }
                    else//要删除的是中间结点
                    {
                        pre->next=root->next;
                    }
                    delete root;
                    return;
            }
            pre=root;
            root=root->next;
        }
    }
    
    /** Returns true if this set contains the specified element */
    bool contains(int key) {
        int ha_val=key%len;
        Node* root=arr[ha_val];
        while(root)
        {
            if(root->val==key) return true;
            root=root->next;
        }
        return false;
    }
private:
    vector<Node*> arr;//数组
};

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet* obj = new MyHashSet();
 * obj->add(key);
 * obj->remove(key);
 * bool param_3 = obj->contains(key);
 */
```

#### 2.设计哈希映射

```c++
struct Node{
    int key;
    int val;
    Node* next;
    Node(int k,int v):key(k),val(v),next(nullptr){}
};
const int len=100;
class MyHashMap {
public:
    /** Initialize your data structure here. */
    MyHashMap() {
        arr=vector<Node*>(len,nullptr);
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        int ha_val=key % len;//hash下标
        Node* root=arr[ha_val];
        if(root==nullptr)//如果这个位置还没有插入过node
        {
            Node* node=new Node(key,value);
            arr[ha_val]=node;
            return;
        }
        while(root)
        {
            if(root->key==key)//已经存在这个key了
            {
                root->val=value;
                return;
            }
            if(root->next==nullptr)//找到队尾也没找到这个key,插入队尾
            {
                Node* node=new Node(key,value);
                root->next=node;
                return;
            }
            root=root->next;
        }
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int ha_val=key % len;
        Node* root=arr[ha_val];
        while(root)
        {
            if(root->key==key)
                return root->val;
            root=root->next;
        }
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int ha_val=key % len;
        Node* root=arr[ha_val];
        Node* pre=root;
        while(root)
        {
            if(root->key==key)
            {
                if(pre==root)//如果要删除是头结点
                    arr[ha_val]=root->next;
                else//要删除的是中间结点
                    pre->next=root->next;
                    
                delete root;
                return;
            }
            pre=root;
            root=root->next;
        }
    }
private:
    vector<Node*> arr;
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */
```

#### 3.有多少小于当前数字的数字

```c++
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        vector<int> map(101,0);//统计每个数出现的次数
        for(const auto& e:nums)
        {
            map[e]++;
        }
        vector<int> res;
        for(const auto&e:nums)
        {
            int count=0;
            for(int i=0;i<e;i++) count+=map[i];//把所有比它小的数，出现的次数加起来
            res.push_back(count);
        }
        return res;
    }
};
```

#### 4.TinyURL 的加密与解密

```c++
//先把原字符串，hash编码（通过字符串散列函数）为一个数字。再用数字产生一个简短的字符串。
//这样做很不好，还是有可能有冲突。（可能两个网址编码后是一样的，第二个网址把第一个覆盖）
//这个题完全没有解码步骤，只是用hashmap给保存了下来
typedef unsigned long long ull;
class Solution {
public:

    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        string shortUrl=ULLToString(hashcode(longUrl));
        shortUrl="http://tinyurl.com/"+shortUrl;
        mymap[shortUrl]=longUrl;
        return shortUrl;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        return mymap[shortUrl];
    }
private:

    const ull base=11;
    const string code= "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    const ull len=code.size();
    unordered_map<string ,string> mymap;

    ull hashcode(const string& s){//hash编码，得到一个数字
        ull hash=0;
        for(auto c:s){//字符串的散列函数
            hash *=base;
            hash +=c;
        }
        return hash;
    }

    string ULLToString(ull n){//从hash编码数字生成简短string
        string s;
        while(n!=0)
        {
            s+=code[n % len];
            n/=len;
        }
        return s;
    }


};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```

```c++
//用计数器当短链。第一进来的叫0，第二个叫1...
//  http://0
//  http://1
//....
class Solution {
public:
    int count = 0;
    map<int, string> mmap;

    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        mmap[count] = longUrl;
        return "http://" + to_string(count++);
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        int key = stol(shortUrl.substr(7, shortUrl.length() - 7));
        return mmap[key];
    }
};
```

#### 5.拼写单词

```c++
//hash map
class Solution {
public:
    int countCharacters(vector<string>& words, string chars) {
        int res=0;
        unordered_map<char,int> mymap;//记录字母表中每个字母出现多少次
        for(const auto& e:chars)
        {
            map[e]++;//第一次出现的，默认是0
        }
        for(const string& word:words)
        {
            unordered_map<char,int> temp_map=mymap;
            bool flag=true;//用于判断是怎样退出的循环
            for(const char& e:word)
            {
                if(temp_map.count(e)==0||temp_map[e]==0)//如果有没出现过的字母或者字母次数不够用了
                {
                    flag=false;
                    break;
                }
                else
                    temp_map[e]--;
            }
            if(flag)//能拼出这个单词，那么长度累加
                res+=word.size();
        }
        return res;
    }
};
```

#### 6.两个数组的交集

```c++
//hash set也就是unordered_set
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> myset1;
        myset1.insert(nums1.begin(),nums1.end());//把nums1导入到set1中
        unordered_set<int> myset2;
        myset2.insert(nums2.begin(),nums2.end());//把nums2导入到set2中
        vector<int> res;
        for(const auto& i:myset1)
        {
            if(myset2.find(i)!=myset2.end())//集合1中的元素，集合2也有
                res.push_back(i);
        }
        return res;
    }
};
```

#### 7.键盘行

```c++
//'A'='a'-32;
class Solution {
public:
    vector<string> findWords(vector<string>& words) {
        vector<string> res;
        string s1="qwertyuiop";
        string s2="asdfghjkl";
        string s3="zxcvbnm";
        unordered_map<char,int> mymap;
        for(const char&e:s1)
        {
            mymap[e]=0;
            mymap[e-32]=0;//对应的大写字母
        }
        for(const char&e:s2)
        {
            mymap[e]=1;
            mymap[e-32]=1;//对应的大写字母
        }
        for(const char&e:s3)
        {
            mymap[e]=2;
            mymap[e-32]=2;//对应的大写字母
        }
        for(const auto&word:words)
        {
            int flag=mymap[word[0]];//记录第一个字母的信息，如果在同一行，其他字母的value应该都和这个flag相等
            for(const char&e:word)
            {
                if(mymap[e]!=flag)
                {   flag=-1;
                    break;
                }
            }
            if(flag!=-1)//如果一路比下来都相等的话，一定不是-1，是012里的一个值
                res.push_back(word);
        }
        return res;
    }
};
```

#### 8.独一无二的出现次数

```c++
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        unordered_map<int,int> mymap;//第二个int记录次数
        for(const auto& e:arr)
        {
            mymap[e]++;
        }
        unordered_set<int> myset;//查重
        for(auto it=mymap.begin();it!=mymap.end();it++)
        {
            myset.insert(it->second);
        }
        return myset.size()==mymap.size();//如果有重复的，那么set的个数肯定小于map个数
    }
};
```

#### 9.子域名访问计数

```c++
//string+map
class Solution {
public:
    vector<string> subdomainVisits(vector<string>& cpdomains) {
        unordered_map<string,int> mymap;
        for(string str:cpdomains)
        {
            int index = str.find(" ");
            int count=stoi(str.substr(0, index));//取出数字

            str=str.substr(index + 1);//删减字符串，从这儿到尾
            mymap[str]+=count;
            while(true)//每次str都在删减，直接寻找第一个"."位置即可
            {               
                index=str.find(".");
                if(index<0)
                    break;
                str=str.substr(index+1);
                mymap[str]+=count;
            }
        }
        vector<string> res;
        for(const auto&e:mymap)
        {
            string temp=to_string(e.second)+" "+e.first;
            res.push_back(temp);
        }
        return res;
    }
};
```

10.

## 十三、串

### 1.什么是串

- 线性存储的一组数据（默认是字符）
- 特殊的操作集
  - 求串的长度
  - 比较两串是否相等
  - 两串相接
  - 求子串
  - 插入子串
  - 匹配子串
  - 删除子串

### 2.串的模式匹配

给定一段文本，从中找出某个指定的关键字。

给定一段文本：string=S0S1...Sn-1

pattern=P0P1...Pm-1

求pattern在string中出现的位置。

### 3.功能实现

#### 1.c的库函数`strstr`

库函数是O(m*n)的

string长m，pattern长n

`char* strstr(char* string,char* pattern)`

```c++
#include<stdio.h>
#include<string.h>
int main()
{
    char string[]="This is a simple example.";
    char pattern[]="simple";
    char* p=strstr(string,pattern);
    if(p==NULL) printf("Not Found.\n")//没有匹配对象的话，将会返回空指针
    else 
        printf("%s\n",p);//这将会输出 simple example.
    return 0;
}
```

#### 2.KMP算法

![image.png](https://i.loli.net/2020/03/30/hblAgs7DWOnqKpt.png)

```c++
void BuildMatch(char *pattern,int *match)
{
    int i;//嫌老写match[j-1]麻烦，记做i
    int m=strlen(pattern);
    match[0]=-1;//match[0]一定是-1
    for(int j=1;j<m;j++)
    {
        i=match[j-1];
        while(i>=0 && pattern[i+1]!=pattern[j])
            i=match[i];
        if(pattern[i+1]==pattern[j])
            match[j]=i+1;
        else match[j]=-1;
    }
}
```

```c++
int KMP(char* string,char* pattern)
{
    int n=strlen(string);
    int m=strlen(pattern);
    int s,p;//string里的指针（下标）s，pattern里的指针（下标）p
    int* match=(int*)malloc(sizeof(int)*m);//m个int单元
    BuildMatch(pattern,match);//建立match数组
    s=p=0;
    while(s<n && p<m)
    {
        if(string[s]==pattern[p]){s++;p++;}
        else if(p>0)p=match[p-1]+1;
        else s++;
    }
    return (p==m)?(s-m):NotFound;
}
```



```c++
#include<stdio.h>
#include<string.h>
int main()
{
    char string[]="This is a simple example.";
    char pattern[]="simple";
    int p=KMP(string,pattern);
    if(p==-1) printf("Not Found.\n")//没有匹配对象的话，将会返回空指针
    else 
        printf("%s\n",string+p);//这将会输出 simple example.
    return 0;
}
```

## 十四、bitmap

https://blog.csdn.net/u012668513/article/details/80170753

先看看这样的一个场景：给一台普通PC，2G内存，要求处理一个包含40亿个不重复并且没有排过序的无符号的int整数，给出一个整数，问如果快速地判断这个整数是否在文件40亿个数据当中？（假如40亿个数据的最大值或者上限值是100亿）

问题思考：

  40亿个int占（40亿*4）/1024/1024/1024 大概为14.9G左右，很明显内存只有2G，放不下，因此不可能将这40亿数据放到内存中计算。要快速的解决这个问题最好的方案就是将数据搁内存了，所以现在的问题就在如何在2G内存空间以内存储着40亿整数。一个int整数在java中是占4个字节的即要32bit位，如果能够**用一个bit位来标识一个int整数**那么存储空间将大大减少，算一下这40亿个int需要的内存空间为 100亿/8/1024/1024大概为1.2G，这样的话我们完全可以将这40亿个int数放到内存中进行处理。

![tVT6Ag.png](https://s1.ax1x.com/2020/05/28/tVT6Ag.png)

```c++
#include <iostream>
#include <cstdio>
#include <vector>
using namespace std;

class Bitmap
{
	private:
		vector<int> v;
	public:
		Bitmap(int Max):v(Max/32+1,0){};//Max是40亿个数据里的最大值

		//set1
		void set(int i)//把数字i置1
		{
			v[i/32] |=(1<<(i%32));
		}
		
		//set0
		void clean(int i)//把数字i取消置1
		{
			v[i/32] &=~(1<<(i%32));
		}

		//if exist or not
		bool isExist(int i)
		{
			bool is=0;
			int temp=1<<(i%32);//模板，只有该位为1，其它为0
			if(temp== (v[i/32]&temp))//比如temp=0000 0100 ，v[i/32]&temp按位与将会得到0000 0100或者0000 0000，所以temp与v[i/32]&temp相等则存在
				is=1;
			return is;
		}
};

int main()
{
	int a[]={0,40,4,100,3};//这里的实现是只能正数，不能是负数。因为是按v[0]第0位代表数字0是否存在，v[0]第1位代表数字1是否存在。
	const int n = sizeof(a) / sizeof(int);//n个数
	int Max=0;
	int Min=INT32_MAX;
	for(int i=0;i<n;i++)
    {
        if(a[i] > Max) Max = a[i];
		if(a[i]<Min) Min=a[i];
    }

	int res[n];//存排序好的数字

	Bitmap mybitmap(Max);//建立bitmap，出入值是这5个值里的最大值，用于构建bitmap
	for(int i=0;i<n;i++) 
		mybitmap.set(a[i]);//把这5个数字存到bitmap里

	int count = 0;
    for(int i=Min;i<=Max;i++)//从输入序列的最小值到最大值范围内查找，减小搜索范围，提高速度
    {
        if(mybitmap.isExist(i))//挨着找，每找到一个就记录一个，这就是已经排序好的
            res[count++] = i;
    }

	for(int i=0;i<count;i++) cout<<res[i]<<" ";
    cout<<endl;
	return 0;
}
```

