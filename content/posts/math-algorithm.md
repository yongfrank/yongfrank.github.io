---
categories: study
date: "2022-10-02T16:37:00Z"
title: 'Swift: Math Algorithm'
---

## BFS

[BFS and DFS Templates Algorithms in Swift](https://holyswift.app/the-simplest-bfs-and-dfs-templates-for-algorithms-in-swift/)

## LRU - Least Recently Used

https://www.jianshu.com/p/e09870b60046
那什么是 LruCache 呢？其实 LRU(Least Recently Used) 的意思就是近期最少使用算法，它的核心思想就是会优先淘汰那些近期最少使用的缓存对象。

```swift
class ListNode {
    var key: Int
    var value: Int
    var next: ListNode?
    var prev: ListNode?
    
    init(key: Int, value: Int) {
        self.key = key
        self.value = value
    }
}

class LRUCache {
    private var cache = [Int: ListNode]()
    // 最大size
    private var max_size = 0
    // 当前size
    private var cur_size = 0
    // 头
    private var head: ListNode?
    // 尾
    private var tail: ListNode?
    
    init(_ capacity: Int) {
        max_size = capacity
    }
    
    public func get(_ key: Int) -> Int {
        if let node = cache[key] {
            moveToHead(node: node)
            return node.value
        }
        return -1
    }
    
    public func put(_ key: Int, _ value: Int) {
        if let node = cache[key] {
            node.value = value
            moveToHead(node: node)
        } else {
            let node = ListNode(key: key, value: value)
            
            addNode(node: node)
            cache[key] = node
            
            cur_size += 1
            if cur_size > max_size {
                removeTail()
                cur_size -= 1
            }
        }
    }
    
    /// 添加节点到头部
    private func addNode(node: ListNode) {
        if self.head == nil {
            self.head = node
            self.tail = node
        } else {
            let temp = self.head!
            self.head = node
            self.head?.next = temp
            temp.prev = self.head
        }
    }
    
    /// 移动到头部
    private func moveToHead(node: ListNode) {
        if node === self.head {
            return
        }
        
        let prev = node.prev
        let next = node.next
        prev?.next = next
        if next != nil {
            next!.prev = prev
        } else {
            self.tail = prev
        }
        let origin = self.head
        self.head = node
        self.head?.next = origin
        origin?.prev = self.head
    }
    
    /// 删除尾部
    @discardableResult
    private func removeTail() -> ListNode? {
        if let tail = self.tail {
            cache.removeValue(forKey: tail.key)
            self.tail = tail.prev
            self.tail?.next = nil
            return tail
        }
        return nil
    }
}

let cache = LRUCache(3)
cache.put(1, 1)
cache.put(2, 2)
cache.put(3, 3)
cache.put(4, 4)
print(cache.get(4))
print(cache.get(3))
print(cache.get(2))
print(cache.get(1))
print("==========")
cache.put(5, 5)
print(cache.get(1))
print(cache.get(2))
print(cache.get(3))
print(cache.get(4))
print(cache.get(5))

/**
print("==========")
4
3
2
-1
==========
-1
2
3
-1
5
==========
*/
```

## Dynamic Programming

[一个例子带你走进动态规划 -- 青蛙跳阶问题](https://cloud.tencent.com/developer/article/1822779)

暴力递归

★leetcode原题：一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 10 级的台阶总共有多少种跳法。

有些小伙伴第一次见这个题的时候，可能会有点蒙圈，不知道怎么解决。其实可以试想：

* 要想跳到第10级台阶，要么是先跳到第9级，然后再跳1级台阶上去;要么是先跳到第8级，然后一次迈2级台阶上去。
* 同理，要想跳到第9级台阶，要么是先跳到第8级，然后再跳1级台阶上去;要么是先跳到第7级，然后一次迈2级台阶上去。
* 要想跳到第8级台阶，要么是先跳到第7级，然后再跳1级台阶上去;要么是先跳到第6级，然后一次迈2级台阶上去。
”
假设跳到第n级台阶的跳数我们定义为f(n)，很显然就可以得出以下公式：

## Tree

### Preorder Traverse

[二叉树的前序遍历](https://juejin.cn/post/7126400860640247815)

```swift
class Solution {
   func preorderTraversal(_ root: TreeNode?) -> [Int] {
        var res: [Int] = []
       if nil == root {
           return res
       }
       
       var stack: [TreeNode?] = []
       var node: TreeNode? = root
       
       while !stack.isEmpty || nil != node {
           while nil != node {
               res.append(node!.val)
               stack.append(node)
               node = node?.left
           }
           node = stack.removeLast()?.right
       }
       return res
    }
}
```

## Prime Number

A prime number is a whole number greater than 1 whose only factors are 1 and itself.

2, 3, 5, 7, 11, 13, 17, 19, 23, 29 are prime numbers.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f0/Primes-vs-composites.svg/440px-Primes-vs-composites.svg.png" alt="Primes vs composites" height="200"/>

### [Fastest Prime Checker by Noah Wilder on the stack overflow](https://stackoverflow.com/questions/31105664/check-if-a-number-is-prime)

> Swift 4.2, Xcode 10.1
>
> This prime checking function and extension is the most efficient as it checks the divisibility of only $\frac{1}{2}\sqrt{n}$ integers.
>
> Complexity: $O(\frac{1}{2} \sqrt{n})$

```swift
///  https://stackoverflow.com/questions/31105664/check-if-a-number-is-prime
///  Check if a number is prime?
import Foundation
func isPrime(for number: Int) -> Bool {
    guard number >= 2 else { return false }
    guard number != 2 else { return true }
    guard number % 2 != 0 else { return false }
    
    /// `stride(from: 3, to: Int(sqrt(Double(number))), by: 2)`
    /// traverse 3 5 7 9 11.... odd number
    ///
    /// `stride(from:to:by:).contains(where:)`
    /// If number % index = 1, then it's not prime.
    /// If the odd number contains number that the specific number can be divided by. Then it's not prime.
    /// So there is a negation operator before stride.
    /// True -> Prime False -> Not Prime
    return !stride(from: 3, to: Int(sqrt(Double(number))), by: 2).contains(where: { index in
        return number % index == 0
    })
}

print(isPrime(for: 4))
```
### Extension
```swift
extension Int {
    ///  A prime number is a whole number greater than 1 whose only factors are 1 and itself.
    var isPrime: Bool {
        guard self >= 2 else { return false }
        guard self != 2 else { return true }
        guard self % 2 != 0 else { return false }
        
        return !stride(from: 3, to: Int(sqrt(Double(self))), by: 2).contains(where: { index in
            return self % index == 0
        })
    }
}

var number = 3
number.isPrime
```

### Get odd number in C

> * Store the size of the array to be returned in the `result_count` variable
> * Allocate the array statically or dynamically.
> 
> [C get odd numbers between a range and store it in int array](https://stackoverflow.com/questions/52989394/c-get-odd-numbers-between-a-range-and-store-it-in-int-array) - by user10543939 on the stack overflow

```c
int* oddNumbers(int start, int end, int* result_count) {
    int *result;
    int i;
    
    /// @brief #define	assert(e) \
    /// (__builtin_expect(!(e), 0) ? __assert_rtn(__func__, __ASSERT_FILE_NAME, __LINE__, #e) : (void)0)
    /// @param start 
    /// @param end 
    /// @param result_count 
    /// @return 
    assert(result_count && start <= end && INT8_MIN < end);

    // if start is even, then add 1 to be odd
    start += !(start % 2);
    // if end is even, then minus 1 to be odd
    end -= !(end % 2);

    // calculate array size
    *result_count = (end - start) / 2 + 1;
    result = malloc(*result_count * sizeof(result));
    
    if(!result) return NULL;

    // fill in numbers in the result
    for (i = 0; start <= end; i++, start += 2) result[i] = start;

    return result;
}

int main() {
    int min = 30;
    int max = 85;
    // address of count
    int count;
    int *numbers = oddNumbers(min, max, &count);
    
    for (int i = 0; i < count; i++) {
        printf("%d ", numbers[i]);
    }
    // Don't forget to `free()` the returned pointer when you are done using it.
    free(numbers);
}
```

### Stride and generics template in C++

```c++
template<typename T>
T * stride(T start, T end, T step) {
    T * arrayOfOutPut;

    int sizeOfArray = (end - start) / step + 1;
    arrayOfOutPut = (T *)malloc(sizeOfArray * sizeof(T));

    for(int i = 0; i < sizeOfArray; i++) {
        arrayOfOutPut[i] = start + i * step;
        std::cout << arrayOfOutPut[i] << " ";
    }

    return arrayOfOutPut;
}

int main() {
    double start = 2;
    double end = 5;
    double step = 0.5;

    int sizeOfArray = (end - start) / step + 1;
    double* a = stride(start, end, step);
    std::cout << std::endl;
    for(int i = 0; i < sizeOfArray; i++) {
        std::cout << a[i] << " ";
    }
}
```

### valarray

```c++
#include <iostream>
#include <valarray>

bool isPrime(int number) {
    if (!(number > 2)) return false;
    if (!(number != 2)) return true;
    if (!(number % 2 != 0)) return false;

    int end = number;
    int start = 3;
    int step = 2;
    int sizeOfArray = sizeOfArray = (end - start) / step + 1;
    
    std::valarray<int> array(sizeOfArray);
    for(int i = 0; i < sizeOfArray; i++) {
        array[i] = i;
    }

    std::slice slcOfArray(start, end, step);

    std::valarray<int> valArrayOutput = std::valarray<int>(array[slcOfArray]);
    
    for(int i = 0; i < sizeOfArray; i++) {
        if (number % valArrayOutput[i] == 0) return false;
    }
    return true;
}

int main() {
    int numberCount = 0;

    int startNumber = 200;
    int endNumber = 299;

    for(int i = startNumber; i <= endNumber; i++) {
        if(isPrime(i)) {
            numberCount += 1;
            std::cout << i;
            if(numberCount == 8) {
                std::cout << std::endl;
                numberCount = 0;
            } else {
                std::cout << " ";
            }
        }
    }
}
```

### Sieve of Eratosthenes

[Sieve of Eratosthenes - GeeksForGeeks](https://www.geeksforgeeks.org/sieve-of-eratosthenes/)

[Khan Academy - Sieve of Eratosthenes](https://www.khanacademy.org/computing/computer-science/cryptography/comp-number-theory/v/sieve-of-eratosthenes-prime-adventure-part-4)

```pseudo
For all numbers a: from 2 to sqrt(n)
    IF a is unmarked THEN
        a is prime
        for all multiples of a (a < n)
            mark multiples as composite

All unmarked numbers are prime!
```

```c++
#include <stdio.h>

void SieveOfEratosthenes(int n) {
    
    bool prime[n + 1];
}

```