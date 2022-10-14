---
layout: default
title: true/false type
nav_order: 2
grand_parent: c++ 
parent: type_traits
permalink: /docs/c++/type_traits/true_type&false_type
---

# true_type & false_type

```cpp
typedef integral_constant<bool,true> true_type;
typedef integral_constant<bool,false> false_type;
```

integral_constant의 인스턴스화하여 bool 값 true와 false를 나타낸다.  

## Example 

```cpp
template<typename T> struct is_pointer : false_type {};
template<typename T> struct is_pointer<T*> : true_type {};
```

