## 数组

### 二分查找

给定一个 n 个元素**有序的（升序）整型数组** nums 和一个目标值 target，写一个函数搜索 nums 中的 target ，如果目标存在则返回下标，否则返回 -1.

**示例1**：

``` tex
输入: nums = [-1,0,3,5,9,12], target = 9     
输出: 4       
解释: 9 出现在 nums 中并且下标为 4     
```

**示例2**：

``` TEX
输入: nums = [-1,0,3,5,9,12], target = 2     
输出: -1        
解释: 2 不存在 nums 中因此返回 -1       
```

- 你可以假设 nums 中的所有元素是不重复的。
- n 将在 [1, 10000]之间。
- nums 的每个元素都将在 [-9999, 9999]之间.



解题的前提：

数组为**有序数组**，同时还强调**数组中元素无重复元素**（有重复元素可能导致返回的元素下标不唯一）。



#### 二分法的两种写法

- > **左闭右闭即[left, right]**
  >
  > - while (left <= right) 要使用 <= ，因为left == right是有意义的，所以使用 <=
  > -  (nums[middle] > target) right 要赋值为 middle - 1，因为当前这个nums[middle]一定不是target，那么接下来要查找if的左区间结束下标位置就是 middle - 1

  **代码如下：**

  ``` java
  class Solution{
      public int search(int[] nums, int target){
          if(target < nums[0] || target > nums[nums.length-1]){
              return -1;
          }
          int left = 0 ;
          int right = nums.length-1;
          while(left <= right){
              int mid = left + ((right-left)>>1);
              if(nums[mid] == target)
                  return mid;
              else if(nums[mid] < target)
                  left = mid + 1;
              eles if(nums[mid] > target)
                  right =mid - 1;
          }
          return -1;
      }
  }
  ```

  

- > **左闭右开即[left, right)**
  >
  > - while (left < right)，这里使用 < ,因为left == right在区间[left, right)是没有意义的
  > - if (nums[middle] > target) right 更新为 middle，因为当前nums[middle]不等于target，去左区间继续寻找，而寻找区间是左闭右开区间，所以right更新为middle，即：下一个查询区间不会去比较nums[middle]

  **代码如下：**

  ``` java
  class Solution {
      public int search(int[] nums, int target) {
          int left = 0, right = nums.length;
          while (left < right) {
              int mid = left + ((right - left) >> 1);
              if (nums[mid] == target)
                  return mid;
              else if (nums[mid] < target)
                  left = mid + 1;
              else if (nums[mid] > target)
                  right = mid;
          }
          return -1;
      }
  }
  ```

  

------



### 移除元素

> 给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
>
> 不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并**原地**修改输入数组。
>
> 元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
>
> 示例 1: 给定 nums = [3,2,2,3], val = 3, 函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。 
>
> 示例 2: 给定 nums = [0,1,2,2,3,0,4,2], val = 2, 函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。
>
> **你不需要考虑数组中超出新长度后面的元素。**
>



<-数组中的元素在内存地址中的是连续的，不能只删除某个元素，而是覆盖->



#### 暴力解法

即用两层 for 循环，一个遍历数组元素，另一个更新数组。

时间复杂度：O(n^2)  空间复杂度：O(1)



```java
class Solution{
    public int removeElement(int[] nums,int val){
        int size = nums.length;
        for(int i=0;i<size;i++){
            if(nums[i] == val){
                for (int j=j+1;j<size;j++){
                    nums[j-1] = nums[j];
                }
                i--;
                size--;
            }
            return size;
        }
    }
}
```



#### 快慢指针（双指针）

**通过一个快指针和一个慢指针在一个 for 循环下完成两个 for 循环的工作。**

- **快指针**：寻找新数组的元素，新数组就是不含有目标元素的数组。
- **慢指针**：指向更新新数组下标的位置。

``` java
class Solution{
    public int removeElement(int nums[], int val){
        int slow = 0;
        for (int fast = 0;fast < nums.length; fast++){
            if(nums[fast] != val){
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }
}
```



------



### 有序数组的平方

给你一个按 **非递减顺序 排序的整数数组** nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

示例 1： 输入：nums = [-4,-1,0,3,10] 输出：[0,1,9,16,100] 解释：平方后，数组变为 [16,1,0,9,100]，排序后，数组变为 [0,1,9,16,100]

示例 2： 输入：nums = [-7,-3,2,3,11] 输出：[4,9,9,49,121]



#### 暴力解法

每个数平方后，排个序。美滋滋~~~~

``` java
class Solustion{
    public int[] sortedSquares(int[] nums){
        for(int i = 0;i < nums.length;i++){
            nums[i] *= nums[i];
        }
        return Arrays.sort(nums);
    }
}
```

时间复杂度：O(n + nlogn),可以说是O(nlogn)的时间复杂度。



#### 双指针法

数组原本是有序的，只不过负数平方后可能是最大数。

所以数组平方的最大值就在数组的两端，不会是中间。

此时可以使用双指针法,i 指向起始位置，j 指向终止位置。

定义一个新数组 result ，和原始数组一样大小，让 k 指向 result 的终止位置。

如果 `nums[i] * nums[i] < nums[j] * nums[j]`，则`result[k--] = nums[j] * nums[j]`

如果 `nums[i] * nums[i] >= nums[j] * nums[j]`，则`result[k--] = nums[i] * nums[i]`

 

``` java
class Solution{
    public int[] sortedSquares(int[] nums){
        int right = nums.length - 1;
        int left = 0;
        int result[] = new int[nums.length];
        int index = result.length - 1;
        while(left <= right){
            if(nums[left] * nums[left] > nums[right] * nums[right]){
                // 正数的相对位置不变，需要调整的是负数平方后的位置
                result[index--] = nums[left] * nums[left];
                ++left;
            }else{
                result[index--] = nums[right] * nums[right];
                --right;
            }
        }
        return result;
    }
}
```

时间复杂度：O(n)



------



### 长度最小的子数组

> 给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。
>
> 示例：
>
> 输入：s = 7, nums = [2,3,1,2,4,3] 输出：2 解释：子数组 [4,3] 是该条件下的长度最小的子数组。
>
> 提示：
>
> - 1 <= target <= 10^9
> - 1 <= nums.length <= 10^5
> - 1 <= nums[i] <= 10^5
>



#### 暴力解法（通不过示例）

``` java
public int minSubArrayLen(int s,int[] nums){
    int min = Integer.MAX_VALUE;
    for(int i = 0;i < nums.length;i++){
        int sum = nums[i];
        if(sum >= s){
            return 1;
            for(int j = i+1;j<nums.length;j++){
                sum += nums[j];
                if(sum >= s){
                    min = Math.min(min,j-i-1);
                    break;
                }
            }
        }
        return min == Integer.MAX_VALUE ?0:min;
    }
}
```



#### 滑动窗口

> 所谓滑动窗口即是，不断地调节子序列的起始位置和终止位置，从而得到我们想要的结果。
>
> 在暴力解法中，是一个 for 循环滑动窗口的起始位置，一个滑动终止位置，完成一个不断搜索区间的过程。
>
> 那么滑动窗口如何用一个for循环来完成这个操作呢。
>
> 首先要思考 如果用一个for循环，那么应该表示 滑动窗口的起始位置，还是终止位置。
>
> 如果只用一个for循环来表示 滑动窗口的起始位置，那么如何遍历剩下的终止位置？
>
> 此时难免再次陷入 暴力解法的怪圈。
>
> 所以 只用一个for循环，那么这个循环的索引，一定是表示 滑动窗口的终止位置。

![209.长度最小的子数组](https://code-thinking.cdn.bcebos.com/gifs/209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.gif)

``` java
class Solution{
    // 滑动窗口
    public int minSubArrayLen(int s,int[] nums){
        int left = 0;
        int sum = 0;
        int result = Integer.MAX_VALUE;
        for(int right = 0;right < nums.length;right++){
            sum += nums[right];
            while(sum >= s){
                result = Math.min(result ,right -left + 1);
                sum -= nums[left++];
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}
```



------

### 螺旋矩阵 ||

给定一个正整数 n，生成一个包含 1 到 n^2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

示例:

输入: 3 输出: [ [ 1, 2, 3 ], [ 8, 9, 4 ], [ 7, 6, 5 ] ]

![image-20230613223358338](D:\MyBlog\MyBlog\algorithm\assets\image-20230613223358338.png)



#### 模拟法 （设定边界）k神

- 生成一个 `n×n` 空矩阵 `mat`，随后模拟整个向内环绕的填入过程：

> 1. 定义当前左右上下边界 `l,r,t,b`，初始值 `num = 1`，迭代终止值 `tar = n * n`；
> 2. 当 num <= tar 时，始终按照 `从左到右` `从上到下` `从右到左` `从下到上` 填入顺序循环，每次填入后：
> 3. 执行` num += 1`：得到下一个需要填入的数字；
> 4. 更新边界：例如从左到右填完后，上边界 `t += 1`，相当于上边界向内缩 1。
> 5. 使用`num <= tar`而不是`l < r || t < b`作为迭代条件，是为了解决当n为奇数时，矩阵中心数字无法在迭代过程中被填充的问题。

- 最终返回 mat 即可。

![image-20230613223957542](D:\MyBlog\MyBlog\algorithm\assets\image-20230613223957542.png)

``` java
class Solution{
    public int[][] generateMatrix(int n){
        int l=0,r=n-1,t=0,b=n-1;
        int[][] mat = new int[n][n];
        int num = 1;
        int tar = n * n;
        while(num<=tar){
            for(int i=l;i<=r;i++) mat[t][i] = num++; // left to right
            t++;
            for(int i=t;i<=b;i++) mat[i][r] = num++; // top to bottom
            r--;
            for(int i=r;i>=l;i--) mat[b][i] = num++; // right to left
            b--;
            for(int i=b;i>=t;i--) mat[i][l] = num++; // bottom to top
            l++;
        }
        return mat;
    }
}
```









## 链表



### 移除链表元素

> 题意：删除链表中等于给定值 val 的所有节点。
>
> 示例 1： 输入：head = [1,2,6,3,4,5,6], val = 6 输出：[1,2,3,4,5]
>
> 示例 2： 输入：head = [], val = 1 输出：[]
>
> 示例 3： 输入：head = [7,7,7,7], val = 7 输出：[]
>

![image-20230617133735982](D:\MyBlog\MyBlog\algorithm\assets\image-20230617133735982.png)



**两种解法**：

- 直接使用原来的链表进行操作
- 设置一个虚拟头结点进行操作

解法2：

``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution{
    public ListNode removeElements(ListNode head,int val){
        if(head == null) return head;
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        
        ListNode pre = dummy;
        ListNode cur = head;
        whiel(cur != null){
            if(cur.val == val){
                pre.next = cur.next;
            }else{
                pre = cur;
            }
            cur = cur.next;
        }
        return dummy.next;
    }
}
```

解法1-1：

``` java
public ListNode removeElements(ListNode head,int val){
    while(head != null && head.val = val){
        head = head.next;
    }
    ListNode cur = head;
    while(cur != null){
        while(cur.next != null && cur.next.val = val){
            cur.next = cur.next.next;
        }
        cur = cur.next;
    }
    return head;
}
```

解法1-2：

``` java
public ListNode removeElements(ListNode head,int val){
    while(head != null && head.val == val){
        head = head.next;
    }
    if(head == null) return head;
    
    ListNode pre = head;
    ListNode cur = head.next;
    while(cur != null){
        if(cur.val == val){
            pre.next = cur.next;
        }else{
            pre = cur;
        }
        cur = cur.next;
    }
    return head;
}
```



------

### 设计链表

> 在链表类中实现这些功能：
>
> - get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
> - addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
> - addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
> - addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val 的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
> - deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。
>

![image-20230617141140817](D:\笔记Md\assets\image-20230617141140817.png)



**单链表**：

``` java
class ListNode{
    int val;
    ListNode next;
    ListNode(){}
    ListNode(int val){this.val = val;}
}

class MyLinkList{
    // size储存链表元素个数
    int size;
     // 虚拟头结点
     ListNode head;
    
    public MyLinkedList() {
      size = 0;
      head = new ListNode(0); 
    }
    //获取第index个节点的数值，注意index是从0开始的，第0个节点就是头结点
    public int get(int index) {
        if(index < 0 || index >= size){
            return -1;
        }
        ListNode currentNode = head;
        for(int i = 0;i <= index;i++){
            currentNode = currentNode.next;
        }
        return currentNode.val;
    }
    
    public void addAtHead(int val) {
        addIndex(0,val);
    }
    
    public void addAtTail(int val) {
        addIndex(size,val);
    }
    
    public void addAtIndex(int index, int val) {
        if(index > size) return;
        if(index < 0){
            index = 0;
        }
        size++;
        
        ListNode pred = head;
        for(int i=0;i<index;i++){
            pred = pred.next;
        }
        ListNode toAdd = new ListNode(val);
        toAdd.next = pred.next;
        pred.next = toAdd;
    }
    
    public void deleteAtIndex(int index) {
        if(index<0 || index >= size){
            return;
        }
        size--;
        if(index == 0){
            head = head.next;
            return;
        }
        ListNode pred = head;
        for(int i = 0;i<index;i++){
            pred = pred.next;
        }
        pred.next = pred.next.next;
    }
}
```





## 哈希表



### 两数之和

> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
>
> **示例:**
>
> 给定 nums = [2, 7, 11, 15], target = 9
>
> 因为 nums[0] + nums[1] = 2 + 7 = 9
>
> 所以返回 [0, 1]



**在什么时候使用哈希法：**当我们需要查询一个元素是否出现过，或者一个元素是否在集合里的时候。





``` java
public int[] twoSum(int[] nums,int target){
    int[] res = new int[2];
    if(nums == null || nums.length == 0){
        return res;
    }
    Map<Integer,Integer> map = new HashMap<>();
    for(int i=0;i<nums.length;i++){
        int temp = target - nums[i];
        if(map.containKey(temp)){
            res[1] = i;
            res[0] = map.get(temp);
            break;
        }
        map.put(nums[i],i);
    }
    return res;
}
```



------

### 三数之和

> 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
>
> **注意：** 答案中不可以包含重复的三元组。
>
> 示例：
>
> 给定数组 nums = [-1, 0, 1, 2, -1, -4]，
>
> 满足要求的三元组集合为： [ [-1, 0, 1], [-1, -1, 2] ]



**双指针**：

``` java
class Solution{
    public List<List<Integer>> threeSum(int[] nums){
        List<List<Integer>> result = new ArrayList<>();
        // 对数组进行排序
        Arrays.sort(nums);
        for(int i = 0;i<nums.length;i++){
            if(nums[i] > 0) return result;
            if(i>0 && nums[i] == nums[i-1]){
                continue;
            }
            int left = i+1;
            int right = nums.length-1;
            while(right>left){
                int sum = nums[i] +nums[left]+nums[right];
                if(sum>0){
                    right--;
                }else if(sum < 0){
                    left++;
                }else{
                    result.add(Arrays.asList(nums[i],nums[left],nums[right]));
                    while(right > left&&nums[right]=nums[right-1]) right--;
                    whiel(right>left&&nums[left] ==nums[left+1]) left ++;
                    right--;
                    left++;
                }
            }
        }
        return result;
    }
}
```

