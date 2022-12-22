---
layout: default
title: RBTree
nav_order: 3
grand_parent: Projects
parent: Containers
permalink: /docs/projects/Containers/redblacktree
---

- [Red-Black Tree](#red-black-tree)
  - [Rotation](#rotation)
  - [Insertion](#insertion)
    - [Recoloring](#recoloring)
    - [Restructuring](#restructuring)
      - [`P`노드 회전](#p노드-회전)
      - [`G`노드 회전](#g노드-회전)
  - [Deletion](#deletion)


# Red-Black Tree

레드-블랙 트리(Red-Black Tree)는 균형 이진 탐색 트리이다. 복잡한 자료구조이지만 효율적이고 우수한 실행 시간을 보인다. 트리에 n개의 요소가 있을때 O(log n)의 시간복잡도로 탐색, 삽입, 제거를 할 수 있다.  

이진 트리의 특수한 형태로 레드-블랙 트리는 각각의 자료를 노드에 저장한다.  
노드는 최대 두 개의 자식 노드를 가질 수 있고, 자식 노드도 최대 두 개의 자식 노드를 가질 수 있어서 이런식으로 계속 연결된다.  
어떤 노드에 자식 노드가 없다면 트리의 가장 자리에 있다고 하여 그 노드를 리프 노트라고 한다. 레드-블랙 트리의 리프 노드는 자료를 가지고 있지 않으며 비어 있다.  
보통 이진 탐색 트리에서 노드들은 다음과 같은 관계를 가지고 있다. 자신이 가진 자료는 자신의 왼쪽에 있는 노드가 가진 자료보다 크거나 같고, 자신의 오른쪽에 있는 노드가 가진 자료보다 작거나 같다.  

레드-블랙 트리는 각각의 노드가 `Red` 또는 `Black`인 색상 속성을 가지고 있는 이진 탐색 트리이다.  
다음과 같은 조건을 충족해야 유효한 레드-블랙 트리라고 한다.  
1. 노드는 `Red` 또는 `Black`이다.
2. 루트 노드는 `Black`이다.
3. 모든 리프 노드는 `Black`이다.
4. `Red` 노드의 자식 노드는 모두 `Black`이다.
5. 어떤 노드에서 시작하여 하위의 리프 노드에 도달하는 경로에 있는 `Black` 노드의 개수는 모두 같다. 

이러한 조건을 만족하면 레드-블랙 트리이며 개략적으로 균형이 잡힌다. 최악의 경우에 시간복잡도가 트리의 높이에 따라 결정되기 때문에 보통의 이진 탐색 트리보다 효율적이라고 할 수 있다.  
레드-블랙 트리의 특성을 만족하며 동작하기 위해서는 색 변환과 트리 회전이 필요하다. 삽입과 삭제가 복잡하게 동작하지만 복잡도는 여전히 O(log n)이다.  

## Rotation

레드-블랙 트리에서는 조건에 따라 노드의 구조를 회전하듯이 변경한다.  
회전은 어떠한 노드를 기준으로 왼쪽 혹은 오른쪽으로 이루어진다.  

```cpp
#define LEFT	0
#define RIGHT	1

enum color_t { BLACK, RED };

struct RBnode {
	RBnode* 		parent;
	RBnode* 		child[2];
	enum color_t	color;
	int				key;
}

struct RBtree {
	RBnode*	root;
}

RBnode* RotateDirRoot(
    RBtree* T,  // red–black tree
    RBnode* P,  // root of subtree (may be the root of T)
    int dir)    // dir ∈ { LEFT, RIGHT }
{
   RBnode* G = P->parent;
   RBnode* S = P->child[1 - dir];
   RBnode* C;

   assert(S != NIL); // pointer to true node required
   C = S->child[dir];
   P->child[1 - dir] = C; 
   if (C != NIL) {
      C->parent = P
   };
   S->child[  dir] = P; 
   P->parent = S;
   S->parent = G;
   if (G != NULL)
      G->child[ P == G->right ? RIGHT : LEFT ] = S;
   else
      T->root = S;
   return S; // new root of subtree
}

#define RotateDir(N,dir) RotateDirRoot(T,N,dir)
#define RotateLeft(N)    RotateDirRoot(T,N,LEFT)
#define RotateRight(N)   RotateDirRoot(T,N,RIGHT)
```


![](../../src/Binary_Tree_Rotation_Animation.gif)



## Insertion

레드-블랙 트리의 삽입은 노드의 색상 속성을 `Red`로 정하며 시작한다. 단순 이진 탐색 트리에서 노드를 삽입하듯이 노드를 위치한다.  
그리고 주위 노드와의 관계를 확인하는데 삼촌 노드(uncle node) 개념을 도입한다. 삼촌 노드는 부모 노드 옆에 있는 노드이다.  
노드를 관계를 다음과 같이 명시하도록 한다. 삽입하는 노드는 `N`, N의 부모 노드를 `P`, P의 부모를 `G`, N의 삼촌 노드를 `U`로 나타내기로 한다.  

`Red`인 `P`노드에 `Red`인 `N`노드가 삽입되면 "`Red` 노드의 자식 노드는 모두 `Black`이다"라는 조건을 위반한다.  
이를 `Double Red`이라고 부르며 `U`노드의 색상에 따라서 해결방법이 달라진다.  

1. `U`노드가 `Red`인 경우 : Recoloring
2. `U`노드가 `Black`인 경우 : Restructuring

### Recoloring

`Double Red`인 상황에서 삼촌인 `U`노드가 `Red`라면 `Recoloring`을 한다.  
`P`노드와 `U`노드를 `Black`으로 변경하고, 조부모인 `G`노드를 `Red`로 변경한다.  
색상을 변경한 이후에 `G`노드를 확인해야 한다. 다음과 같이 두 가지 상황이 있을 수 있다.  

1. `G`노드가 `root`인 경우 : `G`노드를 `Black`으로 변경
2. `G`노드로 인해 `Double Red`가 발생된 경우 : `G`노드의 상위 노드에 대하여 `Recoloring` 또는 `Restructuring`을 진행

두 번째 상황은 `G`노드의 부모 노드의 색상이 `Red`인 경우이다. `Double Red`가 발생되기 때문에 `N`노드을 삽입할 때 발생한 경우와 마찬가지로 처리하면 된다.  

### Restructuring

`Double Red`인 상황에서 삼촌인 `U`노드가 `Black`이라면 `Restructuring`을 한다.  
다음에 나오는 방향은 `U`노드가 `G`노드의 왼쪽 자식이라고 가정한 경우이다. 만약에 오른쪽 자식이라면 방향을 반전시켜야 한다.  

`Restructuring`은 부모인 `P`노드의 어느 쪽에 `N`노드가 연결되어 있는지에 따라서 달라진다.  

1. `N`노드가 `P`노드의 왼쪽인 경우 : `P`노드 회전 후 `G`노드 회전
2. `N`노드가 `P`노드의 오른쪽인 경우 : `G`노드 회전

#### `P`노드 회전

`P`노드는 `N`노드의 오른쪽으로 이동하고, `N`노드는 기존 `P`노드의 위치로 이동하여 시계 방향으로 회전하듯이 위치가 변경된다.

1. `P`노드를 `N`노드의 오른쪽 자식으로 연결
2. 만약에 기존 `N`노드의 오른쪽에 자식이 있었다면 `P`노드의 왼쪽에 연결
3. `N`노드를 `P`노드가 연결되어 있던 `G`노드의 오른쪽에 연결

위의 과정을 마치면 `G`노드 회전을 해야하는 조건이 만들어지므로 다시 한번 회전시킨다.  

#### `G`노드 회전

`G`노드는 `U`노드의 위치로 이동하고, `P`노드는 기존 `G`노드의 위치로 이동하여 반시계 방향으로 회전하듯이 위치가 변경된다.  

1. `G`노드를 `P`노드의 왼쪽 자식으로 연결
2. 기존 `P`노드의 왼쪽 자식은 `G`노드의 오른쪽 자식으로 연결
3. 노드의 색상을 변경한다. `G`노드는 `Red`, `P`노드는 `Black`
   

## Deletion

레드-블랙 트리의 삭제는 기본적으로 이진 탐색 트리의 삭제를 기반한다.  
이진 탐색 트리와 같이 노드를 삭제한 후에 레드-블랙 트리의 규칙을 복원하는 과정이 필요할 수 있다.  
삭제되는 노드의 색상이 `Red`라면 아무 문제가 없지만 `Black`이라면 레드-블랙 트리 규칙에서 벗어나기 때문이다. 
삭제하려는 노드를 `M`, 자리를 대체하는 노드를 `C`, 삭제하려는 노드의 형제를 `S`라고 나타내기로 한다.   
`Black`노드를 제거하여 규칙에서 벗어나는 상황은 다음과 같다.  

1. `C`가 `Red`인 경우 : `C`의 색상을 `Black`으로 변경
2. `C`가 `Black`인 경우 : `Double Black` 처리

`C`가 `Black`이라면 레드-블랙 트리의 다섯번째 조건를 위반하기 때문에 구조를 변경하여 조건을 충족시켜야 한다.  
삭제하려는 노드의 형제인 `S`의 왼쪽 자식을 `SL`, 오른쪽 자식을 `SR`이라고 나타내기로 하며, `C`가 `S`의 왼쪽에 있다고 가정한다.  
다음과 같이 상황에 따라서 `Double Black`를 해결하는 방법이 다르다. 또한, 밸런스를 복구하는 작업은 루트에 도달하기 전까지 계속될 수 있다.  

1. `S`가 `Red`인 경우

```cpp
if (s.color == RED)
    s.color = BLACK
    c.p.color = RED
    left_rotate(c.p)
    s = c.p.right
```

2. `S`가 `Black`이고, `SL`, `SR`이 모두 `Black`인 경우

```cpp
if (s.left.color == BLACK && s.right.color == BLACK)
    s.color = RED
    c = c.p
```

3. `S`가 `Black`이고, `SL`은 `Red`, `SR`은 `Black`인 경우

```cpp
if (s.right.color == BLACK)
    s.left.color = BLACK
    s.color = RED
    right_rotate(s)
    s = c.p.right
```

4. `S`가 `Black`이고, `SR`이 `Red`인 경우

```cpp
s.color = c.p.color
c.p.color = BLACK
s.right.color = BLACK
left_rotate(c.p)
c = root
```

참조 - https://www.youtube.com/watch?v=iw8N1_keEWA&list=RDCMUCzDJwLWoYCUQowF_nG3m5OQ&index=17
