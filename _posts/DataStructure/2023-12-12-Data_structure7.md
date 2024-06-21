---
layout: single
title:  "자료구조Study7_병합정렬 ,퀵 정렬"
categories: DataStructure
tag: [DataStructure]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---


# 퀵 정렬

# 병합 정렬

<https://www.acmicpc.net/problem/2751>

```cpp

#include <iostream>
#include <vector>
using namespace std;

vector<int> A;
vector<int> tmp;

void Merge_Sort(int start, int end);

int main()
{
    // 정렬할 수의 갯수 입력받기
    int N;
    cin >> N;

    // 배열을 먼저 초기화 하기 

    A = vector<int> (N + 1, 0);
    tmp = vector<int> (N + 1, 0);

    // 배열에 입력받기
    for (int i = 1; i <= N; i++)
    {
        cin >> A[i];
    }
    Merge_Sort(1, N);
    //정렬한 배열을 출력하기 
    for (int i = 1; i <= N; i++)
    {
        cout << A[i] << "\n";
    }
    return 0;
}

void Merge_Sort(int start, int end)
{
    //작은 단위로 쪼갠다음 정렬한다 => 재귀함수 
    if (start == end)
    {
        return;
    }

    int middle = start + (end - start) / 2;

    Merge_Sort(start, middle);
    Merge_Sort(middle + 1, end);


    int k = start; // 맨앞에 정렬해주고 뒤쪽으로 계속 넣어줘야해서 변수하나 필요

    int index1 = start;
    int index2 = middle + 1;

    for (int i = start; i<= end; i++)
    {
        tmp[i] = A[i];
    }

    //작은수를 앞으로 보내기

    while (index1 <= middle && index2 <= end)
    {
        if (tmp[index1] > tmp[index2])
        {
            //작은걸 앞으로
            A[k] = tmp[index2];
            k++;
            index2++;
        }
        else
        {
            A[k] = tmp[index1];
            k++;
            index1++;
        }
    }

    // 한쪽그룹이 모두 선택된후 남아있는 그룹쪽...

    //index 1 이 middle 보다 작다 . 왼쪽그룹이 남아있다 ==>그걸계속 해야함 ==>while

    while (index1 <= middle)
    {
        A[k] = tmp[index1];
        k++;
        index1++;
    }
    while (index2 <= end)
    {
        A[k] = tmp[index2];
        k++;
        index2++;
    }
    //
}

```