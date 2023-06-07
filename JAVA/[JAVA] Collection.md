# Collection

## Java Collection Framework (JCF)
- Java에서 Collection이란 데이터의 집합, 그룹을 의미한다.
- JCF : 데이터, collection, 이를 구현하는 클래스를 정의하는 **인터페이스**를 제공한다.

<br>

---
### Java Collection Framework의 상속구조

![java_collections](https://github.com/ksu3101/TIL/blob/master/DS/image/java_collections.jpg)


**Collection**
- List
    - LinkedList
    - Stack
    - Vecter
    - ArrayList
- Set
    - HashSet
    - SortedSet
        - TreeSet

**Map**
- Hashtable
- HashMap
- SortedMap
    - TreeMap

<br>

Collection 인터페이스는

1. List
2. Set
3. Queue

로 크게 3가지 상위 인터페이스로 분류할 수 있다.

> Map은 Collection 인터페이스를 상속받고 있지 않지만 Collection으로 분류된다.

<br>

---
### Collection 인터페이스의 특징

|인터페이스|구현클래스|특징|
|---|---|---|
|Set|HashSet, TreeSet|순서를 유지하지 않는 데이터의 집합. 데이터의 중복을 허용하지 않음|
|List|LinkedList, Vector, ArrayList|순서가 있는 데이터의 집합. 데이터의 중복을 허용|
|Queue|LinkedList, PriorityQueue|List와 유사|
|Map|Hashtable, HashMap, TreeMap|키(key), 값(value)의 쌍으로 이루어진 데이터의 집합. 순서는 유지되지 않음. 중복을 허용하지 않지만 값(value)의 중복은 허용|

<br>

### 1. Set 인터페이스
: 순서를 유지하지 않는 데이터의 집합으로, 데이터의 중복을 허용하지 않음

- **HashSet**
    - 가장 빠른 임의의 접근 속도
    - 순서를 예측할 수 없음
- **TreeSet**
    - 정렬 방법을 지정할 수 있음

<br>

### 2. List 인터페이스
: 순서가 있는 데이터의 집합으로 데이터의 중복을 허용함

- **LinkedList**
    - 양방향 포인터 구조
    - 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용함.
    - 스택, 큐, 양방향 큐 등을 만들기 위한 용도
- **Vector**
    - 내부에서 자동으로 동기화 처리가 일어나 비교적 성능이 좋지 않고 무거움.
- **ArrayList**
    - 단방향 포인터 구조
    - 각 데이터에 대한 인덱스를 가지고 있어 조회할 수 있음.

<br>

### 3. Map 인터페이스
: 키(key), 값(value)의 쌍으로 이루어진 데이터의 집합으로, 순서는 유지되지 않으며, 키(key)의 중복은 허용하지 않고 값(value)의 중복은 허용함.

- **Hashtable**
    - HashMap보다는 느리지만 동기화 지원
    - null값이 올 수 없음.
- **HashMap**
    - 중복과 순서가 허용되지 않음.
    - null값이 올 수 있음.
- **TreeMap**
    - 정렬된 순서대로 키(key)와 값(value)를 저장하여 검색이 빠름

<br>

## Collection을 사용하는 이유

1. **일괄된 API**
  -  Collection 밑에 있는 모든 클래스 Collection에서 상속받아 통일된 메서드를 사용할 수 있게 된다.

2. **프로그래밍 노력 감소**
  - OOP의 추상화의 기본 개념이 성공적으로 구현되어 있다.
      - 추상화란?
          - 공통의 속성이나 기능을 묶어 이름을 붙이는 것
          - ex) 물고기, 사자, 토끼를 동물 또는 생물로 묶는 것

<br>

3. **프로그램 속도 및 품질 향상**
- 유용한 데이터 구조 및 알고리즘은 성능을 향상시킬 수 있다.
- Collection을 통해 최상의 구현을 생각할 필요없이 간단하게 Collection API를 사용하여 구현을 하면 된다.

<br>

# 느낀 점
Collection이라는 존재는 수업시간에 들어 알고있었지만 그 개념 자체에 대한 이해도는 많이 떨어졌었습니다.

하지만 오늘 JCF의 상속구조와 Collection의 각 인터페이스 구조를 훑어보니 아 Collection에는 Set, List, Map이 있고 여기에는 Stack도 있고 Queue도 있구나! 하는 큰 틀을 잡을 수 있게 된 것 같아요!

자료구조 시간에 봤던 Stack, Queue, LinkedList, ArrayList 등등도 여기서 봐서 되게 새롭고 재미있네요.

LinkedList와 ArrayList의 비슷하면서도 다른 점을 알게 되어서 수업시간에는 몰랐던 재미까지 느낄 수 있어서 참 좋았습니다.

앞으로 Collection을 잘 활용해서 성능을 향상시킬 수 있는 코드를 짜는 개발자가 됐으면 좋겠네요.