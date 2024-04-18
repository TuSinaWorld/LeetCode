# Day07

最近因为毕业论文忙的有点。。。故落后了好多好多进度，唉，只能尽力赶了。

####  第454题.四数相加II

该题不难，就是map的简单应用，比起最后两道三数，四数之和简直是简单到不能再简单了。。。

##### 解法：

- 时间复杂度: O(n^2)

```java
public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
    HashMap<Integer, Integer> hashMap = new HashMap<>();
    for(int i : nums1){
        for(int j : nums2){
            if(hashMap.containsKey(i + j)){
                hashMap.put(i + j,hashMap.get(i + j) + 1);
            }else {
                hashMap.put(i + j,1);
            }
        }
    }
    int sum = 0;
    for(int i2 : nums3){
        for(int j2 : nums4){
            if(hashMap.containsKey(-(i2 + j2))){
                sum += hashMap.get(-(i2 + j2));
            }
        }
    }
    return sum;
}
```



####  383. 赎金信

该题不难，简单哈希表的应用，和day06的题目有点类似

#### 解法：

- 时间复杂度: O(n)

```c
bool canConstruct(char* ransomNote, char* magazine) {
    int arr[26] = {};
    while(*magazine){
        arr[*magazine++ - 'a']++;
    }
    while(*ransomNote){
        if(--arr[*ransomNote++ - 'a'] < 0){
            return 0;
        };
    }
    return 1;
}
```



#### 第15题. 三数之和

三数之和，梦破碎的地方。。。

该题不是最适合用map解决，去重机制非常麻烦。

最适方法是双指针，left和right分别位于两侧，过大左移right，过小右移left，其中查重的顺序非常需要注意！！！必须是nums[i] == nums[i - 1],否则会去除可取数组！！！

#### 解法：

- 时间复杂度: O(n^2)

```c
#include <stdio.h>
#include <stdlib.h>

/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *returnColumnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** fourSum(int* nums, int numsSize, int target, int* returnSize, int** returnColumnSizes) {
    int ** arr = (int **)malloc(sizeof(int*) * 1001);
    *returnSize = 0;
    if(numsSize < 4){
        return arr;
    }
    int gap = numsSize / 2;
    while(gap){
        for(int i = gap;i < numsSize;i++){
            int j;
            int temp = nums[i];
            for(j = i;j >= gap && nums[j - gap] > temp;j -= gap){
                nums[j] = nums[j - gap];
            }
            nums[j] = temp;
        }
        gap /= 2;
    }
    for(int i = 0;i < numsSize - 3;i++){
        if((target > 0 && nums[i] > target)){
            break;
        }
        while(i < numsSize - 3 && i > 0 && nums[i] == nums[i - 1]){
            i++;
        }
        for(int j = i + 1;j < numsSize - 2;j++){
            if(target > 0 && ((long)nums[i] + (long)nums[j]) > target){
                break;
            }
            while(j < numsSize - 2 && j > (i + 1) && nums[j] == nums[j - 1]){
                j++;
            }
            int left = j + 1;
            int right = numsSize - 1;
            while(left < right){
                long sum = (long)nums[i] + (long)nums[j] + (long)nums[left] + (long)nums[right];
                if(sum > target){
                    right--;
                }else if(sum < target){
                    left++;
                }else{
                    arr[*returnSize] = (int*)malloc(sizeof(int) * 4);
                    arr[*returnSize][0] = nums[i];
                    arr[*returnSize][1] = nums[j];
                    arr[*returnSize][2] = nums[left];
                    arr[*returnSize][3] = nums[right];
                    (*returnSize)++;
                    while(left < right && nums[left] == nums[left + 1]){
                        left++;
                    }
                    while(left < right && nums[right] == nums[right - 1]){
                        right--;
                    }
                    left++;
                    right--;
                }
            }
        }
    }
    *returnColumnSizes = (int *)malloc(sizeof(int) * (*returnSize));
    for(int i = 0;i < *returnSize;i++){
        (*returnColumnSizes)[i] = 4;
    }
    return arr;
}

```



#### 第18题. 四数之和

该题与上一题有点类似，唯一不同就是多了一层循环与剪枝操作有些不同，这题我练了一下希尔，差点没出事。。。

还有边界控制实在是细节太多，一不小心一个bug修了个把小时。。。这还是三数之和做出来的前提下。。。

#### 解法：

- 时间复杂度: O(n^3)

```c
#include <stdio.h>
#include <stdlib.h>

/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *returnColumnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** fourSum(int* nums, int numsSize, int target, int* returnSize, int** returnColumnSizes) {
    int ** arr = (int **)malloc(sizeof(int*) * 1001);
    *returnSize = 0;
    if(numsSize < 4){
        return arr;
    }
    int gap = numsSize / 2;
    while(gap){
        for(int i = gap;i < numsSize;i++){
            int j;
            int temp = nums[i];
            for(j = i;j >= gap && nums[j - gap] > temp;j -= gap){
                nums[j] = nums[j - gap];
            }
            nums[j] = temp;
        }
        gap /= 2;
    }
    for(int i = 0;i < numsSize - 3;i++){
        if((target > 0 && nums[i] > target)){
            break;
        }
        while(i < numsSize - 3 && i > 0 && nums[i] == nums[i - 1]){
            i++;
        }
        for(int j = i + 1;j < numsSize - 2;j++){
            if(target > 0 && ((long)nums[i] + (long)nums[j]) > target){
                break;
            }
            while(j < numsSize - 2 && j > (i + 1) && nums[j] == nums[j - 1]){
                j++;
            }
            int left = j + 1;
            int right = numsSize - 1;
            while(left < right){
                long sum = (long)nums[i] + (long)nums[j] + (long)nums[left] + (long)nums[right];
                if(sum > target){
                    right--;
                }else if(sum < target){
                    left++;
                }else{
                    arr[*returnSize] = (int*)malloc(sizeof(int) * 4);
                    arr[*returnSize][0] = nums[i];
                    arr[*returnSize][1] = nums[j];
                    arr[*returnSize][2] = nums[left];
                    arr[*returnSize][3] = nums[right];
                    (*returnSize)++;
                    while(left < right && nums[left] == nums[left + 1]){
                        left++;
                    }
                    while(left < right && nums[right] == nums[right - 1]){
                        right--;
                    }
                    left++;
                    right--;
                }
            }
        }
    }
    *returnColumnSizes = (int *)malloc(sizeof(int) * (*returnSize));
    for(int i = 0;i < *returnSize;i++){
        (*returnColumnSizes)[i] = 4;
    }
    return arr;
}

```



#### 总结：

最近耽误的时间太多了，得加吧劲了，不然要被大佬们甩到尾灯都看不见了😂