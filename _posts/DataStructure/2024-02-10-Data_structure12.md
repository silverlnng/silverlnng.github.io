---
layout: single
title:  "자료구조Study12_그래프"
categories: DataStructure
tag: [DataStructure]
toc: true
toc_sticky: true
sidebar:
    nav: "counts"
---

# 그래프

## 인접 행렬

## 인접 리스트

* 인접리스트데는 정점,간선, 그래프를 위한 구조체 가 필요하다.

```cpp
typedef struct tagVertex
{
    ElementType       Data;
    int               Visited;
    int               Index;

    struct tagVertex* Next;
    struct tagEdge*   AdjacencyList; //인접 정점의 목록에 대한 포인터
} Vertex;
```
* 간선을 위한 구조체

```cpp
typedef struct tagEdge
{
    int    Weight;
    struct tagEdge* Next; //Vertex의 AdjacencyList
    Vertex* From;   //간선의 시작
    Vertex* Target; //간선의 끝
} Edge;
```


```cpp
typedef struct tagGraph
{
    Vertex* Vertices;
    int     VertexCount;
} Graph;
```
* 

```cpp
Graph* CreateGraph()
{
    Graph* graph = (Graph*)malloc( sizeof( Graph ) );
    graph->Vertices = NULL;
    graph->VertexCount = 0;

    return graph;
}

void DestroyGraph( Graph* G )
{
    while ( G->Vertices != NULL )
    {
        Vertex* Vertices = G->Vertices->Next;        
        DestroyVertex( G->Vertices );
        G->Vertices = Vertices;        
    }

    free( G );
}

Vertex* CreateVertex( ElementType Data )
{
    Vertex* V = (Vertex*)malloc( sizeof( Vertex ) );
    
    V->Data = Data;
    V->Next = NULL;
    V->AdjacencyList = NULL;
    V->Visited = NotVisited;
    V->Index = -1;

    return V;
}

void DestroyVertex( Vertex* V )
{
    while ( V->AdjacencyList != NULL )
    {
        Edge* Edge = V->AdjacencyList->Next;

        DestroyEdge ( V->AdjacencyList );

        V->AdjacencyList = Edge;
    }

    free( V );    
}

Edge*   CreateEdge( Vertex* From, Vertex* Target, int Weight )
{
    Edge* E   = (Edge*)malloc( sizeof( Edge ) );
    E->From   = From;
    E->Target = Target;
    E->Next   = NULL;
    E->Weight = Weight;

    return E;
}

void    DestroyEdge( Edge* E )
{
    free( E );
}

void AddVertex( Graph* G, Vertex* V )
{
    Vertex* VertexList = G->Vertices;

    if ( VertexList == NULL ) 
    {
        G->Vertices = V;
    } 
    else 
    {
        while ( VertexList->Next != NULL )
            VertexList = VertexList->Next;

        VertexList->Next = V;
    }

    V->Index = G->VertexCount++;
}

void AddEdge( Vertex* V, Edge* E )
{
    if ( V->AdjacencyList == NULL ) 
    {
        V->AdjacencyList = E;
    } 
    else
    {
        Edge* AdjacencyList = V->AdjacencyList;

        while ( AdjacencyList->Next != NULL )
            AdjacencyList = AdjacencyList->Next;

        AdjacencyList->Next = E;
    }
}

void PrintGraph ( Graph* G )
{
    Vertex* V = NULL;
    Edge*   E = NULL;

    if ( ( V = G->Vertices ) == NULL )
        return;

    while ( V != NULL )
    {
        printf( "%c : ", V->Data );        
        
        if ( (E = V->AdjacencyList) == NULL ) {
            V = V->Next;
            printf("\n");
            continue;
        }

        while ( E != NULL )
        {
            printf("%c[%d] ", E->Target->Data, E->Weight );
            E = E->Next;
        }

        printf("\n");

        V = V->Next;    
    }   

    printf("\n");
}
```


### 인접행렬 vs 인접리스트
인접행렬을 이용하면 정점간의 인접여부를 빠르게 알 수 있는만, 인접관계를 행렬 형태로 저장하기 위해 사용하는 메모리의 양이 정점의 크기 제곱이다.
인접리스트는 정점 간의 인접여부를 알아내기 위해 인접리스트를 타고 순차탐색을 해야하는 단점





# 그래프 탐색

## 깊이우선탐색

## 너비우선탐색

```cpp
void BFS( Vertex* V, LinkedQueue* Queue )
{
    Edge* E = NULL;

    printf("%d ", V->Data);
    V->Visited = Visited;
    
    /*  큐에 노드 삽입. */
    LQ_Enqueue( &Queue, LQ_CreateNode( V ) );

    while ( !LQ_IsEmpty( Queue ) )
    {
        Node* Popped = LQ_Dequeue( &Queue );
        V = Popped->Data;
        E = V->AdjacencyList;

        while ( E != NULL )
        {
            V = E->Target;

            if ( V != NULL && V->Visited == NotVisited )
            {
                printf("%d ", V->Data);
                V->Visited = Visited;
                LQ_Enqueue( &Queue, LQ_CreateNode( V ) );
            }

            E = E->Next;
        }
    }
}
```


###