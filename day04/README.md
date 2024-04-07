# DAY04

#### 24. 两两交换链表中的节点

该题不难,重要的是思路和顺序的理解.

(不过我这直接多定义了几个临时节点直接忽略了顺序这一步,也算奇葩了😂)

不过看完解释后还是清楚最小空间消耗时顺序的处理情况的.

##### 解法:

- 时间复杂度：O(n)

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* swapPairs(struct ListNode* head) {
    struct ListNode* cur = (struct ListNode*)malloc(sizeof(struct ListNode));
    cur -> next = head;
    if(head == NULL || head -> next == NULL){
        return head;
    }
    struct ListNode* p1 = cur;
    struct ListNode* p2 = head;
    struct ListNode* p3 = head -> next;
    struct ListNode* p4 = head -> next -> next;
    while(p3 != NULL){
        p1 -> next = p3;
        p3 -> next = p2;
        p2 -> next = p4;
        if(p2 -> next != NULL && p2 -> next -> next != NULL){
            p1 = p2;
            p2 = p2 -> next;
            p3 = p2 -> next;
            p4 = p3 -> next;
        }else{
            break;
        }
    }
    return cur -> next;
}
```



#### 19.删除链表的倒数第N个节点

这题对我而言并不难,思路非常明确:快慢指针确定倒数第N节点前一个节点即可

##### 解法:

- 时间复杂度: O(n)

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* removeNthFromEnd(struct ListNode* head, int n) {
    struct ListNode* flag = (struct ListNode*)malloc(sizeof(struct ListNode));
    flag -> next = head;
    struct ListNode* fast = flag;
    struct ListNode* slow = flag;
    for(int i = 0;i < n;i++){
        if(fast == NULL){
            return head;
        }
        fast = fast -> next;
    }
    while(fast -> next != NULL){
        fast = fast -> next;
        slow = slow -> next;
    }
    struct ListNode* temp = slow -> next;
    slow -> next = temp -> next;
    return head;
}
```

#### 面试题 02.07. 链表相交

该题不难,无非是基本的快慢指针确定相交点,只需确认长度差值即可秒杀.

##### 解法:

- 时间复杂度：O(n + m)

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    struct ListNode *headAA = headA;struct ListNode *headBB = headB;
    int flagA = 0,flagB = 0;
    while(headAA != NULL){
        flagA++;
        headAA = headAA -> next;
    }
    while(headBB != NULL){
        flagB++;
        headBB = headBB -> next;
    }
    headAA = headA;headBB = headB;
    if(flagA < flagB){
        int temp = flagA;struct ListNode *tempL = headAA;
        flagA = flagB;headAA = headBB;
        flagB = temp;headBB = tempL;
    }
    int gap = flagA - flagB;
    for(int i = 0;i < gap;i++){
        headAA = headAA -> next;
    }
    while(headAA != NULL && headBB != NULL){
        if(headAA == headBB){
            return headAA;
        }
        headAA = headAA -> next;headBB = headBB -> next;
    }
    return NULL;
}
```



#### 142.环形链表II

相比前几道题,这道算是上强度了

虽然之前刷过一遍这题,但做着做着居然丢了思路...真是要那啥了😂

这题重点是要推断出第一次相遇时slow指针的运动长度等于n圈(f-s = nb,f = 2s),差a到入环点,推断出这个就不难解决了.

##### 解法:

- 时间复杂度: O(n)

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode *detectCycle(struct ListNode *head) {
    //解题关键:得出f-s = nb,即s = nb;
    struct ListNode *fast = head;
    struct ListNode *slow = head;
    while(fast != NULL && fast -> next != NULL && fast -> next -> next != NULL){
        fast = fast -> next -> next;
        slow = slow -> next;
        if(fast == slow){
            fast = head;
            while(fast != slow){
                fast = fast -> next;
                slow = slow -> next;
            }
            return slow;
        }
    }
    return NULL;
}
```



#### 心得:

这次的重点是快慢指针和链表节点更改的办法,仔细琢磨后还是不难的.

这次刷的题目对我来说并不算难,但这时间确实有点捉襟见肘,不过我会尽一切可能跟上的[]~(￣▽￣)~*

还有就是刷过的题居然思路丢的这么快,看样子刷完这一遍二刷和三刷还是很有必要的~~