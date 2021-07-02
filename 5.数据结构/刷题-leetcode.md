# 1.题目

## 1.链表

### [206. 反转链表（easy反转整个链表）](https://leetcode-cn.com/problems/reverse-linked-list)

解法：反转整个链表

#### 迭代

```c
class Solution {
public:
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
};
```

#### 递归（非常不建议）

```c
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        return reverse(head);
    }

    ListNode* reverse(ListNode* head){
        if(head==nullptr || head->next==nullptr)
            return head;
        ListNode* last=reverse(head->next);
        head->next->next=head;
        head->next=nullptr;
        return last;
    }
};
```

### [92. 反转链表 II（medium）反转left到right上的部分链表](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

解法：反转部分链表[a,b)

#### 迭代

```c
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode*a,*b;
        a=head;b=head;
        ListNode* tmpLeft=nullptr;
        
        int i=0;
        for(;i<left-1;i++){//找节点a
            if(i==left-2)
                tmpLeft=a;
            a=a->next;
            b=b->next;    
        }//i<left-1，出来时a刚好停在left上。i<left，就停在left的下一个上了
        
        for(;i<right;i++){//找节点b
            b=b->next;
        }

        ListNode* tmpHead=reverse(a,b);
        a->next=b;
        if(left==1)
            return tmpHead;
        tmpLeft->next=tmpHead;
        return head;
    }

    ListNode* reverse(ListNode*a,ListNode*b){//经典模板，反转[a,b)上的节点。做这种题一定能过要切记[a,b)区间，反转以后的头部会由reverse函数返回。而尾部，就是a节点！
        ListNode* pre,*cur,*nxt;
        pre=nullptr;cur=a;nxt=nullptr;
        while(cur!=b){
            nxt=cur->next;
            cur->next=pre;
            pre=cur;
            cur=nxt;
        }
        return pre;
    }
};

/*比如
1->2->3->4->5
left=1
right=4
那么a应该是1，b应该是5，因为左开右闭。[1,5)即反转 [1,4]*/
```

### [25. K 个一组翻转链表(hard)](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

解法：反转部分链表[a,b)

**做这种题一定能过要切记[a,b)区间，反转以后的头部会由reverse函数返回。而尾部，就是a节点！**

- **命题1：不足K个也反转（迭代法!）**

  - ```c
    class Solution {
    public:
        ListNode* reverseKGroup(ListNode* head, int k) {
            ListNode*a=head,*b=head;
            ListNode* preA=head;
            ListNode* resHead=nullptr;
            while(a!=nullptr){
                for(int i=0;i<k;i++){
                    if(b==nullptr){
                        break;
                    }
                    b=b->next;
                }
                ListNode* tmpHead=reverse(a,b);
                   
                if(resHead==nullptr){//如果本次是第一次while循环时，记下最终输出时的头结点
                    resHead=tmpHead;//记下最终输出时的头结点
                    preA=a;//记录本轮tail，供下一轮使用
                    a=b;   //左开右闭，因此本轮的b就是下一轮的a    
                    continue;//第一次while循环，即第一个小子序列，那么不用跟上一轮衔接
                }
                preA->next=tmpHead;//衔接上一轮跟本轮的结果   
                preA=a;//记录本轮tail，供下一轮使用
                a=b;//左开右闭，因此本轮的b就是下一轮的a
            }
            return resHead;
        }
    
        ListNode* reverse(ListNode*a,ListNode*b){//经典模板，反转[a,b)上的节点。做这种题一定能过要切记[a,b)区间，反转以后的头部会由reverse函数返回。而尾部，就是a节点！
            ListNode* pre,*cur,*nxt;
            pre=nullptr;cur=a;nxt=nullptr;
            while(cur!=b){
                nxt=cur->next;
                cur->next=pre;
                pre=cur;
                cur=nxt;
            }
            return pre;
        }
    };
    ```

- 命题2：如果节点总数不是 *k* 的整数倍，那么请将最后**剩余的节点保持原有顺序**。（迭代法!）

  - ```c
    class Solution {
    public:
        ListNode* reverseKGroup(ListNode* head, int k) {
        	ListNode*a=head,*b=head;
            ListNode* preA=head;
            ListNode* resHead=nullptr;
            bool flag=true;
            while(a!=nullptr){
                for(int i=0;i<k;i++){
                    if(b==nullptr){
                        flag=false;
                        break;
                    }
                    b=b->next;
                }
                ListNode* tmpHead=a;
                if(flag)
                   tmpHead=reverse(a,b);
                   
                if(resHead==nullptr){//第一次while循环
                    resHead=tmpHead;
                    preA=a;//tail
                    a=b;       
                    continue;
                }
                preA->next=tmpHead;   
                preA=a;//tail
                a=b;
            }
            return resHead;
     	}
        
        ListNode* reverse(ListNode*a,ListNode*b){
            ListNode* pre,*cur,*nxt;
            pre=nullptr;cur=a;nxt=nullptr;
            while(cur!=b){
                nxt=cur->next;
                cur->next=pre;
                pre=cur;
                cur=nxt;
            }
            return pre;
        }
    };
    ```

### [876. 链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

解析：画一下图，链表长度为奇偶数时各是什么情形

```c
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode *slow,*fast;
        slow=head;fast=head;
        while(fast!=nullptr && fast->next!=nullptr){
            slow=slow->next;
            fast=fast->next->next;
        }
        return slow;
    }
};
```



### [234. 回文链表(easy判断是否是回文链表)](https://leetcode-cn.com/problems/palindrome-linked-list/)

解法1：使用递归遍历  

解法2：使用双指针找中点+反转链表

- 最简单的解法（将链表的值遍历到一个数组vector里边，再按一般方法比较）

  - 时间复杂度O(N)，空间复杂度O(N)。空间占用在人工创建的数组上。
  - 略。AC没过。

- 递归**（不推荐）**

  - 时间复杂度O(N)，空间复杂度O(N)。空间占用在系统堆栈上。

  - ```c
    class Solution {
    public:
        bool isPalindrome(ListNode* head) {
            left=head;
            return traverse(head);
        }
        
        ListNode* left;
        
        bool traverse(ListNode* head){
            if(head==nullptr) return true;
            bool res=traverse(head->next);
            //后序遍历
            res=res && head->val==left->val;
            left=left->next;
            return res;
        }
    };
    ```

- 双指针找中点+反转后半部分链表+比较**（推荐）**

  - 时间复杂度O(N)，空间复杂度O(1)。因为没有多占用任何空间。

  - ```c++
    class Solution {
    public:
        bool isPalindrome(ListNode* head) {
    		//快慢指针找中点
            ListNode *slow,*fast;
            slow=head;fast=head;
            while(fast!=nullptr && fast->next!=nullptr){
                slow=slow->next;
                fast=fast->next->next;
            }
            //链表长度为奇数时，slow正好停在中点上，应该把它往后挪一下才是后半段回文
            if(fast!=nullptr)
                slow=slow->next;
            //left就是前半段正序回文。right是倒序回文反转后，所以应该跟left序严格一致
            ListNode* left=head;
            ListNode* right=reverse(slow);
            while(right!=nullptr){
                if(left->val!=right->val)
                    return false;
                left=left->next;
                right=right->next;
            }
            return true;
        }
    
        ListNode* reverse(ListNode*head){//标准模板，从head翻转到nullptr
            ListNode* pre,*cur,*nxt;
            pre=nullptr;cur=head;nxt=nullptr;
            while(cur!=nullptr){
                nxt=cur->next;
                cur->next=pre;
                pre=cur;
                cur=nxt;
            }
            return pre;
        }
    };
    ```

  - 如果嫌弃链表结构被破坏了，可以如下补救。记录一下p、q指针，然后把q再次翻转回来，再衔接上。（参看labuladong p282页）

  - ```c
    class Solution {
    public:
        bool isPalindrome(ListNode* head) {
    		//快慢指针找中点
            ListNode *slow,*fast;
            slow=head;fast=head;
            ListNode* p,*q;
            while(fast!=nullptr && fast->next!=nullptr){
                slow=slow->next;
                fast=fast->next->next;
                if(fast->next->next==nullptr)//记录下链表为偶数时的P
                    p=slow;
            }
            //链表长度为奇数时，slow正好停在中点上，应该把它往后挪一下才是后半段回文
            if(fast!=nullptr){
                p=slow;//记录下链表为奇数时的P
                slow=slow->next;
            }
            
            //left就是前半段正序回文。right是倒序回文反转后，所以应该跟left序严格一致
            ListNode* left=head;
            ListNode* right=reverse(slow);
            q=right;//记录下q
            while(right!=nullptr){
                if(left->val!=right->val){
                    p->next=reverse(q);//再翻转回来，并衔接上
                    return false;
                }
                left=left->next;
                right=right->next;
            }
            p->next=reverse(q);//再翻转回来，并衔接上
            return true;
        }
    
        ListNode* reverse(ListNode*head){//标准模板，从head翻转到nullptr
            ListNode* pre,*cur,*nxt;
            pre=nullptr;cur=head;nxt=nullptr;
            while(cur!=nullptr){
                nxt=cur->next;
                cur->next=pre;
                pre=cur;
                cur=nxt;
            }
            return pre;
        }
    };
    ```

### [141. 环形链表(easy判断有环没环)](https://leetcode-cn.com/problems/linked-list-cycle/)

解析：快慢指针，有环的话快指针一定能追上慢指针

```c
//（举个最特殊的例子，整个链表是个完整的闭环。其他情况下是一样的，当两个指针都进入环时，就退化为这个情况了。两者要相遇，肯定是在那个环里面。）可以看做快指针与慢指针的距离相差一圈，快指针在追赶慢指针。快指针与慢指针的速度相差1，每一轮while，它俩的距离就缩小1。因此慢指针转一圈时，肯定刚好追上。（大部分情况下，慢指针根本跑不够一圈，就被追上了）
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *slow,*fast;
        slow=head;fast=head;
        
        while(fast!=nullptr && fast->next!=nullptr){
            slow=slow->next;
            fast=fast->next->next;
            if(fast==slow)//切记，判断要写在两个指针更新指向的后边。写在前边的话，第一轮循环时fast肯定等于slow等于head。
                return true;
        }
        return false;
    }
};
```

### [142. 环形链表 II（medium链表有环，返回环的入口）](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

解析：先判断有没有环。有环时，从slow和fast的交点处，再继续跑k-m次循环，再相遇时即环的入口。

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow,*fast;
        slow=head;fast=head;
        bool haveRing=false;
        
        while(fast!=nullptr && fast->next!=nullptr){//先判断有环没环
            slow=slow->next;
            fast=fast->next->next;
            if(slow==fast)
            {
                haveRing=true;
                break;
            }
        }
        
        if(!haveRing)
            return nullptr;
        while(head!=slow){//从相遇点，再次跑k-m次循环，再相遇时即环的入口。理由参看labuladong和纸质笔记
            head=head->next;
            slow=slow->next;
        }
        return head;
    }
};
```

### [面试题 02.02. 返回倒数第 k 个节点（easy）](https://leetcode-cn.com/problems/kth-node-from-end-of-list-lcci/)

解析：fast先走k步，然后同速前进。fast到尾部时，slow就停在了倒数第k个节点上

```c
class Solution {
public:
    int kthToLast(ListNode* head, int k) {
        ListNode *slow,*fast;
        slow=head;fast=head;

        for(int i=0;i<k;i++){
            if(fast==nullptr)
                return -1;
            fast=fast->next;
        }

        while(fast!=nullptr){
            slow=slow->next;
            fast=fast->next;
        }
        return slow->val;

    }
};
```

### [21. 合并两个有序链表（easy）](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

解析：

- 方法1：纯粹的比较。不用哨兵

  - ```c++
    class Solution {
    public:
        ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
            if(l1==nullptr) return l2;//避免空链表进入
            if(l2==nullptr) return l1;
            ListNode* head1=l1;
            ListNode* head2=l2;
            ListNode* pre=nullptr;//记录上一次的节点
            
            while(head1!=nullptr && head2!=nullptr){
                if(head1->val <=head2->val){
                    if(pre==nullptr)//如果是第一次进入
                        pre=head1;
                    else{
                        pre->next=head1;
                        pre=head1;
                    }
    
                    head1=head1->next;
                }
                else{
                    if(pre==nullptr)//如果是第一次进入
                        pre=head2;
                    else{
                        pre->next=head2;
                        pre=head2;
                    }
                    head2=head2->next;
                }
            }//while的括号
            
            if(head1==nullptr){//退出时必有一个链表走到尾部了
                pre->next=head2;
            }
            else{
                pre->next=head1;
            }
            
            return (l1->val <=l2->val)?l1:l2;//两者更大那个肯定是合并后的首部。或者是声明一个指针，在第一次进入时保存下来首节点也行。
        }
    };
    ```

  - 备注：如果要求合并时去重的话，多加一个head1->val ==head2->val时的操作，让两个指针都后移。核心思想就是相等时都取head1上的值

  - ```
    else{
        if(pre==nullptr)
        pre=head1;
        else{
        pre->next=head1;
        pre=head1;
        }
        head1=head1->next;
        head2=head2->next;
    }
    ```

- 方法2：使用哨兵节点

  - ```c++
    class Solution {
    public:
        ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
            ListNode* dummy=new ListNode(-1,nullptr);
            ListNode* head=dummy;//用来维系新链表
            while(l1!=nullptr || l2!=nullptr){
                
                if(l1!=nullptr && l2!=nullptr){//两条链表上都有值
                    if(l1->val<=l2->val){//比大小，将较小的那个接到新链表尾部
                        head->next=l1;
                        head=head->next;
                        l1=l1->next;
                    }
                    else{
                        head->next=l2;
                        head=head->next;
                        l2=l2->next;
                    }
                }
                else if(l1==nullptr){//l1链表其实已经走到头了
                        head->next=l2;
                        head=head->next;
                        l2=l2->next;
                        
                }
                else{//l2链表其实已经走到头了
                        head->next=l1;
                        head=head->next;
                        l1=l1->next;
                }
            }
            //退出时，不论谁是最后一个节点，它之后的节点都肯定是Nullptr
            ListNode*res=dummy->next;
            delete dummy;
            return res;
    
        }
    };
    ```

- 总结：

  - 其实 while(head1!=nullptr && head2!=nullptr) 还是 while(l1!=nullptr || l2!=nullptr) 都行。 &&时就是在while外继续处理，||时就是在while里处理
  - 使用哨兵确实简洁很多。

### [160. 相交链表（easy，跟剑指 Offer 52. 两个链表的第一个公共节点一样）](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

解析：双指针，每个指针都跑两圈，必然总移动距离相同。

备注：参看剑指 Offer 52. 两个链表的第一个公共节点。那个写法更为简洁。

- 正确写法：

  - ```c++
    class Solution {
    public:
        ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
            ListNode *ptrA=headA;
            ListNode *ptrB=headB;
    
            while(!(ptrA==nullptr && ptrB==nullptr)){//停下来的条件是两个指针都是nullptr，即两个指针都跑完了两人的总距离时
                if(ptrA==ptrB)
                    return ptrA;
                    
                if(ptrA==nullptr)
                    ptrA=headB;
                else
                    ptrA=ptrA->next;
    
                if(ptrB==nullptr)
                    ptrB=headA;
                else
                    ptrB=ptrB->next;
            }
            return nullptr;
        }
    };
    ```

- 错误：尤其注意，**当一个指针跑到末尾时，指向另一个的开头，这也算移动了一步，可不能让它一个while里移动两步**（虽然大多情况下没有问题，因为两个指针是同样的操作。但是某些特殊案例它就过不了，比如一个链表长度为1，另一个长度为2）

  - ```c++
    class Solution {
    public:
        ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
            ListNode *ptrA=headA;
            ListNode *ptrB=headB;
    
            while(!(ptrA==nullptr && ptrB==nullptr)){//停下来的条件是两个指针都是nullptr，即都跑完了两人的总距离
                if(ptrA==ptrB)
                    return ptrA;
                if(ptrA==nullptr)
                    ptrA=headB;//算移动了一步
                if(ptrB==nullptr)
                    ptrB=headA;
                ptrA=ptrA->next;//又移动了一步
                ptrB=ptrB->next;
            }
            return nullptr;
        }
    };
    ```

### [23. 合并K个升序链表（hard）](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

解析：我使用的是最朴实的思想，几乎不使用额外内存。跟合并2个有序链表的思想一致。每个节点，遍历一个链表数目比如为n。所以是O(n*N)

```c
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        vector<ListNode*> heads;
        for(int i=0;i<lists.size();i++){
            heads.push_back(lists[i]);
        }
        
        ListNode* prePtr=nullptr;
        ListNode* resPtr=nullptr;
        while(1){//每一次while，只往后衔接一个节点。
            int minIndex=0;
            int numOfNullptr=0;
            ListNode* minPtr=nullptr;
            
            for(int i=0;i<lists.size();i++){//比如3条链表，找出这3个当前节点上的最小值。存到minPtr和minIndex里
                if(heads[i]==nullptr)
                {
                    numOfNullptr++;
                    continue;
                }
                if(minPtr==nullptr || minPtr->val > heads[i]->val)
                {
                    minPtr=heads[i];
                    minIndex=i;
                }
            }
            
            if(numOfNullptr==lists.size())//while的终止条件，所有的链表都走到nullptr上了
                break;

            if(prePtr==nullptr)//整个题目的第一次遍历
            {
                prePtr=minPtr;
                resPtr=minPtr;//记录最终的返回头结点
            }
            else
            {
                prePtr->next=minPtr;//衔接节点
                prePtr=minPtr;
            }
            heads[minIndex]=heads[minIndex]->next;//节点动一个
        }
        return resPtr;//返回整个题目的第一次遍历
    }
};
```

### [2. 两数相加（medium）类似于合并两个有序链表](https://leetcode-cn.com/problems/add-two-numbers/)

时间复杂度O(N)，空间复杂度O(1)

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if(l1==nullptr) return l2;
        if(l2==nullptr) return l1;
        
        int addone=0;//进位标志
        ListNode* head=nullptr;//用来一直往后延续新的链表
        ListNode* resPtr=head;//用于记录最终的返回值，其实就是最头部
        
        while(l1!=nullptr|| l2!=nullptr){//只要有一个链表上还有值
            int val1=0,val2=0;
            if(l1!=nullptr){//有值的话就取出来，没值的话就是当0
                val1=l1->val;
            }
            if(l2!=nullptr){//有值的话就取出来，没值的话就是当0
                val2=l2->val;
            }
            
            int res=val1+val2+addone;//第一个数+第二个数+进位
            if(res>=10)//如果超过10
            {
                res=res-10;
                addone=1;//设置进位
            }
            else{
                addone=0;
            }
            
            if(head==nullptr){//如果是第一次进入循环，建立首部，不需要修改某个节点来指向它
                head=new ListNode(res,nullptr);
                resPtr=head;//用于记录最终的返回值，其实就是最头部
            }
            else{//如果不是第一次进入循环，那就得多一个修改前一个节点来指向它的操作
                ListNode* newPtr=new ListNode(res,nullptr);
                head->next=newPtr;
                head=newPtr;//将当前设置为head
            }
            
            if(l1!=nullptr){//如果不为空，那就往后跳一步
                l1=l1->next;
            }
            if(l2!=nullptr){//如果不为空，那就往后跳一步
                l2=l2->next;
            }
        }//while
        
        if(addone==1)//判断最后是不是还有一个进位
        {
            ListNode* newPtr=new ListNode(1,nullptr);
            head->next=newPtr;
        }
        return resPtr;
    }
};
```

### [143. 重排链表（medium 运用了链表找中点、翻转链表等方法）](https://leetcode-cn.com/problems/reorder-list/)

- 方法1：（不推荐）

  - 遍历，存到两个vector里，再来回指向

  - 时间复杂度O(N)、空间复杂度O(N)。因为有递归，也有额外的vector储存

  - ```c++
    class Solution {
    public:
        void reorderList(ListNode* head) {
            traverse(head);//遍历存到vector里
            if(vec_pos.size()<=1) return;
            int i=0;
            
            for(;i<(vec_pos.size()/2);i++){//修改指向    
                vec_pos[i]->next=vec_neg[i];
                if(i+1<vec_pos.size()/2)
                    vec_neg[i]->next=vec_pos[i+1];        
            }
            
            i=i-1;//退出时，i已经走到中点或者中间点的第二点了，需要再倒回来一步
            if((vec_pos.size()%2)==1)//奇偶情况下，不一样
            {
                vec_neg[i]->next=vec_pos[i+1];
                vec_pos[i+1]->next=nullptr;
            }
            else{
                vec_neg[i]->next=nullptr;
            }
            return;
        }
        
        vector<ListNode*> vec_pos;
        vector<ListNode*> vec_neg;
        void traverse(ListNode*head){//经典遍历
            if(head==nullptr)
                return;
            vec_pos.push_back(head);
            traverse(head->next);
            vec_neg.push_back(head);
        }
    };
    ```

- 方法2：（推荐）

  - 第一步：找到原链表的中点。（类似于回文链表里找中点）
  
  - 第二步：将原链表的右半端反转（翻转一部分链表）
  
  - 第三步：将原链表的两端合并。
  
  - 时间复杂度O(N)，空间O(1)
  
  - ```c++
    class Solution {
    public:
        void reorderList(ListNode* head) {
            ListNode* medium=findMedium(head);//寻找中间节点的右一节点（奇数时是右边一个，偶数时是靠右的）
            ListNode* head2=reverse(medium);//翻转后半段链表
            //1->2->3->4->5->null    变成了   1->2->3->4<-5
            //									      ↓
            //                                       null   
            //1->2->3->4->null        变成了  1->2->3<-4
            //                                     ↓
            //                                    null       
            
            while(head2!=nullptr){//调节指向
                ListNode* nxt1=head->next;
                ListNode* nxt2=head2->next;
                head->next=head2;
                head2->next=nxt1;
                head=nxt1;
                head2=nxt2;
            }
            head->next=nullptr;//奇偶时，都是这样。画个图理解一下。奇数时好理解
            					//偶数时，比如退出时是1->4->2->3->3。最后的Head其实跟head2里的最后一个节点时同								//一个
        }
        ListNode* slow,*fast;
    
        ListNode* findMedium(ListNode* head){
            slow=head;fast=head;
            while(fast!=nullptr && fast->next!=nullptr){
                slow=slow->next;
                fast=fast->next->next;
            }
    
            if(fast!=nullptr)
                slow=slow->next;
            return slow;
        }
    
        ListNode* reverse(ListNode* head){
            ListNode* pre,*cur,*nxt;
            pre=nullptr;cur=head;nxt=nullptr;
            while(cur!=nullptr){
                nxt=cur->next;
                cur->next=pre;
                pre=cur;
                cur=nxt;
            }
            return pre;
        }
    
    };
    ```

### [19. 删除链表的倒数第 N 个结点（medium）类似于返回倒数第k个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

解析：快慢指针

- 解法1：（不推荐）

  - fast提前移动n+1步，那么slow将停在要删除的节点的前一个

  - ```c++
    class Solution {
    public:
        ListNode* removeNthFromEnd(ListNode* head, int n) {
            if(n<=0)
                return head;
            
            ListNode *slow,*fast;
            slow=head;fast=head;
            
            bool delHeadFlag=false;//记录是不是要删除头结点
            for(int i=0;i<n+1;i++){//注意是fast提前跑n+1步。那么slow将停在要删除的节点的前一个
                if(fast==nullptr)
                {
                    delHeadFlag=true;
                    break;
                }
                fast=fast->next;
            }
            
            if(delHeadFlag){//如果是删除头结点
                ListNode* tmpHead=head->next;
                delete head;
                return tmpHead;
            }
    
            while(fast!=nullptr){//不是删除头结点，那么快慢指针开始跑。
                slow=slow->next;
                fast=fast->next;
            }
            ListNode* tmp=slow->next->next;
            delete slow->next;
            slow->next=tmp;
            return head;
        }
    };
    ```

- 解法2：（我个人推荐这种吧，这样的话跟之前的返回倒数第K个节点的方法是一模一样的）

  - fast提前移动n步，slow将停在要删除的那个节点。需要一个Pre节点来记录它之前的那个节点

  - ```c++
    class Solution {
    public:
        ListNode* removeNthFromEnd(ListNode* head, int n) {
            if(n<=0)
                return head;
            ListNode *slow,*fast;
            slow=head;fast=head;
    
            for(int i=0;i<n;i++){
                if(fast==nullptr)//n比链表中的节点数还大
                    return head;
                fast=fast->next;
            }
            ListNode*pre=nullptr;//记录要删除的那个节点的前一个
            while(fast!=nullptr){
                pre=slow;
                slow=slow->next;
                fast=fast->next;
            }
            
            if(pre==nullptr)//说明要删除的是头结点
            {
                ListNode *tmpHead=slow->next;
                delete slow;
                return tmpHead;
            }
            //要删除的不是头结点
            pre->next=slow->next;
            delete slow;
    
            return head;
        }
    };
    ```

### [83. 删除排序链表中的重复元素（easy，就是个遍历，多一个指针保存pre节点而已）](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

解析：遍历，相当于双指针pre cur

```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* pre=nullptr;
        ListNode* cur=head;
        while(cur!=nullptr){
            if(pre==nullptr){//第一次进入循环，保存头结点为pre，不进行比较操作
                pre=cur;
                cur=cur->next;
                continue;
            }
            
            if(pre->val==cur->val){//值相等
                ListNode *nxt=cur->next;
                delete cur;
                pre->next=nxt;
                cur=nxt;
            }
            else{//值不等
                pre=cur;
                cur=cur->next;
            }
        }
        return head;//一直是cur在移动,head没动过
    }
};
```

### [82. 删除排序链表中的重复元素 II（medium）](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

解法：3个指针，pre left right，画画图。涉及到可能删除首节点的，设置一个dummy节点非常方便

```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode *dummy=new ListNode(0,nullptr);
        dummy->next=head;
        ListNode *pre=dummy;//用来维系新链表
        ListNode *left=head;//左节点
        ListNode *right=head;//右节点。左节点至右节点之间的节点都要删除（除非它俩紧挨，说明仅一个元素肯定不重复）
        while(right!=nullptr){
            while(right!=nullptr && right->val == left->val)//right指针右移
            {
                right=right->next;
            }

            if(right==left->next){//left节点上是独一无二的元素
                pre->next=left;
                pre=left;
                left=right;
            }
            else{//有重复元素，从left到right之间的节点都要删除
                while(left!=right){
                    ListNode* nxt=left->next;
                    delete left;
                    left=nxt;
                }
            }

        }
        pre->next=nullptr;//这个经常忘记。一定要保证链表结束时，是以nullptr结尾的
        ListNode *res=dummy->next;
        delete dummy;
        return res;
    }
};
```

### [24. 两两交换链表中的节点（medium，跟每k个一组反转链表一样）](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

解法：遇到头结点未知的情况时，都建议用dummy节点。它比k个一组反转简单，因为不用考虑不足k个时是反转还是不反转的情况。

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode *dummy=new ListNode(0,nullptr);//dummy节点
        dummy->next=head;
        ListNode *pre=dummy;
        ListNode *right=pre->next;
        ListNode *left=right;
        while(right!=nullptr){   
            for(int i=0;i<2;i++){//把这个2换成k。就是每个一组反转。for循环是在移动right节点
                if(right==nullptr)
                    break;
                right=right->next;
            }
            ListNode* tmpHead=reverse(left,right);//反转[left,right)
            pre->next=tmpHead;//pre来串起来链表
            pre=left;//pre移动，来维系链表
            left=right;//进入下一段循环
        }
        ListNode *res=dummy->next;
        delete dummy;
        return res;
    }
    
    ListNode* reverse(ListNode* a,ListNode* b){//经典的翻转[a,b)模板
        ListNode *pre,*cur,*nxt;
        pre=nullptr;cur=a;nxt=nullptr;
        while(cur!=b){
            nxt=cur->next;
            cur->next=pre;
            pre=cur;
            cur=nxt;
        }
        return pre;
    }
};
```

### [剑指 Offer 18. 删除链表的节点（easy，遍历）](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

解法：可能会删除头结点，加个dummy开始遍历，双指针一个pre指向dummy 一个cur指向head

```c++
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        ListNode* dummy=new ListNode(0);
        dummy->next=head;
        ListNode* pre=dummy;
        ListNode* cur=head;
        while(cur!=nullptr && cur->val!=val){
            pre=cur;
            cur=cur->next;
        }
        if(cur!=nullptr){
            ListNode* nxt=cur->next;
            pre->next=nxt;
            //delete cur; //理论上不该注释这一行,leetcode的bug
        }
        ListNode *res=dummy->next;
        delete dummy;
        return res;
    }
};
```

注意：呵呵了，这个题delete删除cur节点反而还通过不了，在本地完全没错。理论上必须加上delete才对。

### [剑指 Offer 52. 两个链表的第一个公共节点（easy，类似于环形链表的入口）](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

解法：两个指针来跑，跑完自己的去跑对方的

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *ptrA=headA;
        ListNode *ptrB=headB;
        while(ptrA!=ptrB){//ptrA==ptrB==nullptr时，也会退出，即不相交。两个指针都跑完了自己+对方的长度
            if(ptrA==nullptr)
                ptrA=headB;
            else
                ptrA=ptrA->next;

            if(ptrB==nullptr)
                ptrB=headA;
            else
                ptrB=ptrB->next;
        }
        return ptrA;
    }
};
```

尤其注意，**当一个指针跑到末尾时，指向另一个的开头，这也算移动了一步，可不能让它一个while里移动两步**（虽然大多情况下没有问题，因为两个指针是同样的操作。但是某些特殊案例它就过不了，比如一个链表长度为1，另一个长度为2）

### [剑指 Offer 25. 合并两个排序的链表（easy）](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

解法：设置dummy节点

第一种写法：`while(l1!=nullptr && l2!=nullptr)`

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *dummy=new ListNode(0);
        ListNode *pre=dummy;
        while(l1!=nullptr && l2!=nullptr){
            if(l1->val<=l2->val){
                pre->next=l1;
                l1=l1->next;
            }
            else{
                pre->next=l2;
                l2=l2->next;
            }
            pre=pre->next;
        }
        if(l1==nullptr){//退出时是谁跑完了
            pre->next=l2;
        }
        else{
            pre->next=l1;
        }
        ListNode *res=dummy->next;
        delete dummy;
        return res;
    }
};
```

第二种写法：区别在`while(l1!=nullptr || l2!=nullptr)`

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *dummy=new ListNode(0);
        ListNode *pre=dummy;
        while(l1!=nullptr || l2!=nullptr){

            if(l2==nullptr){
                pre->next=l1;
                l1=l1->next;
            }
            else if(l1==nullptr){
                pre->next=l2;
                l2=l2->next;
            }
            else{
                if(l1->val<=l2->val){
                    pre->next=l1;
                    l1=l1->next;
                }
                else{
                    pre->next=l2;
                    l2=l2->next;
                }         
            }
            pre=pre->next;
        }

        ListNode *res=dummy->next;
        delete dummy;
        return res;
    }
};
```

### [138. 复制带随机指针的链表（medium，hashmap不是最佳，最佳方法待解决）](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

解析：

- 方法1：用hash表。时间复杂度O(2N)，空间复杂度O(N)。这肯定不是最巧妙的方法。但是想法最简单

  - ```c++
    class Solution {
    public:
        Node* copyRandomList(Node* head) {
            Node* cur=head;
            unordered_map<Node*,Node*> mymap;
            
            while(cur!=nullptr){//第一遍遍历，创建节点
                Node* newCur=new Node(cur->val);
                mymap[cur]=newCur;
                cur=cur->next;
            }
            mymap[nullptr]=nullptr;
            cur=head;
            while(cur!=nullptr){//第二遍遍历，修改指向
                mymap[cur]->next=mymap[cur->next];
                mymap[cur]->random=mymap[cur->random];
                cur=cur->next;
            }
            return mymap[head];   
        }
    };
    ```

- 方法2：回溯

- 方法3：参看leetcode

### [328. 奇偶链表（medium，有点像快慢指针）](https://leetcode-cn.com/problems/odd-even-linked-list/)

解法：双指针遍历。遇到一个大坑，之前没有设置successor，误以为while退出时head->next还是第二端起点。其实head->next早就被修改了！！一定要小心，第二段链表的起点。

```c++
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head==nullptr) return nullptr;
        
        ListNode* ptrOdd=head;
        ListNode* ptrEven=head->next;
        ListNode *successor=head->next;//保存第二段链表的起点
        
        while(ptrEven!=nullptr && ptrEven->next!=nullptr){
            ptrOdd->next=ptrEven->next;
            ptrOdd=ptrOdd->next;

            ptrEven->next=ptrOdd->next;
            ptrEven=ptrEven->next;
        }
        ptrOdd->next=successor;//衔接两段链表
        return head;
    }
};
```

### [字节跳动高频题——排序奇升偶降链表](https://mp.weixin.qq.com/s/377FfqvpY8NwMInhpoDgsw)

题目：链表，奇数位置按序增长，偶数位置按序递减，如何能实现链表从小到大排序

```
输入: 1->8->3->6->5->4->7->2->NULL
输出: 1->2->3->4->5->6->7->8->NULL
```

解法：

1. 按奇偶位置拆分链表，得1->3->5->7->NULL和8->6->4->2->NULL。（328. 奇偶链表）

2. 反转偶链表，得1->3->5->7->NULL和2->4->6->8->NULL。（经典的反转整条链表的模板）

3. 合并两个有序链表，得1->2->3->4->5->6->7->8->NULL。（剑指 Offer 25. 合并两个排序的链表）

```c++
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        ListNode* head1=nullptr,*head2=nullptr;
        partition(head,head1,head2);//1.拆分+2.反转
        ListNode* res=merge(head1,head2);//3.合并两个有序链表
        return res;
    }
    
    void partition(ListNode* head,ListNode*& head1,ListNode*& head2){//1.按奇偶位置拆分链表
        if(head==nullptr) return;
        ListNode* ptrOdd=head;
        ListNode* ptrEven=head->next;
        ListNode *successor=head->next;
        while(ptrEven!=nullptr && ptrEven->next!=nullptr){
            ptrOdd->next=ptrEven->next;
            ptrOdd=ptrOdd->next;

            ptrEven->next=ptrOdd->next;
            ptrEven=ptrEven->next;
        }
        ptrOdd->next=nullptr;//ptrOdd可能最后没指向nullptr
        head2=reverse(successor);//2.反转链表
        head1=head;
    }

    ListNode *reverse(ListNode* a){//2.经典的反转整体链表的模板
        ListNode *pre,*cur,*nxt;
        pre=nullptr;cur=a;nxt=nullptr;
        while(cur!=nullptr){
            nxt=cur->next;
            cur->next=pre;
            pre=cur;
            cur=nxt;
        }
        return pre;
    }

    ListNode* merge(ListNode* l1,ListNode* l2){//3.合并两条有序链表
        ListNode *dummy=new ListNode(0);
        ListNode *pre=dummy;
        while(l1!=nullptr && l2!=nullptr){
            if(l1->val<=l2->val){
                pre->next=l1;
                l1=l1->next;
            }
            else{
                pre->next=l2;
                l2=l2->next;
            }
            pre=pre->next;
        }
        if(l1==nullptr){//退出时是谁跑完了
            pre->next=l2;
        }
        else{
            pre->next=l1;
        }
        ListNode *res=dummy->next;
        delete dummy;
        return res;
    }
    
};
```

### [61. 旋转链表（medium）](https://leetcode-cn.com/problems/rotate-list/)

解法：双指针，两个链表合并为一个

```c++
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(head==nullptr || k==0) return head;
        ListNode *cur=head;
        int len=0;
        ListNode *tail=nullptr;//记录链表尾
        
        while(cur!=nullptr){//遍历记录表长len
            if(cur->next==nullptr)
                tail=cur;//记录链表尾
            cur=cur->next;
            len++;  
        }
        
        int moveSteps=0;
        if(k%len==0){//写出来找一下规律就会了
            moveSteps=0;
        }
        else{
            moveSteps=len-k%len;
        }
        
        if(moveSteps==0)//如果不用移动
            return head;
        
        cur=head;
        for(int i=0;i<moveSteps;i++){//cur停在新首部处
            if(i+1==moveSteps){//断开与新首部的链接
                ListNode* nxt=cur->next;
                cur->next=nullptr;
                cur=nxt;
            }
            else
                cur=cur->next;
        }
        tail->next=head;//衔接两段链表
        ListNode *res=cur;
        return res;
    }
};
```

### [86. 分隔链表（medium）](https://leetcode-cn.com/problems/partition-list/)

解法：双指针，两个链表合并为一个

```c++
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        if(head==nullptr) return head;
        
        ListNode *cur=head;
        ListNode *dummyLess=new ListNode(0,nullptr);//less链表的dummy节点
        ListNode *curLess=dummyLess;
        ListNode *dummyGreater=new ListNode(0,nullptr);//Greater链表的dummy节点
        ListNode *curGreater=dummyGreater;
        while(cur!=nullptr){
            if(cur->val<x){
                curLess->next=cur;
                curLess=curLess->next;
            }
            else{
                curGreater->next=cur;
                curGreater=curGreater->next;
            }
            cur=cur->next;
        }
        curLess->next=dummyGreater->next;//衔接两段链表
        curGreater->next=nullptr;//greater链表最后指向nullptr
        
        ListNode* res=dummyLess->next;//首部
        delete dummyLess;
        delete dummyGreater;
        return res;
    }
};
```

## 2.树+二叉搜索树

### [144. 二叉树的前序遍历（medium）](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

- 递归

  - ```c++
    class Solution {
    public:
        vector<int> preorderTraversal(TreeNode* root) {
            traverse(root);
            return res;
        }
        vector<int> res;
        void traverse(TreeNode* root){
            if(root==nullptr)
                return;
            res.push_back(root->val);
            traverse(root->left);
            traverse(root->right);
        }
    };
    ```

### [94. 二叉树的中序遍历（medium）](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

- 递归

  - ```c++
    class Solution {
    public:
        vector<int> inorderTraversal(TreeNode* root) {
            traverse(root);
            return res;
        }
        vector<int> res;
        void traverse(TreeNode* root){
            if(root==nullptr)
                return;
            
            traverse(root->left);
            res.push_back(root->val);
            traverse(root->right);
        }
    };
    ```

### [236. 二叉树的最近公共祖先（medium）](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

- 递归：

  - ```c++
    class Solution {
    public:
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
    };
    ```

### [222. 完全二叉树的节点个数（medium）](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

- 递归

  - ```c++
    class Solution {
    public:
        int countNodes(TreeNode* root) {
            return cntNodes(root);
        }
        
        int cntNodes(TreeNode* root){
            if(root==nullptr)// base case
                return 0;
            int left=cntNodes(root->left);//左子树上的节点数
            int right=cntNodes(root->right);//右子树上的节点数
            return left+right+1;//1是指本节点还要占一个节点
        }
    };
    ```

### [226. 翻转二叉树（easy）](https://leetcode-cn.com/problems/invert-binary-tree/)

- 递归

  - 前序或者后序遍历都可以。但是中序不行。另外，后序遍历更加容易理解。

  - ```c++
    class Solution {
    public:
        TreeNode* invertTree(TreeNode* root) {
            reverse(root);
            return root;//root不会翻转。
        }
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
    };
    ```

- 另一种写法

  - ```c++
    class Solution {
    public:
        TreeNode* invertTree(TreeNode* root) {
            return invert(root);
        }
    
        TreeNode* invert(TreeNode* root){
            if(root==nullptr)
                return nullptr;
    
            TreeNode* tmp=root->left;
            root->left=invert(root->right);
            root->right=invert(tmp);
            
            return root;
        }
    };
    ```

    

### [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)

- 递归（我觉得不如迭代方便）

  - 备注：该写法都没有把每行的最后一个元素指向nullptr，但是默认值就是nullptr，所以也无妨大碍

  - ```c++
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
            connectTwoNode(node1->left,node1->right);// 连接相同父节点的两个子节点
            connectTwoNode(node1->right,node2->left);// 连接跨越父节点的两个子节点
            connectTwoNode(node2->left,node2->right);// 连接相同父节点的两个子节点
        }
    };
    ```

- 迭代（个人推荐）

  - 就是个层序遍历

  - ```c++
    class Solution {
    public:
        Node* connect(Node* root) {
            if(root==nullptr) return nullptr;
            queue<Node*> q;
            q.push(root);
            while(!q.empty()){//经典层序遍历问题
                int len=q.size();   
                for(int i=0;i<len;i++){
                    Node* cur=q.front();
                    q.pop();
                    if(i+1==len){//已经是本层的最后一个值了
                        cur->next==nullptr;
                    }
                    else{
                        cur->next=q.front();//因为已经pop过了，现在的front就是下一个节点
                    }
                    if(cur->left!=nullptr) q.push(cur->left);
                    if(cur->right!=nullptr) q.push(cur->right);
                }
            }
            return root;
        }
    };
    ```

### [114. 二叉树展开为链表（medium）](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

- 递归

  - ![image-20210430110108562](http://pichost.yangyadong.site/img/image-20210430110108562.png)

  - ```c++
    class Solution {
    public:
        void flatten(TreeNode* root) {
            flat(root);
        }
    
        void flat(TreeNode* root){
            if(root==nullptr) //base case
                return;
            flat(root->left);//左右子树递归
            flat(root->right);
            
            //后序遍历位置
            // 1、左右子树已经被拉平成一条链表
            TreeNode* leftnode=root->left;
            TreeNode* rightnode=root->right;
            if(leftnode==nullptr)//如果左边为空，说明不用合并。已经做好了 
                return;
            //2、将右链表接到左链表尾
            while(leftnode->right!=nullptr){//找到左子树的最下端
                leftnode=leftnode->right;
            }
            leftnode->right=rightnode;//将右链表接到左链表尾。
            //3、左右子树颠倒
            root->right=root->left;//右链表置为左链表
            root->left=nullptr;//左链表置空
        }
    };
    ```

### [654. 最大二叉树（medium）](https://leetcode-cn.com/problems/maximum-binary-tree/)

- 递归

  - 区间习惯，左闭右开

  - ```c++
    class Solution {
    public:
        TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
            return build(nums,0,nums.size());
        }
    
        TreeNode* build(vector<int>& nums,int leftIndex,int rightIndex){//左闭右开
            if(leftIndex>=rightIndex)
                return nullptr;
            int maxValueIndex=leftIndex;
            for(int i=leftIndex;i<rightIndex;i++){//寻找最大值的下标
                if(nums[i]>=nums[maxValueIndex])
                    maxValueIndex=i;
            }
            //前序遍历框架
            TreeNode* node=new TreeNode(nums[maxValueIndex]);//创建节点
            node->left=build(nums,leftIndex,maxValueIndex);
            node->right=build(nums,maxValueIndex+1,rightIndex);
            return node;
        }
    };
    ```

  - 备注：前序后序的，没啥区别啊

    - ```c++
      class Solution {
      public:
          TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
              return build(nums,0,nums.size());
          }
      
          TreeNode* build(vector<int>& nums,int leftIndex,int rightIndex){
              if(leftIndex>=rightIndex)
                  return nullptr;
              int maxValueIndex=leftIndex;
              for(int i=leftIndex;i<rightIndex;i++){
                  if(nums[i]>=nums[maxValueIndex])
                      maxValueIndex=i;
              }
      		//后序遍历框架也对，无所谓。就是谁先建立节点的问题
              TreeNode* tmp1=build(nums,leftIndex,maxValueIndex);
              TreeNode* tmp2=build(nums,maxValueIndex+1,rightIndex);
              
              TreeNode* node=new TreeNode(nums[maxValueIndex]);
              node->left=tmp1;
              node->right=tmp2;
              return node;
          }
      };
      ```

### [105. 从前序与中序遍历序列构造二叉树（medium）](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

- 递归

  - 一般我的区间习惯都是左闭右开。这个题比较麻烦，好几个区间，写成左闭右闭了。

  - 这种题里，需要两个序列，是因为空指针没有保存在序列中。如果前序遍历也保存了空指针，那么单靠一个前序遍历其实就能恢复树。参看[297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

  - ```c++
    class Solution {
    public:
        TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
            return build(preorder,0,preorder.size()-1,inorder,0,inorder.size()-1);
        }
    
        TreeNode* build(vector<int>& preorder, int preStart, int preEnd, vector<int>& inorder,int inStart, int inEnd){
            //base case
            if(preStart>preEnd ||inStart>inEnd)// 其实这俩条件不符合时，同时都会不符合。符合时会都符合，写一个就行
                return nullptr;
    
            int rootVal=preorder[preStart];// root 节点对应的值就是前序遍历数组的第一个元素
            int index=0;
            for(int i=inStart;i<=inEnd;i++){// 寻找rootVal 在中序遍历数组中的索引
                if(inorder[i]==rootVal)
                {
                    index=i;
                    break;
                }
            }
    
            int leftSize=index-inStart;//用于更新preOrder里的下标
            TreeNode* root = new TreeNode(rootVal);//创建节点
            root->left=build(preorder,preStart+1,preStart+leftSize,inorder,inStart,index-1);//inorder里的下标更新很好写。preorder区间更新画一画图就出来了
            root->right=build(preorder,preStart+leftSize+1,preEnd,inorder,index+1,inEnd);
            return root;
        }
    };
    ```

### [106. 从中序与后序遍历序列构造二叉树（Medium）](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

- 递归

  - 跟上一题可以说是几乎一样。只是后序是从后往前找

  - 一般我的区间习惯都是左闭右开。这个题比较麻烦，好几个区间，写成左闭右闭了。

  - ```c++
    class Solution {
    public:
        TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
            return build(inorder,0,inorder.size()-1,postorder,0,postorder.size()-1);
        }
    
        TreeNode* build(vector<int>& inorder,int inStart,int inEnd,vector<int>& postorder,int postStart,int postEnd){
            //base case
            if(inStart>inEnd || postStart>postEnd)
                return nullptr;
            int rootVal=postorder[postEnd];// root 节点对应的值就是后序遍历数组的最后一个元素
            int index=0;
            for(int i=inStart;i<=inEnd;i++){// 寻找rootVal 在中序遍历数组中的索引
                if(inorder[i]==rootVal){
                    index=i;
                    break;
                }
            }
            int leftSize=index-inStart;//用于更新postOrder里的下标
            TreeNode* root=new TreeNode(rootVal);//创建节点
            root->left=build(inorder,inStart,index-1,postorder,postStart,postStart+leftSize-1);//inorder里的下标更新很好写。postorder区间更新画一画图就出来了
            root->right=build(inorder,index+1,inEnd,postorder,postStart+leftSize,postEnd-1);
            return root;
        }
    };
    ```

### [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

- PS：一般语境下，单单前序遍历结果是不能还原二叉树结构的，因为缺少空指针的信息，至少要得到前、中、后序遍历中的两种才能还原二叉树。但是这里的 `node`列表包含空指针的信息，所以只使用 `node` 列表就可以还原二叉树。

- 递归，前序

  - 不知道为什么，49用例超时

  - ```c++
    class Codec {
    public:
    
        // Encodes a tree to a single string.
        string serialize(TreeNode* root) {
            serial(root);
            return res;
        }
        string res;
        void serial(TreeNode* root){//辅助函数，递归，前序遍历
            if(root==nullptr){
                res=res+"#,";
                return;
            }
            res=res+ to_string(root->val)+",";
            serial(root->left);
            serial(root->right);
        }
        // queue<string> split(const string& str){
        //     queue<string> res;
        //     if(str.empty()) 
        //         return res;
        //     int pre=0;
        //     for(int i=0;i<=str.size();i++){
        //         if(str[i]==','||i==str.size()){
        //             res.push(str.substr(pre,i-pre));
        //             pre=i+1;
        //         }
        //     }
        //     return res;
        // }
        queue<string> split(const string& str){//分割字符串的函数
            istringstream iss(str);
            queue<string> res;
            string buf;
            while(getline(iss, buf,',')){
                res.push(buf);
            }
        return res;
    }
        // Decodes your encoded data to tree.
        TreeNode* deserialize(string data) {
            queue<string> myqueue=split(data);
            return deserial(myqueue);
        }
        TreeNode* deserial(queue<string>& myqueue){//辅助函数。递归，前序遍历
            if(myqueue.empty())
                return nullptr;
            string cur=myqueue.front();
            myqueue.pop();
            if(cur=="#")
                return nullptr;
            TreeNode* root=new TreeNode(stoi(cur));
            root->left=deserial(myqueue);
            root->right=deserial(myqueue);
            return root;
        }
    };
    ```

- 递归，后序

  - 序列化生成字符串时，是后序

  - 但是反序列化时，要类似前序的生成。

  - ```c++
    class Codec {
    public:
    
        // Encodes a tree to a single string.
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
    
        deque<string> split(const string& str){
            istringstream iss(str);
            deque<string> res;
            string buf;
            while(getline(iss, buf,',')){
                res.push_back(buf);
            }
        return res;
    	}
        // Decodes your encoded data to tree.
        TreeNode* deserialize(string data) {
            deque<string> myqueue=split(data);
            return deserial(myqueue);
        }
        TreeNode* deserial(deque<string>& myqueue){
            if(myqueue.empty())
                return nullptr;
            string cur=myqueue.back();
            myqueue.pop_back();
            if(cur=="#")
                return nullptr;
            TreeNode* root=new TreeNode(stoi(cur));
            
            root->right=deserial(myqueue);
            root->left=deserial(myqueue);
            return root;
        }
    };
    ```

    

### [652. 寻找重复的子树](https://leetcode-cn.com/problems/find-duplicate-subtrees/)

- 递归

  - 使用序列化字符串来表示一颗子树

  - ```c++
    class Solution {
    public:
        vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
            traverse(root);
            return res;
        }
        unordered_map<string,int> myMap;
        vector<TreeNode*> res;
        string traverse(TreeNode* root){
            if(root==nullptr){
                return "#";
            }
            string left=traverse(root->left);
            string right=traverse(root->right);
            string cur=left+","+right+","+to_string(root->val)+",";//后序遍历
            if((++myMap[cur])==2){//第二次出现时记录下来，第一次，第三次，第四次都不记录
                res.push_back(root);
            }
    
            return cur;
        }
    };
    ```

### [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

- 递归

  - 方法1：使用返回值来递归
  
  - ```c++
    class Solution {
    public:
        int maxDepth(TreeNode* root) {
            return traverse(root);
        }
    
        int traverse(TreeNode* root){
            if(root==nullptr)
                return 0;
            int left=traverse(root->left);
            int right=traverse(root->right);
            int maxdep=1+(left>right?left:right);//后序
            return maxdep;
        }
    };
    ```
  
- 方法2：

  - 使用一个变量来维系

  - 在平衡二叉树中有奇效

  - ```c++
    //算树高--2.使用一个变量来维系
    class Solution {
    public:
        int maxDepth(TreeNode* root) {
            int depth;
            traverse(root,depth);
            return depth;
        }
        void traverse(TreeNode* root,int& depth){
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

    

### [559. N 叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)

- 递归

  - ```c++
    class Solution {
    public:
        int maxDepth(Node* root) {
            return traverse(root);
        }
    
        int traverse(Node* root){
            if(root==nullptr)
                return 0;
            int maxdep=0;
            for(auto i:root->children){//遍历每个子节点，记录最大的那个
                int dep=traverse(i);
                if(dep>maxdep)
                    maxdep=dep;
            }
            return maxdep+1;
        }
    };
    ```

### [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

- 递归

  - 跟最大的不太一样，因为必须是到叶子节点，参看这个例子

  - ```
    输入：root = [2,null,3,null,4,null,5,null,6]
    输出：5
    ```

  - ```c++
    class Solution {
    public:
        int minDepth(TreeNode* root) {
            return traverse(root);
        }
    
        int traverse(TreeNode* root){
            if(root==nullptr)
                return 0;
            int left=traverse(root->left);
            int right=traverse(root->right);
            int mindep=-1;
            if(left!=0 && right!=0){//都不等于0时，才能取较小的
                mindep=left<right?left:right;
            }
            else//一旦有等于0的，就取那个非0的。这其实包含了两种情况。叶子节点（left==0,right==0）；中途节点（一个节点为0，另一个节点非0）
                mindep=left==0?right:left;
            return mindep+1;
        }
    };
    ```

### [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

- 递归

  - 要求使用优于O(N)的遍历算法

  - O(logN*logN)
  
  - ```c++
    class Solution {
    public:
        int countNodes(TreeNode* root) {
            return count(root);
        }
    	//结合了普通二叉树和满二叉树的算法
        int count(TreeNode* root){
            if(root==nullptr)
                return 0;
            
            TreeNode* l=root;
            TreeNode* r=root;
            int leftDep=0,rightDep=0;
            while(l!=nullptr){
                l=l->left;
                leftDep++;
            }
            while(r!=nullptr){
                r=r->right;
                rightDep++;
            }
            if(leftDep==rightDep)//利用满二叉树的特性
                return (int)pow(2,leftDep)-1;
            
            return count(root->left)+count(root->right)+1;//普通二叉树的递归法
        }
    };
    ```

### [341. 扁平化嵌套列表迭代器](https://leetcode-cn.com/problems/flatten-nested-list-iterator/)

- 解法1：

  - 递归，当做N叉树的前序遍历来做

  - 遍历一遍存起来

  - ```c++
    class NestedIterator {
    public:
        NestedIterator(vector<NestedInteger> &nestedList) {
            for(auto i:nestedList){//因为没有root结点，只能手动第一轮循环
                traverse(i);
            }
        }
        
        void traverse(NestedInteger& root){//经典的N叉树遍历,将叶子节点的值加入 data 队列
            if(root.isInteger()){
                data.push_back(root.getInteger());
                return;
            }
            for(auto i:root.getList()){
                traverse(i);
            }
        }
        deque<int> data;
        
        int next() {
            int res=data.front();
            data.pop_front();
            return res;  
        }
        
        bool hasNext() {
            return !data.empty();
        }
    };
    ```

  - 不太符合迭代器的使用意义

- 解法2：

  - 使用stl迭代器。

  - 没做，找了个题解。有空再看吧。

  - （这题面试从来没考过，我觉得会解法1的递归遍历就可以了）

  - ```c++
    class NestedIterator {
    public:
        using Itor = vector<NestedInteger>::const_iterator;
        stack<pair<Itor, Itor>> stk;
    
        NestedIterator(vector<NestedInteger> &nestedList) {
            stk.push({nestedList.cbegin(),nestedList.cend()});
        }
        
        int next() {
            return (stk.top().first++)->getInteger();
        }
        
        bool hasNext() {
            while (!stk.empty())
    		{
    			auto& itor = stk.top().first;
    			if (itor == stk.top().second)
    			{
    				stk.pop();
    
    				if (stk.empty())
    					return false;
    
    				stk.top().first++;
    			}
    			else
    			{
    				if (itor->isInteger())
    					return true;
    				else
    				{
    					auto& new_list = stk.top().first->getList();
    					if (new_list.empty())
    					{
    						stk.top().first++;
    					}
    					else
    					{
    						stk.push({ new_list.cbegin(),new_list.cend() });
    					}
    				}
    			}
    
    		}
    		return false;
        }
    };
    ```

### [剑指 Offer 8-二叉树的下一个节点（分情况讨论，非递归）](https://www.nowcoder.com/questionTerminal/9023a0c988684a53960365b889ceaf5e)

- 迭代

  - ```c++
    class Solution {
    public:
        TreeLinkNode* GetNext(TreeLinkNode* pNode) {
            if(pNode==nullptr) return nullptr;
            //1.该节点有右子树。沿着右子树一直向左
            if(pNode->right!=nullptr){
                TreeLinkNode* res=pNode->right;
                while(res->left!=nullptr)
                    res=res->left;
                return res;
            }
            //该节点没有右子树。它还没有父节点。即是root节点，那么就到此为止了。
            if(pNode->next==nullptr)
                return nullptr;
            //2.该节点没有右子树。并且它是父节点的左节点
            if(pNode==pNode->next->left){
                return pNode->next;
            }
            //3.该节点没有右子树。并且它是父节点的右节点
            TreeLinkNode* tmp=pNode;
            while(tmp->next!=nullptr && tmp!=tmp->next->left){
                tmp=tmp->next;
            }
            return tmp->next;
        }
    };
    ```

### [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

- 递归

  - 两个递归函数，一个是递归遍历，一个是递归比较

  - ```c++
    class Solution {
    public:
        bool isSubStructure(TreeNode* A, TreeNode* B) {
            if(B==nullptr)
                return false;
            return trverse(A,B);
        }
        bool trverse(TreeNode* A,TreeNode* B){
            if(A==nullptr)
                return false;
            bool res=treeEqual(A,B);
            if(!res)//如果当前节点不相同，那就递归遍历左右子树
                res=trverse(A->left,B) || trverse(A->right,B);
            return res;
        }
    
        bool treeEqual(TreeNode* A,TreeNode* B){//比较当前这两棵树是不是相同（不是完全意义上的相同，是包含）
            if(B==nullptr)
                return true;
            if(A==nullptr)
                return false;
            if(A->val==B->val){
                return treeEqual(A->left,B->left) && treeEqual(A->right,B->right);
            }
            return false;
        }
    };
    ```

### [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

- 递归

  - 最经典的模板

  - ```c++
    class Solution {
    public:
        TreeNode* mirrorTree(TreeNode* root) {
            return mirror(root);
        }
        
        TreeNode* mirror(TreeNode* root){
            if(root==nullptr)
                return nullptr;
            TreeNode* left=mirror(root->left);
            TreeNode* right=mirror(root->right);
            root->right=left;
            root->left=right;
            return root;
        }
    };
    ```

### [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

- 递归

  - 经典的双节点递归。首先你要判断出来，这个题用一个节点能不能判断出对称。很显然不能，需要双节点

  - ```c++
    class Solution {
    public:
        bool isSymmetric(TreeNode* root) {
            if(root==nullptr)
                return true;
            return isSym(root->left,root->right);
        }
        bool isSym(TreeNode* node1,TreeNode* node2){
            if(node1==nullptr && node2==nullptr)//两个节点都为空
                return true;
            if(node1==nullptr || node2==nullptr)//有一个为空
                return false;
            //两个节点都不为空时
            if(node1->val!=node2->val)//前序遍历
                return false;
            //说明node1->val==node2->val
            bool tmp1=isSym(node1->left,node2->right);//node1的左和node2的右
            bool tmp2=isSym(node1->right,node2->left);//node1的右和node2的左
            return tmp1&&tmp2;
        }
    };
    ```

### [剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

- 解法1：完全按照题目意思

  - 遍历每个节点，每个节点计算左右树高。一个递归是遍历，另一个递归是计算高度

  - 存在重复遍历的问题

  - ```c++
    class Solution {
    public:
        bool isBalanced(TreeNode* root) {
            return traverse(root);
        }
        
        bool traverse(TreeNode* root){//遍历
            if(root==nullptr)
                return true;
            int leftDep=calDep(root->left);
            int rightDep=calDep(root->right);
            if(abs(leftDep-rightDep)>1)//前序遍历！底下这个traverse才是真递归
                return false;
            return traverse(root->left) && traverse(root->right);
        }
        
        int calDep(TreeNode* root){//计算树高
            if(root==nullptr)
                return 0;
            int left=calDep(root->left);
            int right=calDep(root->right);
            return left>right?left+1:right+1;
        }
    };
    ```

- 解法2：

  - 只遍历一遍，将两个递归合二为一。即算了树高，又进行了后序遍历

  - **以我的刷题经验，我们要尽可能避免递归函数中调用其他递归函数**，如果出现这种情况，大概率是代码实现有瑕疵，可以进行类似本文的优化来避免递归套递归。
  
  - **把前序遍历变成后序遍历，让 `traverse` 函数把辅助函数做的事情顺便做掉**。
  
  - ```c++
    class Solution {
    public:
        bool isBalanced(TreeNode* root) {
            int depth;
            return isBalan(root,depth);
        }
        bool isBalan(TreeNode* root,int& depth){
            if(root==nullptr){
                depth=0;
                return true;
            }
            int leftDep,rightDep;//记录树高
            if(isBalan(root->left,leftDep) && isBalan(root->right,rightDep)){//确保左右子树都是平衡二叉树时，才有可能返回true
                if(abs(leftDep-rightDep)<=1){//左右树高差符合平衡二叉树，返回true。不符合就返回false
                    depth=leftDep>rightDep?leftDep+1:rightDep+1;//后序遍历！
                    return true;
                }
            }
            return false;//左右子树已经有不是平衡二叉树的了 或者 当前节点的左右树高差不符合平衡二叉树（即从当前节点开始不是平衡二叉树了）。
        }
    };
    ```
  
  - 剖析：
  
    - ```c++
      //这其实就是后序版本的算树高的扩展。使用一个变量来维系，而不是用返回值。即底下的第二种解法
      
      //算树高--1.使用返回值计算
      int maxDep(TreeNode* root){
          if(root==nullptr)
              return 0;
          int left=maxDep(root->left);
          int right=maxDep(root->right);
          return 1+(left>right?left:right);//后序遍历
      }
      
      //算树高--2.使用一个变量来维系
      class Solution {
      public:
          int maxDepth(TreeNode* root) {
              int depth;
              traverse(root,depth);
              return depth;
          }
          void traverse(TreeNode* root,int& depth){
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

### [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

- 递归

  - ```c++
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
    ```

    

### [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

- 遍历	

  - ```c++
    class Solution {
    public:
        vector<vector<int>> pathSum(TreeNode* root, int target) {
            curSum=0;
            this->target=target;
            traverse(root);
            return res;
        }
    
        vector<vector<int>> res;
        vector<int> curVec;
        int curSum;
        int target;
    
        void traverse(TreeNode* root){//经典前序遍历的基础上改的
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
            traverse(root->left);//左
            curSum-=curVec.back();
            curVec.pop_back();
    
            traverse(root->right);//右
            curSum-=curVec.back();
            curVec.pop_back();
            
        }
    };
    ```

- 遍历2

  - ```c++
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
    ```
    
  - 其实这种写法相当于
  
    - 其实是回溯法
    
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
  
- 遍历3

  - 其实每次复制一个curVec很耗费内存。不如解法2只维护一个curVec。

  - ```c++
    class Solution {
    public:
        vector<vector<int>> pathSum(TreeNode* root, int target) {
            vector<int> curVec;
            traverse(root,0,target,curVec);
            return res;
        }
        vector<vector<int>> res;
        void traverse(TreeNode* root,int curSum,int target,vector<int> curVec){
            if(root==nullptr){
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
            traverse(root->right,curSum,target,curVec);//右
        }
    };
    ```

    

### [129. 求根节点到叶节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

- 递归

  - ```c++
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



### [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

- 解法1：

  - 递归中序全树遍历，O(N)

  - ```c++
    class Solution {
    public:
        int kthSmallest(TreeNode* root, int k) {
            rank=0;
            traverse(root,k);
            return res;
        }
        int rank;//用来记录这是第几个元素
        int res;//用来记录返回值
        void traverse(TreeNode* root,int k){//中序遍历
            if(root==nullptr)
                return;
            traverse(root->left,k);
            rank++;
            if(rank==k){
                res=root->val;
                return;//不加return也行，那效率更低
            }
            traverse(root->right,k);
        }
    };
    ```

- 解法2：

  - 要知道 BST 性质是非常牛逼的，像红黑树这种改良的自平衡 BST，增删查改都是`O(logN)`的复杂度，让你算一个第`k`小元素，时间复杂度竟然要`O(N)`，有点低效了。
  - 不过这需要**每个节点需要记录，以自己为根的这棵二叉树有多少个节点**。即树中有对应的字段
  - 有了`size`字段，外加 BST 节点左小右大的性质，对于每个节点`node`就可以通过`node.left`推导出`node`的排名，从而做到我们刚才说到的对数级算法。
    - 比如一个节点size=6，左子树size=2，右子树size=3。该节点排第3。
  - 当然，`size`字段需要在增删元素的时候需要被正确维护，力扣提供的`TreeNode`是没有`size`这个字段的，所以我们这道题就只能利用 BST 中序遍历的特性实现了，但是我们上面说到的优化思路是 BST 的常见操作，还是有必要理解的。

### [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

- 降序递归

  - ```c++
    class Solution {
    public:
        TreeNode* convertBST(TreeNode* root) {
            sum=0;
            traverse(root);
            return root;
        }
        int sum;
        void traverse(TreeNode* root){
            if(root==nullptr)
                return;
            traverse(root->right);//先右
            sum+=root->val;//中序
            root->val=sum;
            traverse(root->left);//后左
        }
    };
    ```


### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

- 前序递归遍历

  - 主要是想清楚，怎样才算一个二叉搜索树

  - 不仅仅是`左节点<当前节点<右节点`。而是`左子树<当前节点<右子树`

  - 具体到每个节点就是：

    - 当前节点root的左节点的最小值下界是之前记录的min（可能是nullptr或者是root的父节点），最大值上界是当前节点root。
    - 当前节点root的右节点的最小值下界是当前节点root，最大值上界是之前记录的max（可能是nullptr或者是root的父节点）

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
            //当前节点root的左子树最小值下界是之前记录的min（其实是root的父节点），最大值上界是当前节点root。
            //当前节点root的右子树最小值下界是当前节点root，最大值上界是之前记录的max（其实是root的父节点）。
        }
    };
    ```

### [700. 二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

- 解法1：递归遍历（普通二叉树的框架）

  - 跟是不是二叉搜索树没关系。没用它的性质

  - ```c++
    class Solution {
    public:
        TreeNode* searchBST(TreeNode* root, int val) {
            return traverse(root,val);
        }
    
        TreeNode* traverse(TreeNode* root,int val){
            if(root==nullptr)
                return nullptr;
            if(root->val==val)
                return root;
            TreeNode* left=traverse(root->left,val);
            TreeNode* right=traverse(root->right,val);
            return left==nullptr?right:left;
        }
    };
    ```

- 解法2：针对BST的遍历框架

  - ```c++
    class Solution {
    public:
        TreeNode* searchBST(TreeNode* root, int val) {
            return traverse(root,val);
        }
    
        TreeNode* traverse(TreeNode* root,int val){
            if(root==nullptr)
                return nullptr;
            if(root->val==val)
                return root;
            if(root->val<val)
                return traverse(root->right,val);
            if(root->val>val)
                return traverse(root->left,val);
            return nullptr;//这个纯粹是因为编译器报错
        }
    };
    ```

### [701. 二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

- 解法1：

  - 原汁原味的先遍历查找位置，再在主函数中插入

  - 不是特别优雅，但是正确，并且想法自然

  - ```c++
    class Solution {
    public:
        TreeNode* insertIntoBST(TreeNode* root, int val) {
            if(root==nullptr)
                return new TreeNode(val);
            traverse(root,val);
            
            if(res->val>val)
                res->left=new TreeNode(val);
            else if(res->val<val)
                res->right=new TreeNode(val);
            else;
            return root;
        }
        TreeNode* res;
        void traverse(TreeNode* root,int val){
            if(root==nullptr)
                return;
            res=root;
            if(root->val==val){
                return;
            }
            if(root->val>val)
                traverse(root->left,val);
            if(root->val<val)
                traverse(root->right,val);
        }
    };
    ```

- 解法2：更加优雅的递归（这种思想一定要掌握）

  - 递归过程中加入节点。

  - ```c++
    class Solution {
    public:
        TreeNode* insertIntoBST(TreeNode* root, int val) {
            return traverse(root,val);
        }
    
       TreeNode* traverse(TreeNode* root,int val){
            if(root==nullptr)
                return new TreeNode(val);
            if(root->val==val)
                return root;
            if(root->val>val)
                root->left=traverse(root->left,val);
            if(root->val<val)
                root->right=traverse(root->right,val);
            return root;
        }
    };
    ```

### [450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

- 递归，分情况讨论

  - ```c++
    class Solution {
    public:
        TreeNode* deleteNode(TreeNode* root, int key) {
            return traverse(root,key);
        }
    
        TreeNode* traverse(TreeNode* root,int key){
            if(root==nullptr) return nullptr;
            if(root->val==key){//找到节点了
                //删除节点
                //情况1：两个节点都为空
                if(root->left==nullptr && root->right==nullptr){
                    delete root;
                    return nullptr;
                }
                //情况2：仅一个节点为空
                if(root->left==nullptr || root->right==nullptr){
                    TreeNode* res=root->left==nullptr?root->right:root->left;
                    delete root;
                    return res;
                }
                //情况3：两个节点都不为空。通过改值的方法来删除节点
                if(root->left!=nullptr && root->right!=nullptr){
                    //使用右子树中最小的节点来接替自己
                    TreeNode* minNode=root->right;
                    while(minNode->left!=nullptr){
                        minNode=minNode->left;
                    }
                    //停下来时，minNode一定是右子树上的最小值
                    root->val=minNode->val;//将root的值改成minNode，就装作已经删除了root
                    root->right=traverse(root->right,minNode->val);//删去原本的minNode节点。minNode节点一定没有左子树，因此很好删除
                }
    
            }
               
            else if(root->val > key)//没找到节点
                root->left=traverse(root->left,key);
            else if(root->val < key)//没找到节点
                root->right=traverse(root->right,key);
            
            return root;
            
        }
    };
    ```

### [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

- 降序遍历

  - ```c++
    class Solution {
    public:
        int kthLargest(TreeNode* root, int k) {
            count=0;
            traverse(root,k);
            return res;
        }
    
        int count;
        int res;
        void traverse(TreeNode* root,int k){
            if(root==nullptr)
                return;
            traverse(root->right,k);//现在是降序第K。把right和left两句话一换位置就是升序的第K。
            count++;
            if(k==count){
                res=root->val;
                return;
            }         
            traverse(root->left,k);
        }
    };
    ```

### [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先（迭代）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

- 迭代，while

  - 当然用普通二叉树的最近公共祖先的递归解法也可以

  - ```c++
    class Solution {
    public:
        TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
            int maxVal=p->val>q->val?p->val:q->val;
            int minVal=p->val<q->val?p->val:q->val;
            while(1){
                if(root->val > maxVal)//比最大的都大
                    root=root->left;
                else if(root->val < minVal)//比最小的还小
                    root=root->right;
                else//当前节点处于[minVal,maxVal]之间
                    break;
            }
            return root;
        }
    };
    ```

### [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

- 解法1：

  - 层序遍历

  - 利用reverse函数

  - ```c++
    class Solution {
    public:
        vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
            vector<vector<int>> res;
            if(root==nullptr)
                return res;
            queue<TreeNode*> q;
            q.push(root);
            int flag=-1;
            while(!q.empty()){
                flag*=-1;
                int size=q.size();
                vector<int> vec;
                for(int i=0;i<size;i++){
                    TreeNode* root=q.front();
                    q.pop();
                    vec.push_back(root->val);
                    if(root->left!=nullptr) q.push(root->left);
                    if(root->right!=nullptr) q.push(root->right);
                }
                if(flag!=1){//如果是反序
                    reverse(vec.begin(),vec.end());
                }
                res.push_back(vec);
            }
            return res;
        }
    };
    ```

- 解法2：

  - 不使用reverse，在每一层push_back的时候就直接在对应索引处赋值

  - ```c++
    class Solution {
    public:
        vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
            vector<vector<int>> res;
            if(root==nullptr)
                return res;
            queue<TreeNode*> q;
            q.push(root);
            int flag=-1;
            while(!q.empty()){
                flag*=-1;
                int size=q.size();
                vector<int> vec(size,0);
                for(int i=0;i<size;i++){
                    TreeNode* root=q.front();
                    q.pop();
                    if(flag==1)//本层是正序
                        vec[i]=root->val;
                    else//本层是反序
                        vec[size-1-i]=root->val;
                    if(root->left!=nullptr) q.push(root->left);
                    if(root->right!=nullptr) q.push(root->right);
                }
                res.push_back(vec);
            }
            return res;
        }
    };
    ```

### [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

- 层序遍历

  - 从左往右遍历，记录每层最后一个节点

  - ```c++
    class Solution {
    public:
        vector<int> rightSideView(TreeNode* root) {
            vector<int> res;
            if(root==nullptr)
                return res;
            queue<TreeNode*> q;
            q.push(root);
            while(!q.empty()){
                int size=q.size();
                for(int i=0;i<size;i++){
                    TreeNode* cur=q.front();
                    q.pop();
                    if(i==size-1)
                        res.push_back(cur->val);
                    if(cur->left!=nullptr) q.push(cur->left);
                    if(cur->right!=nullptr) q.push(cur->right);
                }
            }
            return res;
        }
    };
    ```

- 层序遍历

  - 从右往左遍历，记录每次第一个节点

  - ```c++
    class Solution {
    public:
        vector<int> rightSideView(TreeNode* root) {
            vector<int> res;
            if(root==nullptr)
                return res;
            queue<TreeNode*> q;
            q.push(root);
            while(!q.empty()){
                int size=q.size();
                for(int i=0;i<size;i++){
                    TreeNode* cur=q.front();
                    q.pop();
                    if(i==0)
                        res.push_back(cur->val);     
                    if(cur->right!=nullptr) q.push(cur->right);
                    if(cur->left!=nullptr) q.push(cur->left);
                }
            }
            return res;
        }
    };
    ```

### [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

与剑指offer55-II. 平衡二叉树一样

- 写法1：

  - 最简洁明了

  - 肯定是完整的递归遍历，O(N)

  - ```c++
    class Solution {
    public:
        bool isBalanced(TreeNode* root) {
            int depth;
            return traverse(root,depth);
        }
    
        bool traverse(TreeNode* root,int& depth){
            if(root==nullptr){
                depth=0;
                return true;
            }
            int leftDep,rightDep;
            bool left=traverse(root->left,leftDep);
            bool right=traverse(root->right,rightDep);
            depth=1+(leftDep>rightDep?leftDep:rightDep);
            return left && right && abs(leftDep-rightDep)<=1;
        }
    };
    ```

- 写法2：

  - 可以不是完整的后序遍历。一旦检查出某个子树非平衡，就不往下遍历了。

  - ```c++
    class Solution {
    public:
        bool isBalanced(TreeNode* root) {
            int depth;
            return traverse(root,depth);
        }
    
        bool traverse(TreeNode* root,int& depth){
            if(root==nullptr){
                depth=0;
                return true;
            }
            int leftDep,rightDep;
            if(traverse(root->left,leftDep)&&traverse(root->right,rightDep)){
                depth=1+(leftDep>rightDep?leftDep:rightDep);
                if(abs(leftDep-rightDep)<=1)
                    return true;
            }
            //左子树或者右子树就不是平衡二叉树   ||  abs(leftDep-rightDep)即当前节点>1，当前节点不是平衡二叉树
            return false;
        }
    };
    ```

### [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

- 前序递归遍历

  - 与**剑指 Offer 34. 二叉树中和为某一值的路径**一样

  - ```c++
    class Solution {
    public:
        vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
            curSum=0;
            target=targetSum;
            traverse(root);
            return res;
        }
        vector<vector<int>> res;
        vector<int> vec;
        int curSum;
        int target;
        void traverse(TreeNode* root){
            if(root==nullptr){
                vec.push_back(0);
                return;
            }
            vec.push_back(root->val);//前序遍历
            curSum+=root->val;
            if(root->left==nullptr && root->right==nullptr && curSum==target)
                res.push_back(vec);
            
            traverse(root->left);
            curSum-=vec.back();
            vec.pop_back();
    
            traverse(root->right);
            curSum-=vec.back();
            vec.pop_back();
        }
    };
    ```

### [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

- 后序遍历	

  - 经典的求树深的框架改编一下就行

  - 遍历每一个节点，以每一个节点为中心点计算最长路径（左子树边长+右子树边长），更新全局变量max。

  - ```c++
    class Solution {
    public:
        int diameterOfBinaryTree(TreeNode* root) {
            res=0;
            traverse(root);
            return res-1;
        }
        int res;
        int traverse(TreeNode* root){
            if(root==nullptr)
                return 0;
            int left=traverse(root->left);
            int right=traverse(root->right);
            if(res<(left+right+1))
                res=left+right+1;
            return 1+(left>right?left:right);
        }
    };
    ```

- 另一个求树深的框架

  - ```c++
    class Solution {
    public:
        int diameterOfBinaryTree(TreeNode* root) {
            res=0;
            int depth=0;
            traverse(root,depth);
            return res-1;//最后求的是边数，减1
        }
        int res;
        void traverse(TreeNode* root,int& depth){//经典的求树深的框架
            if(root==nullptr){
                depth=0;
                return;
            }
    
            int leftDep=0;
            int rightDep=0;
            traverse(root->left,leftDep);
            traverse(root->right,rightDep);
            depth=1+(leftDep>rightDep?leftDep:rightDep);
            if(res<(leftDep+rightDep+1)){
                res=(leftDep+rightDep+1);
            }
        }
    };
    ```

### [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

- 递归

  - 用求树高的模板

  - ```c++
    class Solution {
    public:
        int maxPathSum(TreeNode* root) {
            maxVal=INT_MIN;
            traverse(root);
            return maxVal;
        }
        int maxVal;
        int traverse(TreeNode* root){
            if(root==nullptr)
                return 0;
            int left=traverse(root->left);//到左节点的最大和
            int right=traverse(root->right);
            int res=root->val+max(left,0)+max(right,0);//经过root节点时的最大路径和
            if(res>maxVal)
                maxVal=res;
            return root->val+max(max(left,right),0);//注意：返回的只能是节点+单边，而不能是节点+双边和
        }
    };
    ```


### [958. 二叉树的完全性检验](https://leetcode-cn.com/problems/check-completeness-of-a-binary-tree/)

- 层序遍历

  - nullptr也加入到queue中

  - 遇到第一个Nullptr时退出while循环，开始检测队列里之后的元素还有没有非nullptr的节点。发现任何一个非Nullptr都不是完全二叉树。

  - ```c++
    class Solution {
    public:
        bool isCompleteTree(TreeNode* root) {
            if(root==nullptr)
                return true;
            queue<TreeNode*> q;
            q.push(root);
            bool flag=true;
            while(!q.empty() && flag){
                int size=q.size();
                for(int i=0;i<size;i++){
                    TreeNode* cur=q.front();
                    q.pop();
                    if(cur==nullptr){
                        flag=false;//遇到第一个Nullptr时退出while循环
                        break;
                    }
                    q.push(cur->left);
                    q.push(cur->right);
                }
            }
    
            while(!q.empty()){//检查队列中之后的元素是不是都是Nullptr。如果都是，则是完全二叉树。只要有一个不是nullptr，那就不是完全儿茶素
                if(q.front()!=nullptr)
                    return false;
                q.pop();
            }
            return true;
        }
    };
    ```


### [662. 二叉树最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)

- 层序遍历

  - 原始的层序遍历超时。

  - queue里记录上treenode*和其pos下标。跟原始的层序遍历区别在于，原始的是遍历得到的下标，而现在是计算出来的

  - 队列中保存每个节点的指针以及pos值，左右孩子的pos分别是`2*pos`和`2*pos+1`

  - ```c++
    class Solution {
    public:
        int widthOfBinaryTree(TreeNode* root) {
            if(root==nullptr)
                return 0;
            queue<pair<TreeNode*,unsigned long long>> q;
            q.push({root,1});
            int res=-1;
            while(!q.empty()){
                int size=q.size();
                unsigned long long left=0,right=0;
                left=q.front().second;
                //其实right=q.back().second。直接写在这儿也行
                for(int i=0;i<size;i++){
                    TreeNode* cur=q.front().first;
                    unsigned long long curPos=q.front().second;
                    right=curPos;
                    q.pop();
                    if(cur->left!=nullptr) q.push({cur->left,curPos*2});
                    if(cur->right!=nullptr) q.push({cur->right,curPos*2+1});
                }
                unsigned long long curWidth=right-left+1;//左减右加1就是本层宽度。
                if(res<int(curWidth))
                    res=int(curWidth);
            }
            return res;
        }
    };
    ```

### [100. 相同的树](https://leetcode-cn.com/problems/same-tree/)

- 经典递归

  - ```c++
    class Solution {
    public:
        bool isSameTree(TreeNode* p, TreeNode* q) {
            return isSame(p,q);
        }
    
        bool isSame(TreeNode*p,TreeNode*q){
            if(p==nullptr && q==nullptr)
                return true;
            if(p==nullptr || q==nullptr)
                return false;
            
            return (p->val==q->val && isSame(p->left,q->left) && isSame(p->right,q->right));
        }
    };
    ```

### [572. 另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)

- 双递归函数！

  - 一个递归遍历，另一个递归完成判断

  - ```c++
    class Solution {
    public:
        bool isSubtree(TreeNode* root, TreeNode* subRoot) {
            return traverse(root,subRoot);
        }
    
        bool traverse(TreeNode* root,TreeNode* subRoot){//遍历root的每个节点
            if(root==nullptr)
                return false;
            if(isSame(root,subRoot))
                return true;
            return traverse(root->left,subRoot)||traverse(root->right,subRoot);
        }
    
        bool isSame(TreeNode*p,TreeNode*q){//判断两棵树是不是完全相同
            if(p==nullptr && q==nullptr)
                return true;
            if(p==nullptr || q==nullptr)
                return false;
            
            return (p->val==q->val && isSame(p->left,q->left) && isSame(p->right,q->right));
        }
    };
    ```

### [530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

- 解法1：递归遍历

  - O(N*logN)

  - 很显然这种做法写复杂了

  - ```c++
    class Solution {
    public:
        int getMinimumDifference(TreeNode* root) {
            res=INT_MAX;
            traverse(root);
            return res;
        }
        int res;
        void traverse(TreeNode* root){
            if(root==nullptr)
                return;
            TreeNode* left=root->left;//找寻左子树离root最近的点
            if(left!=nullptr){
                while(left->right!=nullptr)
                    left=left->right;
            }
      
            TreeNode* right=root->right;//找寻右子树离root最近的点
            if(right!=nullptr){
                while(right->left!=nullptr)
                    right=right->left;
            }
    		
            //计算与当前节点最小的abs。curMin
            if(left==nullptr && right==nullptr)
                return;
            int curMin=INT_MAX;
            if(left==nullptr || right==nullptr)
                curMin=abs(root->val - (left==nullptr?right:left)->val);
            else
                curMin=min(abs(root->val-left->val),abs(root->val-right->val));
            
            if(res>curMin)
                res=curMin;
            traverse(root->left);
            traverse(root->right);
        }
    
    
    };
    ```

- 解法2：

  - 中序遍历，比较相邻元素的查就完事儿了

  - 用一个pre记录上一个值，从而避免vector来存储节点值

  - ```c++
    class Solution {
    public:
        int getMinimumDifference(TreeNode* root) {
            res=INT_MAX;
            pre=INT_MAX;
            traverse(root);
            return res;
        }
        int res;
        int pre;
        void traverse(TreeNode* root){
            if(root==nullptr)
                return;
            traverse(root->left);
            if(res>abs(root->val-pre))
                res=abs(root->val-pre);
            pre=root->val;
            traverse(root->right);
        }
    };
    ```

### [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

- 升序递归

  - 解法1：

    - 头尾是后找的，其实可以在traverse途中记录下来头尾

    - ```c++
      class Solution {
      public:
          Node* treeToDoublyList(Node* root) {
              if(root==nullptr)
                  return nullptr;
              pre=nullptr;
              
              //先记录下来头和尾
              Node* head=root;
              while(head->left!=nullptr){//找头和尾
                  head=head->left;
              }
              Node* tail=root;
              while(tail->right!=nullptr){
                  tail=tail->right;
              }
              
              traverse(root);//traverse完，只有头尾没接
              head->left=tail;//头尾相接
              tail->right=head;
              return head;
          }
          Node* pre;
          void traverse(Node* root){//升序递归
              if(root==nullptr)
                  return;
              traverse(root->left);
              if(pre!=nullptr){//最左侧
                  pre->right=root;
                  root->left=pre;
              }
              pre=root;
              traverse(root->right);
          }
      };
      ```

  - 解法2：

    - traverse途中记录下来头尾

    - traverse结束时，pre就是尾。head可以使用`if(pre==nullptr)`来记录下来

    - ```c++
      class Solution {
      public:
          Node* treeToDoublyList(Node* root) {
              if(root==nullptr)
                  return nullptr;
              pre=nullptr;
              traverse(root);
              head->left=pre;//traverse结束时，pre就是尾
              pre->right=head;//head在traverse中记录了下来
              return head;
          }
          Node* pre;
          Node* head;
          void traverse(Node* root){
              if(root==nullptr)
                  return;
              traverse(root->left);
              if(pre!=nullptr){
                  pre->right=root;
                  root->left=pre;
              }
              else//pre==nullptr，说明是最左侧
                  head=root;
              pre=root;
              traverse(root->right);
          }
      };
      ```

### [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

- 经典递归

  - 有点像利用中序和后续遍历来重建二叉树的思路

  - ```c++
    class Solution {
    public:
        bool verifyPostorder(vector<int>& postorder) {
            return verify(postorder,0,postorder.size()-1);
        }
        bool verify(vector<int>& postorder,int le,int ri){
            if(le>ri)
                return true;
            int rootVal=postorder[ri];//区间上的最后一个值就是当前要验证的值
            int index;//记录左右子树的分界
            for(index=le;index<=ri-1;index++){
                if(rootVal<postorder[index])
                    break;
            }
    
            for(int i=index;i<=ri-1;i++){//index之后，如果有值比rootVal小，说明不是二叉搜索树，直接返回false
                if(rootVal>postorder[i])
                    return false;
            }
            //当前节点验证通过，继续验证左右子树
            return verify(postorder,le,index-1) && verify(postorder,index,ri-1);
        }
    };
    ```

### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

- 递归

  - 左右区间，生成树

  - ```c++
    class Solution {
    public:
        TreeNode* sortedArrayToBST(vector<int>& nums) {
            return traverse(nums,0,nums.size()-1);
        }
        TreeNode* traverse(vector<int>& nums,int le,int ri){
            if(le>ri)
                return nullptr;
            int index=(le+ri)/2;
            int rootVal=nums[index];
            TreeNode* cur=new TreeNode(rootVal);
            cur->left=traverse(nums,le,index-1);
            cur->right=traverse(nums,index+1,ri);
            return cur;
        }
    };
    ```

### [429. N 叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

- 层序遍历

  - ```c++
    class Solution {
    public:
        vector<vector<int>> levelOrder(Node* root) {
            vector<vector<int>> res;
            if(root==nullptr)
                return res;
            queue<Node*> q;
            q.push(root);
            while(!q.empty()){
                int size=q.size();
                vector<int> vec;
                for(int i=0;i<size;i++){
                    Node* cur=q.front();
                    q.pop();
                    vec.push_back(cur->val);
                    for(auto i:cur->children){
                        if(i!=nullptr)
                            q.push(i);
                    }
                }
                res.push_back(vec);
            }
            return res;
        }
    };
    ```

### [117. 填充每个节点的下一个右侧节点指针 II（非常不寻常的遍历方法，只针对本题）](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)

- 解法1：

  - 普通的层序遍历。时间O(N)、空间O(N)

  - ```c++
    class Solution {
    public:
        Node* connect(Node* root) {
            if(root==nullptr)
                return nullptr;
            queue<Node*> q;
            q.push(root);
            while(!q.empty()){
                int size=q.size();
                Node* pre=nullptr;
                for(int i=0;i<size;i++){
                    Node* cur=q.front();
                    q.pop();
                    if(pre!=nullptr)
                        pre->next=cur;
                    pre=cur;
                    if(cur->left!=nullptr) q.push(cur->left);
                    if(cur->right!=nullptr) q.push(cur->right);
                }
            }
            return root;
        }
    };
    ```

- 解法2：

  - 优化：层序遍历的时候，上一层的顺序“指示“下一层入队的顺序，那完全可以在”指示的过程“就修改指针，这样就可以不用入队，直接用一个额外指针变量切换到下一层。

  - 时间O(N)、空间O(1）

  - ```java
    class Solution {
    public:
        Node* connect(Node* root) {
            Node* cur=root;
            while(cur!=nullptr){
                Node* dummy=new Node(0);
                Node* head=dummy;
                while(cur!=nullptr){//本层开始遍历
                    //修改的是下一层的指向
                    if(cur->left!=nullptr){
                        head->next=cur->left;
                        head=head->next;
                    }
                    if(cur->right!=nullptr){
                        head->next=cur->right;
                        head=head->next;
                    }
                    cur=cur->next;
                }//本层遍历结束
                cur=dummy->next;//切换到下一层
                delete dummy;
            }
            return root;
        }
    };
    ```

### [107. 二叉树的层序遍历 II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

- 层序遍历+reverse

  - ```c++
    class Solution {
    public:
        vector<vector<int>> levelOrderBottom(TreeNode* root) {
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
            reverse(res.begin(),res.end());
            return res;
        }
    };
    ```

### [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

- 双递归

  - ```c++
    class Solution {
    public:
        int pathSum(TreeNode* root, int targetSum) {
            return traverse(root,targetSum);
        }
        
        int traverse(TreeNode* root,int targetSum){//一个递归是遍历
            //遍历，每个节点都调用一遍辅助函数pathNum。后续遍历，从下到上求和返回。
            if(root==nullptr)
                return 0;
            //后序遍历
            int left=traverse(root->left,targetSum);
            int right=traverse(root->right,targetSum);
            int res=pathNum(root,0,targetSum);
            return res+left+right;
        }
        int pathNum(TreeNode* root,int curSum,int targetSum){//另一个递归是完成任务
            //该函数的意义：返回以root节点开始的路径中，有多少个符合题意的
            if(root==nullptr)
                return 0;
            curSum+=root->val;
            int res=0;
            if(curSum==targetSum)
                res=1;
            return res+pathNum(root->left,curSum,targetSum)+pathNum(root->right,curSum,targetSum);
        }
    };
    ```

### [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

- 解法1：

  - 返回void类型的

  - 不太优雅，但是直观

  - ```c++
    class Solution {
    public:
        TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
            if(root1==nullptr)
                return root2;
            traverse(root1,root2);
            return root1;
        }
        void traverse(TreeNode* root1,TreeNode* root2){
            //root1就不能为空。root2可以为空
            if(root2==nullptr)
                return;
            root1->val+=root2->val;
            if(root1->left!=nullptr)
                traverse(root1->left,root2->left);
            else
                root1->left=root2->left;
    
            if(root1->right!=nullptr)
                traverse(root1->right,root2->right);
            else
                root1->right=root2->right;
        }
    };
    ```

- 解法2：

  - 返回TreeNode*型，更加优雅

  - 跟上边的思路没差别。

  - ```c++
    class Solution {
    public:
        TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
            return traverse(root1,root2);
        }
    
        TreeNode* traverse(TreeNode* root1,TreeNode* root2){
            if(root1==nullptr)
                return root2;
            if(root2==nullptr)
                return root1;
            root1->val+=root2->val;
            root1->left=traverse(root1->left,root2->left);
            root1->right=traverse(root1->right,root2->right);
            return root1;
        }
    };
    ```

### [173. 二叉搜索树迭代器](https://leetcode-cn.com/problems/binary-search-tree-iterator/)

- 递归遍历

  - 时间复杂度：初始化需要 O(n)的时间，其中 n 为树中节点的数量。随后每次调用只需要 O(1)的时间。

  - 空间复杂度：O(n)，因为需要保存中序遍历的全部结果。

  - ```c++
    class BSTIterator {
    public:
        BSTIterator(TreeNode* root) {
            traverse(root);
        }
        queue<int> q;
        void traverse(TreeNode* root){
            if(root==nullptr)
                return;
            traverse(root->left);
            q.push(root->val);
            traverse(root->right);
        }
        
        int next() {
            int res=q.front();
            q.pop();
            return res;
        }
        
        bool hasNext() {
            return !q.empty();
        }
    };
    ```

- 迭代遍历

  - 时间复杂度：显然，初始化和调用 hasNext() 都只需要 O(1) 的时间。每次调用 next() 函数最坏情况下需要 O(n)的时间；但考虑到 n 次调用next() 函数总共会遍历全部的 nn 个节点，因此总的时间复杂度为 O(n)，因此单次调用平均下来的均摊复杂度为 O(1)。

  - 空间复杂度：O(n)，其中 n 是二叉树的节点数量。空间复杂度取决于栈深度，而栈深度在二叉树为一条链的情况下会达到 O(n) 的级别。

  - ```c++
    class BSTIterator {
    public:
        BSTIterator(TreeNode* root) {
            T=root;
        }
    
        stack<TreeNode*> s;
        TreeNode* T;
        int next() {
            while(T)
            {
                s.push(T);
                T=T->left;
            }
            T=s.top();
            s.pop();
            int res=T->val;
            T=T->right;
            return res;
        }
        
        bool hasNext() {
            return T || !s.empty();
        }
    };
    ```


### [257. 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

- 前序遍历

  - ```c++
    class Solution {
    public:
        vector<string> binaryTreePaths(TreeNode* root) {
            if(root==nullptr)
                return res;
            traverse(root,"");
            return res;
        }
        vector<string> res;
        void traverse(TreeNode* root,string curStr){
            if(root==nullptr)
                return;
            curStr+=to_string(root->val)+"->";
            if(root->left==nullptr && root->right==nullptr){
                curStr.pop_back();
                curStr.pop_back();
                res.push_back(curStr);
                return;
            }
            traverse(root->left,curStr);
            traverse(root->right,curStr);
        }
    };
    ```

### [687. 最长同值路径](https://leetcode-cn.com/problems/longest-univalue-path/)

- 递归

  - 与[124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)类似

  - ```c++
    class Solution {
    public:
        int longestUnivaluePath(TreeNode* root) {
            res=0;
            if(root==nullptr)
                return res;
            traverse(root);
            return res-1;
        }
        int res;
        int traverse(TreeNode* root){//经过root节点的最大值
            if(root==nullptr)
                return 0;
            int leftLength=traverse(root->left);
            int rightLength=traverse(root->right);
            if(root->left==nullptr || root->val!=root->left->val){
                leftLength=0;
            }
            if(root->right==nullptr || root->val!=root->right->val){
                rightLength=0;
            }
            if(res<(1+leftLength+rightLength))
                res=1+leftLength+rightLength;
            return 1+(leftLength>rightLength?leftLength:rightLength);//返回单边
        }
    };
    ```

### [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

- 从矩阵右上角看是一颗二叉搜索树

  - ```c++
    class Solution {
    public:
        bool searchMatrix(vector<vector<int>>& matrix, int target) {
            int m=matrix.size();
            int n=matrix[0].size();
            int row=0,col=n-1;
            while(row<=m-1 && col>=0){
                if(matrix[row][col]==target)
                    return true;
                else if(matrix[row][col]>target)
                    col--;
                else if(matrix[row][col]<target)
                    row++;
            }
            return false;
        }
    };
    ```



## 3.图

### [797. 所有可能的路径](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

很像[剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

- 解法1：

  - 每次复制path，非常的不好

  - ```c++
    class Solution {
    public:
        vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
            vector<int> path;
            traverse(graph,0,path);
            return res;
        }
        vector<vector<int>> res;
        void traverse(vector<vector<int>>& graph,int s,vector<int> path){
            // 添加节点 s 到路径中
            path.push_back(s);
            int n= graph.size();
            if (s == n - 1) {
                // 到达终点
                res.push_back(path);
                return;
            }
            //递归每个相邻节点
            for(int v:graph[s]){
                traverse(graph,v,path);
            }
        }
    };
    ```

- 解法2：

  - 使用引用，那么每次维护的就是同一个vector了

  - ```c++
    class Solution {
    public:
        vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
            vector<int> path;
            traverse(graph,0,path);
            return res;
        }
        vector<vector<int>> res;
        void traverse(vector<vector<int>>& graph,int s,vector<int> &path){
            // 添加节点 s 到路径中
            path.push_back(s);
            int n= graph.size();
            if (s == n - 1) {
                // 到达终点
                res.push_back(path);
                return;
            }
            //递归每个相邻节点
            for(int v:graph[s]){
                traverse(graph,v,path);
                path.pop_back();
            }
        }
    };
    ```

- 解法3：

  - 使用全局变量

  - 与解法2完全等同

  - ```c++
    class Solution {
    public:
        vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
            traverse(graph,0);
            return res;
        }
        vector<vector<int>> res;
        vector<int> path;
        void traverse(vector<vector<int>>& graph,int s){
            // 添加节点 s 到路径中
            path.push_back(s);
            int n= graph.size();
            if (s == n - 1) {
                // 到达终点
                res.push_back(path);
                return;
            }
            //递归每个相邻节点
            for(int v:graph[s]){
                traverse(graph,v);
                path.pop_back();
            }
        }
    };
    ```

### [133. 克隆图](https://leetcode-cn.com/problems/clone-graph/)

- DFS

  - ```c++
    class Solution {
    public:
        Node* cloneGraph(Node* node) {
            return traverse(node);
        }
        unordered_map<Node*,Node*> mymap;//相当于visited
        Node* traverse(Node* node){
            //讲道理，node应该没有==nullptr的时候，除非整个图为空
            if(node==nullptr)
                return nullptr;
    
            //已经访问过该node了，直接返回
            if(mymap.count(node)!=0)
                return mymap[node];
    
            Node* copyNode=new Node(node->val);
            mymap[node]=copyNode;
            for(auto i:node->neighbors){
                copyNode->neighbors.push_back(traverse(i));
            }
            return copyNode;
        }
    };
    ```

- BFS

  - ```c++
    class Solution {
    public:
        Node* cloneGraph(Node* node) {
            if(node==nullptr)
                return nullptr;
            unordered_map<Node*,Node*> mymap;
            queue<Node*> q;
            Node* copyNode=new Node(node->val);//为源点复制一个结点
            mymap[node]=copyNode;
            q.push(node);
            while(!q.empty()){
            	//int size=q.size();//把分层去了也行，没什么区别。不用区分分层.
                //for(int i=0;i<size;i++){
                    Node* cur=q.front();//取出父节点
                    q.pop();
                    //构造所有的子节点，并添加至邻接表
                    for(auto i:cur->neighbors){//对于父节点的所有邻接点（子节点）
                        if(mymap.count(i)==0){//如果这个邻接点（子节点）没有被访问过
                            mymap[i]=new Node(i->val);//访问它
                            q.push(i);//子节点入队
                        }
                        mymap[cur]->neighbors.push_back(mymap[i]);//填充邻接表
                    }
                //}
            }
            return mymap[node];//返回原来的源点对应的克隆源点
        }
    };
    ```

### [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

- 拓扑排序

  - ```c++
    class Solution {
    public:
        bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
            vector<int> inDegree(numCourses,0);//记录所有顶点的入度
            vector<vector<int>> neigbor(numCourses);//邻接表
            for(auto vec:prerequisites){
                inDegree[vec[0]]++;//0号位置需要前修课程，入度++
                neigbor[vec[1]].push_back(vec[0]);//1号的邻居时0号。0号的邻居可不是1号。因为是单向图。
            }
            queue<int> q;
            for(int i=0;i<numCourses;i++){//对于图中每个顶点，入度为0的统统入队
                if(inDegree[i]==0)
                    q.push(i);//课程i入队
            }
            int count=0;//用来记录有多少个课程出队
            while(!q.empty()){
                int v=q.front();//从队里拿出来的，必定是入度为0的点
                q.pop();
                count++;
                //输出以后，把V从原图里抹掉，方法是将V的邻接点的入度都减1，就假装删掉了V。
                for(int i:neigbor[v]){//对于刚刚输出的V的所有邻接点i
                    inDegree[i]--;
                    if(inDegree[i]==0)
                        q.push(i);
                }
            }
            return count==numCourses;
        }
    };
    ```

### [210. 课程表 II](https://leetcode-cn.com/problems/course-schedule-ii/)

- 拓扑排序

  - ```c++
    class Solution {
    public:
        vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
            vector<int> res;
            vector<int> inDegree(numCourses,0);//记录所有顶点的入度
            vector<vector<int>> neigbor(numCourses);//邻接表
            for(auto vec:prerequisites){
                inDegree[vec[0]]++;//0号位置需要前修课程，入度++
                neigbor[vec[1]].push_back(vec[0]);//1号的邻居时0号。0号的邻居可不是1号。因为是单向图。
            }
            queue<int> q;
            for(int i=0;i<numCourses;i++){//对于图中每个顶点，入度为0的统统入队
                if(inDegree[i]==0)
                    q.push(i);//课程i入队
            }
            int count=0;//用来记录有多少个课程出队
            while(!q.empty()){
                int v=q.front();//从队里拿出来的，必定是入度为0的点
                res.push_back(v);
                q.pop();
                count++;
                //输出以后，把V从原图里抹掉，方法是将V的邻接点的入度都减1，就假装删掉了V。
                for(int i:neigbor[v]){//对于刚刚输出的V的所有邻接点i
                    inDegree[i]--;
                    if(inDegree[i]==0)
                        q.push(i);
                }
            }
            if(count!=numCourses)
                return vector<int>();
            return res;
        }
    };
    ```

### [1042. 不邻接植花](https://leetcode-cn.com/problems/flower-planting-with-no-adjacent/)

- 遍历。非BFS或者DFS

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

### [997. 找到小镇的法官](https://leetcode-cn.com/problems/find-the-town-judge/)

- 入度表和出度表

  - ```c++
    //很显然，用入度表和出度表。法官满足这样的条件：出度为0，入度为N-1。还说仅有一个法官，其实能满足这个条件的，必定只有一个。所以一旦找到一个，就可以break了
    class Solution {
    public:
        int findJudge(int N, vector<vector<int>>& trust) {
            vector<int> inDegree(N+1,0);
            vector<int> outDegree(N+1,0);
            for(auto vec:trust){
                outDegree[vec[0]]++;
                inDegree[vec[1]]++;
            }
            int res=-1;
            int count=0;
            for(int i=1;i<=N;i++){
                if(inDegree[i]==N-1 && outDegree[i]==0){
                    res=i;
                    count++;
                }
            }
            if(count==1)
                return res;
            return -1;
        }
    };
    ```

### [1306. 跳跃游戏 III](https://leetcode-cn.com/problems/jump-game-iii/)

- 层序遍历

  - ```c++
    class Solution {
    public:
        bool canReach(vector<int>& arr, int start) {
            if(arr.size()==0) return false;
            vector<bool> visited(arr.size(),false);
            queue<int> q;
            q.push(start);
            while(!q.empty()){
                int cur=q.front();
                q.pop();
                visited[cur]=true;
                if(arr[cur]==0)
                    return true;
                if(cur+arr[cur]<=arr.size()-1 && !visited[cur+arr[cur]]) q.push(cur+arr[cur]);
                if(cur-arr[cur]>=0 && !visited[cur-arr[cur]]) q.push(cur-arr[cur]);
            }
            return false;
        }
    };
    ```

### [1334. 阈值距离内邻居最少的城市（多源最短路问题Floyd）](https://leetcode-cn.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)

- 多源最短路问题Floyd

  - ```c++
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

### [743. 网络延迟时间（最短路径问题））](https://leetcode-cn.com/problems/network-delay-time/)

- Floyd

  - ```c++
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

## 4.并查集（我建议能用DFS的，不用并查集，因为并查集背不下来）

### [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

- 解法1：

  - DFS

  - 解决这个问题的传统方法也不困难，先用 for 循环遍历棋盘的**四边**，用 DFS 算法把那些与边界相连的 `O` 换成一个特殊字符（DFS），比如 `#`；然后再遍历整个棋盘，把剩下的 `O` 换成 `X`，把 `#` 恢复成 `O`。这样就能完成题目的要求，时间复杂度 O(MN)。

  - ```c++
    class Solution {
    public:
        void solve(vector<vector<char>>& board) {
            M=board.size();
            N=board[0].size();
            for(int i=0;i<M;i++){
                if(board[i][0]=='O')//第一列
                    dfs(board,i,0);
                if(board[i][N-1]=='O')//最后一列
                    dfs(board,i,N-1);
            }
    
            for(int j=0;j<N;j++){
                if(board[0][j]=='O')//第一行
                    dfs(board,0,j);
                if(board[M-1][j]=='O')//最后一行
                    dfs(board,M-1,j);
            }
    
            for(int i=0;i<M;i++){
                for(int j=0;j<N;j++){
                    if(board[i][j]=='O')
                        board[i][j]='X';
                    if(board[i][j]=='#')
                        board[i][j]='O';
                }
            }
        }
        int M;
        int N;
        void dfs(vector<vector<char>>& board,int m,int n){//把m,n及其周围是'O'的点都设置为'#'
            if(m<0 || n<0 || m>M || n>N)
                return;
            board[m][n]='#';
            /*if((m-1)>=0 && board[m-1][n]=='O')
                dfs(board,m-1,n);
            if((m+1)<M && board[m+1][n]=='O')
                dfs(board,m+1,n);
            if((n-1)>=0 && board[m][n-1]=='O')
                dfs(board,m,n-1);
            if((n+1)<N && board[m][n+1]=='O')
                dfs(board,m,n+1);*/
            //等价于
            vector<vector<int>> d{{1,0}, {0,1}, {0,-1}, {-1,0}};
            for (int k = 0; k < 4; k++) {
                int x = m + d[k][0];
                int y = n + d[k][1];
                if(x<0 || x>=M || y<0 || y>=N)
                    continue;
                if (board[x][y] == 'O')//x,y也是O
                    dfs(board,x,y);
            }
        }
    };
    ```

- 解法2：

  - 并查集

  - 整个棋盘就是个并查集。你可以把那些不需要被替换的`O` 看成一个拥有独门绝技的门派，它们有一个共同祖师爷叫 `dummy`，这些 `O` 和 `dummy` 互相连通，而那些需要被替换的 `O` 与`dummy`不连通。

  - UF构造的时候，默认都是独立节点，即自己的parent就是自己。然后将边界上的O点与dummy连通。然后再扫描内部所有点，内部的O 与上下左右的 O 连通。如果内部的O与边界O近邻，那么会连通到dummy上。如果不紧邻，那么要么自己为伍，要么与别的O一起连接上了，反正跟dummy一定是不连通。所有的X都是自己为伍，自己就是一棵树。
  
  - ```c++
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
    
    class Solution {
    public:
        void solve(vector<vector<char>>& board) {
            int M=board.size();
            int N=board[0].size();
            // 给 dummy 留一个额外位置
            UF uf(M*N + 1);
            int dummy = M*N;
            // 将首列和末列的 O 与 dummy 连通
            for(int i=0;i<M;i++){
                if(board[i][0]=='O')//第一列
                    uf.parent[i*N+0]=dummy;
                if(board[i][N-1]=='O')//最后一列
                    uf.parent[i*N+N-1]=dummy;
            }
    
            for(int j=0;j<N;j++){
                if(board[0][j]=='O')//第一行
                    uf.parent[0*N+j]=dummy;
                if(board[M-1][j]=='O')//最后一行
                    uf.parent[(M-1)*N+j]=dummy;
            }
            // 方向数组 d 是上下左右搜索的常用手法
            vector<vector<int>> d{{1,0}, {0,1}, {0,-1}, {-1,0}};
            for (int i = 0; i < M; i++){
                for (int j = 0; j < N; j++){
                    if(board[i][j] == 'O')//i,j是O
                        for (int k = 0; k < 4; k++) {// 将此 O 与上下左右的 O 连通
                            int x = i + d[k][0];
                            int y = j + d[k][1];
                            if(x<0 || x>=M || y<0 || y>=N)
                                continue;
                            if (board[x][y] == 'O')//x,y也是O
                                uf.unionNode(x * N + y, i * N + j);
                        }
                }
            }
    
            // 所有不和 dummy 连通的点，都要被替换（O被替换成了X，原本的X也被重新替换了一遍成X）
            for (int i = 1; i < M - 1; i++) 
            for (int j = 1; j < N - 1; j++) 
                if (!uf.connected(dummy, i * N + j))
                    board[i][j] = 'X';
    
        }
    };
    ```

### [990. 等式方程的可满足性](https://leetcode-cn.com/problems/satisfiability-of-equality-equations/)

- 并查集

  - ```c++
    class Solution {
    public:
        bool equationsPossible(vector<string>& equations) {
            UF uf(26);
            for(string str:equations){
                if(str[1]=='='){
                    uf.unionNode(str[0]-'a',str[3]-'a');
                }
            }
            for(string str:equations){
                if(str[1]=='!' && uf.connected(str[0]-'a',str[3]-'a'))
                    return false;
            }
            return true;
        }
    };
    ```

### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

- 解法1：

  - 并查集

  - ```c++
    class Solution {
    public:
        int numIslands(vector<vector<char>>& grid) {
            int M=grid.size();
            int N=grid[0].size();
            UF uf(M*N);
            // 方向数组 d 是上下左右搜索的常用手法
            vector<vector<int>> d{{1,0}, {0,1}, {0,-1}, {-1,0}};
            for (int i = 0; i < M; i++){
                for (int j = 0; j < N; j++){
                    if(grid[i][j] =='1')//i,j是1
                        for (int k = 0; k < 4; k++) {// 将此 O 与上下左右的 O 连通
                            int x = i + d[k][0];
                            int y = j + d[k][1];
                            if(x<0 || x>=M || y<0 || y>=N)
                                continue;
                            if (grid[x][y] == '1')//x,y也是1
                                uf.unionNode(x * N + y, i * N + j);
                        }
                }
            }
            unordered_set<int> myset;
            for (int i = 0; i < M; i++){
                for (int j = 0; j < N; j++){
                    if(grid[i][j] =='1')//i,j是1
                        myset.insert(uf.find(i*N+j));
                }
            }
            return myset.size();
        }
    };
    ```

- 解法2：

  - DFS

  - 每次dfs之后，就把那个点的值修改了。因此不需要visited数组。

  - ```c++
    class Solution {
    public:
        int numIslands(vector<vector<char>>& grid) {
            M=grid.size();
            N=grid[0].size();
            int res=0;
            for(int i=0;i<M;i++)
            for(int j=0;j<N;j++){
                if(grid[i][j]=='1'){
                    dfs(grid,i,j);
                    res++;
                }
            }
            return res;
        }
    
        int M;
        int N;
        void dfs(vector<vector<char>>& grid,int m,int n){
            grid[m][n]='0';
            vector<vector<int>> d{{1,0}, {0,1}, {0,-1}, {-1,0}};
            for (int k = 0; k < 4; k++) {// 将此 O 与上下左右的 O 连通
                int x = m + d[k][0];
                int y = n + d[k][1];
                if(x<0 || x>=M || y<0 || y>=N)
                    continue;
                if(grid[x][y] == '1')//x,y也是1
                    dfs(grid,x,y);      
            }
        }
    };
    ```


## 5.回溯法（DFS）

### [51. N 皇后](https://leetcode-cn.com/problems/n-queens/)

- 经典回溯

  - 一行一行的放，复杂度O(N^N)

  - 每一行上for每个列坐标放置
  
  - ```c++
    class Solution {
    public:
        vector<vector<string>> solveNQueens(int n) {
            this->n=n;
            vector<string> board(n,string(n,'.'));
            dfs(board,0);
            return res;
        }
    
        vector<vector<string>> res;
        int n;
        // 路径：board 中小于 row 的那些行都已经成功放置了皇后
        // 选择列表：第 row 行的所有列都是放置皇后的选择
        // 结束条件：row 超过 board 的最后一行
        void dfs(vector<string>& board,int row){
            if(row==n){
                res.push_back(board);
                return;
            }
            for(int j=0;j<n;j++){//列
                // 排除不合法选择
                if(!isValid(board,row,j))
                    continue;
                //做出选择
                board[row][j]='Q';
                // 进入下一行决策
                dfs(board,row+1);//在递归之前做出选择，在递归之后撤销刚才的选择
                //撤销选择
                board[row][j]='.';
            }
        }
        /* 是否可以在 board[row][col] 放置皇后？ */
        bool isValid(vector<string>& board,int row,int col){
            if(row==0)
                return true;
            // 检查列是否有皇后互相冲突
            for(int i=0;i<row;i++){
                if(board[i][col]=='Q')
                    return false;
            }
            // 检查左上方是否有皇后互相冲突
            for(int i=row-1,j=col-1;i>=0&&j>=0;i--,j--){
                if(board[i][j]=='Q')
                    return false;
            }
            // 检查右上方是否有皇后互相冲突
            for(int i=row-1,j=col+1;i>=0&&j<n;i--,j++){
                if(board[i][j]=='Q')
                    return false;
            }
            return true;
        }
    };
    ```

- 另一个写法（类似数独）

  - row、col都进行循环。判断每个坐标位置是否放Q

  - ```c++
    class Solution {
    public:
        vector<vector<string>> solveNQueens(int n) {
            this->n=n;
            vector<string> board(n,string(n,'.'));
            dfs(board,0,0);
            return res;
        }
        vector<vector<string>> res;
        int n;
        void dfs(vector<string>& board,int row,int col){
            if(row>=n){
                res.push_back(board);
                return;
            }
            if(col>=n)
            {   
                if(rowSet(board,row))//如果这一行合理放置了，才能进入下一行
                    dfs(board,row+1,0);    
                return;
            }
            if(isValid(board,row,col)){
                board[row][col]='Q';
                dfs(board,row,col+1);
                board[row][col]='.';
            }
            dfs(board,row,col+1);
    
        }
        //检查这一行上放东西了没有
        bool rowSet(vector<string>& board,int row){
            for(int j=0;j<n;j++){
                if(board[row][j]=='Q')
                    return true;
            }
            return false;
        }
        /* 是否可以在 board[row][col] 放置皇后？ */
        bool isValid(vector<string>& board,int row,int col){
            // 检查行是否有皇后互相冲突
            for(int j=0;j<col;j++){
                if(board[row][j]=='Q')
                    return false;
            }
            // 检查列是否有皇后互相冲突
            for(int i=0;i<row;i++){
                if(board[i][col]=='Q')
                    return false;
            }
            // 检查左上方是否有皇后互相冲突
            for(int i=row-1,j=col-1;i>=0&&j>=0;i--,j--){
                if(board[i][j]=='Q')
                    return false;
            }
            // 检查右上方是否有皇后互相冲突
            for(int i=row-1,j=col+1;i>=0&&j<n;i--,j++){
                if(board[i][j]=='Q')
                    return false;
            }
            return true;
        }
    };
    ```

    

### [37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

- 回溯。并不想得到所有合法的答案，只想要一个答案

  - 时间复杂度:O(9^M)，其中 `M` 是棋盘中空着的格子数量。你想嘛，对每个空格子穷举 9 个数，结果就是指数级的。

  - ```c++
    class Solution {
    public:
        void solveSudoku(vector<vector<char>>& board) {
            dfs(board,0,0);
        }
    
        bool dfs(vector<vector<char>>& board,int i, int j){
            int m = 9, n = 9;
            if(i>=n){//超过最后一行了，返回true
                return true;
            }
    
            if(j>=n){// 穷举到最后一列的话就换到下一行重新开始。
                return dfs(board,i+1,0);   
            }
    
            // 如果该位置是预设的数字，不用我们操心。直接开始dfs下一个
            if (board[i][j] != '.') {
                return dfs(board, i, j + 1);
            } 
    
            for (char ch = '1'; ch <= '9'; ch++) {
                if(!isValid(board,i,j,ch))//排除不合法的选择
                    continue;
    
                board[i][j]=ch;//做出选择
                if(dfs(board,i,j+1))//下一行决策
                    return true;
                board[i][j]='.';//撤销选择
            }
            return false;// 穷举完 1~9，依然没有找到可行解，此路不通
        }
    
        bool isValid(vector<vector<char>>& board,int row, int col,char ch){
            // 判断本列是否存在重复
            for (int i = 0; i < 9; i++) {
                if(board[i][col]==ch) 
                    return false;
            }
    
            // 判断本行是否存在重复
            for (int j = 0; j < 9; j++) {
                if(board[row][j]==ch) 
                    return false;
            }
    
            //判断九宫格是否存在重复
            for(int i=0;i<3;i++)
            for(int j=0;j<3;j++)
                if(board[row/3*3+i][col/3*3+j]==ch)
                    return false;
            
            return true;
        }
    };
    ```

### [698. 划分为k个相等的子集](https://leetcode-cn.com/problems/partition-to-k-equal-sum-subsets/)

- 解法1：

  - 回溯。最暴力的方法。超时了

  - ```c++
    class Solution {
    public:
        bool canPartitionKSubsets(vector<int>& nums, int k) {
            this->k=k;
            vector<int>bucket(k,0);
            return dfs(nums,0,bucket);
        }
        int k;
        bool dfs(vector<int>& nums,int index,vector<int>& bucket){
            // base case
            if (index ==nums.size()) { 
                return helper(bucket);
            }
            // 穷举每个桶
            for(int i=0;i<k;i++){
                // 选择装进第 i 个桶
                bucket[i]+=nums[index];
                // 递归穷举下一个数字的选择
                if(dfs(nums,index+1,bucket))
                    return true;
                // 撤销选择
                bucket[i]-=nums[index];
            }
            return false;
        }
    
        bool helper(vector<int>& bucket){
            int tmp=bucket[0];
            for(int i=1;i<bucket.size();i++){
                if(bucket[i]!=tmp)
                    return false;
            }
            return true;
        }
    };
    ```

- 解法2：

  - **切换到这** **`n`** **个数字的视角，每个数字都要选择进入到** **`k`** **个桶中的某一个**。

  - 数组降序排列，再运用剪枝策略。

  - ```c++
    class Solution {
    public:
        bool canPartitionKSubsets(vector<int>& nums, int k) {
            /* 降序排序 nums 数组 */
            //举个例子：5 4 3 2 1就比 3 1 2 4 5快。把大头分散在各个桶里，可以减少循环次数
            int sum=0;
            for(int i:nums) sum+=i;
            if(sum%k!=0) return false;
            int target=sum/k;
            vector<int> bucket(k,0);
            vector<int> numsCopy=nums;
            sort(numsCopy.begin(),numsCopy.end(),greater<int>());
            return dfs(numsCopy,0,bucket,target);
        }
        bool dfs(vector<int>& nums, int index,vector<int>& bucket,int target){
            if(index==nums.size())
                return true;//其实不用比较所有元素是否相同。因为能全部放到桶里时，肯定是都放对了。
            				//对于那种放不进去的情况，一定执行不到这一步。比如[2,2,2,2,3,4,5]   4
            // 穷举每个桶
            for(int i=0;i<bucket.size();i++){
                // 剪枝，桶装装满了
                if(nums[index]+bucket[i]>target)//排除不合适的桶
                    continue;
                bucket[i]+=nums[index];// 选择nums[index]装进第 i 个桶
                if(dfs(nums,index+1,bucket,target))// 递归穷举下一个数字的选择
                    return true;
                bucket[i]-=nums[index];//撤销选择
            }
            return false;// nums[index] 装入哪个桶都不行
        }
    };
    ```
  
- 解法3：

  - **以桶的视角进行穷举，每个桶需要遍历** **`nums`** **中的所有数字，决定是否把当前数字装进桶中；当装满一个桶之后，还要装下一个桶，直到所有桶都装满为止**。

  - ```c++
    class Solution {
    public:
        bool canPartitionKSubsets(vector<int>& nums, int k) {
            int sum=0;
            for(int i:nums) sum+=i;
            if(sum%k!=0) return false;
            int target=sum/k;
            vector<bool> numsValid(nums.size(),true);
            vector<int> bucket(k,0);
            return dfs(nums,numsValid,bucket,0,target);
        }
        bool dfs(vector<int>& nums,vector<bool>& numsValid,vector<int>& bucket,int bucketIndex,int target){
            if(bucketIndex==bucket.size())// 所有桶都被装满了，而且 nums 一定全部用完了,因为 target == sum / k
                return true;
            if (bucket[bucketIndex] == target) {
            // 装满了当前桶，递归穷举下一个桶的选择
            // 让下一个桶从 nums[0] 开始选数字
                return dfs(nums,numsValid,bucket,bucketIndex+1,target);
            }
            for(int i=0;i<nums.size();i++){
                if(!numsValid[i] || (bucket[bucketIndex]+nums[i])>target)
                    continue;
                bucket[bucketIndex]+=nums[i];
                numsValid[i]=false;
                // 递归穷举下一个数字是否装入当前桶
                if (dfs(nums, numsValid, bucket, bucketIndex, target)){
                    return true;
                }
                numsValid[i]=true;
                bucket[bucketIndex]-=nums[i];
            }
            return false;
        }
    };
    ```

    - 这样写超时了

  - 进行优化，加上一个numsIndexStart

  - ```c++
    class Solution {
    public:
        bool canPartitionKSubsets(vector<int>& nums, int k) {
            int sum=0;
            for(int i:nums) sum+=i;
            if(sum%k!=0) return false;
            int target=sum/k;
            vector<bool> numsValid(nums.size(),true);
            vector<int> bucket(k,0);
            return dfs(nums,numsValid,0,bucket,0,target);
        }
        bool dfs(vector<int>& nums,vector<bool>& numsValid, int numsIndexStart,vector<int>& bucket,int bucketIndex,int target){
            if(bucketIndex==bucket.size())// 所有桶都被装满了，而且 nums 一定全部用完了,因为 target == sum / k
                return true;
            if (bucket[bucketIndex] == target) {
            // 装满了当前桶，递归穷举下一个桶的选择
            // 让下一个桶从 nums[0] 开始选数字
                return dfs(nums,numsValid,0,bucket,bucketIndex+1,target);
            }
            for(int i=numsIndexStart;i<nums.size();i++){
                if(!numsValid[i] || (bucket[bucketIndex]+nums[i])>target)
                    continue;
                bucket[bucketIndex]+=nums[i];
                numsValid[i]=false;
                // 递归穷举下一个数字是否装入当前桶
                if (dfs(nums, numsValid, numsIndexStart+1, bucket, bucketIndex, target)){
                    return true;
                }
                numsValid[i]=true;
                bucket[bucketIndex]-=nums[i];
            }
            return false;
        }
    };
    ```

- 第一个解法，也就是从数字的角度进行穷举，`n` 个数字，每个数字有 `k` 个桶可供选择，所以组合出的结果个数为 `k^n`，时间复杂度也就是 `O(k^n)`。

- 第二个解法，每个桶要遍历 `n` 个数字，选择「装入」或「不装入」，组合的结果有 `2^n` 种；而我们有 `k` 个桶，所以总的时间复杂度为 `O(k*2^n)`。

- 第一种解法即便经过了排序优化，也明显比第二种解法慢很多

- 通俗来说，我们应该尽量「少量多次」，就是说宁可多做几次选择，也不要给太大的选择空间；宁可「二选一」选 `k` 次，也不要 「`k` 选一」选一次。

### [78. 子集](https://leetcode-cn.com/problems/subsets/)

- 解法1：

  - 不推荐

  - 迭代（数组迭代也可以写成递归的形式，都行）。`1+2+4+8+....+2^n-1=2^n`。O(2^n)

  - ```c++
    class Solution {
    public:
        vector<vector<int>> subsets(vector<int>& nums) {
            vector<vector<int>> res={{}};
            for(int i=0;i<nums.size();i++){
                int size=res.size();
                for(int j=0;j<size;j++){//遍历j次
                    vector<int> vec=res[j];
                    vec.push_back(nums[i]);
                    res.push_back(vec);
                }
            }
            return res;
        }
    
    };
    ```

- 解法2：

  - 回溯1

  - 递归遍历数组，判断每个下标位置

  - 不推荐

  - ```c++
    class Solution {
    public:
        vector<vector<int>> subsets(vector<int>& nums) {
            vector<int> track;
            dfs(nums,0,track);
            return res;
        }
        vector<vector<int>> res;
        void dfs(vector<int>& nums,int index,vector<int>& track){
            if(index==nums.size()){
                res.push_back(track);
                return;
            }
            //相当于把两种情况的For循环拆开了，写成了无For的形式。就两种选择，这个节点加入或者是不加入
            track.push_back(nums[index]);//做选择
            dfs(nums,index+1,track);
            track.pop_back();//撤销
            dfs(nums,index+1,track);//做选择
        }
    };
    ```

  - [[1,2,3],[1,2],[1,3],[1],[2,3],[2],[3],[]]

- 解法3：

  - 回溯2

  - 迭代遍历数组，递归下一个元素

  - **推荐**

  - ```c++
    class Solution {
    public:
        vector<vector<int>> subsets(vector<int>& nums) {
            vector<int> track;
            dfs(nums,0,track);
            return res;
        }
        vector<vector<int>> res;
        void dfs(vector<int>& nums,int start,vector<int>& track){
            res.push_back(track);
            if(start==nums.size())
                return;
            for(int i=start;i<nums.size();i++){
                track.push_back(nums[i]);
                dfs(nums,i+1,track);//注意是i+1，不是start+1
                track.pop_back();
            }
        }
    };
    ```

  - [[],[1],[1,2],[1,2,3],[1,3],[2],[2,3],[3]]

### [77. 组合](https://leetcode-cn.com/problems/combinations/)

- 回溯1

  - 递归遍历数组，判断每个下标位置。（缺点，只能按序遍历，无法解决排列问题）

  - ```c++
    class Solution {
    public:
        vector<vector<int>> combine(int n, int k) {
            this->n=n;
            this->k=k;
            vector<int> track;
            dfs(track,1);
            return res;
        }
        vector<vector<int>> res;
        int n;
        int k;
        void dfs(vector<int>& track,int index){
            if(track.size()==k){
                res.push_back(track);
                return;
            }
            if(index==n+1)
                return;
    
            track.push_back(index);
            dfs(track,index+1);
            track.pop_back();
            dfs(track,index+1);
    
        }
    };
    ```

  - [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]

- 回溯2

  - 迭代遍历数组，递归下一个元素

  - **推荐！**

  - ```c++
    class Solution {
    public:
        vector<vector<int>> combine(int n, int k) {
            this->n=n;
            this->k=k;
            vector<int> track;
            dfs(track,1);
            return res;
        }
        vector<vector<int>> res;
        int n;
        int k;
        void dfs(vector<int>& track,int start){
            if(track.size()==k){
                res.push_back(track);
                return;
            }
            // 注意 i 从 start 开始递增
            for (int i = start; i <= n; i++) {
                // 做选择
                track.push_back(i);
                dfs(track,i+1);//start设置为i+1
                // 撤销选择
                track.pop_back();
            }
        }
    };
    ```

  - [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]

### [46. 全排列](https://leetcode-cn.com/problems/permutations/)

- 回溯

  - ```c++
    class Solution {
    public:
        vector<vector<int>> permute(vector<int>& nums) {
            valid=vector<bool>(nums.size(),true);
            vector<int> track;
            dfs(nums,track);
            return res;
        }
    
        vector<vector<int>> res;
        vector<bool> valid;
        void dfs(vector<int>& nums,vector<int>& track){
            if(track.size()==nums.size()){
                res.push_back(track);
                return;
            }
    
            for(int i=0;i<nums.size();i++){
                //排除不合法的选择
                if(!valid[i])
                    continue;
                track.push_back(nums[i]);
                valid[i]=false;
                dfs(nums,track);
                track.pop_back();
                valid[i]=true;
            }
    
        }
    };
    ```

### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

- 回溯

  - 使用数组模板1

  - ```c++
    class Solution {
    public:
        vector<string> generateParenthesis(int n) {
            string track(2*n,'0');
            int left=0;
            int right=0;
            dfs(left,right,track,0);
            return res;
        }
        vector<string> res;
        void dfs(int left,int right,string& track,int index){
            if(left<right)
                return;
            if(index==track.size()){
                if(left==right)
                    res.push_back(track);
                return;
            }
    
            track[index]='(';
            dfs(left+1,right,track,index+1);
            track[index]=')';
            dfs(left,right+1,track,index+1);
        }
    };
    ```

- 回溯2

  - 使用数组模板2

  - 类似[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)https://leetcode-cn.com/problems/combinations/)

  - ```c++
    class Solution {
    public:
        vector<string> generateParenthesis(int n) {
            this->n=n;
            string str="()";
            string track;
            dfs(str,0,track,0,0);
            return res;
        }
        vector<string> res;
        int n;
        void dfs(string& str,int start,string& track,int left,int right){
            if(right>left)
                return;
            if(left>n || right>n)
                return;
            if(left==n && right==n){
                res.push_back(track);
                return;
            }
            for(int i=start;i<str.size();i++){
                track+=str[i];
                if(str[i]=='(')
                    left++;
                else
                    right++;
                dfs(str,start,track,left,right);
                if(str[i]=='(')
                    left--;
                else
                    right--;
                track.pop_back();
            }
        }
    };
    ```

    

### [93. 复原 IP 地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

- 回溯1

  - 使用数组模板1。递归遍历数组，判断每个下标是否加入

  - 只能放入3个"."

  - ```c++
    class Solution {
    public:
        vector<string> restoreIpAddresses(string s) {
            if(s.size()<4)
                return res;
            int pre=-1;
            int count=0;
            int index=1;
            dfs(s,index,count,pre);
            return res;
        }
    
        vector<string> res;
        void dfs(string& s,int index,int count,int pre){//index是当前下标；count统计插入了几个"."；pre记录上一个"."的位置
            if(index>=s.size())
                return;
            if(count==3){
                if(isValid(s,s.size(),pre))
                    res.push_back(s);
                return;
            }
    
            if(isValid(s,index,pre)){//index位置插入"."
                s.insert(index,".");
                count++;
                int tmp=pre;
                pre=index;
                dfs(s,index+1,count,pre);
                s.erase(index,1);
                count--;
                pre=tmp;
            }
            dfs(s,index+1,count,pre);////index位置不插入"."
        }
    
        bool isValid(string& s,int index,int pre){
            pre=pre+1;
            if(index-pre==0)//连插了两个"."，不合理
                return false;
            if(index-pre==1)//两"."之间仅一个字符，肯定合理
                return true;
            if(s[pre]=='0')//以0开头，且不止一个字符，不合理
                return false;
            if(index-pre>3)///两"."之间大于3个字符
                return false;
            string tmp=s.substr(pre,index-pre);//取出数字
            if(stoi(tmp)>255)//数字>255
                return false;
            return true;
        }
    };
    ```

- 回溯2

  - 每一个位置，for 1:3去尝试本段的长度

  - ```c++
    class Solution {
    public:
        vector<string> restoreIpAddresses(string s) {
            if(s.size()<4)
                return res;
            string track;
            dfs(s,0,0,track);
            return res;
        }
    
        vector<string> res;
        void dfs(string& s,int start,int count,string& track){
            if (count ==3){ // 切完3刀了
                if(isValid(s,start,s.size()-start)){//检查一下最后一刀到最后是否合理
                    track+=s.substr(start,s.size()-start);
                    res.push_back(track);
                }
                return;
            }
            for(int i = 1; i <= 3 ; i++) { // 切一刀，可能是1-3个长度
                if(start+i-1>=s.size())
                    break;
                if(!isValid(s,start,i))
                    continue;
                string backup=track;
                track+=s.substr(start,i)+".";
                count++;
                dfs(s,start+i,count,track);
                count--;
                track=backup;
            }
        }
    
        bool isValid(string& s,int start,int i){
            if(start>=s.size() || i<=0 || i>3)
                return false;
            if(s[start]=='0' && i!=1)
                return false;
            string tmp=s.substr(start,i);
            if(stoi(tmp)>255)
                return false;
            return true;
        }
    };
    ```

### [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

- 数组模板2：

  - 跟[78. 子集](https://leetcode-cn.com/problems/subsets/)几乎一样

  - ```c++
    class Solution {
    public:
        vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
            this->target=target;
            vector<int> track;
            dfs(candidates,track,0,0);
            return res;
        }
        vector<vector<int>> res;
        int target;
        void dfs(vector<int>& candidates,vector<int>& track,int start,int sum){
            if(sum==target){
                res.push_back(track);
                return;
            }
            if(sum>target)
                return;
            for(int i=start;i<candidates.size();i++){
                track.push_back(candidates[i]);
                sum+=candidates[i];
                dfs(candidates,track,i,sum);
                sum-=candidates[i];
                track.pop_back();
            }
        }
    };
    ```

### [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

- 经典回溯

  - 使用的数组回溯模板1

  - ```c++
    class Solution {
    public:
        bool exist(vector<vector<char>>& board, string word) {
            m=board.size();
            n=board[0].size();
            valid=vector<vector<bool>>(m,vector<bool>(n,true));
            for(int i=0;i<m;i++)
            for(int j=0;j<n;j++)
                if(dfs(board,word,0,i,j))
                    return true;
            return false;
        }
        vector<vector<int>> d{{0,1},{0,-1},{1,0},{-1,0}};
        vector<vector<bool>> valid;
        int m;
        int n;
        bool dfs(vector<vector<char>>& board,string& word,int index,int row,int col){
            //index用来索引word。row,col就像数独一样，用来索引board
            //if(index>=word.size())
                //return true;
            if(board[row][col]!=word[index])
                return false;
            valid[row][col]=false;//该变量就算使用了
            if((index+1)>=word.size())//注意，这个截止条件。之前是按上边注释的写的。存在问题就是['AB']时，查找'AB'，没法多进入一轮dfs
                return true;			//改成这样之后就好了
            for(int i=0;i<4;i++){
                int x=row+d[i][0];
                int y=col+d[i][1];
                if(x<0 || y<0 || x>=m || y>=n)
                    continue;
                if(!valid[x][y])
                    continue;
                if(dfs(board,word,index+1,x,y))
                    return true;
            }
            valid[row][col]=true;//这个地方之前忘记改了，这是全局变量，一定要注意退出时维护
            return false;
        }
    };
    ```

### [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

- 全排列的升级版

  - 多了一个sort，然后去重的操作

  - ```c++
    class Solution {
    public:
        vector<vector<int>> permuteUnique(vector<int>& nums) {
            valid=vector<bool>(nums.size(),true);
            vector<int> track;
            sort(nums.begin(),nums.end(),less<int>());//排序
            dfs(nums,track);
            return res;
        }
    
        vector<vector<int>> res;
        vector<bool> valid;
        void dfs(vector<int>& nums,vector<int>& track){
            if(track.size()==nums.size()){
                res.push_back(track);
                return;
            }
    
            for(int i=0;i<nums.size();i++){
                //排除不合法的选择
                if(!valid[i])
                    continue;
                //排除 同一树层。不用set
                if(i>0 && nums[i-1]==nums[i] && valid[i-1]==true)//不是第一个数 && 这个数跟前一个数相等 && 前一个可用说明是在决策树的同一层
                    continue;
                track.push_back(nums[i]);
                valid[i]=false;
                dfs(nums,track);
                track.pop_back();
                valid[i]=true;
            }
    
        }
    };
    ```

### [剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

- 当做数组的全排列来做的

  - 跟上边那个题一模一样

  - ```c++
    class Solution {
    public:
        vector<string> permutation(string s) {
            valid=vector<bool>(s.size(),true);
            string track;
            sort(s.begin(),s.end(),less<char>());
            dfs(s,track);
            return res;
        }
        vector<string> res;
        vector<bool> valid;
        void dfs(string& s,string& track){
            if(track.size()==s.size()){
                res.push_back(track);
                return;
            }
            for(int i=0;i<s.size();i++){
                if(!valid[i])
                    continue;
                if(i>0 && s[i]==s[i-1] && valid[i-1]==true)
                    continue;
                valid[i]=false;
                track+=s[i];
                dfs(s,track);
                track.pop_back();
                valid[i]=true;
            }
        }
    };
    ```

- 使用set去重

  - 加了个set来去重

  - ```c++
    class Solution {
    public:
        vector<string> permutation(string s) {
            valid=vector<bool>(s.size(),true);
            string track;
            dfs(s,track);
            //res.assign(myset.begin(), myset.end());//用assign实现set转vector
            for(auto i:myset)
                res.push_back(i);
            return res;
        }
        vector<string> res;
        vector<bool> valid;
        unordered_set<string> myset;
        void dfs(string& s,string& track){
            if(track.size()==s.size()){
                myset.insert(track);
                return;
            }
            for(int i=0;i<s.size();i++){
                if(!valid[i])
                    continue;
                valid[i]=false;
                track+=s[i];
                dfs(s,track);
                track.pop_back();
                valid[i]=true;
            }
        }
    };
    ```

### [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

- 去重1：

  - 使用valid来去重

  - ```c++
    class Solution {
    public:
        vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
            this->target=target;
            vector<int> track;
            sort(candidates.begin(),candidates.end(),less<int>());
            valid=vector<bool>(candidates.size(),true);
            dfs(candidates,0,track,0);
            return res;
        }
        vector<vector<int>> res;
        int target;
        vector<bool> valid;
        void dfs(vector<int>& candidates,int start,vector<int>& track,int sum){
            if(sum==target){
                res.push_back(track);
                return;
            }
            if(sum>target)
                return;
            
            for(int i=start;i<candidates.size();i++){
                if(i>0 && candidates[i]==candidates[i-1] && valid[i-1]==true)
                    continue;
                
                sum+=candidates[i];
                track.push_back(candidates[i]);
                valid[i]=false;
                dfs(candidates,i+1,track,sum);
                track.pop_back();
                sum-=candidates[i];
                valid[i]=true;
            }
        }
    };
    ```

- 去重2：

  - 利用start去重

  - ```c++
    class Solution {
    public:
        vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
            this->target=target;
            vector<int> track;
            sort(candidates.begin(),candidates.end(),less<int>());
            dfs(candidates,0,track,0);
            return res;
        }
        vector<vector<int>> res;
        int target;
    
        void dfs(vector<int>& candidates,int start,vector<int>& track,int sum){
            if(sum==target){
                res.push_back(track);
                return;
            }
            if(sum>target)
                return; 
            for(int i=start;i<candidates.size();i++){
                if(i>0 && candidates[i]==candidates[i-1] && i!=start)
                    continue;
                sum+=candidates[i];
                track.push_back(candidates[i]);
                dfs(candidates,i+1,track,sum);
                track.pop_back();
                sum-=candidates[i];
            }
        }
    };
    ```

## 6.BFS（暴力穷举，找最短路径问题）

### [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

- BFS

  - ```c++
    class Solution {
    public:
        int minDepth(TreeNode* root) {
            if(root==nullptr)
                return 0;
            queue<TreeNode*> q;
            q.push(root);
             // root 本身就是一层，depth 初始化为 1
            int res=1;
            while(!q.empty()){
                int size=q.size();
                /* 将当前队列中的所有节点向四周扩散 */
                for(int i=0;i<size;i++){
                    TreeNode* cur=q.front();
                    q.pop();
                    /* 判断是否到达终点 */
                    if(cur->left==nullptr && cur->right==nullptr)
                        return res;
                    /* 将 cur 的相邻节点加入队列 */
                    if(cur->left!=nullptr) q.push(cur->left);
                    if(cur->right!=nullptr) q.push(cur->right);
                }
                /* 这里增加步数 */
                res++;
            }
            return 0;//其实永远不会在这儿返回
        }
    };
    ```

### [752. 打开转盘锁](https://leetcode-cn.com/problems/open-the-lock/)

- 一定要在入队时加入insert

  - 否则会导致过多的for循环

  - ```c++
    class Solution {
    public:
        int openLock(vector<string>& deadends, string target) {
            unordered_set<string> visited(deadends.begin(),deadends.end());
            if(visited.count("0000"))
                return -1;
            queue<string> q;
            q.push("0000");
            int depth=0;
            visited.insert("0000");
            while(!q.empty()){
                int size=q.size();
                for(int i=0;i<size;i++){
                    string cur=q.front();
                    q.pop();
                    //visited.insert(cur);//从队列里出来时visit复杂度极高，原因没想出来
                    if(cur==target)
                        return depth;
                    for(int j=0;j<4;j++){
                        string up=plusOne(cur,j);
                        string down=minusOne(cur,j);
                        if(visited.count(up)==0){q.push(up);visited.insert(up);}//入队时visit
                        if(visited.count(down)==0){q.push(down);visited.insert(down);}
                    }
                }
                depth++;
            }
            return -1;
        }
    
        // 将 s[j] 向上拨动一次
       string plusOne(string s, int j) {
            if (s[j] == '9')
                s[j] = '0';
            else
                s[j] += 1;
            return s;
        }
        string minusOne(string s,int j){
            if(s[j]=='0')
                s[j]='9';
            else
                s[j]-=1;
            return s;
        }
    };
    ```

### [773. 滑动谜题](https://leetcode-cn.com/problems/sliding-puzzle/)

- BFS

  - **其中比较有技巧性的点在于，二维数组有「上下左右」的概念，压缩成一维后，如何得到某一个索引上下左右的索引**？

  - ```c++
    class Solution {
    public:
        int slidingPuzzle(vector<vector<int>>& board) {
            vector<vector<int>> neighbor = {
                { 1, 3 },
                { 0, 4, 2 },
                { 1, 5 },
                { 0, 4 },
                { 3, 1, 5 },
                { 4, 2 }
            };
            queue<string> q;
            string target="123450";
            string start="";
            for(int i=0;i<2;i++)
            for(int j=0;j<3;j++){
                start+=to_string(board[i][j]);
            }
            q.push(start);
            unordered_set<string> visited;
            visited.insert(start);
            int step=0;
            while(!q.empty()){
                int size=q.size();
                for(int i=0;i<size;i++){
                    string cur=q.front();
                    q.pop();
                    if(cur==target)
                        return step;
                    int index=0;
                    for (; cur[index] != '0'; index++);
                    for(auto j:neighbor[index]){
                        string newStr=cur;
                        newStr[index]=cur[j];//swap 两个数的位置
                        newStr[j]=cur[index];
                         // 防止走回头路
                        if(visited.count(newStr)==0){
                            q.push(newStr);
                            visited.insert(newStr);
                        }
                    }
                }
                step++;
            }
            return -1;
        }
    };
    ```

  - 备注：可以多加一个队列存0的索引，这样就不用每次找 0 在哪了，空间换时间

## 7.数组-二分查找

### [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

- 最原始的二分查找，数组中没有重复的，或者是返回重复数字的任意一个下标就行

  - ```c++
    class Solution {
    public:
        int search(vector<int>& nums, int target) {
            int left=0,right=nums.size()-1;
            while(left<=right){
                int mid=left+(right-left)/2;
                if(nums[mid]==target)
                    return mid;
                else if(nums[mid]>target){
                    right=mid-1;
                }
                else if(nums[mid]<target)
                    left=mid+1;
            }
            return -1;
        }
    };
    ```

### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

- 寻找左边界的二分查找+寻找有边界的二分查找

  - ```c++
    class Solution {
    public:
        vector<int> searchRange(vector<int>& nums, int target) {
            vector<int> res;
            res.push_back(findleft(nums,target));
            res.push_back(findright(nums,target));
            return res;
        }
    
        int findleft(vector<int>& nums, int target)
        {
            int left=0;
            int right=nums.size()-1;
            while(left<=right){
                int mid=left+(right-left)/2;
                if(nums[mid]==target)
                    right=mid-1;
                else if(nums[mid]>target)
                    right=mid-1;
                else if(nums[mid]<target)
                    left=mid+1;
            }
            if(left>=nums.size()|| nums[left]!=target)//left>=nums.size()说明target比nums里的数都大;
                                                    //target比nums里的数都小 或者是 target这个数不在数组里
                return -1;
            return left;
        }
    
        int findright(vector<int>& nums, int target)
        {
            int left=0;
            int right=nums.size()-1;
            while(left<=right){
                int mid=left+(right-left)/2;
                if(nums[mid]==target)
                    left=mid+1;
                else if(nums[mid]>target)
                    right=mid-1;
                else if(nums[mid]<target)
                    left=mid+1;
            }
            if(right<0 || nums[right]!=target)//target比数组里所有的数都大时 || (target比数组里所有的数都小 或者 target这个数不在数组里)
                return -1;
            return right;
        }
    };
    ```

### [875. 爱吃香蕉的珂珂](https://leetcode-cn.com/problems/koko-eating-bananas/)

- 非数组二分。相当于找左边界的二分

  - ```c++
    class Solution {
    public:
        int minEatingSpeed(vector<int>& piles, int h) {
            //相当于找左边界的二分查找
            int left=1,right=getMax(piles);
            while(left<=right){
                int mid=left+(right-left)/2;
                if(canFinish(piles,mid,h))
                    right=mid-1;
                else 
                    left=mid+1;
            }
            //该值一定存在，不必判断边界。即target不可能比nums里的数还大，也不会比nums的数都小，且值是连续存在的
            return left;
        }
    
        bool canFinish(vector<int>& piles,int speed,int h){
            int time = 0;
            for (int n : piles) {
                time += timeOf(n, speed);
            }
            return time <= h;
        }
    
        int timeOf(int n, int speed) {
            return (n / speed) + ((n % speed > 0) ? 1 : 0);
        }   
    
        int getMax(vector<int>& piles){
            int res=-1;
            for(int i:piles)
                if(i>res)
                    res=i;
            return res;
        }
    };
    ```

### [1011. 在 D 天内送达包裹的能力](https://leetcode-cn.com/problems/capacity-to-ship-packages-within-d-days/)

- 相当于是寻找左边界的二分查找，把>=的情况合二为一，即条件成真

- 最小值得是最大的货物重量

- 最大值就是一趟拉走，货物总和

  - ```c++
    class Solution {
    public:
        int shipWithinDays(vector<int>& weights, int days) {
            // int left=1,right=getSum(weights);   //如果是从1开始判断，那么底下的注释也得打开。
            int left=*max_element(weights.begin(),weights.end());//最小值是最大的货物重量，那么就不可能有weights[i]>cap的情况了
            int right=getSum(weights);
            while(left<=right){
                int mid=left+(right-left)/2;
                if(helper(weights,days,mid))
                    right=mid-1;
                else
                    left=mid+1;
            }
            return left;
        }
        int getSum(vector<int>& weights){
            return accumulate(weights.begin(),weights.end(),0);
        }
        bool helper(vector<int>& weights, int days, int cap){
            int cnt=1;//上来就是1船
            int curCap=0;
            for(int i=0;i<weights.size();i++){
                // if(weights[i]>cap)
                //     return false;
                curCap+=weights[i];
                if(curCap>cap){
                    cnt++;
                    if(cnt>days)
                        return false;
                    curCap=weights[i];
                }
            }
            return true;
        }
    };
    ```

### [剑指 Offer 11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

- 旋转数组的二分

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

### [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

- 旋转数组的二分

  - ```c++
    class Solution {
    public:
        int search(vector<int>& nums, int target) {
            int left=0,right=nums.size()-1;
            while(left<right){//注意:left==right时推出
                int mid=left+(right-left)/2;
                if(nums[mid]<nums[right]){//旋转数组模板，这套模板跟原始的二分可不一样。原始的二分都是mid跟target作比较
                    if(target<=nums[right] && target>=nums[mid])//如果处于有序的一端
                        return binarySearch(nums,mid,right,target);
                    else 
                        right=mid-1;
                }
                else if(nums[mid]>nums[right]){
                    if(target<=nums[mid] && target>=nums[left])
                        return binarySearch(nums,left,mid,target);
                    else
                        left=mid+1;
                }
                // else if(nums[mid]==nums[right])
                //     right--;
            }
            return nums[left]==target?left:-1;//left、right都行，再检查一下
        }
    
        int binarySearch(vector<int>& nums, int left,int right,int target){//朴素的二分查找
            while(left<=right){
                int mid=left+(right-left)/2;
                if(nums[mid]==target)
                    return mid;
                else if(nums[mid]>target)
                    right=mid-1;
                else if(nums[mid]<target)
                    left=mid+1;
            }
            return -1;
        }
    };
    ```

- 另一种思路，先找最小，再两个朴素二分。因为不存在重复元素，这样做是合理的。

  - ```c++
    class Solution {
    public:
        int search(vector<int>& nums, int target) {
            int index=findMin(nums);
            int res=binarySearch(nums,0,index-1,target);
            if(res!=-1)
                return res;
            res=binarySearch(nums,index,nums.size()-1,target);
            return res;
        }
    
        int findMin(vector<int>& nums){
            int left=0,right=nums.size()-1;
            while(left<right){
                int mid=left+(right-left)/2;
                if(nums[mid]<nums[right])
                    right=mid;
                else if(nums[mid]>nums[right])
                    left=mid+1;
                //else if(nums[mid]==nums[right])
                    //right--;
            }
            return left;
        }
    
        int binarySearch(vector<int>& nums,int left,int right,int target){
            while(left<=right){
                int mid=left+(right-left)/2;
                if(nums[mid]==target)
                    return mid;
                else if(nums[mid]>target)
                    right=mid-1;
                else if(nums[mid]<target)
                    left=mid+1;
            }
            return -1;
        }
    };
    ```

### [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

- 跟上边那个题一样

  - 套用旋转数组的二分的模板再继续if else细节

  - ```c++
    class Solution {
    public:
        bool search(vector<int>& nums, int target) {
            int left=0,right=nums.size()-1;
            while(left<right){
                int mid=left+(right-left)/2;
                if(nums[mid]<nums[right]){
                    if(target<=nums[right] && target>=nums[mid])
                        return binarySearch(nums,mid,right,target);
                    else 
                        right=mid-1;
                }
                else if(nums[mid]>nums[right]){
                    if(target<=nums[mid] && target>=nums[left])
                        return binarySearch(nums,left,mid,target);
                    else
                        left=mid+1;
                }
                else if(nums[mid]==nums[right])
                    right--;
            }
            return nums[left]==target;
        }
    
        bool binarySearch(vector<int>& nums, int left,int right,int target){
            while(left<=right){
                int mid=left+(right-left)/2;
                if(nums[mid]==target)
                    return true;
                else if(nums[mid]>target)
                    right=mid-1;
                else if(nums[mid]<target)
                    left=mid+1;
            }
            return false;
        }
    };
    ```

- 不可以先找最小，再两个二分了。因为：`11121`像这种案例，findMin返回的下标是0！而不是最后那个1的下标

- 另一种思路

  - ```c++
    class Solution {
    public:
        bool search(vector<int>& nums, int target) {
            int left=0,right=nums.size()-1;
            while(left<=right){
                if(target==nums[right] || target==nums[left])
                    return true;
    
                if(target>nums[right] && target<nums[left])
                    return false;
                else if(target>nums[right] && target>nums[left])
                    right--;
                else if(target<nums[right] && target<nums[left])
                    left++;
                else{//已经处于递增序列上
                    int mid=left+(right-left)/2;
                    if(nums[mid]==target)
                        return true;
                    else if(nums[mid]>target)
                        right=mid-1;
                    else if(nums[mid]<target)
                        left=mid+1;
                }
            }
            return false;
        }
    };
    ```


### 补充题：求一个先升序后降序得数组的最大值

- 如果没有重复元素

  - ```c++
    int binarySearchPeak(vector<int>& arr)
    {
        int left=0;
        int right=arr.size()-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(mid==0||mid==arr.size()-1){
                return -1;
            }
            if(arr[mid-1]<arr[mid]&&arr[mid]>arr[mid+1]){
                return arr[mid];
            }else if(arr[mid-1]<arr[mid]&&arr[mid+1]>arr[mid]){
                left=mid+1;
            }else if(arr[mid-1]>arr[mid]&&arr[mid]>arr[mid+1]){
                right=mid-1;
            }
        }
        return -1;
    }
    ```

- **如果有重复元素**

  - ```c++
    int binarySearchPeak(vector<int>& nums){
        int left=0,right=nums.size()-1;
        while(left<right){
            int mid=left+(right-left)/2;
            if(nums[mid]>nums[mid+1])
                right=mid;
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
    ```

### [1095. 山脉数组中查找目标值](https://leetcode-cn.com/problems/find-in-mountain-array/)

- 先求一个先升序后降序得数组的最大值

- 然后再朴素的两个二分

  - ```c++
    /**
     * // This is the MountainArray's API interface.
     * // You should not implement it, or speculate about its implementation
     * class MountainArray {
     *   public:
     *     int get(int index);
     *     int length();
     * };
     */
    
    class Solution {
    public:
        int findInMountainArray(int target, MountainArray &mountainArr) {
            int left=0,right=mountainArr.length()-1;
            while(left<right){
                int mid=left+(right-left)/2;
                if(mountainArr.get(mid)>mountainArr.get(mid+1))
                    right=mid;
                else if(mountainArr.get(mid)<mountainArr.get(mid+1))
                    left=mid+1;
                else{
                    // if (mountainArr.get(left)> mountainArr.get(right))
                    //     right--;
                    // else
                    //     left++; 
                }
            }
            int maxIndex=left;
            int res=binarySearch(target,mountainArr,0,maxIndex,1);
            if(res!=-1)
                return res;
            res=binarySearch(target,mountainArr,maxIndex+1,mountainArr.length()-1,-1);
            return res;
        }
    
        int binarySearch(int target, MountainArray &mountainArr,int left,int right,int sign){
            target=target*sign;
            while(left<=right){
                int mid=left+(right-left)/2;
                int curMid=mountainArr.get(mid)*sign;
                if(curMid==target)
                    return mid;
                else if(curMid>target)
                    right=mid-1;
                else if(curMid<target)
                    left=mid+1;
            }
            return -1;
        }
    };
    ```

    

### [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

- while内return，即对称的二分搜索

  - ```c++
    class Solution {
    public:
        int mySqrt(int x) {
            int left=0,right=x;
            if(x==0 || x==1)
                return x;
            while(left<=right){
                int mid=left+(right-left)/2;
                if((x/mid)<mid){
                    right=mid-1;
                }
                else if((x/mid)>mid){
                    left=mid+1;
                }
                else if((x/mid)==mid){
                    return mid;
                }
            }
            return right;//还是很大概率在这儿return的。自己判断到底是left还是right
        }
    };
    ```

- while外return。不对称的二分搜索

  - ```c++
    class Solution {
    public:
        int mySqrt(int x) {
            int left=0,right=x;
            while(left<right){
                int mid=left+(right-left)/2;
                if(mid==left)
                    break;
                if((x/mid)<mid){//等价于mid*mid >x，防止mid*mid溢出int范围
                    right=mid-1;
                }
                else if((x/mid)>mid){//等价于mid*mid<x
                    left=mid;
                }
                else if((x/mid)==mid){//等价于mid*mid==x
                    left=mid;
                    right=mid;
                }
            }
            //退出时还得判断一下，因为区间里还有俩数，到底是left还是right
            return (right*right)>x?left:right;
        }
    };
    ```

### 补充题：x的平方根：要求精确到1e-10

- 上一题的改编：对称二分查找模板

  - ```c++
    float mySqrt(float x){
        float left=0,right=x;
        while(!((right-left)<=1e-10 && (right-left)>=0)){//left不能超过right
            float mid=left+(right-left)/2;
            if(mid>(x/mid))
                right=mid;
            else if(mid<(x/mid))
                left=mid;
            else if(abs(mid-x/mid)<=1e-10){
                return mid;
            }
        }
        //根本不会在这儿return
        return -1;
    }
    ```
  
- 上一题的改编：不对称二分查找模板

  - ```c++
    float mySqrt(float x){
        float left=0,right=x;
        while(!((right-left)<=1e-10 && (right-left)>=0)){//left不能超过right
            float mid=left+(right-left)/2;
            if(mid>(x/mid))
                right=mid;
            else if(mid<(x/mid))
                left=mid;
            else if(abs(mid-x/mid)<=1e-10){
                left=mid;
                right=mid;
            }
        }
        //现在退出必然是right-left==0。不用判断left还是right了
        return left;
    }
    ```

### [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

- 跟补充题：求一个先升序后降序得数组的最大值一样

- 只是没有`nums[mid]==nums[mid+1]`的情况

  - ```c++
    class Solution {
    public:
        int findPeakElement(vector<int>& nums) {
            int left=0,right=nums.size()-1;
            while(left<right){
                int mid=left+(right-left)/2;
                if(nums[mid]>nums[mid+1])
                    right=mid;
                else if(nums[mid]<nums[mid+1])
                    left=mid+1;
                else if(nums[mid]==nums[mid+1]){
                    //if(nums[left]>nums[right])
                    //    right--;
                    //else 
                    //    left++;
                }
            }
            return left;
        }
    };
    ```


### [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

- 方法1：

  - 跟[240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)一样，当做二叉搜索树来做

  - ```c++
    class Solution {
    public:
        bool searchMatrix(vector<vector<int>>& matrix, int target) {
            int m=matrix.size();
            int n=matrix[0].size();
            int row=0,col=n-1;
            while(row<=m-1 && col>=0){
                if(matrix[row][col]==target)
                    return true;
                else if(matrix[row][col]>target)
                    col--;
                else if(matrix[row][col]<target)
                    row++;
            }
            return false;
        }
    };
    ```

- 方法2：

  - 两个二分查找

  - ```c++
    class Solution {
    public:
        bool searchMatrix(vector<vector<int>>& matrix, int target) {
            int left=0,right=matrix.size()-1;
            while(left<=right){
                int mid=left+(right-left)/2;
                if(matrix[mid][0]>target)
                    right=mid-1;
                else if(matrix[mid][0]<target)
                    left=mid+1;
                else
                    return true;
            }
            if(right<0)
                right=0;
            int row=right;
            left=0,right=matrix[0].size()-1;
    
            while(left<=right){
                int mid=left+(right-left)/2;
                if(matrix[row][mid]>target)
                    right=mid-1;
                else if(matrix[row][mid]<target)
                    left=mid+1;
                else
                    return true;
             }
             return false;
    
        }
    };
    ```

### [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

- 当做二叉搜索树来做

  - ```c++
    class Solution {
    public:
        bool searchMatrix(vector<vector<int>>& matrix, int target) {
            int m=matrix.size();
            int n=matrix[0].size();
            int row=0,col=n-1;
            while(row<=m-1 && col>=0){
                if(matrix[row][col]==target)
                    return true;
                else if(matrix[row][col]>target)
                    col--;
                else if(matrix[row][col]<target)
                    row++;
            }
            return false;
        }
    };
    ```

### [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

- 方法1：二分

  - O(NlogN)

  - ```c++
    class Solution {
    public:
        int findDuplicate(vector<int>& nums) {
            int left=0,right=nums.size()-1;
            while(left<=right){
                int mid=left+(right-left)/2;
                int cnt=0;
                for(int i:nums){
                    if(i<=mid)
                        cnt++;
                }
                if(cnt<=mid){//[2,2,2,2,2]。可能全是2
                    left=mid+1;
                }
                else if(cnt>mid)
                    right=mid-1;
            }
            return left;
        }
    };
    ```

- 方法二：

  - 当做环形链表来做

  - O(N)

  - ```c++
    class Solution {
    public:
        int findDuplicate(vector<int>& nums) {
            int slow=0,fast=0;
            while(1){
                slow=nums[slow];
                fast=nums[nums[fast]];
                if(slow==fast)
                    break;
            }
            int head=0;
            while(head!=slow){
                head=nums[head];
                slow=nums[slow];
            }
            return head;
        }
    };
    ```

- 方法三：

  - 位图。

  - ```c++
    class Solution {
    public:
        int findDuplicate(vector<int>& nums) {
            bitset<int(10e5+1)> myBitset(0);
            for(int i=0;i<nums.size();i++){
                if(myBitset[nums[i]]==1)
                    return nums[i];
                else
                    myBitset[nums[i]]=1;
            }
            return -1;
        }
    };
    ```

    

### [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

- 左右边界的二分搜索

  - ```c++
    class Solution {
    public:
        int search(vector<int>& nums, int target) {
            int leftIndex=getLeftIndex(nums,target);
            if(leftIndex==-1)
                return 0;
            int rightIndex=getRightIndex(nums,target);
            return rightIndex-leftIndex+1;
        }
    
        int getLeftIndex(vector<int>& nums,int target){
            int left=0,right=nums.size()-1;
            while(left<=right){
                int mid=left+(right-left)/2;
                if(nums[mid]==target)
                    right=mid-1;
                else if(nums[mid]>target)
                    right=mid-1;
                else if(nums[mid]<target)
                    left=mid+1;
            }
            if(left>=nums.size() || nums[left]!=target)
                return -1;
            return left;
        }
    
        int getRightIndex(vector<int>& nums,int target){
            int left=0,right=nums.size()-1;
            while(left<=right){
                int mid=left+(right-left)/2;
                if(nums[mid]==target)
                    left=mid+1;
                else if(nums[mid]>target)
                    right=mid-1;
                else if(nums[mid]<target)
                    left=mid+1;
            }
            if(right<0 || nums[right]!=target)
                return -1;
            return right;
        }
    };
    ```

### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

- 对称二分，自行判断退出时返回left还是right

  - ```c++
    class Solution {
    public:
        int missingNumber(vector<int>& nums) {
            int left=0,right=nums.size()-1;
            while(left<=right){
                int mid=left+(right-left)/2;
                if(nums[mid]==mid){
                    left=mid+1;
                }
                else if(nums[mid]>mid)
                    right=mid-1;
                //else if(nums[mid]<mid)   //不存在这种情况
            }
            return left;
        }
    };
    ```

### 补充题17. 两个有序数组第k小的数

- 递归二分

  - ```c++
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

- 自己写的版本

  - ```c++
    //整个思路是，一次递归淘汰k/2个数。每一轮循环可以将查找范围减少一半。
    //比如要找第4小的数，那就先淘汰2个，再淘汰1个，最后就变查找第一小的数了
    //其实右指针从来没动过，都是永远指向nums1.size()-1
    int findKth(vector<int>& nums1,vector<int>& nums2,int l1,int r1,int l2,int r2,int k){
        if(k>(r1-l1+1+r2-l2+1)) return -1;
        if(l1>r1) return nums2[l2+k-1];
        if(l2>r2) return nums1[l1+k-1];
        if(k==1) return min(nums1[l1],nums2[l2]);
        int nums1HalfK= r1-l1+1>=k/2?nums1[l1+k/2-1]:INT_MAX;//nums1的元素数目都不够k/2了，那就置为INT_MAX。以便淘汰另一数组的前k/2个数字
        int nums2HalfK= r2-l2+1>=k/2?nums2[l2+k/2-1]:INT_MAX;
        if(nums1HalfK<nums2HalfK)
            return findKth(nums1,nums2,l1+k/2,r1,l2,r2,k-k/2);
        else
            return findKth(nums1,nums2,l1,r1,l2+k/2,r2,k-k/2);
    }
    ```

    

### [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

- 方法1：双指针（写的有点繁琐了，建议看下边）

  - O(m+n)

  - ```c++
    class Solution {
    public:
        double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
            int ptr1=0,ptr2=0;
            int m=nums1.size();
            int n=nums2.size();
            if((m+n)%2==1){//奇数
                int cnt=0;
                double res;
                while(1){
                    if(ptr2>=n || (ptr1<m && ptr2<n && nums1[ptr1]<=nums2[ptr2])){
                        res=nums1[ptr1];
                        ptr1++;
                    }
                    else{
                        res=nums2[ptr2];
                        ptr2++;
                    }
                    cnt++;
                    if(cnt==(m+n)/2+1)//1 2 3 4 5；cnt到3时推出循环
                        break;
                }
                return res;//记录第3个数
            }
            else{//偶数
                int cnt=0;
                double res=0;
                int tmp;
                while(1){
     
                    if(ptr2>=n || (ptr1<m && ptr2<n && nums1[ptr1]<=nums2[ptr2])){
                        tmp=nums1[ptr1];
                        ptr1++;
                    }
                    else{
                        tmp=nums2[ptr2];
                        ptr2++;
                    }
                    cnt++;
                    if(cnt==(m+n)/2 || cnt==(m+n)/2+1)//记录第2个和第3个数
                        res+=double(tmp)/2;
                    if(cnt==(m+n)/2+1)//1,2,3,4;cnt到3时退出循环
                        break;
                }
                return res;
            }
            return 0;
        }
    };
    ```

- 更优雅的双指针写法

  - ```c++
    class Solution {
    public:
        double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
            int ptr1=0,ptr2=0;
            int m=nums1.size();
            int n=nums2.size();
    
            int cnt=0;//cnt代表第几个数（1,2,3..）
            double res=0;
            int tmp;
            while(1){//
                if(ptr2>=n || (ptr1<m && ptr2<n && nums1[ptr1]<=nums2[ptr2])){//ptr2跑光了
                    tmp=nums1[ptr1];
                    ptr1++;
                }
                else{
                    tmp=nums2[ptr2];
                    ptr2++;
                }
                cnt++;
                if(cnt==(m+n+1)/2 || cnt==(m+n+2)/2)
                    res+=tmp;
                if(cnt==((m+n+2)/2))
                    break;
            }
            if((m+n)%2==1)//奇数  
                return res;
            else//偶数
                return res/2;
        }
    };
    ```

- 方法2：二分查找

  - O(log(m+n))

  - https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/zong-he-bai-jia-ti-jie-zong-jie-zui-qing-xi-yi-don/

  - 使用一个小trick，可以避免讨论奇偶：

    - 我们分别找第 (m+n+1)/2个数，和(m+n+2)/2个数，然后求其平均值即可，这对奇偶数均适用。假如 m+n 为奇数的话，那么其实 (m+n+1) / 2 和 (m+n+2) / 2 的值相等，相当于两个相同的数字相加再除以2，还是其本身。

  - 查询一下第(m + n + 1) / 2小的数和第(m + n + 2) / 2小的数

    - 即：补充题17. 两个有序数组第k小的数

  - ```c++
    class Solution {
    public:
        double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
            int m=nums1.size();
            int n=nums2.size();
            int k1 = (m + n + 1) / 2;//从1开始数，第几个数
            int k2 = (m + n + 2) / 2;
            int res1=findKth(nums1,0,nums2,0,k1);
            int res2=findKth(nums1,0,nums2,0,k2);
            return double(res1+res2)/2.0;
        }
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
                return findKth(nums1, i,  nums2,j+k/2, k-k/2);
        }
    };
    ```

### 二分查找高效判定子序列：[392. 判断子序列](https://leetcode-cn.com/problems/is-subsequence/)

- 方法1：

  - 双指针就行

  - ```c++
    class Solution {
    public:
        bool isSubsequence(string s, string t) {
            int i=0;
            int j=0;
            while (i < s.size() && j < t.size()) {
                if (s[i] == t[j]) 
                    i++;
                j++;
            }
            return i==s.size();
        }
    };
    ```

- 方法2：

  - 当要比较很多s串时，而t只有1串，那么可以二分查找加速。
  - https://labuladong.gitbook.io/algo/mu-lu-ye-4/er-fen-cha-zhao-pan-ding-zi-xu-lie

## 8.数组-左右指针（类似二分，二分其实就是双指针的一种）

### [167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

- 类似二分，又不是

  - ```c++
    class Solution {
    public:
        vector<int> twoSum(vector<int>& numbers, int target) {
            int left=0,right=numbers.size()-1;
            while(left<right){
                int sum=numbers[left]+numbers[right];
                if (sum == target) 
                    return vector<int>{left+1,right+1};
                else if(sum>target)
                    right--;
                else if(sum<target)
                    left++;
            }
            return vector<int>{-1,-1}; 
        }
    };
    ```

### [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

- 双指针，esay题

  - ```c++
    class Solution {
    public:
        void reverseString(vector<char>& s) {
            int left=0,right=s.size()-1;
            while(left<right){
                char tmp=s[right];
                s[right]=s[left];
                s[left]=tmp;
                left++;
                right--;
            }
        }
    };
    ```

## 9.数组-滑动窗口

### [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

- 普通滑动窗

  - ```c++
    class Solution {
    public:
        string minWindow(string s, string t) {//t是target,s是source?
            unordered_map<char, int> need, window;//记录需要凑齐的字符和窗口中的字符：
            for (char c : t) need[c]++;
            int left = 0, right = 0;//区间左闭右开，所以初始情况下窗口没有包含任何元素
            int valid = 0; //其中 valid 变量表示窗口中满足 need 条件的字符个数，如果 valid 和 need.size 的大小相同，则说明窗口已满足条件，已经完全覆盖了串 T。
            // 记录最小覆盖子串的起始索引及长度
            int start = 0, len = INT_MAX;
            while (right < s.size()) {
            // 开始滑动
                char c = s[right];// c 是将移入窗口的字符
                right++;// 右移窗口
                if(need.count(c)==1){//如果是有效字符
                    window[c]++;
                    if(window[c]==need[c])
                        valid++;
                }
    
                while (valid == need.size()) {// 判断左侧窗口是否要收缩
                    // 在这里更新最小覆盖子串
                    if (right - left < len) {
                        start = left;
                        len = right - left;
                    }
                    char d = s[left];// d 是将移出窗口的字符
                    left++;//左移窗口
                    if(need.count(d)==1){//如果是有效字符
                        if(window[d]==need[d])
                            valid--;
                        window[d]--;          
                    }
                }
            }
            return len==INT_MAX?"":s.substr(start,len);
    
        }
    };
    ```

### [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

- 普通滑动窗

  - ```c++
    class Solution {
    public:
        bool checkInclusion(string s1, string s2) {
            unordered_map<char,int> need,window;
            for(char c:s1) need[c]++;
            int left=0,right=0;
            int valid=0;
            while(right<s2.size()){
                char c=s2[right];
                right++;
                if(need.count(c)==1){
                    window[c]++;
                    if(window[c]==need[c]){
                        valid++;
                    }
                }
                while(right-left>=s1.size()){//本题移动 left 缩小窗口的时机是窗口大小大于 t.size() 时，应为排列嘛，显然长度应该是一样的。
                    
                    if(valid==need.size())//当发现 valid == need.size() 时，就说明窗口中就是一个合法的排列，所以立即返回 true。
                        return true;
                    
                    char d = s2[left];// d 是将移出窗口的字符
                    left++;//左移窗口
                    if(need.count(d)==1){//如果是有效字符
                        if(window[d]==need[d])
                            valid--;
                        window[d]--;          
                    }
                }
            }
            return false;
        }
    };
    ```

### [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

- 普通滑动窗

  - ```c++
    class Solution {
    public:
        vector<int> findAnagrams(string s, string p) {
            unordered_map<char,int> need,window;
            for(char c:p) need[c]++;
            int left=0,right=0;
            int valid=0;
            vector<int> res;
            while(right<s.size()){
                char c=s[right];
                right++;
    
                if(need.count(c)){
                    window[c]++;
                    if(window[c]==need[c])
                        valid++;
                }
    
                while(right-left>=p.size()){//等于的时候一定要进入，因为要进行判断加不加入res
                    if(valid==need.size()){
                        res.push_back(left);
                    }
    
                    char d=s[left];
                    left++;
                    if(need.count(d)){
                        if(window[d]==need[d])
                            valid--;
                        window[d]--;
                    }
                }
    
            }
            return res;
        }
    };
    ```

### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)、[剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

- 普通滑动窗的改编

  - 更新结果的位置换地方了

  - ```c++
    class Solution {
    public:
        int lengthOfLongestSubstring(string s) {
            unordered_map<char,int> window;
            int left=0,right=0;
            int res=0;
            while(right<s.size()){
                char c=s[right];
                right++;
                if(window[c]==0)//不要用count，因为--到0了，依然count为1
                    res=max(right-left,res);
    
                window[c]++;
    
                while(window[c]>1){
                    char d=s[left];
                    left++;
                    window[d]--;
                }
                //在这里更新res其实也可以 res=max(res,right-left);
            }
    	
            return res;
        }
    };
    ```

### [剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

- 使用单调队列

  - ```c++
    class MaxQueue {
    public:
        MaxQueue() {
    
        }
        deque<int> q;//普通队列
        deque<int> helper;//单调队列
        int max_value() {
            if(helper.empty())
                return -1;
            return helper.front();
        }
        
        void push_back(int value) {
            q.push_back(value);
            while(!helper.empty() && helper.back()<value)//单调队列的push_back操作
                helper.pop_back();
            helper.push_back(value);
        }
        
        int pop_front() {
            if(q.empty())
                return -1;
            int res=q.front();
            q.pop_front();
            if(!helper.empty()&&helper.front()==res)//单调队列的pop_front()操作
                helper.pop_front();
            return res;
        }
    };
    
    /**
     * Your MaxQueue object will be instantiated and called as such:
     * MaxQueue* obj = new MaxQueue();
     * int param_1 = obj->max_value();
     * obj->push_back(value);
     * int param_3 = obj->pop_front();
     */
    ```

### [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

- 滑动窗口+暴力法

  - 使用multiset，每轮暴力找max

  - ```c++
    class Solution {
    public:
        vector<int> maxSlidingWindow(vector<int>& nums, int k) {
            vector<int> res;
            multiset<int,greater<int>> myset;
            int left=0,right=0;
            while(right<nums.size()){
                int cur=nums[right];
                myset.insert(cur);
                right++;
    
                while(right-left>=k){
                    res.push_back(*myset.begin());
                    int del=nums[left];
                    myset.erase(myset.find(del));
                    left++;
                }
            }
            return res;
        }
    };
    ```

- 滑动窗口+大顶堆

  - O(nlogn)

  - 比multiset好在，不用每轮都erase。erase比较耗时

  - ```c++
    class Solution {
    public:
        vector<int> maxSlidingWindow(vector<int>& nums, int k) {
            vector<int> res;
    
            priority_queue<pair<int, int>>q;
            int left=0,right=0;
            while(right<nums.size()){
                int cur=nums[right];
                q.push({cur,-1*right});//乘个-1是为了second按下标小的排前边
                right++;
    
                while(right-left>=k){
                    while((-1*q.top().second)>left){
                        q.pop();
                    }
                    res.push_back(q.top().first);
                    left++;
                }
            }
    
            return res;
        }
    };
    ```

- 滑动窗口+单调队列

  - O(n)

  - ```c++
    class Solution {
    public:
        vector<int> maxSlidingWindow(vector<int>& nums, int k) {
            vector<int> res;
            deque<int> q;
            int left=0,right=0;
            while(right<nums.size()){
                int cur=nums[right];
                while(!q.empty() && q.back()<cur)//单调队列的操作。你可以想象，加入数字的大小代表人的体重，把前面体重不足的都压扁了，直到遇到更大的量级才停住。
                    q.pop_back();
                q.push_back(cur);
                right++;
    
                while(right-left>=k){
                    res.push_back(q.front());
                    int del=nums[left];
                    if(!q.empty() &&q.front()==del)//之所以要判断 data.front() == n，是因为我们想删除的队头元素 n 可能已经被「压扁」了，这时候就不用删除了：
                        q.pop_front();
                    left++;
                }
            }
    
            return res;
        }
    };
    ```

- 有的读者可能觉得「单调队列」和「优先级队列」比较像，实际上差别很大的。

  单调队列在添加元素的时候靠删除元素保持队列的单调性，相当于抽取出某个函数中单调递增（或递减）的部分；而优先级队列（二叉堆）相当于自动排序，差别大了去了。

- 使用单调队列算法整体的复杂度依然是 O(N)O(N) 线性时间。要这样想，nums 中的每个元素最多被 push_back 和 pop_back 一次，没有任何多余操作，所以整体的复杂度还是 O(N)。

- 使用优先队列的复杂度：比单调队列复杂在，虽然也是每个元素最多被 push_back 和 pop_back 一次，但是有多余的操作啊！每次push还需要维护二叉堆的有序性，所以复杂度就是O(nlogn)了

### [480. 滑动窗口中位数](https://leetcode-cn.com/problems/sliding-window-median/)

- 暴力法

  - 使用mutiset，就是每轮找中位数

  - ```c++
    class Solution {
    public:
        vector<double> medianSlidingWindow(vector<int>& nums, int k) {
            vector<double> res;
            multiset<double> myset;
            int left=0,right=0;
            while(right<nums.size()){
                int cur=nums[right];
                myset.insert(cur);
                right++;
    
                while(right-left>=k){
                    auto mid1 = myset.begin();
                    std::advance(mid1, (k+1) / 2-1);
                    auto mid2=myset.begin();
                    std::advance(mid2,(k+2)/2-1);
                    res.push_back((*mid1+*mid2)/2);
                    double del=nums[left];
                    myset.erase(myset.find(del));
                    left++;
    
                }
            }
            return res;
        }
    };
    ```

- 大顶堆+小顶堆

  - 不会

### [1438. 绝对差不超过限制的最长连续子数组](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

- multiset暴力法

  - ```c++
    class Solution {
    public:
        int longestSubarray(vector<int>& nums, int limit) {
            int res=INT_MIN;
            multiset<int,greater<int>> myset;//降序排列
            int left=0,right=0;
            while(right<nums.size()){
                int cur=nums[right];
                myset.insert(cur);
                right++;
                int curMax=*myset.begin();
                int curMin=*prev(myset.end());
                while(curMax-curMin>limit){
                    int del=nums[left];
                    myset.erase(myset.find(del));
                    left++;
                    curMax=*myset.begin();
                    curMin=*prev(myset.end());
                }
                //这个位置一定是满足要求的
                res=max(res,right-left);
            }
            return res;
        }
    };
    ```

- 大小顶堆（两个优先队列）

  - 略。不好

- 两个单调队列

  - ```c++
    class Solution {
    public:
        int longestSubarray(vector<int>& nums, int limit) {
            int res=INT_MIN;
            deque<int> q_Max;//单向队列记录最大值
            deque<int> q_Min;//单向队列记录最小值
            int left=0,right=0;
            while(right<nums.size()){
                int cur=nums[right];
                while(!q_Max.empty() && q_Max.back()<cur)
                    q_Max.pop_back();
                q_Max.push_back(cur);
                while(!q_Min.empty() && q_Min.back()>cur)
                    q_Min.pop_back();
                q_Min.push_back(cur);
                right++;
    
                while(q_Max.front()-q_Min.front()>limit){
                    int del=nums[left];
                    if(!q_Max.empty() && q_Max.front()==del)
                        q_Max.pop_front();
                    if(!q_Min.empty() && q_Min.front()==del)
                        q_Min.pop_front();
                    left++;
                }
                res=max(res,right-left);
            }
            return res;
        }
    };
    ```

### [1498. 满足条件的子序列数目](https://leetcode-cn.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/)

- 排序+双指针（也可以叫做滑动窗）

  - 这里边的pow避免溢出的思想非常重要

  - 主要就是取模运算的

  - ```c++
    class Solution {
    public:
        int numSubseq(vector<int>& nums, int target) {
         sort(nums.begin(),nums.end(),less<int>());
            int left=0;
            int right=nums.size()-1;
            int cnt=0;
    
            vector<int> powRecord(nums.size());
            powRecord[0] = 1;
            int mode = 1e9+7;
            for (int i = 1; i < nums.size(); i ++) {
                powRecord[i] = powRecord[i-1] * 2;
                powRecord[i] %= mode;
            }
    
            while(left<=right){
                if(nums[left]+nums[right]>target){
                    right--;
                }
                else{
                    cnt+=powRecord[right-left];
                    cnt %= mode;
                    left++;
                }
            }
            return cnt;
        }
    };
    ```

### [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

- 滑动窗+暴力法

  - ```c++
    class Solution {
    public:
        int characterReplacement(string s, int k) {
            int res=0;
            vector<int> window(26,0);
            int left=0,right=0;
            while(right<s.size()){
                char c=s[right];
                right++;
                window[c-'A']++;
                int maxNums=*max_element(window.begin(), window.end());
                int otherNums=right-left-maxNums;
                while(otherNums>k){
                    char d=s[left];
                    window[d-'A']--;
                    left++;
                    maxNums=*max_element(window.begin(), window.end());
                    otherNums=right-left-maxNums;
                }
                res=max(res,right-left);
            }
            return res;
        }
    };
    ```

- 更巧妙的方法：滑动窗+维护历史最大窗口

  - ```c++
    class Solution {
    public:
        int characterReplacement(string s, int k) {
            int res=0;
            vector<int> window(26,0);
            int left=0,right=0;
            int maxNums=0;
            while(right<s.size()){
                char c=s[right];   
                window[c-'A']++;
                maxNums=max(maxNums,window[c-'A']);//虽然维护的是历史最大值，尽管历史最大值所对应的字母不一定全都出现在当前窗口里
                //维护历史最大值的原因：窗口从根本来说是只增不减的：
                //如果k够用来替换的话，right右移1步，窗口变大（当前维护的最大值变大，不消耗k；当前维护的最大值不变，将消耗k）
                //如果k不够用来替换的话，left和right都右移一步，窗口不变（我们已经不关心比当前小的情况了）。
                //从这个角度来说，也不难理解最后答案是right-left（最后窗口的大小，可扩充的最大窗口）。
                right++;
                int otherNums=right-left-maxNums;
                if(otherNums>k){//不是while!因为窗口是只增不减的
                    char d=s[left];
                    window[d-'A']--;
                    left++;
                }
                res=max(res,right-left);
            }
            return res;
        }
    };
    ```

## 10.数组-去重算法

### [316. 去除重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/)、[1081. 不同字符的最小子序列](https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters/)

- 单调栈

  - ```c++
    class Solution {
    public:
        string removeDuplicateLetters(string s) {
            //单调栈保证了字典序
            stack<char> stk;//「栈」保证了字符出现的顺序和s中出现的顺序一致。
            vector<int> count(26,0);
            //我们用了一个计数器count，当字典序较小的字符试图「挤掉」栈顶元素的时候，在count中检查栈顶元素是否是唯一的，只有当后面还存在栈顶元素的时候才能挤掉，否则不能挤掉。
            for(auto i:s)
                count[i-'a']++;
            vector<bool> inStack(26,false);//保证栈stk中不存在重复元素。
    
            for(int i=0;i<s.size();i++){
                char c=s[i];
                if(!inStack[c-'a']){
                    while(!stk.empty() && stk.top()>c && count[stk.top()-'a']>0){//单调栈
                        inStack[stk.top()-'a']=false;
                        stk.pop();
                    }
                    stk.push(c);
                    inStack[c-'a']=true;
                }
                count[c-'a']--;
            }
    
            string res;
            while(!stk.empty()){
                res.push_back(stk.top());
                stk.pop();
            }
            reverse(res.begin(),res.end());
            return res;
        }
    };
    ```

## 11.单调队列（常用于滑动窗口）

### [1438. 绝对差不超过限制的最长连续子数组](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

- 滑动窗+两个单调队列

  - ```c++
    class Solution {
    public:
        int longestSubarray(vector<int>& nums, int limit) {
            int res=INT_MIN;
            deque<int> q_Max;//单向队列记录最大值
            deque<int> q_Min;//单向队列记录最小值
            int left=0,right=0;
            while(right<nums.size()){
                int cur=nums[right];
                while(!q_Max.empty() && q_Max.back()<cur)
                    q_Max.pop_back();
                q_Max.push_back(cur);
                while(!q_Min.empty() && q_Min.back()>cur)
                    q_Min.pop_back();
                q_Min.push_back(cur);
                right++;
    
                while(q_Max.front()-q_Min.front()>limit){
                    int del=nums[left];
                    if(!q_Max.empty() && q_Max.front()==del)
                        q_Max.pop_front();
                    if(!q_Min.empty() && q_Min.front()==del)
                        q_Min.pop_front();
                    left++;
                }
                res=max(res,right-left);
            }
            return res;
        }
    };
    ```

### [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

- 滑动窗+单调队列

  - ```c++
    class Solution {
    public:
        vector<int> maxSlidingWindow(vector<int>& nums, int k) {
            vector<int> res;
            deque<int> q;
            int left=0,right=0;
            while(right<nums.size()){
                int cur=nums[right];
                while(!q.empty() && q.back()<cur)//单调队列的操作。你可以想象，加入数字的大小代表人的体重，把前面体重不足的都压扁了，直到遇到更大的量级才停住。
                    q.pop_back();
                q.push_back(cur);
                right++;
    
                while(right-left>=k){
                    res.push_back(q.front());
                    int del=nums[left];
                    if(!q.empty() &&q.front()==del)//之所以要判断 data.front() == n，是因为我们想删除的队头元素 n 可能已经被「压扁」了，这时候就不用删除了：
                        q.pop_front();
                    left++;
                }
            }
    
            return res;
        }
    };
    ```

    

## 12.单调栈

### [496. 下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

- 单调栈

  - 栈里存的是val

  - ```c++
    class Solution {
    public:
        vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
            stack<int> stk;
            unordered_map<int,int> mymap;
            // vector<int> greater(nums2.size(),-1);
            for(int i=nums2.size()-1;i>=0;i--){
                int cur=nums2[i];
                while(!stk.empty() && stk.top()<=cur)
                    stk.pop();
                // greater[i]=stk.empty()?-1:stk.top();
                mymap[cur]=stk.empty()?-1:stk.top();
                stk.push(cur);
            }
            vector<int> res;
            for(auto i:nums1)
                res.push_back(mymap[i]);
            return res;
    
        }
    };
    ```

### [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

- 单调栈

  - 栈里存的是index

  - ```c++
    class Solution {
    public:
        vector<int> dailyTemperatures(vector<int>& temperatures) {
            stack<int> stk;
            vector<int> res(temperatures.size(),0);
            for(int i=temperatures.size()-1;i>=0;i--){
                int cur=temperatures[i];
                while(!stk.empty() && temperatures[stk.top()]<=cur)
                    stk.pop();
                res[i]=stk.empty()?0:(stk.top()-i);
                stk.push(i);
            }
            return res;
            
        }
    };
    ```

### [503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

- 单调栈+循环数组

  - 数组扩大一倍（假装）来解决循环数组的问题。利用取余操作来假装

  - 栈里存的是val

  - ```c++
    class Solution {
    public:
        vector<int> nextGreaterElements(vector<int>& nums) {
            stack<int> stk;
            vector<int> res(nums.size(),-1);
            // 假装这个数组长度翻倍了
            for(int i=2*nums.size()-1;i>=0;i--){
                int realIndex=i%nums.size();
                int cur=nums[realIndex];
                while(!stk.empty() && stk.top()<=cur){
                    stk.pop();
                }
                if(i<=nums.size()-1)
                    res[i]=stk.empty()?-1:stk.top();
                stk.push(cur);
            }
            return res;
        }
    };
    ```


## 13.数组-原地修改数组

### [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

- 快慢指针

  - ```c++
    class Solution {
    public:
        int removeDuplicates(vector<int>& nums) {
            if(nums.size()==0)
                return 0;
            int slow=0,fast=0;
            while(fast<nums.size()){     
                if(nums[fast]!=nums[slow]){
                    slow++;
                    nums[slow]=nums[fast];
                }  
                fast++;
            }
            return slow+1;
        }
    };
    ```

### [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

- 快慢指针

  - 反正也是快慢指针，就是跟上一题写法不太一样。因为链表需要delete数据

  - ```c++
    class Solution {
    public:
        ListNode* deleteDuplicates(ListNode* head) {
            if(head==nullptr)
                return nullptr;
            ListNode* slow=head;
            ListNode* fast=head->next;
            while(fast!=nullptr){
                if(fast->val==slow->val){//值相等
                    ListNode* tmp=fast;
                    fast=fast->next;
                    slow->next=fast;
                    delete tmp;
                }
                else{//值不等
                    slow->next=fast;
                    slow=slow->next;
                    fast=fast->next;
                }  
            }
            return head;
        }
    };
    ```

### [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

- 快慢指针

  - 快慢指针的题，slow fast，需要自己来想。没必要跟滑动窗保持一致

  - ```c++
    class Solution {
    public:
        int removeElement(vector<int>& nums, int val) {
            int slow=0,fast=0;
            while(fast<nums.size()){
                if(nums[fast]!=val){
                    nums[slow]=nums[fast];
                    slow++;
                }
                fast++;       
            }
            return slow;
        }
    };
    ```

### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

- 快慢指针

  - 相当于删除所有的0。再把剩下的数组位置全填成0

  - ```c++
    class Solution {
    public:
        void moveZeroes(vector<int>& nums) {
            int slow=0,fast=0;
            while(fast<nums.size()){
                if(nums[fast]!=0){
                    nums[slow]=nums[fast];
                    slow++;
                }
                fast++;
            }
            while(slow<nums.size()){
                nums[slow]=0;
                slow++;
            }
        }
    };
    ```
## 14.数组-twoSum问题

### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

- hashmap

  - O(N)时间、O(N)空间

  - ```c++
    class Solution {
    public:
        vector<int> twoSum(vector<int>& nums, int target) {
            unordered_map<int,int> map;
            for(int i=0;i<nums.size();i++){
                map[nums[i]]=i;
            }
            for(int i=0;i<nums.size();i++){
                int cur=nums[i];
                int want=target-cur;
                if(map.count(want)!=0 && map[want]!=i)
                    return vector<int>{i,map[want]};
            }
            return vector<int>{-1,-1};
        }
    };
    ```


## 15.数组-nSum问题

### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

- 在2数之和的基础上改编

  - ```c++
    class Solution {
    public:
        vector<vector<int>> threeSum(vector<int>& nums) {
            return twoSumTarget(nums,0);
        }
        
        vector<vector<int>> twoSumTarget(vector<int>& nums, int target){
            sort(nums.begin(),nums.end());
            vector<vector<int>> res;
            
            // 穷举 threeSum 的第一个数
            for(int i=0;i<nums.size();){
                int cur=nums[i];
                vector<vector<int>> tuples= twoSumTarget(nums,target-cur,i+1);
                for(auto vec:tuples){//tuples为空就不会进入for循环
                    vec.push_back(cur);
                    res.push_back(vec);
                }
                while(i<nums.size() && nums[i]==cur)// 跳过第一个数字重复的情况，否则会出现重复结果
                    i++;
            }
            return res;
        }
    
        vector<vector<int>> twoSumTarget(vector<int>& nums, int target,int start){
            //排序挪到主函数里了
            vector<vector<int>> res;
            int left=start,right=nums.size()-1;
            while(left<right){
                int curLeft=nums[left];
                int curRight=nums[right];
                int sum=curLeft+curRight;
                if(sum<target){
                    while(left<right && nums[left]==curLeft)
                        left++;
                }
                else if(sum>target){
                    while(left<right && nums[right]==curRight)
                        right--;
                }
                else{
                    res.push_back({nums[left],nums[right]});
                    
                    while(left<right && nums[left]==curLeft)//去重
                        left++;
                    while(left<right && nums[right]==curRight)
                        right--;
                }
            }
            return res;
    
        }
    };
    ```

- nSum递归模板

  - ```c++
    class Solution {
    public:
        vector<vector<int>> threeSum(vector<int>& nums) {
            sort(nums.begin(),nums.end());
            return nSumTarget(nums,3,0,0);
        }
    
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
    };
    ```

    

### [18. 四数之和](https://leetcode-cn.com/problems/4sum/)

- nSum递归模板

  - ```c++
    class Solution {
    public:
        vector<vector<int>> fourSum(vector<int>& nums, int target) {
            sort(nums.begin(),nums.end());
            return nSumTarget(nums,4,0,target);
        }
    
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
    };
    ```


## 16.动态规划

### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

- 动规

  - O(n*k)  n是amount，k是coins数目

  - ```c++
    class Solution {
    public:
        int coinChange(vector<int>& coins, int amount) {
            vector<int> dp(amount + 1,amount + 1);//数组里所有的数都是这个值
            //为啥 dp 数组初始化为 amount + 1 呢，因为凑成 amount 金额的硬币数最多只可能等于 amount（全用 1 元面值的硬币），所以初始化为 amount + 1 就相当于初始化为正无穷，便于后续取最小值。
            
            //base case
            dp[0]=0;
            // 外层 for 循环在遍历所有状态的所有取值
            for(int i = 0; i < dp.size(); i++){
                // 内层 for 循环在求所有选择的最小值
                for(auto coin:coins){//当前选择，选哪块儿硬币   
                    if (i - coin < 0) continue;// 子问题无解，跳过
                    dp[i] = min(dp[i], 1 + dp[i - coin]);//dp(11)=min{dp(10)+1,dp(9)+1,dp(6)+1}
                }
            }
            return dp[amount]==amount+1?-1:dp[amount];
        }
    };
    ```

- 换种写法

  - ```c++
    class Solution {
    public:
        int coinChange(vector<int>& coins, int amount) {
            vector<int> dp(amount+1,0);
            dp[0]=0;
            for(int i=1;i<=amount;i++){
                int tmp_min=amount+1;//取最大值
                for(int coin:coins){
                    if(i<coin)
                        continue;
                    tmp_min=min(tmp_min,dp[i-coin]);    //min{dp(10),dp(9),dp(6)}
                }
                dp[i]=tmp_min+1;//最小值加1
            }
            return dp[amount]>=amount+1?-1:dp[amount];
        }
    };
    ```


### [931. 下降路径最小和](https://leetcode-cn.com/problems/minimum-falling-path-sum/)

- 普通迭代动规

  - 扩张一下数组，以避免边界问题

  - ```c++
    class Solution {
    public:
        int minFallingPathSum(vector<vector<int>>& matrix) {
            int m=matrix.size();
            int n=matrix[0].size();
            vector<int> dp(n+2,0);//n+2。外围直接扩两个数，避免边界问题
            dp[0]=INT_MAX;
            dp[n+1]=INT_MAX;
            for(int j=1;j<=n;j++)
                dp[j]=matrix[0][j-1];//注意操作matrix时是j-1。因为dp有n+2列，而matrix只有n列
    
            vector<int> nextRow(n+2,0);
            nextRow[0]=INT_MAX;
            nextRow[n+1]=INT_MAX;
            for(int i=1;i<m;i++){
                for(int j=1;j<=n;j++){
                    nextRow[j]=min(min(dp[j-1],dp[j]),dp[j+1])+matrix[i][j-1];
                }
                dp=nextRow;
            }
            
            int res=INT_MAX;
            for(auto i:dp){
                res=min(res,i);
            }
            return res;
        }
    
    };
    ```

- 带备忘录的递归回溯（其实就是动规的另一种形式）：又称为：动态规划（记忆化搜索）

  - 不带备忘录的回溯的话，每次调用dp(matrix,i,j)都要反复重新递归，会超时的。

    - 会多次重复的调用同一个dp(matrix,i,j)，这就是存在重叠子问题。
    - 用备忘录的方法消除重叠子问题。将 `dp(matrix, i, j)` 的计算结果存进 `memo[i][j]`

  - 备忘录记录的值跟迭代动归的dp矩阵记录的值是一个意思

  - ```c++
    class Solution {
    public:
        int minFallingPathSum(vector<vector<int>>& matrix) {
            m=matrix.size();
            n = matrix[0].size();
            memo=vector<vector<int>>(m,vector<int>(n,INT_MAX));
            int res = INT_MAX;
    
            // 终点可能在最后一行的任意一列
            for (int j = 0; j < n; j++) {
                res =min(res,dp(matrix,m-1,j));
            }
    
            return res;
        }
        int m,n;
        vector<vector<int>> memo;
        int dp(vector<vector<int>>& matrix,int i,int j){
               // 非法索引检查
            if (i < 0 || j < 0 || i >= m || j >= n) {
                // 返回一个特殊值
                return INT_MAX;
            }
    
            // base case
            if (i == 0) {
                return matrix[i][j];
            }
    
            // 3、查找备忘录，防止重复计算
            if (memo[i][j] != INT_MAX) {
                return memo[i][j];
            }
            // 状态转移
            memo[i][j]= matrix[i][j] + min(min(dp(matrix, i - 1, j), dp(matrix, i - 1, j - 1)),dp(matrix, i - 1, j + 1));
            return memo[i][j];
        }
    };
    ```

### [494. 目标和](https://leetcode-cn.com/problems/target-sum/)

- 方法1：回溯

  - 使用模板一，每个目标选择加入或者不加入

  - 树的高度就是 `nums` 的长度嘛，所以说时间复杂度就是这棵二叉树的节点数，为 `O(2^N)`

  - ```c++
    class Solution {
    public:
        int findTargetSumWays(vector<int>& nums, int target) {
            res=0;
            this->target=target;
            dfs(nums,0,0);
            return res;
        }
        int target;
        int res;
        void dfs(vector<int>& nums,int sum,int index){
            if(index==nums.size()){
                if(sum==target){
                    res++;
                }
                return;
            }
            //相当于把两种情况的For循环拆开了，写成了无For的形式。就两种选择，这个节点加入或者是不加入
            sum+=nums[index];//做选择
            dfs(nums,sum,index+1);
            sum-=2*nums[index];//撤销
            dfs(nums,sum,index+1);//做选择
        }
    };
    ```

  - 回溯的另一种写法：

    - 其实跟上边一样。只是一个以返回值的形式，一个以return int的形式。但是以这个return int形式才可以使用备忘录剪枝，消除子问题

    - ```c++
      class Solution {
      public:
          int findTargetSumWays(vector<int>& nums, int target) {
              this->target=target;
              return dfs(nums,0,0);
          }
          int target;
          int dfs(vector<int>& nums,int sum,int index){
              if(index==nums.size()){
                  if(sum==target){
                      return 1;
                  }
                  return 0;
              }
              //相当于把两种情况的For循环拆开了，写成了无For的形式。就两种选择，这个节点加入或者是不加入
              return dfs(nums,sum+nums[index],index+1)+dfs(nums,sum-nums[index],index+1);
      
          }
      };
      ```

      

- 方法2：

  - 带备忘录的递归回溯（dp）又称为：动态规划（记忆化搜索）

  - 比如在不同的选择下，得到了一样的sum，出现了两个「状态」完全相同的递归函数，无疑这样的递归计算就是重复的。**只要我们能够找到一个重叠子问题，那一定还存在很多的重叠子问题**。

  - 暴力回溯中 `dfs(nums,sum,index+1); `中的状态`(sum,index+1)`是可以用备忘录技巧进行优化的。因为它存在重复多次调用的问题。同一状态，可能被调用了好多好多遍。

  - 时间复杂度：O(n*∑abs(nums[i]))，空间复杂度`O(n*∑abs(nums[i]))`

  - ```c++
    class Solution {
    public:
        int findTargetSumWays(vector<int>& nums, int target) {
            this->target=target;
            return dp(nums,0,0);
        }
        int target;
        unordered_map<string,int> memo;
        int dp(vector<int>& nums,int sum,int index){//代表考虑前 index 个数，当前计算结果为 sum 的可行方案数  (memo["sum","index"])
            if(index==nums.size()){
                if(sum==target){
                    return 1;
                }
                return 0;
            }
            // 把它俩转成字符串才能作为哈希表的键
            string key = to_string(sum) + "," + to_string(index);
            // 避免重复计算
            if (memo.count(key)) {
                return memo[key];
            }
            //相当于把两种情况的For循环拆开了，写成了无For的形式。就两种选择，这个节点加入或者是不加入
            int result= dp(nums,sum+nums[index],index+1)+dp(nums,sum-nums[index],index+1);
            memo[key]=result;
            return result;
        }
    };
    ```

- 方法3：动态规划-递推

  - `dp[index][sum]`定义为到第Index个数为止（index已选择），有可能构成的每个sum的个数

  - 时间复杂度：O(n*∑abs(nums[i]))，空间复杂度`O(n*∑abs(nums[i]))`

  - ```c++
    class Solution {
    public:
        int findTargetSumWays(vector<int>& nums, int target) {
            int sum = 0;
            for (int i = 0; i < nums.size(); i++) {
                sum += abs(nums[i]);
            }
            // 绝对值范围超过了sum的绝对值范围则无法得到
            if (abs(target) > sum) return 0;
            int m = nums.size();
            // - 0 +
            int n = sum * 2 + 1;
            vector<vector<int>> dp(m,vector<int>(n,0));// dp[index][sum]  index已经选择过了
            // 初始化
            if (nums[0] == 0) {
                dp[0][sum] = 2;
            } else {
                dp[0][sum + nums[0]] = 1;
                dp[0][sum - nums[0]] = 1;
            }
    
            for(int i=1;i<m;i++){
                for(int j=0;j<n;j++){
                    int l=j-nums[i]<0?0:j-nums[i];
                    int r=j+nums[i]>=n?n-1:j+nums[i];
                    dp[i][j]=dp[i-1][l]+dp[i-1][r];
                }
            }
    
            return dp[m-1][target+sum];
        }
    };
    ```

- 方法4：动态规划-优化-01背包

## 17.动态规划-子序列问题

### -----模板一-----

### [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

- 迭代动规

  - dp[i] 表示**以 nums[i] 这个数结尾的**最长递增子序列的长度。dp[0]=1，basecase

  - <img src="https://gblobscdn.gitbook.com/assets%2F-Mc28-yQjBQGNRby1JtX%2Fsync%2F4490d13fdde822af5c3752f86ea78e2577583546.jpeg?alt=media" alt="img" style="zoom:50%;" />
  
  - ```c++
    class Solution {
    public:
        int lengthOfLIS(vector<int>& nums) {
            vector<int> dp(nums.size(),0);
            dp[0]=1;
            for(int i=0;i<dp.size();i++){
                int maxVal=0;
                for(int j=0;j<i;j++){
                    if(nums[j]<nums[i])//在所有比第i值小的数里，找最大的
                        maxVal=max(maxVal,dp[j]);
                }
                dp[i]=maxVal+1;
            }
            int res=0;
            for(auto i:dp)
                res=max(i,res);
            return res;
        }
    };
    ```

- 如果需要输出路径

  - 整一个record数组记录一下

  - ```c++
    class Solution {
    public:
        int maxSubArray(vector<int>& nums) {
            vector<int> dp(nums.size());
            vector<vector<int>> record(nums.size());
            record[0].push_back(nums[0]);
            dp[0]=nums[0];
            for(int i=1;i<dp.size();i++){
                if(dp[i-1]>=0){
                    dp[i]=dp[i-1]+nums[i];
                    record[i]=record[i-1];
                    record[i].push_back(nums[i]);
                }
                else{
                    dp[i]=nums[i];
                    record[i].push_back(nums[i]);
                }
            }
    
            int res=INT_MIN;
            int index=0;
            for(int i=0;i<dp.size();i++){
                if(dp[i]>res){
                    res=dp[i];
                    index=i;
                }
            }
            for(auto i:record[index])
                cout<<i<<" ";
            cout<<endl;
            return res;
        }
    };
    ```

    

### [354. 俄罗斯套娃信封问题](https://leetcode-cn.com/problems/russian-doll-envelopes/)

- 二维问题，把第一维升序排列后，对第二维进行dp

  - 退化成了最长递增子序列问题，只不过要多加一个if判断

  - <img src="https://gblobscdn.gitbook.com/assets%2F-Mc28-yQjBQGNRby1JtX%2Fsync%2Fdf0cc5fb3a72c69e2c0493d1900715a513dcd7b5.jpg?alt=media" alt="img" style="zoom:50%;" />

    - 第二维可以不进行排序。多一个if判断就行了（即`envelopes[i][0]>envelopes[j`][0]第一维不能相等）

  - **dp[i] 表示以 envelopes`[i][1]` 这个数结尾的**最长递增子序列的长度。dp[0]=1，basecase

  - ```c++
    class Solution {
    public:
        int maxEnvelopes(vector<vector<int>>& envelopes) {
            sort(envelopes.begin(),envelopes.end(),
                [](vector<int> a,vector<int> b){return a[0]<b[0];});//按照第一维进行升序排列
            vector<int> dp(envelopes.size());
            dp[0]=1;
            for(int i=1;i<dp.size();i++){
                int maxVal=0;
                for(int j=0;j<i;j++){
                    if(envelopes[i][1]>envelopes[j][1] && envelopes[i][0]>envelopes[j][0])//在所有比第i值小的数里，找最大的。另外，第																							一维不能相等
                        maxVal=max(maxVal,dp[j]);
                }
                dp[i]=maxVal+1;
            }
    
            int res=0;
            for(int i:dp)
                res=max(res,i);
            return res;
        }
    };
    ```

### [面试题 08.13. 堆箱子](https://leetcode-cn.com/problems/pile-box-lcci/)

- 三维问题，把第一维升序排列后，对第二维和第三维进行dp

  - 退化成了最长递增子序列问题，只不过要多加两个if判断

  - **dp[i] 表示以 box`[i][2]` 这个数结尾的**最长递增子序列的长度。dp[0]=1，basecase

  - ```c++
    class Solution {
    public:
        int pileBox(vector<vector<int>>& box) {
            vector<int> dp(box.size(),0);
            sort(box.begin(),box.end(),[](vector<int>a,vector<int>b){return a[0]<b[0];});//不论多少维，只需要对第一维排序。
            																			//剩下的维度靠if判断来维系正确
            dp[0]=box[0][2];
            for(int i=0;i<dp.size();i++){
                int maxVal=0;
                for(int j=0;j<i;j++){
                    if(box[i][1]>box[j][1] && box[i][2]>box[j][2] && box[i][0]>box[j][0])//在所有比第2维和第三维小的数里，找最大的。另																						外，第一维不能相等
                        maxVal=max(maxVal,dp[j]);
                }
                dp[i]=maxVal+box[i][2];
            }
            int res=0;
            for(int i:dp)
                res=max(res,i);
            return res;
        }
    };
    ```

### [673. 最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)

- 在最长递增子序列基础上，多了一个count数组，记录个数

  - dp[i] 表示**以 nums[i] 这个数结尾的**最长递增子序列的长度。dp[0]=1，basecase

  - count[i]**以 nums[i] 这个数结尾的**最长递增子序列的个数

  - ```c++
    //最长递增子序列的长度
    class Solution {
    public:
        int lengthOfLIS(vector<int>& nums) {
            vector<int> dp(nums.size(),0);
            dp[0]=1;
            for(int i=0;i<dp.size();i++){
                int maxVal=0;
                for(int j=0;j<i;j++){
                    if(nums[j]<nums[i])//在所有比第i值小的数里，找最大的
                        maxVal=max(maxVal,dp[j]);
                }
                dp[i]=maxVal+1;
            }
            int res=0;
            for(auto i:dp)
                res=max(i,res);
            return res;
        }
    };
    //可以跟上边对比一下，基本一致，但是多了个count数组来记录
    //最长递增子序列的个数
    class Solution {
    public:
        int findNumberOfLIS(vector<int>& nums) {
            vector<int> dp(nums.size(),0);//以i结尾的最长子序列的长度
            vector<int> count(nums.size(),0);//记录以i结尾的最长子序列的个数
            dp[0]=1;
            count[0]=1;
            for(int i=1;i<dp.size();i++){
                int maxVal=0;
                int cnt=1;
                for(int j=0;j<i;j++){
                    if(nums[i]>nums[j]){//肯定要满足比尾元素大
                        if(dp[j]>maxVal){//遇到一个更长的序列
                            maxVal=dp[j];//更新长度
                            cnt=count[j];//更新个数
                            
                        }
                        else if(dp[j]==maxVal)//一样长的序列
                            cnt+=count[j];//那么只更新个数
                    }
                }
                dp[i]=maxVal+1;//长度要在之前的长度上加1，因为下标i也加入序列了
                count[i]=cnt;//个数记录下来  
            }
            
            //1,2,5,4,3     结果就是【1,2,5】、【1,2,4】、【1,2,3】，一共3个
            
            int tmpMax=0;//最长的长度
            for(int i:dp)
                tmpMax=max(tmpMax,i);
            int res=0;
            for(int i=0;i<dp.size();i++)
                if(dp[i]==tmpMax)
                    res+=count[i];
            return res;
            /*int maxVal=0;//最长的长度
            int res=0;//个数
            for(int i=0;i<dp.size();i++){
                if(dp[i]>maxVal){
                    maxVal=dp[i];
                    res=count[i];
                }
                else if(dp[i]==maxVal)
                    res+=count[i];
            }*/
            return res;
    
        }
    };
    ```

    

### [53. 最大连续子序和](https://leetcode-cn.com/problems/maximum-subarray/)

- dp 数组的含义：**以 nums[i] 为结尾的**「最大子数组和」为 dp[i]。

  - 时间复杂度O(N)，空间复杂度O(N)

  - ```c++
    class Solution {
    public:
        int maxSubArray(vector<int>& nums) {
            vector<int> dp(nums.size());
            dp[0]=nums[0];
            for(int i=1;i<dp.size();i++){
                if(dp[i-1]>=0)
                    dp[i]=dp[i-1]+nums[i];
                else
                    dp[i]=nums[i];
            }
    
            int res=INT_MIN;
            for(int i:dp)
                res=max(res,i);
            return res;
        }
    };
    ```

### [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

- dp定义：以s[i]结尾的括号个数

  - ```c++
    //()(())。用这个例子来想递推方程
    class Solution {
    public:
        int longestValidParentheses(string s) {
            if(s.size()==0)
                return 0;
            vector<int> dp(s.size(),0);
            dp[0]=0;
            for(int i=1;i<dp.size();i++){
                if(s[i]=='(')//当前字符是左括号，必定填0
                    dp[i]=0;
                else if(s[i]==')')
                    if(s[i-1]=='(')//当前字符是右括号。如果能跟前一个组成一对儿。
                        dp[i]=i-2>=0?dp[i-2]+2:2;//
                    else if(s[i-1]==')')//当前字符是右括号。前一个也是右括号
                        dp[i]= i-dp[i-1]-1 >=0 && s[i-dp[i-1]-1]=='('//((()))这种情况时
                        ?dp[i-1]+2+(i-dp[i-1]-2>=0 ?dp[i-dp[i-1]-2]:0)//()(())。还得把之前的数补回来
                        :0;
            }
    
            int res=0;
            for(int i:dp)
                res=max(res,i);
            return res;
        }
    };
    ```

- 栈

  - 只有左括号的下标可以入栈。

  - 计算当前长度时：

    - 如果栈里还有元素：当前长度=当前右括号下标 - 栈顶左括号下标
    - 如果栈里没有元素了：当前长度=当前右括号下标 - 上一个没被匹配的右括号（它其实就是个分界线）

  - `"))))|(()()"、"))))|(())"`用这两个例子来指引写代码

  - ```c++
    class Solution {
    public:
        int longestValidParentheses(string s) {
            stack<int> stk;
            int res=0;
            int lastRightIdx=-1;
            for(int i=0;i<s.size();i++){
                if(s[i]=='(')//左括号下标入栈
                    stk.push(i);
                else if(s[i]==')'){
                    if(!stk.empty()){//当前的右括号有可以匹配的左括号
                        stk.pop();//把匹配的左括号给pop出来
                        int tmpLen;
                        //  "))))|(()()"、"))))|(())"
                        //竖线看做分界符
                        if(!stk.empty()) tmpLen=i-stk.top();//栈里还有左括号，那么tmpLen就是当前右括号-尚未匹配的左括号
                        else tmpLen=i-lastRightIdx;//栈里没有左括号了，那么tmpLen就是当前右括号-分界符
                        res=max(res,tmpLen);
                    }
                    else if(stk.empty())//当前的右括号没有可以匹配的左括号。那么这个右括号就是新的分界符
                        lastRightIdx=i;
                }
            }
            return res;
        }
    };
    ```

    

### ----模板二----

### [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

**一个字符串从开头到第i个数 和 另一个字符串从开头到第j个数，为止的最优解**

- 先写暴力递归

  - ```c++
    class Solution {
    public:
        int minDistance(string word1, string word2) {
            return dp(word1,word2,word1.size()-1,word2.size()-1);
        }
    
        int dp(string& word1, string& word2,int i,int j){
            //base case
            if(i<0) return j+1;
            if(j<0) return i+1;
    
            
            if(word1[i]==word2[j]){//相等，skip，不占用操作数
                return dp(word1,word2,i-1,j-1);
            }
            else{//操作数+1里取最优的   替换、删除、插入
                return min(min(dp(word1,word2,i-1,j-1)+1,//替换
                    dp(word1,word2,i-1,j)+1),//删除
                    dp(word1,word2,i,j-1)+1);//插入
            }
        }
    };
    ```

- 在写递归+memo（动归记忆化搜索）

  - ```c++
    class Solution {
    public:
        int minDistance(string word1, string word2) {
            return dp(word1,word2,word1.size()-1,word2.size()-1);
        }
        unordered_map<string,int> memo;
        int dp(string& word1, string& word2,int i,int j){// s1[0..i] 和 s2[0..j] 的最小编辑距离
            //base case
            if(i<0) return j+1;
            if(j<0) return i+1;
    
            string key=to_string(i)+","+to_string(j);
            if(memo.count(key)==1)
                return memo[key];
    
            if(word1[i]==word2[j]){//相等，skip，不占用操作数
                memo[key]=dp(word1,word2,i-1,j-1);
            }
            else{//操作数+1里取最优的   替换、删除、插入
                memo[key]= min(min(dp(word1,word2,i-1,j-1)+1,//替换
                    dp(word1,word2,i-1,j)+1),//删除
                    dp(word1,word2,i,j-1)+1);//插入
            }
            return memo[key];
        }
    };
    ```

- 递推动规

  - `dp[i][j] 代表s1[0..i] 和 s2[0..j] 的最小编辑距离`，dp第一行和第一列代表空串时的basecase。
  
  - <img src="https://gblobscdn.gitbook.com/assets%2F-Mc28-yQjBQGNRby1JtX%2Fsync%2F4519d891e61e27733dfdddee345772e522dc2aaa.jpg?alt=media" alt="img" style="zoom:50%;" />
  
  - ```c++
    class Solution {
    public:
        int minDistance(string word1, string word2) {
            int m=word1.size()+1;
            int n=word2.size()+1;
            vector<vector<int>> dp(m,vector<int>(n,0));//s1[0..i] 和 s2[0..j] 的最小编辑距离
            //base case
            for(int i=0;i<m;i++)//第0行（或者第0列）代表字符串为空
                dp[i][0]=i;
            for(int j=0;j<n;j++)
                dp[0][j]=j;
    
            for(int i=1;i<m;i++)
            for(int j=1;j<n;j++){
                if(word1[i-1]==word2[j-1])//字符串里的下标还是从0开始的
                    dp[i][j]=dp[i-1][j-1];
                else
                    dp[i][j]=min(min(dp[i-1][j-1],dp[i-1][j]),dp[i][j-1])+1;
            }
            return dp[m-1][n-1];
        }
    };
    ```

### [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

- dp 数组的含义：

  - **一个字符串从开头到第i个数 和 另一个字符串从开头到第j个数，为止的最优解**

  - `dp[i][j] 代表s1[0..i] 和 s2[0..j] 的最长公共子序列`，dp第一行和第一列代表空串时的basecase。
  
  - ```c++
    class Solution {
    public:
        int longestCommonSubsequence(string text1, string text2) {
            int m=text1.size();
            int n=text2.size();
            vector<vector<int>> dp(m+1,vector<int>(n+1,0));
            for(int i=1;i<m+1;i++){
                for(int j=1;j<n+1;j++){
                    if(text1[i-1]==text2[j-1])
                        dp[i][j]=dp[i-1][j-1]+1;
                    else
                        dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
                }
            }
            return dp[m][n];
        }
    };
    ```

### [718. 最长重复子数组（最长公共子串）](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)

- dp

  - 因为是子串即连续问题，肯定得往数组里填-1代表false了

  - dp数组代表如果[0,i]和[0,j]满足，那么数值就是长度。如果不满足，就是-1

  - 可以理解为必须包含i和j。比较像以i结尾

  - ```c++
    class Solution {
    public:
        int findLength(vector<int>& nums1, vector<int>& nums2) {
            int m=nums1.size();
            int n=nums2.size();
            vector<vector<int>> dp(m+1,vector<int>(n+1,0));
            for(int i=1;i<m+1;i++){
                for(int j=1;j<n+1;j++){
                    if(nums1[i-1]==nums2[j-1])
                        dp[i][j]=dp[i-1][j-1]==-1?1:dp[i-1][j-1]+1;
                    else
                        dp[i][j]=-1;
        
                }
            }
    		
            //遍历找返回值
            int res=0;
            for(int i=0;i<m+1;i++)
            for(int j=0;j<n+1;j++)
                if(dp[i][j]>res)
                    res=dp[i][j];
            return res;
    
        }
    };
    ```

    

### [583. 两个字符串的删除操作](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)

- dp 数组的含义：

  - **一个字符串从开头到第i个数 和 另一个字符串从开头到第j个数，为止的最优解**

  - `dp[i][j] 代表s1[0..i] 和 s2[0..j] 的最少步数`，dp第一行和第一列代表空串时的basecase。
  
  - ```c++
    class Solution {
    public:
        int minDistance(string word1, string word2) {
            int m=word1.size();
            int n=word2.size();
            vector<vector<int>> dp(m+1,vector<int>(n+1,0));
            for(int i=0;i<m+1;i++)
                dp[i][0]=i;
            for(int j=0;j<n+1;j++)
                dp[0][j]=j;
            for(int i=1;i<m+1;i++){
                for(int j=1;j<n+1;j++){
                    if(word1[i-1]==word2[j-1])
                        dp[i][j]=dp[i-1][j-1];
                    else
                        dp[i][j]=min(dp[i-1][j],dp[i][j-1])+1;
                }
            }
            return dp[m][n];
        }
    };
    ```

### [712. 两个字符串的最小ASCII删除和](https://leetcode-cn.com/problems/minimum-ascii-delete-sum-for-two-strings/)

- dp 数组的含义：

  - **一个字符串从开头到第i个数 和 另一个字符串从开头到第j个数，为止的最优解**

  - `dp[i][j] 代表s1[0..i] 和 s2[0..j] 的最小ascii删除和`，dp第一行和第一列代表空串时的basecase。
  
  - ```c++
    class Solution {
    public:
        int minimumDeleteSum(string s1, string s2) {
            int m=s1.size();
            int n=s2.size();
            vector<vector<int>> dp(m+1,vector<int>(n+1,0));
            dp[0][0]=0;
            for(int i=1;i<m+1;i++)
                dp[i][0]=s1[i-1]+dp[i-1][0];
            for(int j=1;j<n+1;j++)
                dp[0][j]=s2[j-1]+dp[0][j-1];
            for(int i=1;i<m+1;i++){
                for(int j=1;j<n+1;j++){
                    if(s1[i-1]==s2[j-1])
                        dp[i][j]=dp[i-1][j-1];
                    else
                        dp[i][j]=min(dp[i-1][j]+s1[i-1],dp[i][j-1]+s2[j-1]);
                }
            }
            return dp[m][n];
        }
    };
    ```

### ----模板三----

### [516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

- 二维dp数组的含义：

  - 在子串s[i..j]中，最长回文子序列的长度为`dp[i][j]`

  - 其实上边两个字符串的题目，都是从s1[0..i] 和 s2[0..j] 。这个题只有一个字符串。不再是从0开始了，而是从[i..j]上的最优解

  - <img src="http://pichost.yangyadong.site/img/image-20210620154929823.png" alt="image-20210620154929823" style="zoom: 80%;" />
  
  - ```c++
    class Solution {
    public:
        int longestPalindromeSubseq(string s) {
            int size=s.size();
            vector<vector<int>> dp(size,vector<int>(size,0));
            for(int i=0;i<size;i++)
                dp[i][i]=1;
            for(int index=1;index<size;index++){//调整斜线
                for(int i=0,j=index;i<size && j<size;i++,j++){//{[0,1] [1,2] [2,3]..} , {[0,2] [1,3] [2,4]..}
                    if(s[i]==s[j])
                        dp[i][j]=dp[i+1][j-1]+2;
                    else
                        dp[i][j]=max(dp[i][j-1],dp[i+1][j]);
                }
            }
            return dp[0][size-1];
        }
    };
    ```

### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

- 方法1：

  - dp: 二维dp数组定义为： 在子串s[i..j]中，如果[i,j]是回文串，那么值为其长度。如果[i,j]不是回文串，那么值为-1

  - ```c++
    class Solution {
    public:
        string longestPalindrome(string s) {
            int size=s.size();
            vector<vector<int>> dp(size,vector<int>(size,0));
            for(int i=0;i<size;i++)
                dp[i][i]=1;
            for(int index=1;index<size;index++){//调整斜线
                for(int i=0,j=index;i<size && j<size;i++,j++){//{[0,1] [1,2] [2,3]..} , {[0,2] [1,3] [2,4]..}
                    if(s[i]==s[j])
                        dp[i][j]=dp[i+1][j-1]==-1?-1:dp[i+1][j-1]+2;  //如果当前两个字符相等，并且[i+1,j-1]上也是回文串。那么长度加2
                    										//如果当前两个字符相等，但是[i+1,j-1]上不是回文串。那么[i,j]也不是回文串
                    else
                        dp[i][j]=-1;//如果当前两个字符不等，那么，[i,j]一定不是回文串
                }
            }
            
            //遍历找最长。
            int resI=0;
            int resJ=0;
            for(int i=0;i<size;i++)
            for(int j=0;j<size;j++){
                if(dp[i][j]>=dp[resI][resJ]){
                    resI=i;
                    resJ=j;
                }
            }
    
            return s.substr(resI,resJ-resI+1);
        
        }
    };
    ```

- 方法2：

  - 中心扩散法

  - 它是少有的比动规还优秀的方法。因为动归需要dp数组占用空间，而这种方法不需要。

  - 使用两个指针l r来避免奇偶讨论。

  - ```c++
    class Solution {
    public:
        string longestPalindrome(string s) {
            string res;
            for(int i=0;i<s.size();i++){
                string tmp1=Palindrome(s,i,i);//奇数时
                if(tmp1.size()>res.size())
                    res=tmp1;
                string tmp2=Palindrome(s,i,i+1);//偶数时
                if(tmp2.size()>res.size())
                    res=tmp2;
            }
            return res;
        }
    
        string Palindrome(string s,int l,int r){//寻找以l,r为中心，往外扩散的最大回文子串
            while(l>=0 && r<s.size() &&s[l]==s[r]){
                l--,r++;
            }
            return s.substr(l+1,r-(l+1));
        }
    };
    ```

## 18.动态规划-背包问题

### ----模板01背包-----

### [经典01背包题目](https://www.acwing.com/problem/content/2/)

- 01背包

  - 状态：可选择的物品、背包的容量；

  - 选择：是否把第i个物品装入背包

  - `dp[i][w]`的定义如下：对于前i个物品，当前背包的容量为w，这种情况下可以装的最大价值是`dp[i][w]`。

  - **base case 就是`dp[0][..] = dp[..][0] = 0`**，因为没有物品或者背包没有空间的时候，能装的最大价值就是 0。

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

- 状态压缩

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

### ----子集背包----

### [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

- 转化为01背包

  - 如果有办法在N个数里装满sum/2。那么剩下的数直接装到另一个背包里，就划分好子集了

  - 问题转化为了N个数里装满sum/2的背包

  - `dp[i][w]`的定义如下：对于前i个物品，当前背包的容量为w，这种情况下是否可以装满背包就是`dp[i][w]`。

  - base case:`dp[0][..]=false`，当没有物品可装时，肯定装不满，所以是false。`dp[...][0]=true`，当要装的空间为0时，一定装的满。其实左上角为true为false问题不大，因为没有元素需要它来递推。

  - ```c++
    class Solution {
    public:
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
    };
    ```

- 状态压缩

  - **注意到** **`dp[i][j]`** **都是通过上一行** **`dp[i-1][..]`** **转移过来的**，之前的数据都不会再使用了。

  - 我们可以进行状态压缩，将二维 `dp` 数组压缩为一维，节约空间复杂度：

  - 必须逆序遍历，因为两行状态是存在一个vector里了，从后往前才不会导致两行状态存放冲突。

  - ```c++
    class Solution {
    public:
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
    };
    ```

### ----完全背包----

### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

- 原始二维dp

  - dp数组定义为：若只使用前 `i` 个物品（只是用前i种硬币），当金额为 `j` 时，最少使用 `dp[i][j]` 个硬币装满背包。

  - basecase：`dp[0][..] = amount+1（相当于INT_MAX）， dp[..][0] = 0（其实一开始想不出来，画画矩阵就知道了）`

  - ```c++
    class Solution {
    public:
        int coinChange(vector<int>& coins, int amount) {
            int m=coins.size();
            vector<vector<int>> dp(m+1,vector<int>(amount+1,INT_MAX-1));
            //上边完成了初始化basecase 第一行为无穷大。因为可能有+1的操作，需要设为INT_MAX-1。但是dp数组里存的最大值，只可能是INT_MAX-1。
            //因为底下的min(dp[i-1][j],dp[i][j-coins[i-1]]+1)最差的情况也是min(INT_MAX-1,INT_MAX)，dp数组里只可能出现INT_MAX-1
            //base case。第一列为0
            for(int i=1;i<m+1;i++)
                dp[i][0]=0;
            for(int i=1;i<m+1;i++){
                for(int j=1;j<amount+1;j++){
                    if(j<coins[i-1])
                        dp[i][j]=dp[i-1][j];
                    else
                        dp[i][j]=min(dp[i-1][j],dp[i][j-coins[i-1]]+1);
                }
            }
    	return dp[m][amount]==INT_MAX-1?-1:dp[m][amount];
        }
    };
    ```
  
- 原始二维dp+状态压缩

  - 这个题状态压缩，一定要看。`dp[i][j]=dp[i-1][j]+dp[i][j-coins[i-1]];`。因此，dp用的状态是本行的！并且仅用上一行的同一位置！因此一定还是正序遍历，这样两行状态的存储才不会矛盾。

  - ```c++
    class Solution {
    public:
        int coinChange(vector<int>& coins, int amount) {
            int m=coins.size();
            // vector<vector<int>> dp(m+1,vector<int>(amount+1,amount+1));
            vector<int> dp(amount+1,amount+1);
            //base case。第一列为0
            // for(int i=1;i<m+1;i++)
            //     dp[i][0]=0;
            dp[0]=0;
            for(int i=1;i<m+1;i++){
                for(int j=1;j<amount+1;j++){
                    if(j<coins[i-1])
                        // dp[i][j]=dp[i-1][j];
                        dp[j]=dp[j];
                    else
                        // dp[i][j]=min(dp[i-1][j],dp[i][j-coins[i-1]]+1);
                        dp[j]=min(dp[j],dp[j-coins[i-1]]+1);
                }
            }
            if(dp[amount]>amount)
                return -1;
            else
                return dp[amount];
    
        }
    };
    ```

### [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

- 原始二维dp

  - dp数组定义为：若只使用前 `i` 个物品，当背包容量为 `j` 时，有 `dp[i][j]` 种方法可以装满背包。

  - base case 为 `dp[0][..] = 0， dp[..][0] = 1（如果一开始想不出来，画画矩阵，就知道了）`。因为如果不使用任何硬币面值，就无法凑出任何金额；如果凑出的目标金额为 0，那么“无为而治”就是唯一的一种凑法。

  - ```c++
    class Solution {
    public:
        int change(int amount, vector<int>& coins) {
            int m=coins.size();
            vector<vector<int>> dp(m+1,vector<int>(amount+1,0));
            //base case。第一列为1
            for(int i=1;i<m+1;i++)
                dp[i][0]=1;
    
            for(int i=1;i<m+1;i++){
                for(int j=1;j<amount+1;j++){
                    if(j<coins[i-1])//金额不够
                        dp[i][j]=dp[i-1][j];
                    else
                        dp[i][j]=dp[i-1][j]+dp[i][j-coins[i-1]];
                }
            }
            return dp[m][amount];
        }
    };
    ```

- 原始二维dp+状态压缩

  - 这个题状态压缩，一定要看。`dp[i][j]=dp[i-1][j]+dp[i][j-coins[i-1]];`。因此，dp用的状态是本行的！仅用上一行的同一位置！因此一定还是正序遍历，这样两行状态的存储才不会矛盾。

  - ```c++
    class Solution {
    public:
        int change(int amount, vector<int>& coins) {
            int m=coins.size();
            // vector<vector<int>> dp(m+1,vector<int>(amount+1,0));
            vector<int> dp(amount+1,0);
            //base case。第一列为1
            // for(int i=1;i<m+1;i++)
            //     dp[i][0]=1;
            dp[0]=1;
    
            for(int i=1;i<m+1;i++){
                for(int j=1;j<amount+1;j++){
                    if(j<coins[i-1])//金额不够
                        //dp[i][j]=dp[i-1][j];
                        dp[j]=dp[j];
                    else
                        //dp[i][j]=dp[i-1][j]+dp[i][j-coins[i-1]];
                        dp[j]=dp[j]+dp[j-coins[i-1]];
                }
            }
            return dp[amount];
        }
    };
    ```

    

## 排序-快排

### [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

- 原始快排

  - ```c++
    class Solution {
    public:
        vector<int> getLeastNumbers(vector<int>& arr, int k) {
            quickSort(arr,0,arr.size()-1);
            vector<int> res(arr.begin(),arr.begin()+k);//左闭右开
            return res;
        }
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
    };
    ```

- 单侧排序改进快排

  - O(N)

  - ```c++
    class Solution {
    public:
        vector<int> getLeastNumbers(vector<int>& arr, int k) {
            this->k=k;
            quickSort(arr,0,arr.size()-1);
            vector<int> res(arr.begin(),arr.begin()+k);
            return res;
        }
        int k;
        void quickSort(vector<int>& nums,int l,int r){
            if(l>=r)
                return;
            int i=l,j=r;
            while(i<j){
                while(i<j && nums[j]>=nums[l]) j--;
                while(i<j && nums[i]<=nums[l]) i++;
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
    };
    ```

### [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

- 单侧排序改进快排

  - O(N)

  - ```c++
    class Solution {
    public:
        int findKthLargest(vector<int>& nums, int k) {
            this->k=k;
            quickSort(nums,0,nums.size()-1);
            return nums[k-1];
        }
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
    };
    ```

### [912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)

- 快排+三数选取优化基准数/随机化哨兵

  - ```c++
    class Solution {
    public:
        vector<int> sortArray(vector<int>& nums) {
            vector<int> res=nums;
            quickSort(res,0,res.size()-1);
            return res;
        }
    
        void quickSort(vector<int>& nums,int l,int r){
            if(l>=r)
                return;
            int i=l,j=r;
      //方案1.增加三数选取优化基准数的选择。相当于人工先把(l、mid、r)这三个位置排好。这3数排成升序降序要跟快排序一致。
            int m=l+(r-l)/2;
            if(nums[l]>nums[r]) swap(nums[l],nums[r]);
            if(nums[m]>nums[r]) swap(nums[m],nums[r]);//前两行保证arr[r]始终为这三个数中最大的一个
            if(nums[m]>nums[l]) swap(nums[m],nums[l]);//最后这行代码保证arr[l]为中间元素，这样下面代码不必再做修改
            
         /*方案2.随机化选取哨兵。随机选择一个下标，然后跟l交换。
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

    

# 2.多线程题目

## 0.基础知识

### 1.std::thread和.join()

对于创建的线程，一般会在其销毁前调用join和detach函数；

弄清楚这两个函数的调用时机和意义，以及调用前后线程状态的变化非常重要。

- join 会使当前线程阻塞，直到目标线程执行完毕；
  - 只有处于活动状态线程才能调用join，可以通过joinable()函数检查;
  - joinable() == true表示当前线程是活动线程，才可以调用join函数；
  - 默认构造函数创建的对象是joinable() == false;
  - join只能被调用一次，之后joinable就会变为false，表示线程执行完毕；
  - 调用 ternimate()的线程必须是 joinable() == false;
  - 如果线程不调用join()函数，即使执行完毕也是一个活动线程，即joinable() == true，依然可以调用join()函数；
- detach 将thread对象及其表示的线程分离；
  - 调用detach表示thread对象和其表示的线程完全分离；
  - 分离之后的线程是不在受约束和管制，会单独执行，直到执行完毕释放资源，可以看做是一个daemon线程；
  - 分离之后thread对象不再表示任何线程；
  - 分离之后joinable() == false，即使还在执行；

```c++
#include<iostream>
#include<thread>

using namespace std;

void thread1() {
    for(int i=0;i<20;++i)
        cout << "thread1..." << endl;
}

void thread2() {
    for (int i = 0; i<20; ++i)
        cout << "thread2..." << endl;
}

int main(int argc, char* argv[]) {
    thread th1(thread1);   //实例化一个线程对象th1，该线程开始执行
    
    //th1.joinable()一开始是true，即可以让父进程等待th1
    //执行完th1.join()后，th1.joinable()就变成false了
    if(th1.joinable())
    {
        cout<<"waiting for th1"<<endl;
        th1.join();//让父进程等待th1执行完，再往下执行。即让主进程进入阻塞等待。
    }

    thread th2(thread2);
    if(th2.joinable())
    {
        cout<<"waiting for th2"<<endl;
        th2.join();
    }
    cout << "main..." << endl;
    return 0;
}
```

### 2.detach()

- 将当前线程对象所代表的执行实例与该线程对象分离，使得线程的执行可以单独进行。一旦线程执行完毕，它所分配的资源将会被释放。
- detach是用来分离线程，这样线程可以独立地执行，不过这样由于没有thread对象指向该线程而失去了对它的控制，当对象析构时线程会继续在后台执行，但是当主程序退出时并不能保证线程能执行完。如果没有良好的控制机制或者这种后台线程比较重要，最好不用detach而应该使用join。
  - **也就是detach()出去的子进程，很有可能在main()执行完时，detach出去的子进程还没执行完，但是main（）执行完了以后就把子进程需要执行的函数段（比如thread1()函数)内存给删除了，这样会出错**

```c++
#include<iostream>
#include<thread>

using namespace std;

void thread1() {
    for(int i=0;i<20;++i)
        cout << "thread1..." << endl;
}

void thread2() {
    for (int i = 0; i<20; ++i)
        cout << "thread2..." << endl;
}

int main(int argc, char* argv[]) {
    thread th1(thread1);   //实例化一个线程对象th1，该线程开始执行
    thread th2(thread2);
    th1.detach();
    th2.detach();
    cout << "main..." << endl;
    return 0;
}
```

### 3.mutex

- **头文件是，mutex是用来保证线程同步的，防止不同的线程同时操作同一个共享数据。**
- **#include：该头文件主要声明了与互斥量(mutex)相关的类，包括 std::mutex 系列类，std::lock_guard, std::unique_lock, 以及其他的类型和函数。**

```c++
//mutex是不安全的，当一个线程在解锁之前异常退出了，那么其它被阻塞的线程就无法继续下去。
//mutex只能是信号量为1，若想自定义信号量应该用sem_t
mutex m;//互斥锁，即信号量为1
m.lock();//即P(m)
m.unlock();//即V(m)
```

```c++
//使用lock_guard则相对安全，它是基于作用域的，能够自解锁，当该对象创建时，它会像m.lock()一样获得互斥锁，当生命周期结束时，它会自动析构(unlock)，不会因为某个线程异常退出而影响其他线程。
//即lock_guard自动做到了m.lock()和m.unlock()，并且还更安全一点
mutex m;

{
    lock_guard<mutex> lockGuard(m);
}

```

```c++
//std::unique_lock<std::mutex> ;一般跟条件变量一起使用，当条件变量满足某些条件时，可以自动解锁，lock_guard就不行。若单纯用unique_lock，不加条件变量，那跟lock_guard功能一样
mutex m;
{
	std::unique_lock<std::mutex> lk(m);//即在这里上锁，直到作用域尾解锁。跟lock_guard功能一样
}
```



### 4.获取线程ID

线程ID是一个线程的标识符，C++标准中提供两种方式获取线程ID；

1. thread_obj.get_id();
2. std::this_thread::get_id()

有一点需要注意，就是空thread对象，也就是不表示任何线程的thread obj调用get_id返回值为0；

此外当一个线程被detach或者joinable() == false时，调用get_id的返回结果也为0。

```c++
cout << t.get_id() << ' ' << this_thread::get_id() << endl;
//t.detach();
t.join();
cout << t.get_id() << ' ' << std::this_thread::get_id() << endl;
```

### 5.semaphore

```c++
sem_t sem1;//声明一个信号量
int sem_init(&sem1,0,0);//第一个参数是要初始化的信号量;第二个参数=0即只能线程间来互斥或者同步，第二个参数≠0则可以用来进程间的互斥或者同步;第三个数字代表信号量初值
//sem_wait(&sem_z)==0是sem_wait函数返回成功了。不管申请没申请到，这步的返回值都是0。否则就是未知error了
sem_wait(&sem1);//即P(sem1)。P操作里，一旦没申请成功，有block()这一步
sem_post(&sem1);//即V(sem1)。要记住，V（）操作里包含有wakeup()这一步
```

### 6.条件变量

```c++
 std::condition_variable con1;//条件变量。相当于一种资源的阻塞队列。
 std::mutex my_mutex;//互斥量，就是最简单的进程互斥。
std::unique_lock<std::mutex> lk(my_mutex);

con1.wait(lk,[this]()->bool{return counter==2;}); // 阻塞当前线程，直到条件变量被唤醒。并且会释放lk，即解锁，好让其他线程执行。 [this]()->bool{return counter==2;}是Lambda表达式

 con1.notify_one();//若任何线程在con1上等待，则调用 notify_one 会解阻塞(唤醒)等待线程之一。
        //换句话说，con1.notify_one()是con1.wait()的唤醒

//伪唤醒，底层设计时没有保证每次执行wait都是由notify唤醒，所以需要自己加个条件（比如lambda表达式）判断一下，是不是真的可以执行了
```

### 7.原子操作

<img src="https://s1.ax1x.com/2020/04/27/JhUBdK.png" alt="JhUBdK.png" style="zoom:67%;" />

```c++
//atomic是原子操作，只对当前变量同步（读和写），不对操作（中间步骤）同步，操作同步用锁来实现
//答案是，不能保证
h2.load();
h2.store();
保证了别的线程中已发生的修改（比如已经store了个新值），我在本线程执行到load时，保证一定是能看到的，也就是load到的一定是最新值。可是别的线程都还没有store，我本线程就Load了，那肯定是旧值（未发生改动的值）啊。也就是说，一个线程store()，一个线程load()，这并不会自动限制说有store的线程先执行，有load的线程后执行。而是如果已经发生过store了，那么load的一定是最新值。如果store发生都还没发生，那load的肯定是没store之前的值。
```

### 8.总结

#### 1.两种题型

- 一种是要在一个线程里循环执行的
  - 比如交替打印零与奇偶数，只有3个线程，但是要打印0-15。
  - 这种的典型特征是，函数一进去就得写个while(1)
- 一种是一个进程只执行一次，系统会创建很多线程
  - 比如H2O生成
  - 这种一般就得考虑函数一进去就得加锁了。

#### 2.四种方法

- 信号量法（sem_t)            ：比较难
  - 注意结束时，其它wait进程的释放
- unique_lock<>lk(mutex) + 条件变量            ：还行
  - 这两句话一定紧挨着写，有些判断写在它俩之间好像不起作用
  - 注意结束时，其它wait进程的释放
- flag+yield():         一般是一进来就while得那种，用flag+yield()                   ：还行
  - 有时候是mutex+flag+yield()  ：一般一进来非while型，只执行一次型，用mutex+yield()
  - 其实跟unique_lock<>lk(mutex) + 条件变量法很像，只是少了很多代码，大体上框架一致。不用加锁了，不用唤醒了。
  - 这种可以放心大胆不要锁，是因为其他进程都在yield谦让，它们不会执行去修改数据。每一种cur下，应该输出的进程没执行完，是不会有其他进程执行的。
  - 有两种思路，一种是全部控制权在一个线程里，它来分发flag。就是说不管执行哪个线程，那个控制线程都得执行一遍。另一种思路就是各自判断，各自为战，4个线程等权。
  - 这种方式一般可以无锁。（一直while)
- 原子操作（不推荐）
  - 用`atomic<int> i`的，一般都不对。用`atomic<bool> flag`的，好像跟flag+yield()没什么区别
  - 结论是...原子操作好像在这里没用

## 1.按序打印

### 方法1：使用std::mutex 互斥锁，默认信号量为1

```c++
//mutex这个类是互斥锁，即默认信号量为1
//lock()操作相当于P()
//unlock()操作相当于V()
class Foo {
public:
    Foo() {
        sem1.lock();//先把两个信号量降为1，再进程同步
        sem2.lock();//用于进程同步的信号量一般是0
    }

    void first(function<void()> printFirst) {
        // printFirst() outputs "first". Do not change or remove this line.
        printFirst();
        sem1.unlock();//V(sem1)
    }

    void second(function<void()> printSecond) {
        sem1.lock();//P(sem1)
        // printSecond() outputs "second". Do not change or remove this line.
        printSecond();       
        sem2.unlock();//V(sem2)
    }

    void third(function<void()> printThird) {
        sem2.lock();//P(sem2)
 		// printThird() outputs "third". Do not change or remove this line.
        printThird();
    }
private:
    std::mutex sem1,sem2;
};
//符合之前讲的进程同步，
```

### 方法2：自定义信号量(使用semaphore.h)

```c++
#include <semaphore.h>
class Foo {
private:
    sem_t sem1,sem2;
public:
    Foo() {
        sem_init(&sem1,0,0);//第二个参数=0，即单进程的线程共享。即只能同步线程
        sem_init(&sem2,0,0);//若第二个参数≠0，则多进程之间共享。即可以用来同步进程
        //第三个参数是信号量初值，设为0
    }

    void first(function<void()> printFirst) {

        // printFirst() outputs "first". Do not change or remove this line.
        printFirst();
        sem_post(&sem1);//V(sem1)
    }

    void second(function<void()> printSecond) {
        sem_wait(&sem1);//P(sem1)
        // printSecond() outputs "second". Do not change or remove this line.
        printSecond();
        sem_post(&sem2);//V(sem2)
    }

    void third(function<void()> printThird) {
        sem_wait(&sem2);//P(sem2)
        // printThird() outputs "third". Do not change or remove this line.
        printThird();
    }
};
```

### 方式3：互斥锁+条件变量

```c++
//条件变量condition_variable只能和unique_lock一起使用
//condition.wait()函数都在会阻塞时，自动释放锁权限，即调用unique_lock的成员函数unlock()，以便其他线程能有机会获得锁
//三个线程各自上锁，
class Foo {
public:
    Foo() {
        
    }

    void first(function<void()> printFirst) {
        std::unique_lock<std::mutex> lk(my_mutex);//即在这里上锁，直到作用域尾解锁
        // printFirst() outputs "first". Do not change or remove this line.
        printFirst();
        counter++;
        con1.notify_one();//若任何线程在con1上等待，则调用 notify_one 会解阻塞(唤醒)等待线程之一。
        //换句话说，con1.notify_one()是con1.wait()的唤醒
    }

    void second(function<void()> printSecond) {
        std::unique_lock<std::mutex> lk(my_mutex);
        con1.wait(lk,[this]()->bool{return counter==2;}); // 阻塞当前线程，直到条件变量被唤醒。并且会释放lk，即解锁。 [this]()->bool{return counter==2;}是Lambda表达式
        // printSecond() outputs "second". Do not change or remove this line.
        printSecond();
        counter++;
        con2.notify_one();
    }

    void third(function<void()> printThird) {
         std::unique_lock<std::mutex> lk(my_mutex);
         con2.wait(lk,[this]()->bool{return counter==3;});//counter没到3，说明之前两个没执行，那么进程到con2上阻塞等待,并解了上一步的lk.
        // printThird() outputs "third". Do not change or remove this line.
        printThird();
    }
private:
    int counter=1;
    std::condition_variable con1;//条件变量。相当于一种资源的阻塞队列。
    std::condition_variable con2;
    std::mutex my_mutex;//互斥量，就是最简单的进程互斥。
};

```

## 2.交替打印字符串(与第6题差不多)

### 1. 采用信号量

```c++
#include<semaphore.h>
class FizzBuzz {
private:
    int n;//1-n输出
    int cur;//
    sem_t sem_fizz;//3个同步信号量
    sem_t sem_buzz;
    sem_t sem_fizz_buzz;
    sem_t sem_num;//1互斥信号量？

public:
    FizzBuzz(int n) {
        this->n = n;
        cur = 1;
        sem_init(&sem_fizz, 0, 0);//信号量初始化
        sem_init(&sem_buzz, 0, 0);
        sem_init(&sem_fizz_buzz, 0, 0);
        sem_init(&sem_num, 0, 1);
    }

    // printFizz() outputs "fizz".

    /*入参std::function<void()>是一个模板类对象，
    它可以用一个函数签名为void()的可调用对象来进行初始化；
    上述实现里面是一个传值调用。我们来看一下它的调用过程，*/
    void fizz(function<void()> printFizz) {
        while(cur <= n){
            sem_wait(&sem_fizz);
            if(cur > n) break;
            printFizz();
            cur++;
            sem_post(&sem_num);
        }
    }

    // printBuzz() outputs "buzz".
    void buzz(function<void()> printBuzz) {
        while(cur <= n){
            sem_wait(&sem_buzz);
            if(cur > n) break;
            printBuzz();
            cur++;
            sem_post(&sem_num);
        }
    }

    // printFizzBuzz() outputs "fizzbuzz".
    void fizzbuzz(function<void()> printFizzBuzz) {
        while(cur <= n){
            sem_wait(&sem_fizz_buzz);
            if(cur > n) break;
            printFizzBuzz();
            cur++;
            sem_post(&sem_num);
        }
    }

    // printNumber(x) outputs "x", where x is an integer.
    void number(function<void(int)> printNumber) {
        while(cur <= n){
            sem_wait(&sem_num);
            if(cur > n) break;
            if(cur % 3 == 0 && cur % 5 == 0){
                sem_post(&sem_fizz_buzz);
            }else if(cur % 3 == 0){
                sem_post(&sem_fizz);
            }else if(cur % 5 == 0){
                sem_post(&sem_buzz);
            }else{
                printNumber(cur);
                cur++;
                sem_post(&sem_num);
            }
        }

        // 以下三个post通过更新sem_fizz等信号量，调动其他线程运行，进而结束所有线程
        sem_post(&sem_fizz);
        sem_post(&sem_buzz);
        sem_post(&sem_fizz_buzz);
    }
};
```

### 2. atomic（该方法错误）

```c++
//该方法错误！因为i.load()是原子的，不代表往下执行也是原子的。比如number函数中，此时i=15，进入了while,然后线程切了出去，再回来时，i已经是16了。
class FizzBuzz {
private:
    int n;
    int cur;

public:
    FizzBuzz(int n) {
        this->n = n;
        i=1;
    }

    // printFizz() outputs "fizz".
    void fizz(function<void()> printFizz) {
        while(i.load(memory_order_acquire)<=n){//i.load(memory_order_acquire)保证了这个线程后边的所有读写操作不能被排在它之前
            if(i%3==0 && i%5!=0){
                printFizz();
                i++;
            }
            else{
                this_thread::yield();
            }
        }
    }

    // printBuzz() outputs "buzz".
    void buzz(function<void()> printBuzz) {
        while(i.load()<=n){//i.load()默认的是memory_order_seq_cst
            if(i%5==0 && i%3!=0){
                printBuzz();
                i++;
            }
            else{
                this_thread::yield();
            }
        }
    }

    // printFizzBuzz() outputs "fizzbuzz".
	void fizzbuzz(function<void()> printFizzBuzz) {
        while(i.load()<=n){
            if(i%5==0 && i%3==0){
                printFizzBuzz();
                i++;
            }
            else{
                this_thread::yield();
            }
        }
    }

    // printNumber(x) outputs "x", where x is an integer.
    void number(function<void(int)> printNumber) {
        while(i.load()<=n){

            if(i%5!=0 && i%3!=0){
                printNumber(i);
                i++;
            }
            else{
                this_thread::yield();
            }
        }
    }
};
```

### 3.flag+yield（可以有控制线程，也可以等权）

#### 1.number是控制线程法

```c++
//number线程是控制flag的线程，其它线程结束后都得执行一次number线程，等待Number线程配置flag。
class FizzBuzz {
private:
	int n;
	int cur;
	int flag;//flag=0 number。flag=1 fizz。 flag=2 buzz,flag=3 fizzbuzz
public:
	FizzBuzz(int n) {
		this->n = n;
		cur = 1;
		flag = 0;
	}

	// printFizz() outputs "fizz".
	void fizz(function<void()> printFizz) {
		
		while (1) {
			while (flag!=1 && cur<=n) this_thread::yield();
			if (cur > n) break;//用于结束线程
			printFizz();
			cur++;
			flag = 0;//交还控制权
		}
	}

	// printBuzz() outputs "buzz".
	void buzz(function<void()> printBuzz) {
		while (1) {
			while (flag!=2 && cur <= n) this_thread::yield();
			if (cur > n) break;//用于结束线程
			printBuzz();
			cur++;
			flag = 0;
		}
	}

	// printFizzBuzz() outputs "fizzbuzz".
	void fizzbuzz(function<void()> printFizzBuzz) {
		while (1)
		{
			while (flag!=3 && cur <= n) this_thread::yield();
			if (cur > n) break;//用于结束线程
			printFizzBuzz();
			cur++;
			flag = 0;
		}
	}

	// printNumber(x) outputs "x", where x is an integer.
	void number(function<void(int)> printNumber) {
		while (1)
		{
			while (flag!=0 && cur <= n) this_thread::yield();
			if (cur % 3 == 0 && cur % 5 != 0) flag = 1;
			else if (cur % 3 != 0 && cur % 5 == 0) flag = 2;
			else if (cur % 3 == 0 && cur % 5 == 0) flag = 3;
			else flag = 0;
			if (cur > n) break;//用于结束线程
			if (flag != 0) continue;//flag不是0，说明不该本线程执行，continue出去
			printNumber(cur);
			cur++;
		}
	}
};
```

#### 2.  4个线程等权

```c++
//4个线程等权，各自判断。就是不要number来控制flag了
class FizzBuzz {
private:
	int n;
	int cur;
public:
	FizzBuzz(int n) {
		this->n = n;
		cur = 1;
	}

	// printFizz() outputs "fizz".
	void fizz(function<void()> printFizz) {

		while (1) {
			while (!(cur % 3 == 0 && cur % 5 != 0) && cur <= n) this_thread::yield();
			if (cur > n) break;//用于结束线程
			printFizz();
			cur++;
		}
	}

	// printBuzz() outputs "buzz".
	void buzz(function<void()> printBuzz) {
		while (1) {
			while (!(cur % 5 == 0 && cur % 3 != 0) && cur <= n) this_thread::yield();
			if (cur > n) break;
			printBuzz();
			cur++;
		}
	}

	// printFizzBuzz() outputs "fizzbuzz".
	void fizzbuzz(function<void()> printFizzBuzz) {
		while (1)
		{
			while (!(cur % 5 == 0 && cur % 3 == 0) && cur <= n) this_thread::yield();
			if (cur > n) break;
			printFizzBuzz();
			cur++;
		}
	}

	// printNumber(x) outputs "x", where x is an integer.
	void number(function<void(int)> printNumber) {
		while (1)
		{
			while (!(cur % 5 != 0 && cur % 3 != 0) && cur <= n) this_thread::yield();
			if (cur > n) break;
			printNumber(cur);
			cur++;
		}
	}
};
```

### 附测试例程

```c++
int main(int argc, char** argv) {
	FizzBuzz yyd(15);
	std::function<void(int)> printNumber = [](int n) {cout << n << " "; };
	std::function<void()> printFizz = []() {cout << " fizz "; };
	std::function<void()> printBuzz = []() {cout << " Buzz "; };
	std::function<void()> printFizzBuzz = []() {cout << " fizzBuzz "; };
	std::thread th[4];

	th[0] = std::thread(&FizzBuzz::fizz, &yyd, printFizz);
	th[1] = std::thread(&FizzBuzz::buzz, &yyd, printBuzz);
	th[2] = std::thread(&FizzBuzz::fizzbuzz, &yyd, printFizzBuzz);
	th[3] = std::thread(&FizzBuzz::number, &yyd, printNumber);


	for (auto& ts : th) {
		if (ts.joinable()) ts.join();
	}

	return 0;
}
```

## 3.哲学家问题

### 1.必须同时抓起俩筷子(mutex)

```c++
//同时抓起俩筷子
class DiningPhilosophers {
private:
    std::mutex lks[5];//每只筷子一个互斥锁
    std::mutex guid;//保证拿左右筷子一气呵成
public:
    DiningPhilosophers() {
        
    }

    void wantsToEat(int philosopher,
                    function<void()> pickLeftFork,
                    function<void()> pickRightFork,
                    function<void()> eat,
                    function<void()> putLeftFork,
                    function<void()> putRightFork) {
        int l=philosopher;//左筷子编号
        int r=(philosopher+1)%5;//右筷子编号
        guid.lock();//拿筷子过程要锁住
		lks[l].lock();//锁左筷子，即占据资源
        lks[r].lock();//锁右筷子
        pickLeftFork();//拿左筷子
        pickRightFork();//拿右筷子
        guid.unlock();//解锁拿筷子过程，其他线程（哲学家）可以拿筷子了
        eat();//吃
        putRightFork();//放筷子
        putLeftFork();
        lks[l].unlock();//放完筷子，将筷子资源释放
        lks[r].unlock();
    }
};
```

### 2.限制5位哲学家只能4位同时就餐(mutex+sem_t)

```c++
//限制就餐人数
#include<semaphore.h>
class DiningPhilosophers {
    private:
        sem_t count;//用于锁住入口，限制吃饭人数
        std::mutex locks[5];//用于锁住5只筷子
public:
    DiningPhilosophers() {
        sem_init(&count,0,4);//初始化信号量，即最多进入4个人
    }

    void wantsToEat(int philosopher,
                    function<void()> pickLeftFork,
                    function<void()> pickRightFork,
                    function<void()> eat,
                    function<void()> putLeftFork,
                    function<void()> putRightFork) {
        int l=philosopher;//左筷子编号
        int r=(philosopher+1)%5;//右筷子编号
		sem_wait(&count);//锁住入口，饭桌上只让有4个人
        locks[l].lock();//锁住左筷子，占据左筷子资源
        locks[r].lock();//锁住右筷子
        pickLeftFork();
        pickRightFork();
        eat();        
        putLeftFork();
        putRightFork();
        locks[l].unlock();//释放左筷子资源
        locks[r].unlock();//释放右筷子资源
        sem_post(&count);//空出一个就餐位置来
    }
};
```

### 3.奇偶哲学家拿筷子顺序分开(mutex)

```c++
//奇偶分开，拿筷子顺序不一样。
//这样相邻两个哲学家要进餐时就会发生争抢筷子,一定不会都拿起左筷子
class DiningPhilosophers {
    private:
        mutex locks[5];//用于锁住5只筷子
public:
    DiningPhilosophers() {
    }

    void wantsToEat(int philosopher,
                    function<void()> pickLeftFork,
                    function<void()> pickRightFork,
                    function<void()> eat,
                    function<void()> putLeftFork,
                    function<void()> putRightFork) {
        int l=philosopher;//左筷子编号
        int r=(philosopher+1)%5;//右筷子编号
        if(l%2==0)
        {//先左后右
            locks[l].lock();//锁住左筷子，占据左筷子资源
            locks[r].lock();//锁住右筷子
            pickLeftFork();
            pickRightFork();
            eat();        
            putLeftFork();
            putRightFork();
            locks[l].unlock();//释放左筷子资源
            locks[r].unlock();//释放右筷子资源
        }                
        else{//先右后左
            locks[r].lock();//锁住右筷子
            locks[l].lock();//锁住左筷子，占据左筷子资源
            pickLeftFork();
            pickRightFork();
            eat();        
            putLeftFork();
            putRightFork();
            locks[l].unlock();//释放左筷子资源
            locks[r].unlock();//释放右筷子资源           
        }

    }
};
```

### 4.筷子编码，只能先拿较小的(mutex)

```c++
//筷子编码，必须先拿左右里边较小的
//这样在循环拿起左筷子的时候，到最后一个哲学家时，它是右筷子编号小，左筷子编号大，而右筷子被0号哲学家拿走了，他只能等待，而不能拿起他的左筷子
#include<semaphore.h>
class DiningPhilosophers {
    private:
        mutex locks[5];//用于锁住5只筷子
public:
    DiningPhilosophers() {
    }

    void wantsToEat(int philosopher,
                    function<void()> pickLeftFork,
                    function<void()> pickRightFork,
                    function<void()> eat,
                    function<void()> putLeftFork,
                    function<void()> putRightFork) {
        int l=philosopher;//左筷子编号
        int r=(philosopher+1)%5;//右筷子编号
        if(l<r)
        {//左边小，先拿左边
            locks[l].lock();//锁住左筷子，占据左筷子资源
            locks[r].lock();//锁住右筷子
            pickLeftFork();
            pickRightFork();
            eat();        
            putLeftFork();
            putRightFork();
            locks[l].unlock();//释放左筷子资源
            locks[r].unlock();//释放右筷子资源
        }                
        else{//右边小，先拿右边
            locks[r].lock();//锁住右筷子
            locks[l].lock();//锁住左筷子，占据左筷子资源
            pickLeftFork();
            pickRightFork();
            eat();        
            putLeftFork();
            putRightFork();
            locks[l].unlock();//释放左筷子资源
            locks[r].unlock();//释放右筷子资源           
        }

    }
};
```

## 4.交替打印FooBar

### 1.用两个mutex

```c++
//类似于生产者消费者的同步思想
class FooBar {
private:
    int n;
    mutex lock1,lock2;
public:
    FooBar(int n) {
        this->n = n;
        lock2.lock();
    }

    void foo(function<void()> printFoo) {
        
        for (int i = 0; i < n; i++) {
            lock1.lock();
        	// printFoo() outputs "foo". Do not change or remove this line.
        	printFoo();
            lock2.unlock();
        }
    }

    void bar(function<void()> printBar) {
        
        for (int i = 0; i < n; i++) {
            lock2.lock();
        	// printBar() outputs "bar". Do not change or remove this line.
        	printBar();
            lock1.unlock();
        }
    }
};
```

### 2.用flag+yield（没有控制线程，都等权）

```c++
class FooBar {
private:
	int n;
	int flag;//flag=true执行bar,flag=false执行foo
public:
	FooBar(int n) {
		this->n = n;
		flag = 0;
	}

	void foo(function<void()> printFoo) {

		for (int i = 0; i < n; i++) {
			while (flag!=0) this_thread::yield();//flag=false,执行该线程。flag=true,yield出去
			// printFoo() outputs "foo". Do not change or remove this line.
			printFoo();
			flag = 1;
		}
	}

	void bar(function<void()> printBar) {

		for (int i = 0; i < n; i++) {
			while (flag!=1) this_thread::yield();//flag=true,执行该线程
			// printBar() outputs "bar". Do not change or remove this line.
			printBar();
			flag = 0;
		}
	}
};
```

### 3.用mutex+条件变量

```c++
class FooBar {
private:
    int n;
    int count;
    std::condition_variable con1;
    std::condition_variable con2;
    std::mutex mymutex;
public:
    FooBar(int n) {
        this->n = n;
        count=1;
    }

    void foo(function<void()> printFoo) {
        
        for (int i = 0; i < n; i++) {
            std::unique_lock<std::mutex> lk(mymutex);
            con1.wait(lk,[this](){return count==1;});
        	// printFoo() outputs "foo". Do not change or remove this line.
        	printFoo();
            count++;
            con2.notify_one();
        }
    }

    void bar(function<void()> printBar) {
        
        for (int i = 0; i < n; i++) {
            std::unique_lock<std::mutex> lk(mymutex);
            con2.wait(lk,[this](){return count==2;});
        	// printBar() outputs "bar". Do not change or remove this line.
        	printBar();
            count--;
            con1.notify_one();
        }
    }
};
```

### 附测试例程

```c++
int main(int argc, char** argv) {
	FooBar yyd(10);
	std::function<void()> printFoo = []() {cout <<  " Foo "; };
	std::function<void()> printBar = []() {cout << " Bar "; };
	std::thread th[2];

	th[0] = std::thread(&FooBar::foo, &yyd, printFoo);
	th[1] = std::thread(&FooBar::bar, &yyd, printBar);


	for (auto& ts : th) {
		if (ts.joinable()) ts.join();
	}

	return 0;
}
```



## 5.H2O生成

### 1.mutex lock+条件变量

```c++
class H2O {
private:
	int count_H;
	mutex mymutex;
	std::condition_variable con1;
	std::condition_variable con2;
public:
	H2O() {
		count_H = 0;
	}

	void hydrogen(function<void()> releaseHydrogen) {
		unique_lock<mutex> lk(mymutex);
		con1.wait(lk, [this]() {return count_H < 2; });
		// releaseHydrogen() outputs "H". Do not change or remove this line.
		releaseHydrogen();
		count_H++;
		if (count_H == 2) {
			con2.notify_one();//唤醒一个O
		}
	}

	void oxygen(function<void()> releaseOxygen) {
		unique_lock<mutex>lk(mymutex);
		con2.wait(lk, [this]() {return count_H== 2; });
		// releaseOxygen() outputs "O". Do not change or remove this line.
		releaseOxygen();	
		con1.notify_one();//唤醒两个H
		con1.notify_one();
		count_H = 0;
	}

};
```

### 2.信号量

```c++
//limit_H是用于hydrogen进程互斥，最多两个进行
//limit_O是用于oxygen进程互斥，最多1个进行
//O、H是用于进程同步的，其实也是表资源量的（它们的一半就表示真实的资源量）
//hydrogen进程一次产生1个H，消耗半个O;oxygen进程一次产生1个O，消耗两个H
//但是没有半个信号量，同时乘2。即hydrogen进程一次产生2个H，消耗1个O;oxygen进程一次产生2个O，消耗4个H
#include<semaphore.h>
class H2O {
private:
    sem_t limit_O;
    sem_t limit_H;
    sem_t O;
    sem_t H;
public:
    H2O() {
        sem_init(&limit_O,0,1);
        sem_init(&limit_H,0,2);
        sem_init(&O,0,0);
        sem_init(&H,0,0);
    }

    void hydrogen(function<void()> releaseHydrogen) {
        sem_wait(&limit_H);
        sem_post(&H);
        sem_post(&H);
        sem_wait(&O);
        // releaseHydrogen() outputs "H". Do not change or remove this line.
        releaseHydrogen();
        sem_post(&limit_H);
    }

    void oxygen(function<void()> releaseOxygen) {
        sem_wait(&limit_O);
        sem_post(&O);
        sem_post(&O);
        sem_wait(&H);
        sem_wait(&H);
        sem_wait(&H);
        sem_wait(&H);
        // releaseOxygen() outputs "O". Do not change or remove this line.
        releaseOxygen();
        sem_post(&limit_O);
    }
};
```

### 3.mutex+flag+yield()

```c++
class H2O {
private:
	int h2;//flag
	mutex omutex;
	mutex hmutex;
public:
	H2O() {
		h2 = 0;
	}

	void hydrogen(function<void()> releaseHydrogen) {
		hmutex.lock();
		while (h2>1) this_thread::yield();//while里一定要写不满足时的条件
		// releaseHydrogen() outputs "H". Do not change or remove this line.
		releaseHydrogen();
		h2++;
		hmutex.unlock();
	}

	void oxygen(function<void()> releaseOxygen) {
		omutex.lock();
		while (h2 != 2) this_thread::yield();

		// releaseOxygen() outputs "O". Do not change or remove this line.
		releaseOxygen();
		h2=0;
		omutex.unlock();
	}
};
```

### 附测试例程

```c++
int main(int argc, char** argv) {
	H2O yyd;
  //setbuf(stdout, NULL);
  std::function<void()> releaseHydrogen=[](){cout<<" H ";};
  std::function<void()> releaseOxygen=[](){cout<<" O ";};
  const int temp=4;
	std::thread th[temp*3];
  for(int i=0;i<temp;i++){
    th[i]=std::thread(&H2O::oxygen,&yyd,releaseOxygen);
  }
for(int i=temp;i<temp*3;i++)
{
  th[i]=std::thread(&H2O::hydrogen,&yyd,releaseHydrogen);
}

	for (auto& ts : th) {
		if (ts.joinable()) ts.join();
	}

	return 0;
}
```



## 6.打印零与奇偶数

### 1.sem

```c++
//要注意结束时释放线程
#include<semaphore.h>
class ZeroEvenOdd {
private:
    int n;
    int cur;
    sem_t sem_e;
    sem_t sem_o;
    sem_t sem_z;
public:
    ZeroEvenOdd(int n) {
        this->n = n;
        cur=0;
        sem_init(&sem_e,0,0);
        sem_init(&sem_o,0,0);
        sem_init(&sem_z,0,1);
    }

    // printNumber(x) outputs "x", where x is an integer.
    void zero(function<void(int)> printNumber) {
        while(1)//
        {
            sem_wait(&sem_z);
            cur++;
            if(cur>n)
                break;
            printNumber(0);
            if(cur%2==0)
                sem_post(&sem_e);
            else
                sem_post(&sem_o);
        }
        sem_post(&sem_e);//用于让even和odd进程结束
        sem_post(&sem_o);
    }

    void even(function<void(int)> printNumber) {
        while(1)
        {
            sem_wait(&sem_e);
            if(cur>n) return;
            printNumber(cur);
            sem_post(&sem_z);
        }
    }

    void odd(function<void(int)> printNumber) {
        while(1)
        {
            sem_wait(&sem_o); //这里就用到了cur的非同步特性，wait的时候cur可能是0。解开的时候cur变成1了。
            if(cur>n) return;//这步要放在wait后边，为了最后结束线程
            printNumber(cur);
            sem_post(&sem_z);
        }
    }
};
```

### 2. mutex lock+条件变量

```c++
//要注意结束时释放线程
class ZeroEvenOdd {
private:
	int n;
	int cur;
	int flag;//0标志运行zero线程。2标志运行even线程。1标志运行odd线程
	mutex mymutex;
	condition_variable con_z;
	condition_variable con_e;
	condition_variable con_o;
public:
	ZeroEvenOdd(int n) {
		this->n = n;
		cur = 0;
		flag = 0;
	}

	// printNumber(x) outputs "x", where x is an integer.
	void zero(function<void(int)> printNumber) {
		while (1)
		{
			unique_lock<mutex> lk(mymutex);
			con_z.wait(lk, [this]() {return flag == 0; });
			cur++;
			if (cur > n) {break; }
			printNumber(0);
			if (cur % 2 == 1)
			{
				flag = 1;
				con_o.notify_one();
			}
			else
			{
				flag = 2;
				con_e.notify_one();
			}
		}

		flag = 2; con_e.notify_one();//退出的时候，还要记得把even和odd线程结束了。这里先结束even，在even里结束odd
	}

	void even(function<void(int)> printNumber) {
		while (1) {
			unique_lock<mutex> lk(mymutex);
			con_e.wait(lk, [this]() {return flag == 2; });
			if (cur > n) {  break; };
			//if(cur>n) break;
			printNumber(cur);
			flag = 0;
			con_z.notify_one();
		}
		flag = 1; con_o.notify_one();//退出的时候，把odd线程结束了
	}

	void odd(function<void(int)> printNumber) {
		while (1) {
			unique_lock<mutex> lk(mymutex);
			con_o.wait(lk, [this]() {return flag == 1; });
			if(cur>n) break;//不用再结束别的线程了,别的线程都退出了
			printNumber(cur);
			flag = 0;
			con_z.notify_one();
		}
	}
};
```

### 3.flag+yield()（一定得让zero做控制线程，因为它每次都要输出）

```c++
//要注意结束时释放线程
//这种可以放心大胆不要锁，是因为其他进程都在yield谦让，它们不会执行去修改数据。一种cur下，应该输出的进程没执行完，是不会往下走的。
class ZeroEvenOdd {
private:
	int n;
	int cur;
	int flag;

public:
	ZeroEvenOdd(int n) {
		this->n = n;
		cur = 0;
		flag = 0;
	}

	// printNumber(x) outputs "x", where x is an integer.
	void zero(function<void(int)> printNumber) {
		while (1)
		{
			while (flag != 0 && cur<=n) this_thread::yield();
			cur++;
			if (cur > n) break;
			printNumber(0);
			if (cur % 2 == 1)
			{
				flag = 1;
			}
			else
			{
				flag = 2;
			}
		}
	}

	void even(function<void(int)> printNumber) {
		while (1) {
			while (flag != 2 && cur<=n) this_thread::yield();
			if (cur > n) break;//even进程会自动结束
			printNumber(cur);
			flag = 0;//归还控制权
		}
	}

	void odd(function<void(int)> printNumber) {
		while (1) {
			while (flag != 1 && cur<=n) this_thread::yield();
			if (cur > n) break;//odd进程会自动结束
			printNumber(cur);
			flag = 0;
		}
	}
};
```

### 附测试例程

```c++
int main(int argc, char** argv) {
	ZeroEvenOdd yyd(15);
	std::function<void(int)> printNumber = [](int n) {cout << n << " "; };
	std::thread th[3];

	th[0] = std::thread(&ZeroEvenOdd::zero, &yyd, printNumber);
	th[1] = std::thread(&ZeroEvenOdd::even, &yyd, printNumber);
	th[2] = std::thread(&ZeroEvenOdd::odd, &yyd, printNumber);


	for (auto& ts : th) {
		if (ts.joinable()) ts.join();
	}

	return 0;
}
```



# 3. 重点模板（参看另一个markdown）

## 链表

- 递归遍历（前后序遍历）

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

- 迭代遍历

  - ```c
    vector<int> res;//保存遍历结果
    void traverse(ListNode* head){
        while(head){
            res.push_back(head->val);
            head=head->next;
        }
    }
    ```

- 迭代反转整个链表

  - ```c
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

  - ```c
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

  - 备注：经典模板，反转[a,b)上的节点。做这种题一定要切记[a,b)区间。反转以后的子序列，头部会由reverse函数返回。而子序列的尾部，就是a节点！

- 判断有没有环（快慢指针）

  - ```c++
    bool hasCycle(ListNode *head) {
        ListNode *slow,*fast;
        slow=head;fast=head;
    
        while(fast!=nullptr && fast->next!=nullptr){
            slow=slow->next;
            fast=fast->next->next;
            if(fast==slow)//切记，判断要写在两个指针更新指向的后边。写在前边的话，第一轮循环时fast肯定等于slow等于head。
            return true;
        }
        return false;
    }
    ```

- 寻找中间节点（快慢指针）

  - ```c++
    class Solution {
    public:
        ListNode* middleNode(ListNode* head) {
            ListNode *slow,*fast;
            slow=head;fast=head;
            while(fast!=nullptr && fast->next!=nullptr){
                slow=slow->next;
                fast=fast->next->next;
            }
            return slow;
        }
    };
    //链表只有一个节点时，将返回该该节点
    ```

  - 变形：偶数时，寻找中间靠右的节点；奇数时，寻找中间节点的右节点。（画画图就懂了）

  - ```c++
    //用于回文链表、重排链表等题目中
    ListNode* findMedium(ListNode* head){
        ListNode *slow=head;
        ListNode *fast=head;
        while(fast!=nullptr && fast->next!=nullptr){
            slow=slow->next;
            fast=fast->next->next;
        }
    
        if(fast!=nullptr)//这是用来区分奇偶的
            slow=slow->next;
        return slow;
    }
    //链表只有一个节点时，将返回nullptr
    ```

- 凡是 **涉及到多条链表合并成一个**的；**头结点不一定是谁的**；**有可能删除头节点**的。都建议使用dummy节点。

  - 用dummy时，dummy不要移动，构造一个pre=dummy，让pre去移动维系新链表。dummy->next用来在最后return

- 

## 树

- 二叉树迭代遍历（前中后序遍历）

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

    

- 二叉树递归遍历（前中后序遍历）

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

    - 补充：N叉树递归遍历（前后序遍历）

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

- 二叉树的层序遍历

  - ```c++
    //vector<vector<int>>式，要区分每一层
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
    
  - ```c++
    //vector<int>式，不用区分每一层
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
    ```
    
  - 备注：

    - 把底下这两句颠倒一下顺序，就是从右往左的层序遍历

    - ```c++
              if(root->left!=nullptr) q.push(root->left);
              if(root->right!=nullptr) q.push(root->right);
      ```

- 通用递归框架

  - ```c++
    TreeNode* func(TreeNode* root){
    	//base case。递归出口
    	if(xxx)
    		return;
        //前序位置
      	TreeNode* xxx=new TreeNode();
        //递归
        TreeNode* left=func(root->left);
        TreeNode* right=func(root->right);
        //后序位置
        if(left xxx)
            ...
        if(right xxxx)
            ...
        return root;
    }
    ```

- 三种遍历直观图

  - ![image-20210504201718475](https://i.loli.net/2021/05/04/peYPM4VJ9X5KDSv.png)
  
- 单节点的经典递归框架

  - ```c++
    //剑指offer27.镜像翻转
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

  - ```c++
    //剑指offer26.判断树的子结构
    bool trverse(TreeNode* A,TreeNode* B){
        if(A==nullptr)
        	return false;
        bool res=treeEqual(A,B);
        if(!res)//如果当前节点不相同，那就递归遍历左右子树
        	res=trverse(A->left,B) || trverse(A->right,B);
        return res;
    }
    ```
    
  - ```c++
    //返回void的遍历
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
    
    //返回TreeNode*的遍历
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
    ```

- 双节点的经典递归框架

  - ```c++
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

  - ```c++
    //剑指offer第28.对称的二叉树
    class Solution {
    public:
        bool isSymmetric(TreeNode* root) {
            if(root==nullptr)
                return true;
            return isSym(root->left,root->right);
        }
        bool isSym(TreeNode* node1,TreeNode* node2){
            if(node1==nullptr && node2==nullptr)//两个节点都为空
                return true;
            if(node1==nullptr || node2==nullptr)//有一个为空
                return false;
            //两个节点都不为空时
            if(node1->val!=node2->val)//前序遍历
                return false;
            //说明node1->val==node2->val
            bool tmp1=isSym(node1->left,node2->right);//node1的左和node2的右
            bool tmp2=isSym(node1->right,node2->left);//node1的右和node2的左
            return tmp1&&tmp2;
        }
    };
    ```

- 二叉搜索树

  - 中序递归，升序遍历

    - ```c++
      void traverse(TreeNode root) {
          if (root == null) return;
          traverse(root.left);
          // 中序遍历代码位置
          print(root.val);
          traverse(root.right);
      }
      ```

  - 降序遍历

    - ```c++
      void traverse(TreeNode root) {
          if (root == null) return;
          // 先递归遍历右子树
          traverse(root.right);
          // 中序遍历代码位置
          print(root.val);
          // 后递归遍历左子树
          traverse(root.left);
      }
      ```

- 针对二叉搜索树的遍历框架

  - ```c++
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
        if(root->bal>target)//当前值大于目标，那么去左子树找
            root->left=BST(root->left,target);
        return root;
    }
    ```
  
- 二叉搜索树的有效性（二叉搜索树的定义）

  - 对于任何一个节点root，root的左子树节点值<=root的值。root的右子树节点值>=root的值。
  
  - 具体到每个节点就是：
  
    - 当前节点root的左节点的最小值下界是之前记录的min（可能是nullptr或者是root的父节点），最大值上界是当前节点root。
    - 当前节点root的右节点的最小值下界是当前节点root，最大值上界是之前记录的max（可能是nullptr或者是root的父节点）
  
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
  
- 求树高的框架

  - ```c++
    //这其实就是后序版本的算树高的扩展。使用一个变量来维系，而不是用返回值。即底下的第二种解法
    
    //算树高--1.使用返回值计算
    int maxDep(TreeNode* root){
        if(root==nullptr)
            return 0;
        int left=maxDep(root->left);
        int right=maxDep(root->right);
        return 1+(left>right?left:right);//后序遍历
    }
    
    //算树高--2.使用一个变量来维系
    class Solution {
    public:
        int maxDepth(TreeNode* root) {
            int depth;
            traverse(root,depth);
            return depth;
        }
        void traverse(TreeNode* root,int& depth){
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

  - 注意：第二个写法，经常使用。它其实把把前序遍历变成后序遍历，让 `traverse` 函数把辅助函数做的事情顺便做掉。如果traverse时除了需要返回树高，还要返回别的东西 比如true false，那么非常建议使用第二种求树高的方法。

    - 比如  剑指 Offer 55 - II. 平衡二叉树

    


## 双指针

- 快慢指针

  - 判断链表有没有环（快慢指针）

  - ```c
    bool hasCycle(ListNode *head) {
        ListNode *slow,*fast;
        slow=head;fast=head;
    
        while(fast!=nullptr && fast->next!=nullptr){
            slow=slow->next;
            fast=fast->next->next;
            if(fast==slow)//切记，判断要写在两个指针更新指向的后边。写在前边的话，第一轮循环时fast肯定等于slow等于head。
            return true;
        }
        return false;
    }
    ```

- 二叉树

  - 

# 4.相关函数

## 1.stl

- stack
  - push、top、pop、empty、size
- queue
  - push、front、pop、empty、size
- deque（双端队列）
  - push_back、back、pop_back、push_front、front、pop_front、empty、size
- priority_queue（优先队列）：默认大顶堆
  - push、top、pop、empty、size 
- vector（动态数组）
  - push_back、back、pop_back、front、empty、size
  - 备注：
    - vector没有push_front和pop_front
- unordered_set（哈希集合）
  - count、insert、erase、empty、size
  - `size_type count(const key_type& key)`：判断key是否存在于set中。count的结果只可能是0或者1，因为set中不会出现重复的key
  - `pair<iterator,bool> insert(const key_type& key)`：向集合中插入一个元素key
  - `size_type erase(const key_type& key)`：删除哈希集合中的元素key
- unordered_map（哈希表）
  - count、erase、empty、size
  - `size_type count(const key_type& key)`：判断key是否存在于hash表中。count的结果只可能是0或者1，因为hash表中不会出现重复的key
  - `size_type erase(const key_type& key)`：通过key清除哈希表中的键值对
  - 备注：
    - 使用方括号[]访问unordered_map中的键key时，如果key不存在，会自动创建key，对应的值为值类型的默认值
- - 

## 2.include

```c++
#include<iostream>  //cin cout
#include<list>   // list  双向链表
#include<unordered_map>   //无序的hashmap
#include<vector>  //vector  动态数组
#include<limits.h>   //INT_MAX、LONG_LONG_MAX、__DBL_MAX__
#include<sstream>    //istringstream
#include<algorithm>   //sort
```



## 3.函数

### 1.stoi

- 字符串string转化为int

  - ```c++
    string str="12";
    int a=stoi(str);
    ```

  - 注意：数字超出Int表示范围会报异常

- 字符串`char*`转为int

  - ```c++
    const char* str="12";
    int a=atoi(str);
    ```

### 2.getline

默认分隔符为回车。

- 从标准输入读入数据

  - ```c++
    string buf;
    while(getline(cin,buf,','))//以','为分割，从标准输入读入数据
        cout<<buf<<" ";
        //ctrl+D可以打破循环，ctrl+D相当于是EOF
    ```

- 从其他istream读入数据

  - ```c++
    string buf;
    string str="1,#,2,%,3,"
    istringstream iss(str);
    while(getline(iss,buf,','))//以','为分割，从istringstream读入数据
    	cout<<buf<<" ";
    ```

- 完整的例子

  - getline(cin, line)读出一整行

  - ```c++
    int main(){
        int C;
        cin>>C;
        cin.ignore();//cin之后还有getline，切记cin之后加一句cin.ignore();消除回车
        
        vector<int> W;
        string line;
        getline(cin,line);
        string buf;
        istringstream iss(line);
        while(getline(iss,buf,','))
            W.push_back(stoi(buf));
        
        vector<int> V;
        getline(cin,line);
        iss= istringstream(line);
        while(getline(iss,buf,','))
            V.push_back(stoi(buf));
    }
    ```

- 笔试建议

  - ```c++
    //split函数
    vector<int> split(string str, char pattern) {
    	vector<int> result;
    	string buf;
    	istringstream iss(str);
    	while (getline(iss, buf, pattern))
    		result.push_back(stoi(buf));
    	return result;
    }
    
    
    int main() {
    	int bagweight = 0;
    	string input1;
    	string input2;
    	cin >> bagweight;
    	cin >> input1;
    	cin >> input2;
    	vector<int> weight = split(input1, ',');
    	vector<int> value = split(input2, ',');
    }
    //4
    //2,1,3
    //4,2,3
    ```

    

### 3.reverse

- reverse：翻转容器里的数据

  - ```
    std::reverse(trianglePoints.begin(), trianglePoints.end());
    ```

- reverse_copy：不是改变原来的容器，而是翻转之后放在新容器里面

  - ```
    reverse_copy ( BidirectionalIterator first,
                                    BidirectionalIterator last, OutputIterator result );
    ```

### 4.典型数据类型的最大值最小值

`#include<limits.h> `

- int的最大值
  - `int a=INT_MAX`
- int的最小值
  - `int a=INT_MIN;`
- double的最大值
  - `double a=DBL_MAX`
- double的最小值
  - `double a=-DBL_MAX`（注意不是DBL_MIN)

### 5.排序sort

**除了优先队列：都是greater表示降序，less表示升序**

- 升序排序

  - ```c++
    vector<int> nums{3,1,2,5};
    sort(nums.begin(),nums.end(),less<int>());
    ```

- 降序排序

  - ```c++
    vector<int> nums{3,1,2,5};
    sort(nums.begin(),nums.end(),greater<int>());
    ```

- `equal_to<Type>、not_equal_to<Type>、greater<Type>、greater_equal<Type>、less<Type>、less_equal<Type>`
- 比较函数返回true说明，第一个数放在第二个数前边

### 5.stl对容器内元素求和accumulate

- accumulate

  - ```c++
    #include <numeric>
    int sum = accumulate(vec.begin() , vec.end() , 0);  
    ```

  - accumulate带有三个形参：头两个形参指定要累加的元素范围，第三个形参则是累加的初值。

### 6.stl求容器内元素的最大值最小值

- max_element

  - ```c++
    #include<algorithm> 
    
    vector<int> vec = {1,2,3,4,5};
    auto maxPosition = max_element(vec.begin(), vec.end());//迭代器类型
    int maxVal = *max_element(vec.begin(),vec.end());//最大值 5
    int index= maxPosition - vec.begin();//最大值的下标 4
    ```

- min_element

  - ```c++
    #include<algorithm> 
    
    vector<int> vec = {1,2,3,4,5};
    auto maxPosition = min_element(vec.begin(), vec.end());//迭代器类型
    int minVal = *min_element(vec.begin(),vec.end());//最小值 1
    int index= maxPosition - vec.begin();//最大值的下标 0
    ```


### 7.priority_queue

**除了优先队列：都是greater表示降序，less表示升序**

默认是less，即大顶堆（降序）

- 大顶堆

  - ```c++
    priority_queue<int, vector<int>, less<int> > a
    ```

- 小顶堆

  - ```c++
    priority_queue<int, vector<int>, greater<int> > a
    ```

- pair

  - ```c++
    priority_queue<pair<int, int>,vector<pair<int, int>>,less<pair<int, int>>> q;
    ```

### 8.prev

- 取最后一个元素
  - `int curMin=*prev(myset.end());`

## 4.人工实现函数+小trick

### 1.人工实现split函数

- 第一种实现

  - 遍历

  - ```c++
    //存到queue里。以逗号为分隔符，split str。
    queue<string> split(const string& str){
        queue<string> res;
        if(str.empty()) 
            return res;
        int pre=0;
        for(int i=0;i<=str.size();i++){//string不管是不是以逗号结尾都没关系。因为i<=str.size()来保证了
            if(str[i]==','||i==str.size()){
                res.push(str.substr(pre,i-pre));
                pre=i+1;
            }
        }
        return res;
    }
    ```

  - ```c++
    //存到vector里。以逗号为分隔符，split str。
    vector<string> split(const string& str){
        vector<string> res;
        if(str.empty()) 
            return res;
        int pre=0;
        for(int i=0;i<=str.size();i++){
            if(str[i]==','||i==str.size()){
                res.push_back(str.substr(pre,i-pre));
                pre=i+1;
            }
        }
        return res;
    }
    ```

- 第二种实现

  - getline函数

  - ```c++
    #include<iostream>
    #include<string>
    #include<vector>
    #include<sstream>
    using namespace std;
    
    vector<string> split(const string& str){//string不管是不是以逗号结尾都没关系
        stringstream ss(str);
        vector<string> res;
        string buf;
        while(getline(ss, buf,',')){
            res.push_back(buf);
        }
        return res;
    }
    
    int main(){
        string test="1,2,3,#,%,90,22,#,#,";
        vector<string> tmp=split(test);
        for(auto i:tmp){
            cout<<i<<",";
        }
        cout<<endl;
        return 0;
    }
    ```
    
  - 备注：
  
    - ```c++
      //使用istringstream类也可以
      vector<string> split(const string& str){//string不管是不是以逗号结尾都没关系
          istringstream iss(str);
          vector<string> res;
          string buf;
          while(getline(iss, buf,',')){
              res.push_back(buf);
          }
          return res;
      }
      ```
  
      
  
    

### 2.将二维数组映射到一维数组，利用方向数组 `d` 来简化上下左右多方向邻接遍历

- 二维数组映射到一维数组

  - 二维坐标 `(x,y)` 可以转换成 `x * n + y` 这个数（`m` 是棋盘的行数，`n` 是棋盘的列数）。敲黑板，**这是将二维坐标映射到一维的常用技巧**。

- 利用方向数组 `d` 来简化上下左右多方向邻接遍历

  - ```c++
    // 方向数组 d 是上下左右搜索的常用手法
    vector<vector<int>> d{{1,0}, {0,1}, {0,-1}, {-1,0}};
    for (int i = 0; i < M; i++){
        for (int j = 0; j < N; j++){
            if(board[i][j] == 'O')//i,j是O
                for (int k = 0; k < 4; k++) {// 将此 O 与上下左右的 O 连通
                    int x = i + d[k][0];
                    int y = j + d[k][1];
                    if(x<0 || x>=M || y<0 || y>=N)
                        continue;
                    if (board[x][y] == 'O')//x,y也是O
                        uf.unionNode(x * N + y, i * N + j);
                }
        }
    }
    ```


### 3.**二维数组有「上下左右」的概念，压缩成一维后，如何得到某一个索引上下左右的索引**？

```c++
//对于一个2*3的矩阵，拉成一维向量后，怎么表示每个下标的邻居
vector<vector<int>> neighbor = {
    { 1, 3 },
    { 0, 4, 2 },
    { 1, 5 },
    { 0, 4 },
    { 3, 1, 5 },
    { 4, 2 }
};
```

![image-20210519221430456](http://pichost.yangyadong.site/img/image-20210519221430456.png)

### 4.奇偶数组找中位数的trick，避免奇偶讨论

使用一个小trick，可以避免讨论奇偶：
我们分别找第 (m+1)/2个数，和(m+2)/2个数，然后求其平均值即可，这对奇偶数均适用。假如 m 为奇数的话，那么其实 (m+1) / 2 和 (m+2) / 2 的值相等，相当于两个相同的数字相加再除以2，还是其本身。

### 5.二分查找，乘以个符号，本来处理升序的，就可以处理降序了

```c++
int binarySearch(int target, MountainArray &mountainArr,int left,int right,int sign){
    //sigh等于1时处理升序；等于-1处理降序。
    target=target*sign;
    while(left<=right){
        int mid=left+(right-left)/2;
        int curMid=mountainArr.get(mid)*sign;
        if(curMid==target)
            return mid;
        else if(curMid>target)
            right=mid-1;
        else if(curMid<target)
            left=mid+1;
    }
    return -1;
}
```

### 6.单调队列

https://leetcode-cn.com/problems/sliding-window-maximum/solution/dan-diao-dui-lie-by-labuladong/

```c++
class MonotonicQueue {
private:
    deque<int> data;
public:
    void push(int n) {
        while (!data.empty() && data.back() < n){//单调队列的 push 方法依然在队尾添加元素，但是要把前面比新元素小的元素都删掉：你可以想象，加入数字的大小代表人的体重，把前面体重不足的都压扁了，直到遇到更大的量级才停住。
            data.pop_back();
        }
        data.push_back(n);
    }
    
    int max() { return data.front(); }
    
    void pop(int n) {
        if (!data.empty() && data.front() == n){//之所以要判断 data.front() == n，是因为我们想删除的队头元素 n 可能已经被「压扁」了，这时候就不用删除了：
            data.pop_front();
        }
    }
};
```

### 7.取余

- 模运算与基本四则运算有些相似，但是除法例外。其规则如下：
  -  **(a + b) % p = (a % p + b % p) % p** 
  - (a - b) % p = (a % p - b % p) % p 
  - **(a * b) % p = (a % p  *   b % p) % p** 
  - a ^ b % p = ((a % p)^b) % p   没用
  - 结合律： **(a+b+c) % p=((a+b)%p + c)%p = (a + (b+c)%p)% p = =(a%p + b%p +c%p )%p**
  - ((a*b) % p * c)% p = (a * (b*c) % p) % p
  - 交换律： (a + b) % p = (b+a) % p 
  - (a * b) % p = (b * a) % p 
  - 分配律： ((a +b)% p * c) % p = ((a * c) % p + (b * c) % p) % p 



# 5.总结

- 链表
  - 遍历、反转、部分反转、快慢双指针（链表找中点、回文链表找中点、判断链表有没有环、找环形链表的环入口）、等速双指针（返回倒数第k个节点、合并两个有序链表）
  - 凡是 **涉及到多条链表合并成一个**的；**头结点不一定是谁的**；**有可能删除头节点**的。都建议使用dummy节点。
    - 用dummy时，dummy不要移动，构造一个pre=dummy，让pre去移动维系新链表。dummy->next用来在最后return
  - 特别小心：链表最末尾设置nullptr、涉及到两段链表合并的一定要注意第二段链表的开头节点(后继者，successor)保存了没有
  - 对应剑指offer上的题目：6、18、22、23、24、25、52
-    树
  - 一般是单节点递归。偶尔会用双节点的。要自己判断清楚单节点能不能完成任务。
  
  - 二叉树递归遍历（前中后序遍历）、N叉树递归遍历（前后序遍历）、二叉树的层序遍历、通用递归框架
  
  - 凡是要删除、插入某个节点时，一定要用这种递归模板
  
    - ```c++
      //返回TreeNode*的遍历
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
      ```
  
    - 例如：450.删除二叉搜索树中的节点、701. 二叉搜索树中的插入操作、
  
  - 针对二叉搜索树的遍历框架
  
    - ```c++
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
          if(root->bal>target)//当前值大于目标，那么去左子树找
              root->left=BST(root->left,target);
          return root;
      }
      ```
  
  - **以我的刷题经验，我们要尽可能避免递归函数中调用其他递归函数**，如果出现这种情况，大概率是代码实现有瑕疵。尝试**把前序遍历变成后序遍历，让 `traverse` 函数把辅助函数做的事情顺便做掉**。
- 二叉搜索树
  - 二叉搜索树与树不太一样。二叉搜索树有时候不能用递归框架。
  - BST 相关的问题，要么利用 BST 左小右大的特性提升算法效率，要么利用中序遍历的特性满足题目的要求
  - 升序和降序都要掌握。
  - 

# 6.刷leetcode时的error

- heap-use-after-free
  - 单链表结尾没有nullptr
  - （剑指Offer第18题）莫名bug，不要delete节点
  - for循环上的变量尺寸在变化。
    - `比如for(auto i:vec)  vec.push_back(...)`;
- heap-buffer-overflow
  - 数组访问越界
- member access within null pointer of type 'ListNode'
  - 访问了nullptr->next
- stack-overflow
  - 无限递归
- runtime error: addition of unsigned offset to 0x602000000130 overflowed to 0x60200000012c (stl_vector.h)
  - vector的下标有问题
  - 是不是把字符串和int搞错了

