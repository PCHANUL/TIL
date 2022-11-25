---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

# Today I Learned <!-- omit in toc -->

- [22.11.25](#221125)
	- [변환 생성자의 묵시적 변환](#변환-생성자의-묵시적-변환)
- [22.11.24](#221124)
	- [exception guarantees](#exception-guarantees)
- [22.11.13](#221113)
	- [포인터의 레퍼런스](#포인터의-레퍼런스)
- [22.11.10](#221110)
	- [Tree iterator](#tree-iterator)

---

## 22.11.25

### 변환 생성자의 묵시적 변환

c++에서는 변환 생성자에서 묵시적으로 형변환이 일어나는 경우가 있다. 컴파일러가 자동으로 임시 객체를 생성하여 형변환을 일으킨다. 묵시적 변환 생성자가 사용자 모르게 호출될 가능성을 차단하려면 explict 키워드를 사용하면 된다.  

const 변수를 사용하다보면 non-const 변수와 함께 함수의 인자로 들어가는 경우가 있다. 




참조 : https://swblossom.tistory.com/12


## 22.11.24

### exception guarantees

1. No-throw Guarantee
	함수가 예외를 발생하지 않음을 보장하고 항상 성공적으로 수행을 마친다.
	
2. Strong Guarantee
   데이타베이스에서 rollback 하는 것과 같이 예외가 발생했을때, 객체의 상태가 함수 수행 전과 같이 보존되는것을 보장한다. Transactional Guarantee라고도 한다. 

3. Basic Guarantee
	예외가 발생했을때 객체가 함수 수행 이전과 다른 값을 가질 수 있고, side effects 가 있을 수 있다. 하지만 오브젝트의 invariants 가 보존되고 resource leak 이 없는 상태이다.

4. No Guarantee (exception unsafe)
	이것은 exception 발생시 resource leak 을 포함하여 오브젝트의 상태에 대해서 어떤 보증도 없는 상태이다.

참조 : https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=mug896&logNo=140168091611

## 22.11.13

map 컨테이너의 메서드 중에서 `insert`는 삽입하려는 값이 존재하는지 먼저 확인한다. 이후에 값을 특정 위치에 삽입한다. 이 과정은 불필요하게 컨테이너의 요소를 두번 탐색한다. 한번의 탐색으로 동작이 완료되도록 최적화가 필요하다.  
삽입하려는 값의 존재를 확인하는 과정에서 삽입되어야하는 위치를 얻을 수 있다. 값이 존재하지 않으면 그 위치에 새로운 요소를 연결한다. 새로운 요소를 연결하는 과정에서 포인터의 레퍼런스를 사용하면 포인터 값을 깔끔하게 정리할 수 있다.  

### 포인터의 레퍼런스

포인터의 레퍼런스는 이중 포인터와 비슷하다.  
만약에 함수 내에서 인자를 이중 포인터로 받으면 두가지를 변경할 수 있다.  

- 이중 포인터가 가리키는 포인터의 값
- 이중 포인터가 가리키는 포인터가 가리키는 값

이중 포인터 대신에 포인터의 레퍼런스를 인자로 받는 경우에도 마찬가지로 두가지를 변경할 수 있다.  

- 포인터 레퍼런스가 가리키는 포인터의 값
- 포인터 레퍼런스가 가리키는 포인터가 가리키는 값

```cpp
void	func(int*& x, int* y)
{
	x = y;
}

int main(void) 
{
	int   a = nullptr;
	int*  b = 10;

	func(a, &b);

	std::cout << *a << ' ' << b << std::endl;
}

/* prints
10 10
*/
```

다음은 이진 트리에 노드를 삽입하는 함수이다.  
`new_node`의 `parent` 포인터를 변경하고, `child` 포인터 값을 `new_node`로 변경한다.  
포인터 레퍼런스를 사용하였기 때문에 `new_node`가 `parent`의 `left`인지 `right`인지 몰라도 된다.  

```cpp
void insert_node_at(__node_pointer parent, __node_pointer& child, __node_pointer new_node)
{
	new_node->left = nullptr;
	new_node->right = nullptr;
	new_node->parent = parent;
	child = new_node;
}
```





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

