# DAY02

#### 977.有序数组的平方

该题目不难,使用双指针同时从头和尾向中间遍历并将较大平方放至新数组末尾即可.

不过我做的时候思路走了点弯路,想先找到距离0最近的数在放至新数组头部,然后指针向两边遍历...但实际操作起来要判断的情况多了不少,且凭空把O(n)变成了"O(2n)",代码还欠缺美观性,还是感觉不行的.

##### 解法:

- 时间复杂度：O(n)

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* sortedSquares(int* nums, int numsSize, int* returnSize) {
    (*returnSize) = numsSize;
    int* arr = (int*)malloc(sizeof(int) * numsSize);
    for(int left = 0,right = numsSize - 1,result = numsSize - 1;left <= right;){
        if(nums[left] * nums[left] > nums[right] * nums[right]){
            arr[result--] = nums[left] * nums[left++];
        }else{
            arr[result--] = nums[right] * nums[right--];
        }
    }
    return arr;
}
```



#### 209.长度最小的子数组

做这道题时一开始思路就有,也大致清楚滑动指针的概念,也清楚要以循环代表的应该是终止点...

但起始点如何处理,和如何处理我却一脸懵逼(っ °Д °;)っ

我的思路是每次满足和大于target时起始点后移,再求新数组的和.然而这样求和本身又是一层循环,这里卡了半天,看到解析的sum -= nums[i++];才发现自己的思路有点...傻😂.

不过这一个脑抽点过去后该题就畅通无阻了~

##### 解法:

- 时间复杂度：O(n)

```c
int minSubArrayLen(int target, int* nums, int numsSize) {
    int length = 0;
    int result = 0;
    int sum = 0;
    for(int end = 0,start = 0;end >= start && end < numsSize;end++){
        sum += nums[end];
        length++;
        while(sum >= target){
            result = (result == 0 || result > length) ? length : result;
            length--;
            sum -= nums[start++];
        }
    }
    return result;
}
```



####  59.螺旋矩阵II

与前两题比起来,这题真是卡了我半天还死活解答不出来...阐述了什么叫一通操作猛如虎,抬头一看满屏红😂

刚开始尝试用递归的思路去做...然后不出意外的出意外了...条件判断写成小山一般,结果还死活通不过...

不过看了解析后茅塞顿开,这种对题目规律总结的准确度与写出简洁美观代码的能力的确是我所欠缺的...

题目思路基本上就是模拟矩阵生成的规律,一圈圈往下走,同时每圈都保持左闭右开,确保不发生边界混乱的问题~

知道这点此题就不难了~

(还能怎么办,多练了只能~)

##### 解法:

- 时间复杂度 O(n^2)

```c
int** generateMatrix(int n, int* returnSize, int** returnColumnSizes){
    int** arr = (int**)malloc(sizeof(int*) * n);
    *returnSize = n;
    *returnColumnSizes = (int*)malloc(sizeof(int) * n);
    for(int i = 0;i < n;i++){
        *(arr + i) = (int *)malloc(sizeof(int) * n);
        *((*returnColumnSizes) + i) = n;
    }
    int loop = n / 2;
    int mid = n / 2;
    int offset = 1;
    int startX = 0,startY = 0;
    int count = 1;
    while(loop--){
        int i,j;
        for(i = startX;i < n - offset;i++){
            arr[startY][i] = count++;
        }
        for(j = startY;j < n - offset;j++){
            arr[j][i] = count++;
        }
        for(;i > startX;i--){
            arr[j][i] = count++;
        }
        for(;j > startY;j--){
            arr[j][i] = count++;
        }
        startX++;startY++;offset++;
    }
    if(n % 2){
        arr[mid][mid] = count;
    }
    return arr;
}
```



#### 总结:

能感觉得出来自己基础还是比较薄弱的那一档,而且对于算法的核心不够清晰明了,对算法题目规律的总结有待加强.今天刷题的滑动窗口之巧妙与螺旋矩阵代码之干净简洁都给我留下了很深的印象,还是希望自己有朝一日也能成为秒杀这种题目的大佬?😂

至于数组总结...今天实在太晚了,先欠一会,只能看后续时间安排了(≧∇≦)ﾉ.