---
title:  "우선순위 큐 다익스트라" 

categories:
  - OS
tags:
  - [자료구조, 알고리즘, 다익스트라, Dijkstra]

toc: true
toc_sticky: true

date: 2024-01-08
last_modified_at: 2024-01-08
---


# 우선순위 큐 다익스트라

### 일반적인 다익스트라 알고리즘에 대해 간단히.

기본적인 다익스트라 알고리즘은 가장 짧은 경로를 찾는다. 이미 다 알고있을테니 자세한 설명은 생략하고, 특정한 노트(Start)에서 다른 노드들(Ends)로 가는 각각의 최단 경로로 가는 최단 거리를 찾아주는 알고리즘이다. 당연히 음의 시간이 소요되는(시간을 거스르는) 경우가 없을 때 정상 작동한다.

우선 순위 큐를 사용하지 않는 다익스트라 코드를 살펴보자.

```cpp
for(int idx = 0; idx < N - 1; idx++){
	min_node = get_smallest_node();
	visited[min_node] = true;

	for(int jdx = 0; jdx < N; jdx++){
		if(!visited[jdx]){
			if(dist[min_node] + weight[min_node][jdx] < dist[jdx])
				dist[jdx] = dist[min_node] + weight[min_node][jdx];
		}
	}
}
```

가벼운 골자만 작성하였을 때, 코드를 보면 결국 걸리는 시간이 더 작을 경우에 그걸로 갱신시켜주는 알고리즘이다. 시간복잡도는 살펴본다면 for문이 2개이니 O(Vertex^2)일거다.

당연히 백준같이 대부분 1초의 시간제한이 있는 거라면, 10000의 Vertex가 있다면 대부분 시간 초과가 뜬다. 당연히 더 시간 복잡도가 좋은 방법을 찾아야 할 테고. 

그래서 우선순위 큐를 사용한다. 

### 우선 순위 큐(Priority Queue)를 사용한 다익스트라.

거두절미하고 코드를 살펴보자.

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>
using namespace std;

int V,E;
int S;

struct info{
    int e,v;
};
vector<info>v1[20001];
int value[20001];

priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin>>V>>E>>S;

    for(int x = 0; x < E; x++){
        int s,e,v;
        cin>>s>>e>>v;
        v1[s].push_back({e,v});
    }

    for(int x = 1; x<=V; x++){
        value[x] = 987654321;
    }

    value[S] = 0;
    pq.push({0,S});

    while(!pq.empty()){
        int v = pq.top().first;
        int e = pq.top().second;

        pq.pop();

        int sz = v1[e].size();
        for(int idx = 0; idx < sz; idx++){
            int E = v1[e][idx].e;
            int V = v1[e][idx].v;

            if(v + V < value[E]){
                value[E] = v + V;
                pq.push({v+ V, E});
            }
        }
    }

    for(int idx = 1; idx <= V; idx++){
        if(value[idx] == 987654321) cout<<"INF\n";
        else cout<<value[idx]<<"\n";
    }
    return 0;
}
```

우선 순위 큐에서 자동으로 정렬을 해주기에 가장 값이 적은 Vertex를 따로 구할 필요가 없다. 즉 자동으로 우선순위 큐에서 탐색해준다.

시간 복잡도를 보자. 배열로 V^2로 되어있는 건 연결되어 있건, 연결되어 있지 않건 모두 탐색해서 Vertex^2의 시간이 걸린다. 하지만 우선 순위 큐를 사용한 알고리즘은 v1벡터에 ‘연결된’ 간선들만 넣어주고, 그것만 빼내서 값을 계산하기에 O(V logE)라고 생각하면 되겠다.