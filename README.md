---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

# Today I Learned <!-- omit in toc -->

* [22.11.10](#221110)
  * [Tree iterator](#tree-iterator)

---

## 22.11.10

### Tree iterator

Red-Black Tree의 반복자를 구현해본다.  
tree에서 제공되는 반복자는 `bidirectional iterator`이므로 `++` 또는 `--` 연산자로 반복자를 이동시킬 수 있다.  
호출된 연산자는 tree에서 다음 node를 찾게 된다. 예를 들어, `++` 연산자는 다음과 같은 과정을 거친다.  

- 현재 위치의 node에게 right child가 있는지 확인한다. 
  - right child가 있다면, 이 node에서 시작하여 가장 left에 있는 node를 찾은 후 반환한다.
  - right child가 없다면, 자신의 parent에게 자신이 left child인지 확인한다.
    - 자신이 left child이라면, parent를 반환한다.
    - 자신이 left child가 아니라면, 자신이 parent가 되어 left child인지 확인하는 과정을 반복한다.

컨테이너의 `end()` 멤버함수는 마지막 원소의 다음 공간을 가리키는 반복자를 반환한다.  
이를 위해서 tree에 end_node를 추가해야한다. end_node는 tree의 parent이며, tree는 end_node의 left child이어야 한다.  
다음과 같은 이유로 end_node의 left child는 tree이어야 한다.  
연산자가 다음 node를 찾는 과정에서 child가 없다면 parent를 확인하며 올라가다가 root에 닿는다.  
예를 들어, 마지막 원소를 가리키는 반복자의 `++` 연산자를 호출하면 자신이 left child일때까지 parent를 확인하며 올라간다.  
결국 마지막에 root는 end_node의 left child이기 때문에 end_node를 반환하게 된다.  
반대로 가장 앞의 원소에서 반복자의 `--` 연산자를 호출하면 자신이 right child인지 확인하기 때문에 end_node를 반환하지 않는다.  

