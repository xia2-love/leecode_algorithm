# leecode_algorithm
# 力扣知识点总结

力扣题型总共分为以下几个部分：

数组、链表、哈希表、字符串、栈与队列、树、回溯、贪心、动态规划、图论、高级数据结构。

分析思路：

>1、暴力求解
>
>2、求解方式是否可以覆盖所有情况
>
>3、是否可以优化重复子问题
>
>4、是否可以利用各种容器

## 一、数组类型

题解方法：

1、双指针、双索引滑动窗口

双指针法（快慢指针法）： 通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。

双指针用法：两个快慢指针从初始位置出发，两个快慢指针分别从初始、结束位置出发。

适用题目：（1）明显针对双指针的问题，（2）可以将三维问题降为二维问题求解，

进阶：多指针可以用来标记多个边界问题

使用：易与哈希表结合使用，使用双指针的时候要找到维护的条件

**总结**

- 双指针的题目需要知道**维护什么**，比如维护区间的元素和、元素乘积或者0的个数
- 有时候正向难考虑就可以逆向思考，**正难则反**
- 有时根据题目条件无法沟站区间的二段性，这时候可以考虑加入隐含条件，让区间具有**二段性**，例如 LC.395
- 有些题目难以找到左边界右移的条件，例如 LC.76，需要**两层思考**：窗口是否满足要求以及如何判断
- 有些题目需要两个指针一个在左端，一个在右端，然后两个指针向中点移动。

2、基数排序法

3、三路快速排序法，二分法（两路查找）、线性查找

三路快速排序是双路快速排序的进一步改进，三路排序算法把排序的数据分为三部分，分别为小于 v，等于 v，大于 v，v 为标定值，这样三部分的数据中，等于 v 的数据在下次递归中不再需要排序，小于 v 和大于 v 的数据也不会出现某一个特别多的情况），通过此方式三路快速排序算法的性能更优。

二分法主要解决问题为：查询时间变为O(logN)

**左闭右开**：搜索区间范围是 `[left, right)`，此时循环条件是`left < right`

参考代码：

```cpp
int searchInsert(vector<int>& nums, int target) {
    int len=nums.size();
    int left=0;
    int right=len-1;
    int mid=(right-left)/2;
    while(left<right)
    {
        if(target==nums[mid])
            return mid;
        else
        {
            if(target<nums[mid])
            {
                right=mid;
                mid=left+(right-left)/2;
            }
            else
            {
                left=mid+1;
                mid=left+(right-left)/2;
            }
        }
    }
    if(nums[mid]<target)
        return mid+1;
    return mid;
}
```
常见问题：
> 1. 二分查找不一定要求数组有序
> 2. 二分查找取 mid 时何时+1
> 容易造成死循环（可以手动模拟一遍防止出错）

总结

> 其实二分的题目刷多了就有经验了，最主要的分析出**二分判断的条件**，根据题意判断出是在左区间还是右区间查找元素。
> 另外对于二分意图不明显的题目需要尝试分析出题目的隐藏题意，**最大值最小或者最小值最大**基本上可以看做是二分的代名词。参考题目：[2560. 打家劫舍 IV](https://leetcode.cn/problems/house-robber-iv/) 
> 注意问题的**单调性**：二分的值越大，越能/不能满足要求；二分的值越小，越不能/能满足要求，这就是单调性，就可以二分答案。
> 有的题目是套用模板法，有的题目则是没有所谓的「左闭右开」「左闭右闭」的概念，重点是认真理解题意，根据题目的条件分析如何缩减区间，参考题目：[liweiwei](https://leetcode.cn/problems/search-insert-position/solution/te-bie-hao-yong-de-er-fen-cha-fa-fa-mo-ban-python-/)


线性查找：查找时间为O(n),for循环遍历一遍查找

4、堆排序（优先队列），时间复杂度为O(nlogn)

堆：
首先堆中插入元素就是末尾插入，然后调整堆；堆中取元素一般就是取首元素，然后也是调整堆

```cpp
// 堆的长度为 n，节点 i 开始，从顶至底堆化
void siftDown(vector<int> &nums, int n, int i) {
    while (true) {
        int l = 2 * i + 1;
        int r = 2 * i + 2;
        int p = i;
        // 比父节点大就交换
        if (l < n && nums[l] > nums[p]) {
            p = l;
        }
        if (r < n && nums[r] > nums[p]) {
            p = r;
        }
        // 如果节点 i 最大或者索引 l, r 越界就无需堆化，直接退出
        if (p == i) break;
        swap(nums[i], nums[p]);
        i = p; // 循环向下堆化
    }
}

void heapSort(vector<int> &nums) {
    // 建堆操作：堆化除叶节点以外的其他所有节点
    // 必须从最后一个非叶子节点开始，这种建堆的方式比较高效，复杂度为O(n)
    for (int i = nums.size() / 2 - 1; i >= 0; -- i) {
        siftDown(nums, nums.size(), i);
    }
    
    // 从堆中提取最大元素，循环 n-1 轮
    for (int i = nums.size()-1; i > 0; -- i) {
        // 交换根结点与最右叶结点（交换首元素与尾元素）
        swap(nums[0], nums[i]);
        
        // 从根节点为起点，从顶至底进行堆化
        siftDown(nums, i, 0);
    }
}
```


优先队列的使用：

***大顶堆与小顶堆***
> 大顶堆（降序）(默认是大根堆)
> priority_queue<int,vector<int>,less<int> > big_heap2;
> 小顶堆（升序）
> priority_queue<int,vector<int>,greater<int> > small_heap;

***注意事项***
如果使用less和greater，需要头文件：
#include <functional>
> push()函数插入元素
> pop()函数从队列中删除元素时，优先级最高的元素将首先被删除。
> top()	此函数用于寻址优先队列的最顶层元素。

5、数组序号计数

6、快速排序
平均时间复杂度：O(nlogn) 最坏时间复杂度O(n^2)

代码参考：
```cpp
void quickSort(int arr[], int left, int right) {
    if (left >= right) {
        return;
    }
    int pivot = arr[left]; // 选择左边的元素作为基准值
    int i = left + 1, j = right;
    while (i <= j) {
        if (arr[i] < pivot && arr[j] > pivot) { // 当i指向的元素小于基准值，j指向的元素大于基准值时，交换它们的位置
            swap(arr[i++], arr[j--]);
        } else if (arr[i] >= pivot) { // 当i指向的元素大于等于基准值时，i向右移动
            i++;
        } else if (arr[j] <= pivot) { // 当j指向的元素小于等于基准值时，j向左移动
            j--;
        }
    }
    swap(arr[left], arr[j]); // 将基准值放到正确的位置上
    quickSort(arr, left, j - 1); // 对基准值左边的元素进行递归排序
    quickSort(arr, j + 1, right); // 对基准值右边的元素进行递归排序
}
```

7、桶排序

8、模拟。注意点：边界条件以及处理重复子问题
一般会处理限制条件进行剪枝，或者要记录是否到过某个位置，会结合bfs和dfs或者回溯+剪枝一起考。

9、回溯法
适用题目：穷举所有类型，进阶：回溯+剪枝

子集型回溯

| 题目                                                         | 说明                                                         | 题解                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [78. 子集](https://leetcode.cn/problems/subsets/)            | 和 LC.39 类似，按照 begin 为起点遍历回溯就可以               | [通过](https://leetcode.cn/submissions/detail/395238958/)    |
| [90. 子集 II](https://leetcode.cn/problems/subsets-ii/)      | 在 LC.78 的基础上有重复元素的考虑，和 LC.40 类似的剪枝 :fire: | [通过](https://leetcode.cn/submissions/detail/395250094/)    |
| [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/) | 枚举字符之间的逗号，按照 idx 顺序回溯，判断回文              | [通过](https://leetcode.cn/submissions/detail/395280098/)    |
| [698. 划分为k个相等的子集](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/) | **抽象成 k 个桶**，每个桶的容量为子集和大小                  | [通过](https://leetcode.cn/submissions/detail/396107337/)    |
| [473. 火柴拼正方形](https://leetcode.cn/problems/matchsticks-to-square/) | 和 LC.698 一模一样，**抽象成 4 个桶**                        | [通过](https://leetcode.cn/submissions/detail/366042332/)    |
| [2305. 公平分发饼干](https://leetcode.cn/problems/fair-distribution-of-cookies/) | k 个桶，但是桶大小未知，从大到小DFS回溯                      | [通过](https://leetcode.cn/submissions/detail/396110148/)    |
| [854. 相似度为 K 的字符串](https://leetcode.cn/problems/k-similar-strings/) | DFS 回溯，剪枝，有点难...                                    | [通过](https://leetcode.cn/submissions/detail/396114920/)    |
| [1255. 得分最高的单词集合](https://leetcode.cn/problems/maximum-score-words-formed-by-letters/) | 参考灵神的子集型回溯，选或不选的思想，注意恢复现场 :fire:    | [0x3F](https://leetcode.cn/problems/maximum-score-words-formed-by-letters/solution/hui-su-san-wen-si-kao-hui-su-wen-ti-de-t-kw3y/) |

**参考灵神思路总结一下：**

1. 输入视角：第 i 个元素「选还是不选」，这个比较直接
2. **答案视角**：第 i 个元素「枚举选哪个数」，也之前都是这个思路，也就是 begin 变量规定顺序

**参考代码随想录的剪枝技巧：**

- for 循环横向遍历，递归是纵向遍历

- 区分一下什么是 [树枝去重 vs 树层去重](https://programmercarl.com/0090.%E5%AD%90%E9%9B%86II.html#%E6%80%9D%E8%B7%AF)



组合型回溯

| 题目                                                         | 说明                                                         | 题解                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [39. 组合总和](https://leetcode.cn/problems/combination-sum/) | 组合问题需要按照某种顺序搜索：每一次搜索的时候设置 **下一轮搜索的起点** `begin`，也可以排序之后加速剪枝 | [通过](https://leetcode.cn/submissions/detail/171894367/)    |
| [40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii/) | 和 LC.39 区别是这个有重复元素，需要去掉当前层（for循环中）**第二个数值重复的节点** :fire: | [剪枝](https://leetcode.cn/problems/combination-sum-ii/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-m-3/225211) |
| [77. 组合](https://leetcode.cn/problems/combinations/)       | 和 LC.39 类似，按照 begin 为起点遍历然后回溯就可以，剪枝：剩余元素个数需要多余 k | [通过](https://leetcode.cn/submissions/detail/395236585/)    |
| [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/) | 和 LC.40 类似，这题没有重复元素，两个剪枝：**小于最小的 \|\| 大于最大的** :fire: | [0x3F](https://leetcode.cn/problems/combination-sum-iii/solution/hui-su-bu-hui-xie-tao-lu-zai-ci-pythonja-feme/) |
| [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/) | 直接 DFS 思路（选或不选）更简单，比较剩余左右括号数量剪枝    | [LWW](https://leetcode.cn/problems/generate-parentheses/solution/hui-su-suan-fa-by-liweiwei1419/) |

- **按照灵神的思路总结一下：** 和子集型回溯类似，加剪枝技巧，有时候使用「选或者不选」的思路比较简单，比如 LC.22 括号生成，有时候使用「选哪一个」思路比较简单，比如 LC.131 分割回文子串，具体做题时需要按照题目意思选择比较直接的思路

- **参考代码随想录的去重思想**：https://programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html



排列型回溯

| 题目                                                         | 说明                                                         | 题解                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [46. 全排列](https://leetcode.cn/problems/permutations/)     | 回溯入门问题，无重复元素的排列问题                           | [通过](https://leetcode.cn/submissions/detail/395204984/)    |
| [47. 全排列 II](https://leetcode.cn/problems/permutations-ii/) | 去重是关键，排序比较，**上一个相同的元素撤销之后再剪枝** :fire: | [剪枝图](https://leetcode.cn/problems/permutations-ii/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liwe-2/) |
| [51. N 皇后](https://leetcode.cn/problems/n-queens/)         | 第 row 行填入 path[row] 这个数，求 path 满足条件的全排列     | [Carl](https://programmercarl.com/0051.N%E7%9A%87%E5%90%8E.html) |

- 参考灵神的写法：感觉排列行回溯使用「选哪一个」的思路比较简单，使用 used 数组记录当前 path 中有哪些元素使用过了，然后恢复现场
- 参考[代码随想录](https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html)：注意 LC.47 中如果有重复元素需要使用 `used[i-1] == false` 来限制同一层重复元素，说明 i-1 是之前「被恢复的」元素，已经使用过了，所以剪掉


10、动态规划法
> 重点：思考状态转移
> 思路：暴力求解，从暴力求解中寻求数组和递推关系中存放所有情况

11、最大前缀和

12、自定义排序

参考代码：
```cpp
class compare
{
public:
	bool operator()(vector<int>& a, vector<int>& b)
	{
		return a[1] < b[1];
	}
};
```

补充：

常见排序算法包括「冒泡排序」、「插入排序」、「选择排序」、「快速排序」、「归并排序」、「堆排序」、「基数排序」、「桶排序」。如下图所示，为各排序算法的核心特性与时空复杂度总结。

![sort](https://pic.leetcode-cn.com/1629483637-tmENTT-Picture2.png)

## 二、链表
'''‘’‘
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
 '''
 
学会建链表、头插法、尾插法、用指针标记位置、双指针

## 三、哈希表
> map存放key-value数对，注意key不能变，value可以改变。使用的时候要注意是否需要存放重复的健，以及是否需要排序。（set中key和value一致）
> 如果是需要重复扫描定位，可以利用map存储，降低时间，提高代码的简洁性

## 四、字符串
1、模拟对字符串的使用

2、使用动态规划求解

## 五、栈与队列
多用在模拟中

## 六、树
多结合：递归法、bfs和dfs+剪枝+回溯求解

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
 '''
1、二叉树：四种遍历；二叉搜索树
2、多叉树

## 七、回溯
技巧：注意用函数书写重复代码

## 八、贪心
左右比较问题，可否把问题拆分，先比较左，再比较右，再综合比较。

自定义排序问题

## 九、动态规划
二维动态规划：画表法、学会拆分子问题并包含所有重复子问题

三维动态规划：降为二维动态规划处理

## 十、图论
图往往结合并查集、邻接矩阵、图的数据结构去考

```cpp

// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }    Node(int _val, vector
<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
```

## 十一、并查集
概念：并查集主要用于解决一些 **元素分组** 问题，通过以下操作管理一系列不相交的集合：

- 合并（Union）：把两个不相交的集合合并成一个集合
- 查询（Find）：查询两个元素是否在同一个集合中

实现：
1、初始化所有节点的祖先是自己。

2、查找当前边的两个节点的祖先是否相同.

如果不同，则联合这两个节点；

如果相同，则找到，中断退出。

适用场景：确认是否有共同祖先，不必知道每一个祖先是谁。

参考代码：
   ```cpp
class Solution {
public:
    //查询
    int Find(vector<int>& parent, int index) {
        if (parent[index] != index) {
            parent[index] = Find(parent, parent[index]);
        }
        return parent[index];
    }
    //合并
    void Union(vector<int>& parent, int index1, int index2) {
        parent[Find(parent, index1)] = Find(parent, index2);
    }

    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        //初始化父母节点
        vector<int> parent(n + 1);
        iota(parent.begin(), parent.end(), 0);
        for (auto& edge: edges) {
            int node1 = edge[0], node2 = edge[1];
            if (Find(parent, node1) != Find(parent, node2)) {
                Union(parent, node1, node2);
            } else {
                return edge;
            }
        }
        return vector<int>{};
    }
};
```


## 十二、状态压缩
状态总数少。能用异或等符号穷举所有从1-n的情况。


# 思考总结：
1、正向思考和反向思考

2、先计算一下求解的时间和空间复杂度，估计一下是否会超时或者超出内存