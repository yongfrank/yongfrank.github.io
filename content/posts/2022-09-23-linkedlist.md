---
categories: study
date: "2022-09-23T11:00:00Z"
title: Swift Linked List
---

<!--
 * @Author: Frank Chu
 * @Date: 2022-09-23 11:06:09
 * @LastEditors: Frank Chu
 * @LastEditTime: 2022-09-23 14:00:11
 * @FilePath: /blog/_posts/2022-09-23-linkedlist.md
 * @Description: 
 * 
 * Copyright (c) 2022 by Frank Chu, All Rights Reserved. 
-->

[Linked List by Kelvin Tan](https://daddycoding.com/2019/08/15/linked-list/)

[LinkedList in Swift by DamonLu on juejin.com](https://juejin.cn/post/7035278534037733407)

## Definition

Linked list is a chain of nodes. Nodes have two responsibilities:

* Hold a value.
* Hold a reference to the next node in which a nil value represent the end of the list.

## Node and LinkedList

Node has to be defined in Class, because of the reference.

```swift
//  The <T> represent generic data where you could use 
//  any data types such as Int, String or others.
//  Next is an optional because the last node might not have a next reference.
//  Node is a reference type, so we use class.
class Node<T> {
    var data: T
    var next: Node?
    
    init(data: T, next: Node? = nil) {
        self.data = data
        self.next = next
    }
}

//  To print out a proper linked list, insert the following code under Node.
//  The code above enable the print statement to be shown as below:
//  10 -> 4 -> 6
extension Node: CustomStringConvertible {
    var description: String {
        guard let next = next else { return "\(self.data)" }
        return "\(self.data) -> " + String(describing: next)
    }
}

//  A linked list essentially contain a head and a tail of Node type.
struct LinkedList<T> {
    var head: Node<T>?
    var tail: Node<T>?
    
    init() { }
    
    var isEmpty: Bool {
        head == nil
    }
    
    //  Push gives you the capability
    //  to adding a value at the beginning of the list.
    mutating func push(_ data: T) {
        //  Create a new node and assign it as the head
        head = Node(data: data, next: head)
        
        //  Check if there is tail, 
        //  if there isn’t tail, assign tail as head.
        if tail == nil {
            tail = head
        }
    }
    
    //  Pop gives you the capability 
    //  of removing the value at the front
    mutating func pop() -> T? {
        defer {
            //  Assigning the head down a node.
            head = head?.next
            if self.isEmpty {
                tail = nil
            }
        }
        print("pop: \(head?.data)")
        return head?.data
    }
    
    mutating func append(_ data: T) {
        //  If the list is empty, 
        //  it will use push to create a new node.
        guard !isEmpty else {
            push(data)
            return
        }
        //  It will add a new node 
        //  at the end of the node by assigning it as next.
        tail?.next = Node(data: data)
        
        //  The newly added node is now the tail
        tail = tail?.next
    }
    
    mutating func removeLast() -> T? {
        //  If the head is empty, 
        //  then the list is empty so return nil.
        guard let head = head else { return nil }
        
        //  If the list only has one node, 
        //  instead of removing last, it will pop.
        guard head.next != nil else {
            return self.pop() 
        } 
        
        //  The loop to reach to the end 
        //  of the linked list and assign, 
        //  this is where if the loop is at the end, 
        //  it will be stored as current 
        //  while the second last is stored in prev.
        var prev = self.head
        var current = head
        while let next = current.next {
            prev = current
            current = next
        }
        //  Take out the last note 
        //  and assign the second last as last node.
        prev?.next = nil
        tail = prev
        print("remove: \(current.data)")
        return current.data
    }
}

//  It's like Array<Int>
var linkedList: LinkedList<Int> = LinkedList<Int>()

//  Push some numbers to see your newly created linked list.
linkedList.push(3)
linkedList.push(5)

linkedList.pop()
linkedList.append(1000)
linkedList.append(2000)
print(linkedList.head?.description ?? "")
linkedList.removeLast()
print(linkedList.head?.description ?? "")
```
