# 1.框架

## 1.链表

### 1.遍历框架

- 线性迭代

  - ```c++
    /* 基本的单链表节点 */
    class ListNode {
        int val;
        ListNode* next;
    }
    
    vector<int> res;//保存遍历结果
    void traverse(ListNode* head){
        while(head){
            res.push_back(head->val);
            head=head->next;
        }
    }
    ```

- 递归遍历

  - ```c++
    /* 基本的单链表节点 */
    class ListNode {
        int val;
        ListNode* next;
    }
    
    vector<int> res;//保存遍历结果
    void traverse(ListNode* head){
        if(head==nullptr)
            return;
        //前序遍历，即正序打印链表  res.push_back(head->val);
        traverse(head->next);
        //后序遍历，即倒序打印链表  res.push_back(head->val);
    }
    ```

### 2.反转链表框架

#### 递归

- 递归反转整个链表

  - https://labuladong.gitbook.io/algo/shu-ju-jie-gou-xi-lie/shou-ba-shou-shua-lian-biao-ti-mu-xun-lian-di-gui-si-wei/di-gui-fan-zhuan-lian-biao-de-yi-bu-fen

  - ```c++
    /* 基本的单链表节点 */
    class ListNode {
        int val;
        ListNode* next;
    }
    
    ListNode* reverse(ListNode* head){
        //第一个条件仅适用于为链表空时。只要链表上非空，那么一定会在待反转的链表只有一个节点（只有一个节点的条件就是head->next==nullptr）时，返回该节点（其实就是原链表的尾部节点）
        if(head==nullptr || head->next==nullptr) 
            return head;
        ListNode* last=reverse(head->next);//将head->next之后的节点都反转到位了，并返回“反转完成后的链表头结点”
        head->next->next=head;  //2号节点现在指向nullptr，应该指向head
        head->next=nullptr;     //反转以后，head变成了链表尾，因此next要置nullptr
        return last;
    }
    ```

- 递归反转前N个节点

  - ```c
    /* 基本的单链表节点 */
    class ListNode {
        int val;
        ListNode* next;
    }
    
    ListNode* successor;//用于保存断裂的那个节点（后继节点）
    ListNode* reverseN(ListNode* head,int n){
        if(n==1)//待反转的链表只有一个节点（只有一个节点的条件就是n==1）时，返回该节点（其实就是第n个节点）
        {
            successor=head->next;//保存断裂的那个节点（后继节点）
            return head;
        }
        ListNode* last=reverseN(head->next,n-1);//将head->next之后的节点都反转到位了，并返回“反转完成后的链表头结点”
    
        head->next->next=head;//2号节点现在指向nullptr，应该指向head
        head->next=successor;//反转以后，head要接上successor节点
        return last;
    }
    ```

- 递归反转从left到right区域的链表

  - ```c++
    /* 基本的单链表节点 */
    class ListNode {
        int val;
        ListNode* next;
    }
    
    ListNode* reverseBetweenHelper(ListNode* head, int left, int right){
        if(left==1)//待反转的链表就是从1开始的，那就是reverseN问题
        	return reverseN(head,right);
        head->next=reverseBetweenHelper(head->next,left-1,right-1);//延续上left左和left的节点（以及从head到left左的节点顺序，没变而已）
        return head;
    }
    
    ListNode* successor;
    ListNode* reverseN(ListNode* head,int n){
        if(n==1)
        {
        successor=head->next;
        return head;
        }
        ListNode* last=reverseN(head->next,n-1);
        head->next->next=head;
        head->next=successor;
        return last;
    }
    ```

- k个一组反转链表

  - ```c++
        ListNode* reverseKGroupHelper(ListNode* head, int k) {
            if(head==nullptr)
                return nullptr;
            ListNode* a=head;
            ListNode* b=head;
            for(int i=0;i<k;i++){
                if(b==nullptr)
                {
                    return head;//不足k个就不反转         //b=nullptr;break;//不足k个也要反转
                }
                    
                b=b->next;
            }
            ListNode* newHead=reverse(a,b);
            a->next=reverseKGroupHelper(b,k);
            return newHead;
        }
        ListNode* reverse(ListNode* a,ListNode* b){//反转区间[a,b)内的元素，左闭右开
            ListNode* pre=nullptr;
            ListNode* cur=a;
            ListNode* nxt=nullptr;
            while(cur!=b){
                nxt=cur->next;
                cur->next=pre;
                pre=cur;
                cur=nxt;
            }
            //出while时cur==b，因此pre就是原顺序中b的前一个
            return pre;
        }
    ```

#### 迭代

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

- 迭代反转前N个节点

  - ```c++
    ListNode* reverseList(ListNode* head,int n) {
        ListNode* pre=nullptr;
        ListNode* cur=head;
        ListNode* nxt=nullptr;
        for(int i=0;i<n;i++){
            nxt=cur->next;//保存下一个节点
            cur->next=pre;//当前节点的下一个设置为上一个
            pre=cur;//cur变pre
            cur=nxt;//nxt变cur
        }
        //退出时，cur指向第n+1个,pre指向第n个。因为第n个已经翻转过了，退出时的cur肯定是还没翻转的，那就是第n+1个。
        head->next=cur;
        return pre;
    }
    ```

- 迭代反转从left到right区域的链表（还有另一种写法，在刷题-leetcode中）

  - ```c++
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* pre=nullptr;
        ListNode* cur=head;
        ListNode* nxt=nullptr;
        ListNode* tmpPreLeft=nullptr;
        ListNode* tmpLeft=nullptr;
        int count=0;
        
        if(left==right)//不用翻转
            return head;
        
        while(1){
            ++count;
            if(count==left) //记录拆分节点
            {
                tmpPreLeft=pre;
                tmpLeft=cur;
            }
            if(count<left){
                nxt=cur->next;
                pre=cur;
                cur=nxt;
                continue;
            }
            if(count>right){
                break;
            }
    
            nxt=cur->next;//保存下一个节点
            cur->next=pre;//当前节点的下一个设置为上一个
            pre=cur;//cur变pre
            cur=nxt;//nxt变cur
        }
            
        if(left!=1)    //如果left是头结点。那么影响最终的头结点是head还是别的
            tmpPreLeft->next=pre;   
        else
            head=pre;
    
        if(nxt!=nullptr)   //如果right是尾结点
            tmpLeft->next=cur;
        else
            tmpLeft->next=nullptr;
    
        return head;
    }
    ```

## 2.树

### 1.二叉树遍历框架

- 递归遍历

  - ```c++
    /* 基本的二叉树节点 */
    class TreeNode {
        int val;
        TreeNode* left, right;
    }
    
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

- 层序遍历（BFS，广度优先）

  - ```
    
    ```

### 2.N叉树遍历框架

- 递归遍历

  - ```c
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

- 层序遍历

  - ```
    
    ```

    

