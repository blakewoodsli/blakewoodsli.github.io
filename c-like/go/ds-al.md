# 数据结构与算法

- [数据结构与算法](#数据结构与算法)
  - [基础](#基础)
    - [basic](#basic)
    - [数组与链表：](#数组与链表)
    - [栈与队列：](#栈与队列)
    - [树与图：](#树与图)
    - [哈希表与集合：](#哈希表与集合)
    - [排序与搜索算法：](#排序与搜索算法)
    - [动态规划：](#动态规划)
    - [综合问题：](#综合问题)
  - [算法编程题](#算法编程题)
    - [反转字符串](#反转字符串)
    - [两数之和](#两数之和)
    - [go 生产者-消费者模型](#go-生产者-消费者模型)
    - [合并区间](#合并区间)
    - [找出字符串中第一个匹配项的下标](#找出字符串中第一个匹配项的下标)
    - [给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。](#给定一个字符串-s-请你找出其中不含有重复字符的-最长子串-的长度)

## 基础

### basic

1. 贪心算法和动态规划的区别

   - 贪心算法：局部最优，划分的每个子问题都最优，得到全局最优，但是不能保证是全局最优解，所以对于贪心算法来说，解是从上到下的，一步一步最优，直到最后。
   - 动态规划：将问题分解成重复的子问题，每次都寻找左右子问题解中最优的解，一步步得到全局的最优解.重复的子问题可以通过记录的方式，避免多次计算。所以对于动态规划来说，解是从小到上，从底层所有可能性中找到最优解，再一步步向上。
   - 分治法：和动态规划类似，将大问题分解成小问题，但是这些小问题是独立的，没有重复的问题。独立问题取得解，再合并成大问题的解。

2. 堆排序

   - 堆排序：将待排序数组看作一个树状数组，建立一个二叉树堆。通过对这种数据结构进行每个元素的插入，完成排序工作。
   - 排序算法不稳定，时间复杂度 O(nlogn)，空间复杂度 O(1)

3. 快速排序

   - 随机选择一个基准元素，通过一趟排序将要排序的数据分割成独立的两部分，一部分全部小于等于基准元素，一部分全部大于等于基准元素，再按此方法递归对这两部分数据进行快速排序。
   - 排序算法不稳定，时间复杂度 O(nlogn)，空间复杂度 O(logn)。

4. 快排存在的问题，如何优化

   - 时间复杂度最快平均是 O（nlogn）,最慢的时候是 O(n2);辅助空间也是 O(logn)；最开始学快排时最疑惑的就是这个东西不知道怎么得来的，一种是通过数学运算可以的出来，还有一种是通过递归树来理解就容易多了
   - 最坏情况下的性能：在最坏的情况下，快速排序的时间复杂度可以达到 O(n^2)，即当待排序数组已经有序或接近有序时，分割操作可能会导致子数组的大小不均衡，进而导致递归深度增加。为了解决这个问题，可以采用随机化的快速排序或者使用其他排序算法（如归并排序）作为快速排序的辅助。
   - 对于小规模数组的性能：当待排序数组规模较小时，递归调用快速排序可能会带来较大的开销。可以通过设置一个阈值，当子数组的大小小于该阈值时，采用插入排序或其他简单的排序算法来处理。
   - 重复元素的处理：如果待排序数组中存在大量重复元素，传统的快速排序算法可能导致重复元素的分布不均匀，从而影响性能。为了解决这个问题，可以采用三路快速排序（Three-Way Quicksort）算法，将数组分为小于、等于和大于切分元素的三部分。
   - 栈溢出的问题：当递归深度过大时，可能会导致栈溢出。为了避免这个问题，可以采用非递归的快速排序算法，使用循环和栈结构来代替递归调用。
   - 总结来说，快速排序在一般情况下具有较好的性能，但在某些特殊情况下可能存在问题。通过随机化、使用其他排序算法辅助、设置阈值、采用三路快速排序等优化策略，可以提高快速排序的性能和稳定性。

### 数组与链表：

- 什么是数组和链表？它们的区别是什么？
  - 数组是一种线性数据结构，它由一系列相同类型的元素组成，并按顺序存储在连续的内存空间中。数组的访问是基于索引的，可以通过索引快速访问数组中的元素。数组的大小在创建时确定，并且通常具有固定的大小。在数组中，插入和删除操作的效率较低，因为需要移动其他元素来保持顺序。
  - 链表也是一种线性数据结构，但它不需要连续的内存空间。链表中的元素（节点）包含数据和指向下一个节点的指针。链表通过这些指针将节点连接在一起。链表的大小可以动态增长或缩小。由于链表中的元素可以在内存中任意位置分布，因此插入和删除操作的效率较高。但是，链表的访问需要从头节点开始遍历链表，因此访问元素的效率较低。
- 如何在数组和链表中进行插入、删除和查找操作？
  - 数组：
    - 插入：需要将插入位置之后的元素向后移动，然后将新元素放入插入位置。
    - 删除：需要将删除位置之后的元素向前移动，然后删除最后一个元素。
    - 查找：可以通过索引直接访问数组中的元素，时间复杂度为 O(1)。
  - 链表：
    - 插入：在目标位置之后创建新节点，并更新前一个节点的指针。
    - 删除：更新前一个节点的指针，跳过待删除节点。
    - 查找：需要从头节点开始遍历链表，直到找到目标元素或到达链表末尾，时间复杂度为 O(n)。
- 如何反转一个链表？

  - 使用三个指针，prev、current 和 next。从头节点开始，将当前节点的指针指向前一个节点，然后更新指针位置，继续反转下一个节点，直到到达链表末尾。最后，返回新的头节点，即原链表的尾节点

  ```go
    type ListNode struct {
      Val  int
      Next *ListNode
    }

    func reverseLinkedList(head *ListNode) *ListNode {
        var prev *ListNode
        current := head

        for current != nil {
            next := current.Next
            current.Next = prev
            prev = current
            current = next
        }

        return prev
    }
  ```

- 如何检测链表中的环？
  - 快慢指针是指两个指针，一个指针每次移动一个节点，称为慢指针，另一个指针每次移动两个节点，称为快指针。如果链表中存在环，那么快指针最终会追上慢指针，形成一个循环。如果链表没有环，那么快指针最终会到达链表的末尾。

### 栈与队列：

- 什么是栈和队列？它们的应用场景是什么？

  - **栈**是一种具有特定操作顺序的线性数据结构，遵循**后进先出（LIFO，Last In First Out）**的原则。栈的常见操作有入栈（将元素添加到栈顶）和出栈（将栈顶元素移除）。栈的应用场景包括函数调用栈、表达式求值、括号匹配、浏览器的历史记录等。

  - **队列**也是一种线性数据结构，遵循**先进先出（FIFO，First In First Out）**的原则。队列的常见操作有入队（将元素添加到队尾）和出队（将队头元素移除）。队列的应用场景包括任务调度、消息传递、缓存管理等。

    以下是如何实现栈和队列的示例代码：

- 如何实现一个栈和一个队列？

  - **栈的实现**：

    ```go
    goCopy code
    type Stack struct {
        data []int
    }

    func (s *Stack) Push(val int) {
        s.data = append(s.data, val)
    }

    func (s *Stack) Pop() int {
        if len(s.data) == 0 {
            return -1 // or any error value
        }
        lastIndex := len(s.data) - 1
        val := s.data[lastIndex]
        s.data = s.data[:lastIndex]
        return val
    }
    ```

  - **队列的实现**：

    ```go
    goCopy code
    type Queue struct {
        data []int
    }

    func (q *Queue) Enqueue(val int) {
        q.data = append(q.data, val)
    }

    func (q *Queue) Dequeue() int {
        if len(q.data) == 0 {
            return -1 // or any error value
        }
        val := q.data[0]
        q.data = q.data[1:]
        return val
    }
    ```

- 如何使用栈实现括号匹配？

  - ```go
    func isMatchingPairs(expression string) bool {
        stack := make([]rune, 0)

        for _, char := range expression {
            switch char {
            case '(', '[', '{':
                stack = append(stack, char)
            case ')', ']', '}':
                if len(stack) == 0 {
                    return false
                }
                top := stack[len(stack)-1]
                stack = stack[:len(stack)-1]
                if !isMatchingPair(top, char) {
                    return false
                }
            }
        }

        return len(stack) == 0
    }

    func isMatchingPair(left, right rune) bool {
        switch left {
        case '(':
            return right == ')'
        case '[':
            return right == ']'
        case '{':
            return right == '}'
        }
        return false
    }
    ```

- 如何使用队列实现树的层次遍历？

### 树与图：

- 什么是二叉树、二叉搜索树和平衡树？
- 如何进行树的遍历（前序、中序、后序和层次遍历）？
- 如何判断一个二叉树是否是平衡树？
- 如何找到树中两个节点的最低公共祖先？

### 哈希表与集合：

- 什么是哈希表和集合？它们的特点是什么？

  - **哈希表**（Hash Table）是一种根据键（Key）直接访问值（Value）的数据结构。它通过将键映射到数组中的一个位置来实现快速的插入、查找和删除操作。哈希表的特点包括：
    - 基于键的快速访问：通过哈希函数计算键的索引，可以在常数时间内访问元素。
    - 冲突解决：不同键可能映射到相同的索引，需要使用冲突解决方法（如链表法或开放寻址法）来处理冲突。
    - 空间效率：哈希表使用动态数组实现，可以根据需要自动调整大小，具有较高的空间利用率。
  - **集合**（Set）是一种无序且不包含重复元素的数据结构。集合的特点包括：
    - 唯一性：集合中的元素不重复，每个元素只能出现一次。
    - 无序性：集合中的元素没有固定的顺序。

- 如何解决哈希冲突问题？
- 如何实现一个简单的哈希表或集合？
- 如何判断两个数组是否有重复元素？

### 排序与搜索算法：

- 常见的排序算法有哪些？它们的时间复杂度是多少？
- 如何实现快速排序和归并排序？

  - **快速排序**：

    ```go
    func quickSort(arr []int, low, high int) {
        if low < high {
            pivot := partition(arr, low, high)
            quickSort(arr, low, pivot-1)
            quickSort(arr, pivot+1, high)
        }
    }

    func partition(arr []int, low, high int) int {
        pivot := arr[high]
        i := low - 1

        for j := low; j < high; j++ {
            if arr[j] < pivot {
                i++
                arr[i], arr[j] = arr[j], arr[i]
            }
        }

        arr[i+1], arr[high] = arr[high], arr[i+1]
        return i + 1
    }
    ```

  - **归并排序**：

    ```
    func mergeSort(arr []int) []int {
        if len(arr) <= 1 {
            return arr
        }

        mid := len(arr) / 2
        left := mergeSort(arr[:mid])
        right := mergeSort(arr[mid:])
        return merge(left, right)
    }

    func merge(left, right []int) []int {
        merged := make([]int, 0, len(left)+len(right))
        i, j := 0, 0

        for i < len(left) && j < len(right) {
            if left[i] <= right[j] {
                merged = append(merged, left[i])
                i++
            } else {
                merged = append(merged, right[j])
                j++
            }
        }

        merged = append(merged, left[i:]...)
        merged = append(merged, right[j:]...)
        return merged
    }
    ```

- 如何在有序数组中进行二分查找？
- 如何在旋转有序数组中进行搜索？

### 动态规划：

- 什么是动态规划？它的核心思想是什么？

  - **动态规划**（Dynamic Programming）是一种通过将问题分解为子问题并以递推的方式求解的优化技术。它的核心思想是利用已经计算过的子问题的解来避免重复计算，从而提高效率。

    动态规划的核心思想可以概括为以下几个步骤：

    1. 定义问题的状态：将原问题划分为若干个子问题，确定状态表示问题的变量。
    2. 确定状态转移方程：根据子问题之间的关系，确定状态之间的转移方式，并建立状态转移方程。
    3. 定义初始条件：确定最简单的子问题的解，作为初始条件。
    4. 使用递推或迭代的方式计算问题的解：根据状态转移方程，从初始条件开始递推或迭代计算问题的解。

  - 动态规划的应用场景非常广泛，包括但不限于以下几个方面：

    - 最优化问题：如背包问题、最长递增子序列、最短路径等。
    - 组合问题：如组合数、排列数、子集划分等。
    - 概率问题：如概率计算、期望值计算等。
    - 资源分配问题：如任务调度、作业分配等。
    - 字符串问题：如编辑距离、最长公共子序列等。

- 如何识别和解决动态规划问题？
- 如何实现一个简单的动态规划算法？
- 举例说明动态规划的应用场景。

### 综合问题：

- 如何判断一个链表是否存在环，并找到环的入口节点？

- 如何找到一个数组中的两个数，使其和等于给定的目标值？

- 如何找到一个数组中的最长连续子序列的长度？

- 如何实现 LRU 缓存算法？

  - LRU（Least Recently Used）缓存算法是一种常见的缓存淘汰策略，它根据数据的访问顺序来淘汰最近最少使用的数据。使用了一个哈希表 `cache` 来存储键值对，并使用双向链表 `lruList` 来维护数据的访问顺序。每当访问一个元素时，将其移动到链表的头部，保证头部的元素是最近使用的元素。当容量不足时，淘汰链表尾部的元素。

  - ```go

    type LRUCache struct {
        capacity int
        cache    map[int]*list.Element
        lruList  *list.List
    }

    type cacheItem struct {
        key   int
        value int
    }

    func Constructor(capacity int) LRUCache {
        return LRUCache{
            capacity: capacity,
            cache:    make(map[int]*list.Element),
            lruList:  list.New(),
        }
    }

    func (c *LRUCache) Get(key int) int {
        if elem, ok := c.cache[key]; ok {
            c.lruList.MoveToFront(elem)
            return elem.Value.(*cacheItem).value
        }
        return -1
    }

    func (c *LRUCache) Put(key int, value int) {
        if elem, ok := c.cache[key]; ok {
            c.lruList.MoveToFront(elem)
            elem.Value.(*cacheItem).value = value
        } else {
            if c.lruList.Len() >= c.capacity {
                oldest := c.lruList.Back()
                delete(c.cache, oldest.Value.(*cacheItem).key)
                c.lruList.Remove(oldest)
            }
            newItem := &cacheItem{key: key, value: value}
            newElem := c.lruList.PushFront(newItem)
            c.cache[key] = newElem
        }
    }
    ```

## 算法编程题

### 反转字符串

题目描述：编写一个函数，将输入的字符串反转。
示例输入：`"hello"` 示例输出：`"olleh"`

```go
func reverseString(s string) string {
    runes := []rune(s)
    n := len(runes)
    for i := 0; i < n/2; i++ {
        runes[i], runes[n-i-1] = runes[n-i-1], runes[i]
    }
    return string(runes)
}
```

### 两数之和

题目描述：给定一个整数数组和一个目标值，在数组中找出和为目标值的两个数，并返回它们的索引。
示例输入：`nums = [2, 7, 11, 15], target = 9` 示例输出：`[0, 1]`

```
goCopy code
func twoSum(nums []int, target int) []int {
    numMap := make(map[int]int)
    for i, num := range nums {
        complement := target - num
        if j, ok := numMap[complement]; ok {
            return []int{j, i}
        }
        numMap[num] = i
    }
    return nil
}
```

### go 生产者-消费者模型

题目描述：使用多个 goroutine 实现一个生产者-消费者模型的并发程序，其中生产者生成随机数并发送到通道，消费者从通道接收随机数并打印出来。

```go
func producer(ch chan<- int) {
    for i := 0; i < 10; i++ {
        num := rand.Intn(100)
        ch <- num
    }
    close(ch)
}

func consumer(ch <-chan int, wg *sync.WaitGroup) {
    for num := range ch {
        fmt.Println("Consumed:", num)
    }
    wg.Done()
}

func main() {
    ch := make(chan int)
    var wg sync.WaitGroup
    wg.Add(1)

    go producer(ch)
    go consumer(ch, &wg)

    wg.Wait()
}
```

在上述代码中，生产者函数 `producer` 会生成随机数并发送到通道 `ch`，消费者函数 `consumer` 从通道接收随机数并打印出来。`sync.WaitGroup` 用于等待消费者完成。通过使用多个 goroutine 实现了生产者和消费者的并发执行。

### 合并区间

以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。

示例 1：

输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2：

输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。

链接：https://leetcode.cn/problems/merge-intervals

```
type Interval struct {
    Start int
    End   int
}

func mergeIntervals(intervals []Interval) []Interval {
    if len(intervals) <= 1 {
        return intervals
    }

    // 按照区间的起始位置进行排序
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i].Start < intervals[j].Start
    })

    merged := make([]Interval, 0)
    merged = append(merged, intervals[0])

    for i := 1; i < len(intervals); i++ {
        prev := &merged[len(merged)-1]
        curr := intervals[i]

        // 判断当前区间是否与前一个区间重叠
        if curr.Start <= prev.End {
            if curr.End > prev.End {
                prev.End = curr.End
            }
        } else {
            merged = append(merged, curr)
        }
    }

    return merged
}
```

### 找出字符串中第一个匹配项的下标

https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

**示例 1：**

```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

**示例 2：**

```
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```

**提示：**

- `1 <= haystack.length, needle.length <= 104`
- `haystack` 和 `needle` 仅由小写英文字符组成

```go
func strStr(haystack, needle string) int {
    n, m := len(haystack), len(needle)
outer:
    for i := 0; i+m <= n; i++ {
        for j := range needle {
            if haystack[i+j] != needle[j] {
                continue outer
            }
        }
        return i
    }
    return -1
}

```

### 给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

[无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
 
