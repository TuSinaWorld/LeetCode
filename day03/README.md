# Day03

####  **203.移除链表元素** 

该题不难(说是不难,但并非删除单个节点而是所有符合要求的节点还是让我愣了一下),就是我用的解法并非虚拟头节点,而是特殊情况处理.不过虚拟头节点的解法我也掌握,就懒得再写一遍了(≧∇≦)ﾉ

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
struct ListNode* removeElements(struct ListNode* head, int val) {
    while(head != NULL && head -> val == val){
        struct ListNode* tmp = head;
        head = head -> next;
        free(tmp);
    }
    if(head == NULL){
        return head;
    }
    for(struct ListNode* slow = head,*fast = head -> next;fast != NULL;){
        if(fast -> val == val){
            struct ListNode* tmp = fast;
            slow -> next = fast -> next;
            fast = fast -> next;
            free(tmp);
        }else{
            fast = fast -> next;
            slow = slow -> next;
        }
    }
    return head;
}
```



#### **707.设计链表** 

相比于上道题的举足轻重,这题差点写的我吐血...

主要是刚转c不久,动态定义结构体的语法老是出点细微的小错误,比如next没指向NULL啥的.别看这不起眼,真改起来差点废了我半条命...

不过好在,还是写出来了...有算法思路的情况下哪怕语法有点问题也还是能改出来的...就是这时间...

##### 解法:

```c
typedef struct MyLinkedList{
    int val;
    struct MyLinkedList *next;
} MyLinkedList;


MyLinkedList* myLinkedListCreate() {
    MyLinkedList *head = (MyLinkedList*)malloc(sizeof(MyLinkedList));
    head -> next = NULL;
    return head;
}

int myLinkedListGet(MyLinkedList* obj, int index) {
    MyLinkedList* temp = obj -> next;
    for(int i = 0;;i++,temp = temp -> next){
        if(temp == NULL){
            return -1;
        }else if(i == index){
            return temp -> val;
        }
    }
    return -1;
}

void myLinkedListAddAtHead(MyLinkedList* obj, int val) {
    MyLinkedList *node = (MyLinkedList*)malloc(sizeof(MyLinkedList));
    node -> val = val;
    node -> next = obj -> next;
    obj -> next = node;
}

void myLinkedListAddAtTail(MyLinkedList* obj, int val) {
    MyLinkedList *node = (MyLinkedList*)malloc(sizeof(MyLinkedList));
    node -> val = val;
    node -> next = NULL;
    MyLinkedList* temp = obj;
    for(;temp -> next != NULL;temp = temp -> next);
    temp -> next = node;
}

void myLinkedListAddAtIndex(MyLinkedList* obj, int index, int val) {
    MyLinkedList* temp = obj;
    for(int i = 0;i <= index && temp != NULL;i++,temp = temp -> next){
        if(i == index){
            MyLinkedList* node = (MyLinkedList*)malloc(sizeof(MyLinkedList));
            node -> val = val;
            node -> next = temp -> next;
            temp -> next = node;
        }
    }
}

void myLinkedListDeleteAtIndex(MyLinkedList* obj, int index) {
    MyLinkedList* temp = obj;
    for(int i = 0;i <= index && temp != NULL;i++,temp = temp -> next){
        if(i == index){
            if(temp -> next == NULL){
                return;
            }
            MyLinkedList* temp2 = temp -> next;
            temp -> next = temp2 -> next;
            free(temp2);
        }
    }
}

void myLinkedListFree(MyLinkedList* obj) {
    while(obj != NULL){
        MyLinkedList* temp = obj;
        obj = obj -> next;
        free(temp);
    }
}

/**
 * Your MyLinkedList struct will be instantiated and called as such:
 * MyLinkedList* obj = myLinkedListCreate();
 * int param_1 = myLinkedListGet(obj, index);
 
 * myLinkedListAddAtHead(obj, val);
 
 * myLinkedListAddAtTail(obj, val);
 
 * myLinkedListAddAtIndex(obj, index, val);
 
 * myLinkedListDeleteAtIndex(obj, index);
 
 * myLinkedListFree(obj);
*/
```



#### **206.反转链表** 

这道题我是直接头插法秒了,看到解析后才发现还有空间复杂度为O(1)的解法,有时间我就把新解法补上[]~(￣▽￣)~*

##### 解法:

```c
#include <stdio.h>
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode* first = (struct ListNode*)malloc(sizeof(struct ListNode));
    first -> next =NULL;
    while(head != NULL){
        struct ListNode* temp = head;
        head = head -> next;
        temp -> next = first -> next;
        first -> next = temp;
    }
    return first -> next;
}
```



#### 总结:

今天的题目难度并不高,重点是掌握链表的一些基础操作,至于我的话...除了熟悉了算法,可能还熟悉了语法😂(转语言的无奈).