---
layout: default
title: Today I Learned
nav_order: 1
has_children: true
child_nav_order: desc
permalink: /
---

# Today I Learned <!-- omit in toc -->

* [22.11.17](#221117)
* [Red-Black Tree Removal](#red-black-tree-removal)
* [22.11.13](#221113)
  * [포인터의 레퍼런스](#포인터의-레퍼런스)
* [22.11.10](#221110)
  * [Tree iterator](#tree-iterator)

---

## 22.11.17

## Red-Black Tree Removal

이진 검색 트리에서 2개의 `자식`을 가진 노드를 제거할 때, `노드 왼쪽의 최댓값`이나 `노드 오른쪽의 최솟값`을 찾아서 제거하려는 노드의 자리로 옯긴다.  
여기에서 자리로 옯긴다는 말은 단순한 값의 복사를 의미하며, 복사한 뒤에 노드는 제거된다.  
값을 대체하고 제거되는 노드는 무조건 2개보다 적은 `자식`을 가지고 있다. 왜냐하면 이 노드는 `최댓값` 또는 `최솟값`으로 자신보다 큰 값이 없거나, 자신보다 작은 값이 없기 때문이다.  
이와 같은 이유로 2개의 `자식`을 가진 노드의 제거도 1개의 `자식`을 가진 노드의 제거로 볼 수 있다. 그러므로 1개의 `자식`을 가진 노드를 제거하는 방법만 알면 모두 해결할 수 있다.  

`M`을 삭제하고자 하는 노드, `C`를 `M`의 선택된 자식이라고 부르겠다.  

- `M`이 `RED`인 경우 : `M`을 `C`로 치환
- `M`이 `BLACK`이고, `C`가 `RED`인 경우 : 치환하고, `C`를 `BLACK`으로 변경
- `M`이 `BLACK`이고, `C`도 `BLACK`인 경우 : `Double Black`

`Double Black` 상태에서 노드 하나를 제거하면 양 쪽의 검은 노드의 갯수가 달라지기 때문에 해결할 방법이 필요하다.  
`M`을 `C`로 치환하여 `N`으로 부르고, `N`의 형제를 `S`, `N`의 부모를 `P`, `S`의 왼쪽 자식을 `SL`, `S`의 오른쪽 자식을 `SR`이라고 부르겠다.  
`Double Black`인 경우에 다음과 같은 상황이 있다.  

1. `P`, `S`, `SL`, `SR`이 `BLACK`인 경우
  - `S`를 `RED`로 변경
  - `P`를 `N`으로 정함
  - 다음 단계를 진행

2. `N`이 새로운 `root`인 경우
  - 제거 수정 완료

3. `S`가 `RED`인 경우
  - `P`를 왼쪽으로 회전
  - `P`는 `BLACK`, `S`는 `RED`로 색을 변경
  - `N`의 새로운 형제를 `S`로 정함
  - 다음 단계를 진행
    - `SR`이

4. `P`가 `RED`이고, `S`, `SL`, `SR`이 `BLACK`인 경우
  - `P`와 `S`의 색을 변경

5. `S`가 `BLACK`이고, `SL`이 `RED`, `SR`이 `BLACK`인 경우
  - `S`와 `SL`의 색을 변경
  - `S`를 오른쪽으로 회전
  - `SL`을 새로운 `S`로 정하여 다음을 진행

6. `S`가 `BLACK`이고, `SR`이 `RED`, `N`이 `P`의 왼쪽 자식인 경우
  - `P`를 왼쪽으로 회전
  - `P`와 `S`의 색을 변경
  - `SR`를 `BLACK`으로 변경




참조 - https://ko.wikipedia.org/wiki/%EB%A0%88%EB%93%9C-%EB%B8%94%EB%9E%99_%ED%8A%B8%EB%A6%AC

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

