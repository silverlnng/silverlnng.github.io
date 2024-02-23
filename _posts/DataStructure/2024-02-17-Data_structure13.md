---
layout: single
title:  "자료구조Study13_문자열검색 알고리즘_카프-라빈"
categories: DataStructure
tag: [DataStructure]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# 문자열 검색
<br>
문자열 안에 존재하는 특정 단어를 빠르게 찾아내는 것이 목적
<br>
* 본문 : 탐색대상이 되는 문자열
* 패턴 : 검색어를 의미
* 이동 : 본문에서의 탐색위치를 옮기는 것
<br>

## 고지식한 검색(Naive Search ,Brute Force Search)

* 본문 : ABCABACDC , 패턴 : BA 

(1) 본문의 첫번째 문자부터 패턴과 비교 . 본문의 첫번째 문자(A) =/ 패턴의 첫번째 문자(B) 이므로 다음 문자로 이동.<br>
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/b7249a93-86fb-467c-862f-ef0cdac21851)<br>
<br>

(2) 본문의 두번째 문자로 이동. 본문 두번째 문자 (B) = 패턴의 첫번째 문자(B) , 본문 세번째문자(C) =/ 패턴의 두번째 문자(A)이므로 본문의 다음 문자로 이동
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/daa5467c-561a-4277-b504-96400d638e1a)   
<br>

(3) 
<br> 
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/9bda356f-6fca-462d-9f5a-0a330309f1be)  
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/22df3b74-c515-4cea-a254-7d6c1229e77e)  
   

(4) 본문의 새위치에 있는 문자 (B) = 패턴의 첫번째 문자(B) , 본문의 다음 위치에 있는 문자(A) = 패턴의 두번째 문자(A) 본문과 패턴이 일치하므로 검색 종료.<br>   
   
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/c8874aef-ae81-48e1-a1ea-b2a0d9ce2fa7)<br>
   
* 본문의 길이 N , 패턴의 길이 M 이라고 했을때 최악의 경우 N*M번 비교 수행


### 고지식한 검색 구현

```cpp
using namespace std;
 
void search(string& pat, string txt)
{
    int M = pat.size();
    int N = txt.size();
 
    /* A loop to slide pat[] one by one */
    for (int i = 0; i <= N - M; i++) 
    {
        int j;
 
        /* For current index i, check for pattern match */
        for (j = 0; j < M; j++)
            if (txt[i + j] != pat[j])
                break;
 
        if (j == M) // if pat[0...M-1] = txt[i, i+1, ...i+M-1]
            cout << "Pattern found at index " << i << endl;
    }
}
 
// Driver's Code
int main()
{
    string txt = "AABAACAADAABAAABAA";
    string pat = "AABA";
 
    // Function call
    search(pat, txt);
    return 0;
}
```


## 카프-라빈 알고리즘 
<br>
* 해시함수를 이용.
* 패턴의 해시값 , 본문안에 있는 하위 문자열의 해시값만 비교 
<br>

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/09fa0f4b-f1ca-4ac9-b4dd-1b560efc6dfa)


### 해시 함수의 비용을 줄이기
<br>
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/3e83d398-8d3f-46bc-8db4-65daca2d0af5)
<br>
* H1 은 S[0] (제거된 문자) ,S[4] (새로운 문자) 만 이용해서 구할 수 있게 식이 간추려 진다.

* 새로운 해시함수 정의 
<br>
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/2eaba2ef-86a3-41f0-8444-9cad8d1ceabf) 

* 이 해시함수로 총 탐색시간이 패턴의 길이와 상관없고 본문의 길이에만 영향을 받게되어 탐색성능이 훨씬 향상
<br>
* 다만 새 해시 함수는 i가 0일 때, 즉 이전 해시 값이 미리 구해지지 않은 경우에는 사용할 수 없으므로 최초의 해시 값올 구할 때는 기존의 해시 함수를 사용.

### 해시값 일정범위 안에 가두기
<br>
* 문자열의 길이가 늘어나면 해시값도 따라 커진다는 문제 -> 해시값을 일정 범위 안에 가두기
<br>
* 해시 함수 결과를 특정 값 ("q") 으로 나눈 나머지를 해시값으로 사용
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/79331f4e-5525-4a2d-95fd-43d0a508b7e3)      

---

            
### 예시
   
본문:ABACCEFABADD   
   
패턴:CCEFA   
   
q의 값 : 2147483647(int의 최대값)   
   
   
(1) 패턴의 해시값과 본문[0~4] 의 해시값을 구하기. 맨 처음 단계는 패턴이나 본문 모두 활용할수있는 '이전 해시값' 는 상태로 
---

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/9e4b7de9-6a38-4b3a-a226-47e0b1b01186) 
---

다음의 해시함수 활용.   
---

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/fc548a85-4831-414a-8947-7c908043bedd)
---

   
(2) 이제는  본문[0~4]의 해시값이 있어서 활용할수 있는 단계. 
---

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/a71c84e1-5071-4360-9666-22085668b567)  
---

다음의 해시함수 사용
---

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/ec1c418e-eeb2-413c-b4f9-78d63186632b)   
---


### 카프-라빈 알고리즘 구현 부분

* KarpRabin 헤더파일
   
```cpp
#pragma once
#ifndef KARPRABIN_H
#define KARPRABIN_H

int KarpRabin(char* Text, int TextSize, int Start,
    char* Pattern, int PatternSize);

int Hash(char* String, int Size);
int ReHash(char* String, int Start, int Size, int HashPrev, int Coefficient);
#endif
```

* KarpRabin cpp 파일
   
```cpp
#include "KarpRabin.h"
#include <stdio.h>
#include <math.h>

int KarpRabin(char* Text, int TextSize, int Start, char* Pattern, int PatternSize)
{
    int i = 0;
    int j = 0;
    int Coefficient = pow(2, PatternSize - 1); 
    // 2의 PatternSize - 1 제곱 .  함수안에서 매번 계산하는것은 효율적이지 못하다.
    // 밖에서 변수로만들어서 계산한후 안으로 가져와서 사용하기 ! 


    int HashText = Hash(Text, PatternSize);
    int HashPattern = Hash(Pattern, PatternSize);

    for (i = Start; i <= TextSize - PatternSize; i++)
    {
        HashText = ReHash(Text, i, PatternSize - 1, HashText, Coefficient);

        if (HashPattern == HashText)
        {
            for (j = 0; j < PatternSize; j++)
            {
                if (Text[i + j] != Pattern[j])
                    break;
            }

            if (j >= PatternSize)
                return i;
        }
    }

    return -1;
}

int Hash(char* String, int Size)    //최초한번 [0,패턴의 길이] 만큼 구하는 함수
{
    int i = 0;
    int HashValue = 0;

    for (i = 0; i < Size; i++)
    {
        HashValue = String[i] + (HashValue * 2);
        // i = 0 , HashValue = String[0] + 0*2
        // 
        // i = 1 , HashValue = String[1] + String[0]*2

        /* i = 2 , HashValue = String[2] + (String[1] + String[0]*2)*2
                             = String[2] + String[1]*2 + String[0]*2*2 */

        /* i = 3 , HashValue = String[3] + (String[2] + (String[1] + String[0]*2)*2)*2
                             = String[3] + String[2]*2 + String[1]*2*2 + String[0]*2*2*2 */
    }
    return HashValue;
}

//Hash() 함수를 통해 (초기 해시값)을  제거된 문자 , 새롭게 추가된 문자만 이용해서 계산하는 함수 
int ReHash(char* String, int Start, int Size, int HashPrev, int Coefficient) 
{
    if (Start == 0)
        return HashPrev;

    return String[Start + Size - 1] +
        ((HashPrev - Coefficient * String[Start - 1]) * 2);
}
```
* KarpRabin test 파일
   
```cpp
#include <stdio.h>
#include <string.h>
#include "KarpRabin.h"
#include <math.h>

#define MAX_BUFFER 512

int main(int argc, char** argv)
{
    char  Text[MAX_BUFFER] = "My name is pattern";
    char* Pattern =(char*)"pattern";
    int   PatternSize = 4;
    int h0, h1, rh1;

    int result = KarpRabin(Text, strlen(Text), 0, Pattern, strlen(Pattern));
    printf("%d\n", result);

    h0 = Hash(Pattern, PatternSize);
    h1 = Hash(Pattern + 1, PatternSize);
    rh1 = ReHash(Pattern, 1, PatternSize, h0, (int)pow(2, PatternSize - 1));

    printf("h0:%d\n", h0);
    printf("h1:%d\n", h1);
    printf("rh1:%d\n", rh1);

    return 0;
}
```