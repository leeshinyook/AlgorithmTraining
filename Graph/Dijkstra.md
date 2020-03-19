# 다익스트라 알고리즘

> 최단경로를 구하는 알고리즘 중 하나이며, 음수인 간선이 없는 경우 효율이 굉장히 좋은 알고리즘이다.

- 다익스트라 알고리즘도 벨만-포드 알고리즘처럼 각 노드까지의 거리를 저장하고 탐색 과정에서 값을 줄여간다.
- 아직 처리하지 않은 노드 중 가장 작은 노드를 찾고, 그 노드에서 시작하는 모든 간선을 쭉 살펴본다.
- 이 과정에서 노드까지의 거리를 줄일 수 있다면 줄인다.

- 가중치가 음수인 간선이 없다는 사실을 활용해서 그래프에 있는 모든 간선을 `단 한 번`만 처리한다.
  - 즉, 매우 효율적인 알고리즘이다.
  - 각 노드를 처리한 뒤에는 해당 노드까지의 거리가 절대 변하지 않는다.



## 구현

> 아직 처리하지 않은 노드 중 거리가 최소인 노드를 효율적으로 찾을 수 있어야 한다. 
>
> 이 과정에서 사용할 수 있는 적절한 자료 구조로는 아직 처리하지 않은 노드를 거리를 기준으로 저장하는 
>
> `우선순위 큐` 가 있고, 이를 사용하면 다음으로 처리할 노드를 `Log`시간에 구할 수 있다.

- 그래프는 인접리스트 형태로 저장된다.
  - adj[a] 에는 (b, w) 형태 구성.
- 우선순위 큐를 정의한다.
  - Priority_queue<pair<int, int>> q
  - (-d, w) 형태로 값을 저장하는데 이는 노드 x까지의 거리가 d임을 의미한다.
  - 즉, 우선순위 큐에 저장되는 값은 각 노드까지의 거리에 음수를 취한 값이다.

~~~c++
for (int i = 1; i <= n; i++) {
  distance[i] = INF;
}
distance[x] = 0;
q.push({0, x});
while(!q.empty()) {
  int a = q.top().second;
  q.pop();
  if(processed[a]) continue; // processed는 노드를 처리하였는지 체크하는 배열이다.
  processed[a] = true;
  for (auto u : adj[a]) {
    int b = u.first, w = u.second;
    if (distance[a] + w < distance[b]) {
      distance[b] = distance[a] + w;
      q.push({-distance[b], b}); 
    }
  }
}
~~~



### 실습

> 백준온라인저지의 1753, 최단경로 문제를 참고하자.

~~~c++
#include <iostream>
#include <vector>
#include <climits>
#include <algorithm>
#include <queue>
#define INF INT_MAX

using namespace std;
bool processed[20001];
int dist[20001];
vector<pair<int, int>> adj[20001];
priority_queue<pair<int, int>> q;
int V, E, S;
int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cin >> V >> E;
    cin >> S;
    for(int i = 0; i <= 20000; i++) {
        dist[i] = INF;
    }
    for(int i = 0; i < E; i++) {
        int a, b, w;
        cin >> a >> b >> w;
        adj[a].push_back({b, w});
    }
    dist[S] = 0;
    q.push({0, S}); // dist, w

    while(!q.empty()) {
        int a = q.top().second;
        q.pop();
        if(processed[a]) continue;
        for(auto u : adj[a]) {
            int b = u.first, w = u.second;
            if(dist[a] + w < dist[b]) {
                dist[b] = dist[a] + w;
                q.push({-dist[b], b});
            }
        }
    }

    for(int i = 1; i <= V; i++) {
        if(dist[i] == INF) {
            cout << "INF" << "\n";
        } else {
            cout << dist[i] << "\n";
        }
    }

}
~~~

문제 그대로 시작점에서의 각 노드까지의 최단경로를 저장해서 출력을 하면된다.

여기서 가중치 w에 대한 조건이 10이하의 `자연수` 이므로, 이를 보고 다익스트라알고리즘을 떠올리면 성공이다.

