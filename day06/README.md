# Day06

今天的题目总体来说难度都挺简单,属于那种看一眼题解就能恍然大悟的那种(就是为啥我老是第一时间想不出来😂)

不过设计Map和Set的题目一多起来,C是不好用了,只能捡起老本行Java写写了,毕竟算法最重要的还是思想~

#### 242.有效的字母异位词

该题不难,但巧妙的解法是关键(想我做时差点用每个字符的ascii码对应的数组位数来记录每个数组中字符出现的次数,再相互比较...美观程度爆炸...)

不过看了看思路瞬间就改好了😂

##### 解法:

- 时间复杂度: O(n)

```c
bool isAnagram(char* s, char* t) {
    int letter[26] = {};
    while(*s){
        letter[*(s++) - 'a']++;
    }
    while(*t){
        letter[*(t++) - 'a']--;
    }
    for(int i = 0;i < 26;i++){
        if(letter[i]){
            return 0;
        }
    }
    return 1;
}
```



#### 349. 两个数组的交集

set秒了,啥?c语言没set?

整了半天还是不想为强行提高c语言熟练度不用set,只能捡起老本行Java了

##### 解法:

时间复杂度: O(n + m)

```c
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> set1 = new HashSet<>();
        HashSet<Integer> result = new HashSet<>();
        set1 = (HashSet<Integer>) Arrays.stream(nums1).boxed().collect(Collectors.toSet());
        for(int j : nums2){
            if(set1.contains(j)){
                result.add(j);
            }
        }
        return result.stream().mapToInt(x -> x).toArray();
    }
}
```



#### 第202题. 快乐数

简单,但我刚开始没思路,把它当作数学问题而忽略了相同sum则不为快乐数的简单事实.

不过清楚这一点后,set(java)就能秒杀了.

##### 解法:

- 时间复杂度: O(logn)

```c
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> sumH = new HashSet<>();
        int sum = n;
        while(sum != 1){
            if(sumH.contains(sum)){
                return false;
            }
            sumH.add(sum);
            sum = getSum(sum);
        }
        return true;
    }

    public int getSum(int n) {
        int sum = 0;
        while(n != 0){
            sum += (n % 10) * (n % 10);
            n /= 10;
        }
        return sum;
    }
}
```



####  1. 两数之和

这题难度相对略高,但也比较容易.

不过刚开始想尝试c来解决,这样做两个数倒是能找出来,但下标还得去数组中再找一遍就太那啥了.

所以还是Java吧,能同时记录两个值的HashMap就是好用

##### 解法:

- 时间复杂度: O(n)

```c
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> resultH = new HashMap<Integer, Integer>();
        int [] result = new int[2];
        for(int i = 0;i < nums.length;i++){
            if(resultH.containsKey(target - nums[i])){
                result[0] = i;
                result[1] = resultH.get(target - nums[i]);
                return result;
            }
            if(!resultH.containsKey(nums[i])){
                resultH.put(nums[i],i);
            }
        }
        return null;
    }
}
```



#### 总结:

今天题目主要关于HashMap,Set,简单hash表等等,熟悉写法是一方面,另一方面则是要熟悉怎么使用,什么时候用这些Map啊,Set之类的.我觉得等什么时候能精准快速的判断出何等场合该使用什么,那就大抵合格了.

