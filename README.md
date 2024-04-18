# leecode_algorithm
# 力扣知识点总结

力扣题型总共分为以下几个部分：
数组、链表、哈希表、字符串、栈与队列、树、回溯、贪心、动态规划、图论、高级数据结构。
分析要点：
1、解题思路，
2、各大题型常用方法，
3、目前做题数量和能力记录
4、经典题型和高频题型


##一、数组类型

#p 题解方法：
1、双指针、双索引滑动窗口
双指针法（快慢指针法）： 通过一个快指针和慢指针在一个for循环下完成两个for循环的工作。
双指针用法：两个快慢指针从初始位置出发，两个快慢指针分别从初始、结束位置出发。
适用题目：（1）明显针对双指针的问题，（2）可以将三维问题降为二维问题求解，
进阶：多指针可以用来标记多个边界问题
2、基数排序法
3、三路快速排序法，二分法（两路查找）、线性查找
二分法主要解决问题为：查询时间变为O(logN)
4、堆排序
5、数组序号计数
6、快速排序
7、桶排序
8、模拟。注意点：边界条件。
9、回溯法
适用题目：穷举所有类型，进阶：回溯+剪枝
10、动态规划法
11、最大前缀和
12、自定义排序
13、map
如果是需要重复扫描定位，可以利用map存储，降低时间，提高代码的简洁性

## 二、链表
'''
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

## 三、哈希表（50%）
理解数据结构

## 四、字符串（50%）
1、模拟对字符串的使用
2、使用动态规划求解

## 五、栈（34%）与队列（10%）
模拟

## 六、树
多结合：递归法、bfs和dfs+剪枝+回溯求解
'''
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
1、二叉树（58%）：四种遍历；二叉搜索树
2、多叉树（少）

## 七、回溯（26%）
技巧：注意用函数书写重复代码

## 八、贪心（42%）
左右比较问题，可否把问题拆分，先比较左，再比较右，再综合比较。
自定义排序问题

## 九、动态规划（48%）
二维动态规划：画表法、学会拆分子问题并包含所有重复子问题
三维动态规划：降为二维动态规划处理

## 十、图论（10%）
图往往结合并查集、邻接矩阵、图的数据结构去考
'''
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
'''

## 十一、并查集（8%）
1、初始化所有节点的祖先是自己。
2、查找当前边的两个节点的祖先是否相同
如果不同，则联合这两个节点；
如果相同，则找到，中断退出。
适用场景：确认是否有共同祖先，不必知道每一个祖先是谁。


## 十二、状态压缩（16%）
状态总数少。能用异或等符号穷举所有从1-n的情况。


思考总结：
1、正向思考和反向思考
2、