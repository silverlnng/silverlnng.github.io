---
layout: single
title:  "자료구조Study4_스택,큐"
categories: DataStructure
tag: [DataStructure]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# 스택
<br>

* 선입후출 = 후입선출
* 자동메모리 , 거의 대부분의 네트워크 프로토콜이 스택을 기반으로 구성

## 배열로 구현하는 스택
<br>

* 배열을 이용한 구현은 동적으로 스택의 용량을 조절하기 어렵다
    * 생성 초기에 사용자가 부여한 용량만큼

<br>

* 스택의 각 층을 구성하는 노드
    * 노드가 존재하는 위치는 배열의 인덱스로 알 수 있기 때문에 포인터가 필요없다

<br>

```cpp
typedef struct tagNode
{
    int Data;
} Node;
```

* 스택 구조체
    * 용량 :
    * 최상위 노드의 위치 : 삽입,제거시 최상위 노드에 접근 해야한다.
    * 노드 배열 : 스택에 쌓이는 노드를 보관
<br>


```cpp
typedef struct tagArrayStack
{
    int Capacity;
    int Top;
    Node* Nodes;
} ArrayStack;
```

![image](https://github.com/silverlnng/NetworkClass/assets/112385982/203a396c-392d-4bee-bb15-4a1aee1d6304)

## 기본연산

### 생성연산

<br>

```cpp
void  AS_CreateStack(ArrayStack** Stack, int Capacity)
{
    /*  스택을 자유저장소에 생성 */
    (*Stack) = (ArrayStack*)malloc(sizeof(ArrayStack));

    /*  입력된 Capacity만큼의 노드를 자유저장소에 생성 */
    (*Stack)->Nodes = (Node*)malloc(sizeof(Node)*Capacity);

    /*  Capacity 및 Top 초기화 */
    (*Stack)->Capacity = Capacity;
    (*Stack)->Top = 0;
}
```

### 삽입연산
<br>

* Top은 실제 최상위 노드 +1

<br>

```cpp
void AS_Push(ArrayStack* Stack, ElementType Data)
{
    int Position = Stack->Top;

    Stack->Nodes[Position].Data = Data;

    Stack->Top++;
}
```

### 제거연산
<br>

* 실제로 제거 되는 건 없고 Top을 조절하고 최상위 노드 값을 반환한다. 
    * 실제 최상위노드 = Top - 1

<br>

```cpp
ElementType AS_Pop(ArrayStack* Stack)
{
    int Position = --( Stack->Top );
    // 선행증감자로 Top을 먼저 1 감소시킨다.
    return Stack->Nodes[Position].Data;
    //가장 최상위 노드를 반환
}
```





## 링크드 리스트로 구현하는 스택

<br>

* 스택의 용량에 제한을 두지 않아도 된다
* 인덱스로 노드로 접근 하지않는다. 자신의 위에 위치하는 노드에 대한 포인터가 있어야한다

## 스택의 응용

# 큐

## 삽입, 제거 연산

## 순환 큐

## 링크드 큐
