# DAY01

#### 二分查找法

​				----704. 二分查找

二分查找法不用多说,老朋友了,但左闭右开的格式还是有点模糊了,这次温习了一下

##### 左闭右闭:

指目标target在左闭右闭的区间内.

- 算法时间复杂度:O(log n)
- 算法空间复杂度:O(1)

代码实现(c):

```c
int search(int* nums, int numsSize, int target) {
    for(int left = 0,right = numsSize - 1;left <= right;){
        int mid = (left + right) / 2;
        if(*(nums + mid) == target){
            return mid;
        }else if(*(nums + mid) < target){
            left = mid + 1;
        }else{
            right = mid - 1;
        }
    }
    return -1;
}
```

##### 左闭右开:

指目标target在左闭右开的区间内(不可能为右边界值).

- 算法时间复杂度:O(log n)
- 算法空间复杂度:O(1)

代码实现(c):

```c
int search(int* nums, int numsSize, int target) {
    for(int left = 0,right = numsSize;left < right;){
        int mid = (left + right) / 2;
        if(target < *(nums + mid)){
            right = mid;
        }else if(target > *(nums + mid)){
            left = mid + 1;
        }else{
            return mid;
        }
    }
    return -1;
}
```

##### 二分查找变种:

基于左开右闭基础上一种每次少比较一次的变种方法

- 算法时间复杂度:O(log n)
- 算法空间复杂度:O(1)

代码实现(c):

```c
其它(1):
int search(int* nums, int numsSize, int target) {
    int mid = (numsSize - 1) / 2;
    for(int left = 0,right = numsSize - 1;left < right;){
        if(target > *(nums + mid)){
            left = mid + 1;
        }else{
            right = mid;
        }
        mid = (left + right) / 2;
    }
    return (*(nums + mid) == target ? mid : -1);
}
```

#### 移除元素(双指针法):

​				----27. 移除元素

双指针对我来说也算半个老朋友了,使用频率很高,对于这道题而言,因为不限顺序,我一开始想到的就是一个指针指向数组最前,另一个指向最后,最少交换位置的解法(简单的那种快慢指针解法反而没想到[]~(￣▽￣)~*),只能说确实还要加强练习~

##### 解法1:

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```c
int removeElement(int* nums, int numsSize, int val) {
    int fast = 0,slow = 0;
    while(fast < numsSize){
        while(fast < numsSize && nums[fast] == val){
            fast++;
        }
        if(fast >= numsSize)
            break;
        nums[slow++] = nums[fast++];
        
    }
    return slow;
}
```

因为刚开始没想到该种解法,看到文章的示意图急匆匆来做的,所以解法思路相对有点混乱,是在fast指向val时将fast往前移动,但这样其实要多做好几次越界判断,并不如直接移动fast,在fast不指向val时将slow前移的思路.但差距并不算太大,也就懒得改了(≧∇≦)ﾉ.

##### 解法2:

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```c
int removeElement(int* nums, int numsSize, int val) {
    int left,right;
    for(left = 0,right = numsSize - 1;left <= right;){
        while(left <= right && nums[left] != val)
            left++;
        while(left <= right && nums[right] == val)
            right--;
        if(left < right)
            nums[left++] = nums[right--];
    }
    return left;
}
```



做这个解法的刚开始时,我试图把整个循环的条件控制成left < right,然后交换(对,是交换)nums[left]和nums[right]的值,这样就能让nums[left] != val和nums[right] == val满足,然而这种解法忽略了数组长度为1时的情况...这种情况下循环根本不会进入,left将恒为0,其实做特殊判断也大概率可行,但我还是希望我的代码完美一点,于是就有了这一份解答.



#### 心得:

第一天的题目难度确实不是太高,但一写就暴露出来我的很多短板,还是希望自己能坚持下去,提高算法能力的~
