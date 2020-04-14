---
title: 数据结构和算法之美
date: 2019-12-16 20:39:54
tags: Reading
categories: Reading
---

> 记录学习`数据结构和算法之美`专栏重要知识点。

# 时间复杂度
O(1)
O(n)
O(logn)
O(nlogn)
O(n^2)

# 空间复杂度
额外的空间才算
例子：栈的空间是O(1)不是O(n)


<!--more-->

# 6. 链表
单链表：
循环链表： 约瑟夫环
双向链表： Java中的 LinkedHashMap就是
双向循环链表
空间换时间

# 7. 书写链表六个技巧
> 链表的书写好坏，可以看出一个人，写代码是否细心，考虑问题是否全面，思维是否缜密。
> 所以作为面试题常见

## 技巧
1. 理解`指针`和`引用`， 警惕指针和内存泄露
2. 哨兵：去除边界条件，简化
3. 边界处理
4. 举例画图
5. 多写多练

## 练习题-LeetCode
1. 206 单链表反转
2. 141 链表中环的检测
3. 21  两个有序链表合并
4. 19  删除链表倒数第N个节点
5. 876 求链表中间节点

# 8. 栈
> 操作受限的线性表
> 入栈
> 出栈

## 种类
数组：顺序栈
链表：链式栈

## 复杂度
时间：O(1)
空间：O(1)

## 动态扩容==顺序栈==
出栈：O(1)
入栈：O(n)  重新申请内存和数据搬移
均摊时间复杂度： O(1)   一般都等于最好的情况。

## 应用
1. 函数调用:    临时变量
2. 表达式求值：  2个栈，一个存数，一个存运算符
3. 括号匹配：  
```
用栈保存未匹配的左括号，依次从左开始扫描字符串。
扫描到左括号，则入栈，
当扫描到右括号，则从栈顶取一个左括号。
如果匹配，比如（）、「」、『』，
则继续扫描。
1.遇到不能配对的右括号
2.栈中没有数据
则说明非法。
如果栈为空，则字符串合法。
```
5. 浏览器历史记录  2个栈，一个存历史，一个存未来

# 9. 队列
> 操作受限的线性表
> 入队
> 出队

## 种类
数组：顺序队列
链表：链式队列

## 复杂度
时间：O(1)
空间：O(1)

## 扩展
1. 循环队列：解决数据搬移问题
2. 阻塞队列： 生产者-消费者模型
3. 并发队列： 线程安全的队列

## 应用
线程池：
当没有空闲资源时，通过队列缓解
无界队列：基于链表实现的
有界队列：基于数组实现的，针对时间敏感的系统。
重点：合适的大小，太大会响应过慢，太小性能浪费

# 10. 递归
> 如何用三行代码，找到最终推荐人？

## 分3步
1. 可以分成几个子问题
2. 问题和子问题，除了规模不同，求解思路完全一致
3. 存在终止条件

## 解题思路
1. 写出递归公式
2. 找到终止条件

## 应用
1. DFS深度优先搜索
2. 前中后序二叉树遍历
例子：3. 斐波那契数列
```
int f(int n) {
    if (n == 1) return 1;
    if (n == 2) return 2;
    return f(n-1) + f(n-2);
}
```

改进： 使用散列表存储中间值
```
int f(int n) {
    if (n == 1) return 1;
    if (n == 2) return 2;
    if (hasSolvedList.containsKey(n)) {
        return hasSovledList.get(n);
    }
    
    int ret = f(n-1) + f(n-2);
    hasSovledList.put(n, ret);
    return ret;
}
```

## 问题
1. 堆栈溢出
    1. 提前预防，比如限制最大深度
2. 重复计算
    1. 通过`散列表`之类的数据结构存储中间值
3. 空间复杂度，会积累。

## 递归改非递归
改进  非递归方案
```
int f(int n) {
    if (n == 1) return 1;
    if (n == 2) return 2;
    
    int ret = 0;
    int pre = 2;
    int prepre = 1;
    for (int i = 3; i <= n; ++i) {
        ret = pre + prepre;
        prepre = pre;
        pre = ret;
    }
    return ret;
}
```

## 调试递归
1. 打印日志发现，递归值
2. 结合条件断点进行调试

# 11. 排序
> 为啥`插入排序`比`冒泡排序`更受欢迎？

## O(n^2)
1. 冒泡排序
* [x] 2. 插入排序
3. 选择排序

## O(nlogn)
4. 归并排序
5. 快速排序

## O(n), 不基于比较
6. 计数排序
7. 基数排序
8. 桶排序


# 16. 二分查找，快速定位IP对应的省份地址
> 普通二分查找

```java
/// 二分查找(针对已经排好序的数组, 并且不能有重复值)
public int bsearch(int[] a, int n, int value) {
    int low = 0;
    int high = n - 1;
    
    while (low < high) {
        //找到中间下标
        //这种写法,有可能会溢出
        int mid = (low + high) / 2;
        //所以使用
//        int mid = low + ((high - low) >>1);
        
        if (a[mid] == value) {
            //找到目标
            return mid;
            
        } else if (a[mid] < value) {
            //比目标小, 则从大区间接着找
            low = mid + 1;
        } else {
            //比目标大, 则从小区间找
            high = mid - 1;
        }
    }
    
    //没有符合要求的
    return -1;
}
```

> 变形1：查找第一个值等于给定的元素

```java
/// 查找第一个值等于给定值
public int bsearch(int[] a, int n, int value) {
    int low = 0;
    int high = n - 1;
    
    while (low <= high) {
        int mid = low + ((high - low) >> 1);
        
        if (a[mid] > value) {
            high = mid - 1;
        } else if (a[mid] < value) {
            low = mid + 1;
        } else {
            //如果是第一个元素
            //或者与前一个元素值不等
            if ((mid == 0) ||
                (a[mid - 1] != value)) {
                //就是要找的
                return mid;
            } else {
                high = mid - 1;
            }
        }
    }
    return -1;
}
```

> 变形2：查找最后一个值等于给定的元素

```Java
/// 查找最后一个值等于给定值
public int bsearch(int[] a, int n, int value) {
    int low = 0;
    int high = n - 1;
    
    while (low <= high) {
        int mid = low + ((high - low) >> 1);
    
        if (a[mid] > value) {
            high = mid - 1;
        } else if (a[mid] < value) {
            low = mid + 1;
        } else {
            //如果是最后一个元素
            //或者与后一个元素和值不等
            if ((mid == n - 1) ||
                (a[mid + 1] != value)) {
                //就是要找的
                return mid;
            } else {
                high = mid + 1;
            }
        }
    }
    return -1;
}
```

> 变形3: 查找第一个大于给定值

```Java
/// 变形3: 查找第一个大于给定值
public int bsearch3(int[] a, int n, int value) {
    int low = 0;
    int high = n - 1;
    
    while (low <= high) {
        int mid = low + ((high - low) >> 1);
        
        if (a[mid] >= value) {
            //如果是第一个元素
            //或者前面的元素比它小,则就是第一个大于它的值
            if ((mid == 0) ||
                (a[mid - 1] < value)) {
                return mid;
            } else {
                high = mid - 1;
            }
        } else {
            //小于,直接在大区间找
            low = mid + 1;
        }
    }

    return -1;
}
```


> 变形4: 查找最后一个小于等于给定值

```Java
/// 变形4: 查找最后一个小于等于给定值
public int bsearch3(int[] a, int n, int value) {
    int low = 0;
    int high = n - 1;
    
    while (low <= high) {
        int mid = low + ((high - low) >> 1);
        
        if (a[mid] > value) {
            high = mid - 1;
        } else {
            //如果是最后一个元素
            //或者后面的元素比它大,则就是最后一个小于等于的给定值
            if ((mid == n - 1) ||
                (a[mid + 1] > value)) {
                return mid;
            } else {
                low = mid + 1;
            }
        }
    }

    return -1;
}
```

# 17. 跳表（Redis）
> 链表加多级索引的结构，就是跳表
> 空间换时间
> 时间复杂度O(logn)
> 空间复杂度O(n)
> 
> 插入操作时间复杂度O(logn), 链表的插入复杂度是O(1)，因为有序，所以需要先查找，即O(logn)
> 删除操作，也需要删除索引里面的

## Redis核心操作
1. 插入一个数据
2. 删除一个数据
3. 查找一个数据
4. 迭代输出有序序列
5. ----------以上4条，红黑树也可以-----------
6. 按照区间查找数据（这个跳表效率更高）

查找、插入、删除时间复杂度小 O(logn)
红黑树更复杂，不容易理解

# 18. 散列表
## 散列函数
    1. md5、SHA、CRC等哈希算法
```java
int hash(Object key) {
    int h = key.hashCode();
    return (h ^ (h >>> 16)) & (capitity -1);
}
```

## 装载因子

## 散列冲突解决办法
1. 开放寻址法
    1. 如果冲突，就再算一个
    2. 线性探测
    3. 二次探测
    4. 双重散列  
2. 链表法

## 工业级散列
1. 快速的查询、插入、删除操作
2. 内存占用合理，不能占用过多内存
3. 性能稳定，极端情况下，散列表的性能也不会退化到无法接受的情况

设计方案
1. 设计合适的散列函数
2. 定义装载因子阈值，设计动态扩容策略
3. 选择合适的散列冲突解决方法

# 20. 散列表为啥和链表一起使用
例子：LRU淘汰算法（最近最少使用）
一个节点,存四个值
```
prev data next hnext
               存拉链
```
则 O(1) 即可完成所有操作               

## Redis有序集合

## Java LinkedHashMap
通过双向链表和散列表组合

## 为何要组合
因为散列表虽然高效，但是无序。
需要双向链表排序，方便增加、删除

# 21. 哈希算法的应用
> 鸽巢原理， 10个窝，11个鸟，必然有一个窝里有2只，所以就冲突了。

1. 唯一标识符
2. 校验数据完整性和正确性
3. 安全加密， 会有散列冲突。主要在于权衡安全性和计算时间
4. 散列函数， 更看中的是散列的平均性和执行效率

## 加密算法
1. MD5：  Message-Digest Algorithm.      MD5消息摘要算法
2. SHA：  Secure Hash Algorithm.         安全散列算法
3. DES：  Data Encryption Standard.      数据加密标准
4. AES：  Advanced Encryption Standard.  高级加密标准

场景：脱库、彩虹表、加盐、区块链

## 分布式中应用
1. 负载均衡
    1. 让同一台客户端，请求都由同一台服务器处理
    2. 哈希算法，对客户端IP地址进行计算，将取到的值与服务器列表大小取模运算，则都会到同一台服务器
2. 数据分片
3. 分布式存储


               