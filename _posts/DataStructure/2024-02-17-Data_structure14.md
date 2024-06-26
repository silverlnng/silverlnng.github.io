---
layout: single
title:  "자료구조Study14_문자열검색 알고리즘_KMP,보이어"
categories: DataStructure
tag: [DataStructure]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# 문자열 검색

문자열 안에 존재하는 특정 단어를 빠르게 찾아내는 것이 목적

* 본문 : 탐색대상이 되는 문자열
* 패턴 : 검색어를 의미
* 이동 : 본문에서의 탐색위치를 옮기는 것

# KMP 알고리즘
<br>
<span style="font-size:150%">패턴과 본문내의 문자열을 한 차례 비교하고 나면 , 다음 단계의 검색에서 사용할 수 있는 '어떤 정보'가 남으며 이 정보를 이용하면 불필요한 비교를 피할수있음을 이용</span>

*

## 접두부 , 접미부 , 경계
* 접두부 : 문자열의 머리부분
* 접미부 : 문자열의 꼬리부분
    * 빈 문자열도 접두부, 접미부가 될 수 있다.
* 경계 : 서로 일치하는 접두부와 접미부
   
<span style="font-size:150%">|BAABABAA|</span>   
   
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/f6314d41-a908-44b6-b2d7-19b9431f4c36)


## 예시
본문:BAABAABAB
패턴:BAABAB
<br>
(1) 본문과 일치했던 패턴의 문자열까지 보기   
-> 0~4번 문자열 (BAABA) 까지는 서로 일치. 5번 문자에서 불일치   
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/11c938d6-be4c-46b1-a733-39f0d2d295af)

<br>
(2) 일치했던 패턴의 문자열까지 경계를 가지고 있는지 확인하기   
->
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/5165077a-9224-43a3-91fe-1c3932b22cd9)   
<br>

(3) 본문의 접미부 = 패턴의 접두부를 활용하기   
-> 본문과 패턴의 [0~4] 는 서로 일치하기 때문에 경계도 서로 같다 .  
   

(4) 패턴을 일치하는 부분문자열 길이 에서 경계의 길이 만큼 뺀것만큼 이동시키고 불일치가 발견된 곳에서 검색을 재개   
->일치하는 부분 문자열 BAABA 의 길이 (5) 에서 경계의 길이 (2)를 빼만큼 이동시키고 불일치가 발견된 5번 부터 검색을 재개
   
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/7292746c-0f2b-4d86-bb54-a36cd02bbe67)   



* 본문의 길이가 N일 때, 최대 N번만큼만 비교를 수행하면 본문 내에서 패턴과 일치하는 문자열의 위치를 알아낼 수 있습니다

## 경계 정보를 미리 계산하기
BAABABAA 패턴

(1) 첫번째 문자부터 불일치


(2) 2번째 문자부터 불일치


(3) 5번째 문자에서 불일치가 일어나는 경우


(4)
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/4bd2dea3-357c-4744-8a38-6828cd99f9de)


이동거리 = 일치 접두부의 길이 - 최대 경계 너비

## KMP 알고리즘 구현부분

* KarpRabin.h
   
```cpp
#ifndef KNUTHMORRISPRAT_H
#define KNUTHMORRISPRAT_H

#include <stdio.h>

int  KnuthMorrisPratt(char* Text, int TextSize, int Start, 
                      char* Pattern, int PatternSize );

void Preprocess( char* Pattern, int PatternSize, int* Border );

#endif
```

* KarpRabin.cpp
   
```cpp
#include "KnuthMorrisPratt.h"
#include <stdlib.h>

void Preprocess( char* Pattern, int PatternSize, int* Border ) 
{
    int i = 0;
    int j = -1;

    Border[0] = -1;

    while ( i < PatternSize )
    {
        while ( j > -1 && Pattern[i] != Pattern[j] )
            j = Border[j];

        i++; 
        j++;

        Border[i] = j;
    }
}

int  KnuthMorrisPratt(char* Text, int TextSize, int Start, 
                      char* Pattern, int PatternSize )
{
    int i = Start;
    int j = 0;
    int Position = -1;
    
    int* Border = (int*)calloc( PatternSize + 1, sizeof( int ) );

    Preprocess( Pattern, PatternSize, Border );

    while ( i < TextSize )
    {
        while ( j >= 0 && Text[i] != Pattern[j] )
            {j = Border[j];}

        i++;
        j++;

        if ( j == PatternSize )
        {
            Position = i - j;
            break;
        }
    }

    free( Border );
    return Position;
}
```

* Test_KarpRabin.cpp
   
```cpp
#include <stdio.h>
#include <string.h>
#include <time.h>

#include "KnuthMorrisPratt.h"

#define MAX_BUFFER 512

int main(int argc, char** argv)
{
    char* FilePath;
    FILE* fp;

    char  Text[MAX_BUFFER];
    char* Pattern;
    int   PatternSize = 0;
    int   Line        = 0;
    
    if ( argc < 3 )
    {
        printf("Usage: %s <FilePath> <Pattern>\n", argv[0] );
        return 1;
    }

    FilePath = argv[1];
    Pattern  = argv[2];

    PatternSize = strlen( Pattern );

    if ( (fp = fopen( FilePath, "r"))  == NULL )
    {
        printf("Cannot open file:%s\n", FilePath);
        return 1;
    } 

    while ( fgets( Text, MAX_BUFFER, fp ) != NULL )
    {
        int Position = 
            KnuthMorrisPratt( Text, strlen(Text), 0, Pattern, PatternSize );
        
        Line++;

        if ( Position >= 0 ) 
        {
            printf("line:%d, column:%d : %s", Line, Position+1, Text);
        }
    }

    fclose( fp );

    return 0;
}
```
   
# 보이어 무어 알고리즘
   
* 패턴을 오른쪽에서 왼쪽으로 비교. 이동은 왼쪽에서 오른쪽으로.
    * 패턴의 가장 오른쪽 끝에 있는 문자가 불일치하고, 불일치한 본문의 문자가 패턴내에 존재하지 않는 경우 , 패턴의 길이만큼 이동한 다음 검색을 재개

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/2724be43-7016-4ef1-b3bb-6a56866c14a4)


   
## 나쁜 문자 이동
   
(1) 패턴에서 나쁜문자를찾습니다.   

(2) 찾아낸패턴의 나쁜문자의 위치가 본문의 나쁜문자위치와 일치하게 패턴을 이동시킵니다.   

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/18eaa30a-e0fb-4f3d-a281-cc6fd91468ae)


## 착한 접미부 이동
   
* 패턴을 오른쪽에서 부터 비교하기 때문에 패턴에는 본문과 일치하는 접미부가 나타낼수있다. 이렇게 일치하는 접미부를 착한접미부라고 한다. 

(1) 불일치가 일어났을때 착한 접미부와 동일한 문자열이 패턴의 착한 접미부 왼쪽에 존재하는 경우 

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/2b5d6505-1f0b-4679-a6ac-5b39a094e9f8)

(2) 착한접미부의 접미부가 패턴의 접두부와 일치하는 경우

* 착한접미부의 전체가 아닌 착한접미부의 접미부 이다 !

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/81044909-fb8e-4492-b988-1b9100872ed6)

## 전처리 과정

### 나쁜문자 이동 테이블
   
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/0fb010bb-79ec-47b2-976b-905cfb706f8e)
   

### 착한접미부 이동 테이블

* 착한접미부 이동 테이블을 만드는 이유
    * 착한 접미부의 존재여부를 검색하기 전에 확인하고 존재한다면 이동거리를 테이블안에 저장해두고 , 불일치가 일어났을때 착한접미부를 확인하자마자 바로 이동거리를 얻는 것

(1) 불일치가 일어났을때 착한 접미부와 동일한 문자열이 패턴의 착한 접미부 왼쪽에 존재하는 경우 


* 패턴의 오른쪽에서부터 왼쪽으로 부터 하나씩 추가해가는 접미부 X , 이 접미부를 경계로 가지는 패턴내의 가장큰 접미부 Y.
    * X의 시작위치 - Y의 시작위치를 이동거리로 입력합니다.

* 주의사항 
    * 접미부 X가 다른 접미부의 경계가 되지않는 경우 
    * 접미부 X가 경계라 하더라도 이 경계가 왼쪽방향으로 더 큰 경계로 확장될 수 있는 경우
        * 이동거리를 0 으로 비워두고 두번째 경우에 처리하기


#### 예시: AABABA
##### 이동거리
* 문자열의 5번에 있는 A : A를 경계로 가지는 다른 접미부 ABA , ABABA , AABABA 세가지이다.   
AABABA 는 패턴의 시작부분부터 포함하고 있어서 선택 대상에서 제외   

이동거리 ABABA 의 시작위치는 1, A의 시작위치는 5 . 이동거리 5-1=4 입니다.

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/d72ae595-1d09-4713-8843-fa5d53b2f518)


* 4에서 시작하는 접미부 BA : BA를 경계로 갖는 다른 접미부 BABA 인데, 왼쪽방향으로 확장이 가능 . ABABA 는 BA를 왼쪽으로 한칸 확장한 ABA가 경계 . 따라서 BA 의 이동거리는 0
   
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/81c26ee2-b2f6-4b2f-9d22-219712b64b08)

* 3에서 시작하는 접미부 ABA : ABA를 경계로 갖는 최대의 접미부는 ABABA입니다.

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/068490e8-a93b-4fe2-ad4d-3d94b2acb183)

* 나머지 접미부들(BABA, AEABA, AABABA)은 길이가 원래 문자열 길이의 1/2보다 크기 때문
에 경계가 될 수 없습니다. 이들의 이동 거리는 모두 0 이다.

##### 접미부의 가장넓은 경계의 시작위치
    

![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/7ffd173e-e515-45d1-bc32-2177ebe3ecef)


![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/e8d9f2e9-7312-45c0-b316-016270546b7e)


![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/546bcdf3-7adc-4947-9ebe-183f2d05cd99)


![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/e1c7168f-e8bd-41c7-91c6-7d6e1023259a)






* (1) 첫 번째 경우




* (2) 두 번째 경우
    * 두 번째 경우는 가장 넓은 경계의 시작 위치가 곧 이동 거리가 됩니다. 첫 번째 경우에 대한 이동 거리는 이미 처리를 했으므로 이제 우리는 테이블 내에서 이동 거리가 0인 항목에 대해서만 패턴을 왼쪽부터 읽으면서 처리를 하면 됩니다. 

가장 넓은 경계의 시작 위치를 이동 거리로 입력할 때의 규칙은 다음과 같습니다.         

(1) 첫 “접미부의 가장 넓은 경계의 시작 위치”를 이동 거리로 입력한다(위 예제에서는 5).      

(2) 경계의 너비보다 접미부가 짧아지기 전까지 나타나는 모든 경계는 동일한 이동 거리를 입력한다.      

(3) 경계의 너비보다 접미부가 짧아지면 “접미부의 시작 위치 - 1”에 있는 “접미부의 가장 넓은 경계의 시작 위치”를 이동 거리로 입력한다.      

자, 패턴을 왼쪽부터 읽으면서 각 경계의 이동 거리를 입력해보겠습니다.    
먼저 첫 접미부의 가장 넓은 경계의 시작 위치가 5이므로 A(테이블[0])의 이동 거리를 5로 입력합니다.   
이 이동 거리는 경계의 길이가 5가 되기 전까지 계속 사용합니다.   
AA(테이블[1]), AAB(테이블[2]), AABAB(테이블[4]) 모두 5를 입력합니다.   
      
![image](https://github.com/silverlnng/DatastructureStudy/assets/112385982/3e63ae23-4cf0-45aa-8c2a-6be46ba19642)

테이블[6] 이후에는 이동 거리가 6으로 변경됩니다. 접미부의 가장 넓은 경계의 시작 위치[5]가 6이기 때문입니다.   
하지만 이 예제 테이블에는 이동 거리가 0으로 남아있는 곳이 없으므로 이것으로 처리가 완료되었습니다.   


## 보이어 무어 구현부분


* BoyerMoore.cpp

```cpp
int  BoyerMoore(char* Text, int TextSize, int Start, 
                char* Pattern, int PatternSize )
{
    int BCT[128];
    int* Suffix = (int*)calloc( PatternSize + 1, sizeof( int ) );
    int* GST  = (int*)calloc( PatternSize + 1, sizeof( int ) );
    int i = Start;
    int j = 0;

    int Position = -1;

    BuildBCT( Pattern, PatternSize, BCT );
    BuildGST( Pattern, PatternSize, Suffix, GST );
    
    while (i <= TextSize - PatternSize)
    {
        j = PatternSize - 1;

        while ( j >= 0 && Pattern[j] == Text[i+j] ) 
            j--;

        if (j<0)
        {
            Position = i;
            break;
        }
        else 
        {
            i+= Max( GST[j+1], j-BCT[ Text[i+j] ])  ;
        }
    }

    free ( Suffix );
    free ( GST  );

    return Position;
}

```

```cpp
void BuildBCT( char* Pattern, int PatternSize, int* BCT )
{
    int i;
    int j;

    for ( i=0; i<128; i++ ) 
        BCT[i]=-1;

    for ( j=0; j<PatternSize; j++ )
        BCT[ Pattern[j] ]=j;
}
```

```cpp
void BuildGST( char* Pattern, int PatternSize, int* Suffix, int* GST )
{
    /*  Case 1 */
    int i = PatternSize;
    int j = PatternSize + 1;

    Suffix[i]=j; 
    
    while (i>0)
    {
        while (j<=PatternSize && Pattern[i-1] != Pattern[j-1])
        {
            if ( GST[j] == 0 ) 
                GST[j]=j-i;

            j=Suffix[j];
        }

        i--; 
        j--;
        
        Suffix[i] = j;
    }

    /*  Case 2 */
    j = Suffix[0];

    for ( i = 0; i <= PatternSize; i++ )
    {
        if ( GST[i] == 0 ) 
            GST[i] = j;

        if ( i == j ) 
            j = Suffix[j];
    }
}
```