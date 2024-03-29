# 简单难度题目集

## 1.两数之和

#### 题干
>给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。你可以按任意顺序返回答案。


#### 示例
>示例 1：
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1]

>示例 2：
输入：nums = [3,2,4], target = 6
输出：[1,2]

>示例 3：
输入：nums = [3,3], target = 6
输出：[0,1]


#### 题解

1. 暴力枚举
   ```c++
   class Solution {
    public:
        vector<int> twoSum(vector<int>& nums, int target) {
            int n = nums.size();
            for (int i = 0; i < n; ++i) {
                for (int j = i + 1; j < n; ++j) {
                    if (nums[i] + nums[j] == target) {
                        return {i, j};
                    }
                }
            }
            return {};
        }
    };
    
2. 哈希表
    >注意到方法一的时间复杂度较高的原因是寻找 target - x 的时间复杂度过高。因此，我们需要一种更优秀的方法，能够快速寻找数组中是否存在目标元素。如果存在，我们需要找出它的索引。
    使用哈希表，可以将寻找 target - x 的时间复杂度降低到从 O(N)O(N)O(N) 降低到 O(1)O(1)O(1)。
    这样我们创建一个哈希表，对于每一个 x，我们首先查询哈希表中是否存在 target - x，然后将 x 插入到哈希表中，即可保证不会让 x 和自己匹配。
   ```c++
   class Solution {
    public:
        vector<int> twoSum(vector<int>& nums, int target) {
            unordered_map<int, int> hash;
            for(int i = 0; i < nums.size(); i++) {
                if(hash.find(target - nums[i]) != hash.end()) {
                    return {i, hash[target - nums[i]]};
                }
                hash[nums[i]] = i;
            }
            return {};
        }
    };


## 2.罗马数字转整数

#### 题干
>罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。
字符$~~~~~~~~$数值
I$~~~~~~~~~~~~~~$1
V$~~~~~~~~~~~~~$5
X$~~~~~~~~~~~~~$10
L$~~~~~~~~~~~~~~$50
C$~~~~~~~~~~~~~$100
D$~~~~~~~~~~~~~~$500
M$~~~~~~~~~~~~~$1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1 。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

>通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。

>这个特殊的规则只适用于以下六种情况：
I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个罗马数字，将其转换成整数。
#### 示例
>示例 1:
输入: s = "III"
输出: 3

>示例 2:
输入: s = "IV"
输出: 4

>示例 3:
输入: s = "IX"
输出: 9

>示例 4:
输入: s = "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.

>示例 5:
输入: s = "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.

#### 题解

>通常情况下，罗马数字中小的数字在大的数字的右边。若输入的字符串满足该情况，那么可以将每个字符视作一个单独的值，累加每个字符对应的数值即可。

>例如 XXVII 可视作 X+X+V+I+I=10+10+5+1+1=27。

>若存在小的数字在大的数字的左边的情况，根据规则需要减去小的数字。对于这种情况，我们也可以将每个字符视作一个单独的值，若一个数字右侧的数字比它大，则将该数字的符号取反。
例如 XIV 可视作 X−I+V=10−1+5=14。

```c++
    class Solution {
    private:
        unordered_map<char, int> symbolValues = {
            {'I', 1},
            {'V', 5},
            {'X', 10},
            {'L', 50},
            {'C', 100},
            {'D', 500},
            {'M', 1000},
        };

    public:
        int romanToInt(string s) {
            int ans = 0;
            int n = s.length();
            for (int i = 0; i < n; ++i) {
                int value = symbolValues[s[i]];
                if (i < n - 1 && value < symbolValues[s[i + 1]]) {
                    ans -= value;
                } else {
                    ans += value;
                }
            }
            return ans;
        }
    };
```


## 3.环形链表
#### 题干
>判断链表中有没有环
#### 题解
>访问一个结点，若不在哈希表中加入进去，若找到了则存在环


## 4.相交链表
#### 题干
>判断两个链表是否相交
#### 题解
>将一个链表中的所有节点存在哈希表中，遍历第二个链表查看是否有重复结点

## 5.快乐数
#### 题干
>编写一个算法来判断一个数 n 是不是快乐数。
「快乐数」 定义为：
对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
如果这个过程 结果为 1，那么这个数就是快乐数。
如果 n 是 快乐数 就返回 true ；不是，则返回 false
#### 题解

