---
layout: default
title: pair
nav_order: 2
parent: c++ 
permalink: /docs/c++/pair
---

- [pair](#pair)
  - [Template parameters](#template-parameters)
  - [Member types](#member-types)
  - [Member variables](#member-variables)
  - [Member functions](#member-functions)
  - [Non-member function overloads](#non-member-function-overloads)
  - [Non-member class specializations](#non-member-class-specializations)

# pair

이 클래스는 서로 다른 타입의 값 쌍을 연결한다. 개별 값은 공개 맴버인 `first`와 `second`를 통해 접근할 수 있다.  

## Template parameters

- `T1` : 맴버 `first`의 타입
- `T2` : 맴버 `second`의 타입

## Member types

| member type | definition                         | notes                  |
| ----------- | ---------------------------------- | ---------------------- |
| first_type  | The first template parameter (T1)  | Type of member first.  |
| second_type | The second template parameter (T2) | Type of member second. |

## Member variables

- `first` : 첫번째 값
- `second` : 두번째 값

## Member functions
- (constructor) : 페어 구성
- operator= : 컨텐츠 할당
- swap : 컨텐츠 교환

## Non-member function overloads
- relational operators : 페어의 관계 연산자
- swap : 컨텐츠 교환
- get : 요소 가져오기

## Non-member class specializations
- tuple_element<pair> : 페어의 튜플 요소 타입
- tuple_size<pair> : 페어에 대한 튜플 특성
