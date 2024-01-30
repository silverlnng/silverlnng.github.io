---
layout: single
title:  "자료구조Study10_해시 테이블"
categories: DataStructure
tag: [DataStructure]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# 해시 함수

해시값으로 부터 원 데이터를 역산하는것은 사실상 불가능 하다. 데이터 입력과 출력의 흐름이 단 방향으로 이루어지며, 이것이 암호화와 크게 다른점이다.

## 해시 함수의 활용
사용자가 입력한 패스워드를 서버에 저장할 때 해시함수를 사용. 패스워드를 그대로 서버에 저장하면 제3자가 패스워드를 훔쳐볼 수 있어서  안된다. 패스워드의 해시값을 생성해서 해시값을 저장한다.<br>
그리고 사용자가 다시 패스워드를 입력했을때 이번에 입력한 해시값 과 데이터베이스의 해시값과 비교한다. 해시값으로 부터 원데이터를 역산하는것은 사실상 불가능 하기 때문에 설령 해시값이 노출 됐다고 해도 원래의 패스워드를 알아낼수없다.

# 해시 테이블



# 충돌 해결하기

# 개방 해싱
해시 테이블의 주소 바깥에 새로운 공간을 할당하여 문제를 해결

## 체이닝 
해시 함수가 서로 다른 키에 대해 같은 주소값을 반환해서 충돌이 발생하면 각 데이터를 해당 주소에 있는 링크드 리스트에 삽입하여 문제를 해결하는 기법

![image](https://github.com/silverlnng/NetworkClass/assets/112385982/e7e167ab-3d0d-4fc0-ad4d-ddb05b982a75)

### 삽입 연산
새로운 데이터를 링크드 리스트의 테일로 만든 다면 순차탐색을 통해서 테일 노드를 찾아 삽입해야한다. 그래서 새데이터를 링크드리스트의 가장 앞(헤드 의 앞)에 삽입하여 이러한 순차탐색을 수행하지않아서 성능저하를 막는다.

```cpp
void CHT_Set( HashTable* HT, KeyType Key, ValueType Value )
{
    int Address = CHT_Hash( Key, strlen(Key), HT->TableSize );
    Node* NewNode = CHT_CreateNode( Key, Value );

    /*  해당 주소가 비어 있는 경우 */
    if ( HT->Table[Address] == NULL )
    {
        HT->Table[Address] = NewNode;
    } 
    /*  해당 주소가 비어 있지 않은 경우 */
    else
    {    
        List L = HT->Table[Address];
        NewNode->Next         = L;
        HT->Table[Address] = NewNode;

        printf("Collision occured : Key(%s), Address(%d)\n", Key, Address );
    }
}
```
###


# 폐쇄 해싱

## 이중 해싱

## 재해싱