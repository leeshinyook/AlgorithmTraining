# 벨만-포드 알고리즘

> 시작 노드에서부터 다른 모든 노드로 가는 최단 경로를 구하는 알고리즘

- 길이가 음수인 사이클을 포함하지 않는 모든 종류의 그래프를 처리할 수 있다.
- 시작 노드에서 다른 모든 노드까지의 길이를 모두 추적한다.



### 어떻게?

1. 거리의 초기값은 시작 노드의 경우 0이고, 다른 모든 노드의 경우 무한대의 값으로 설정된다.

   > 무한대를 설정할 때 TIP

   ~~~c++
   #include <iostream>
   #define INF 0x3f3f3f3f // 무한대
   using namespace std;
   
   int main() {
     
   }
   ~~~

   > 문제를 풀때, 2배를 하여도 int의 범위를 벗어나지 않는 0x3f3f3f3f를 많이 사용한다.
   >
   > 2배를 할 필요가 없다면, 0x7f7f7f7f 를 사용하기도 한다.

2. 그리고 이 값을 계속해서 줄여나가는 과정을 더는 줄일 수 있는 값이 없을 때 까지, 반복한다.

라운드마다 모든 간선을 살펴보며, 그 간선이 거리를 줄이는데 사용될 수 있는지 확인한다.

~~~c++
#include <iostream>
#define INF 0x7f7f7f7f
using namespace std;

// 벨만-포드 알고리즘
// 다른 모든 노드로 가능 최단 경로를 구하는 알고리즘.

// 입력받은 출발점에서 도착점까지의 최단경로를 출력하십시요.
int adj[100][100];
int dist[100];
int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    int V, E , S , T; // V : 노드의 개수, E : 엣지의 개수, S: 시작점, T: 끝점
    int a, b, w; // (a, b) = w (가중치);

    cin >> V >> E >> S >> T;

    for(int i = 1; i <= V; i++) {
        for(int j = 1; j <= V; j++) {
            adj[i][j] = INF; // 모든 노드를 무한대로 설정
        }
    }
    for(int i = 1; i <= V; i++) {
        dist[i] = INF; // 거리도 무한대로 설정.
    }

    for(int j = 0; j < E; j++) {
        cin >> a >> b >> w;
        adj[a][b] = adj[b][a] = w; //
    }

    adj[S][S] = 0; // 시작점을 기준점으로 하기 위해 0을 할당한다.
    dist[S] = 0; // dist배열은 가중치가 저장된다.

    for(int i = 1; i <= V; i++) {
        for(int j = 1; j <= V; j++) {
            if(i == j || adj[i][j] == INF) continue; // 예외처리
            dist[j] = min(dist[j], dist[i] + adj[i][j]);
        }
    }
    for(int i = 1; i <= T; i++) {
        cout << dist[i] << " ";
    }
    cout << dist[T];
}
~~~



- 이를 이용해 문제를 하나 풀어보자.

> 선별한 문제는 백준알고리즘의 [타임머신](https://www.acmicpc.net/problem/11657) 11657번이다.

~~~c++
#include <iostream>
#include <vector>
#define INF 987654321
using namespace std;

// 벨만-포드 알고리즘
// 다른 모든 노드로 가능 최단 경로를 구하는 알고리즘.

// 입력받은 출발점에서 도착점까지의 최단경로를 출력하십시요.
vector<pair<int, int>> adj[502];
int dist[502];
bool cycle = false;
int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    int V, E , S , T; // V : 노드의 개수, E : 엣지의 개수, S: 시작점, T: 끝점
    int a, b, w; // (a, b) = w (가중치);

    cin >> V >> E;
    S = 1;
    T = V;

    for(int i = 0; i <= V + 1; i++) {
        dist[i] = INF; // 거리도 무한대로 설정.
    }

    for(int j = 0; j < E; j++) {
        cin >> a >> b >> w;
        adj[a].push_back({b, w});
    }

    dist[S] = 0; // dist배열은 가중치가 저장된다.

    for(int i = 1; i <= V; i++) {
        for(int j = 1; j <= V; j++) {
            for(auto &n : adj[j]) {
                if(dist[j] != INF && dist[n.first] > n.second + dist[j]) {
                    dist[n.first] = n.second + dist[j];
                    if(i == V) cycle = true;
                }
            }
        }
    }

    if(cycle) {
        cout << "-1";
        return 0;
    }

    for (int i = 2; i <= T; i++) {
        if (dist[i] == INF) {
            cout << "-1" << '\n';
        } else {
            cout << dist[i] << '\n';
        }
    }

}
~~~

> 이차원배열로 문제를 해결하기는 어렵기때문에(두 노드 사이에 여러개의 경로가 존재하는경우), 벡터를 이용하여 해결한다.
>
> 이 문제는 음수사이클을 찾아내는게 포인트였다.
>
> 최단거리를 찾는 알고리즘은 다익스트라, 벨만포드 등이 있지만, 이 음수사이클을 유무를 확인하는 것은 벨만포드
>
> 알고리즘을 이용하여야한다. for문을 한 번 더 돌렸는데,  음수 사이클이 없다면, n - 1라운드를 돌면 그 dist의 값이 이후로 
>
> 변하지 않을 것이다. 한번 더 돌려보는것 총, n라운드를 진행하여, 그 값이 변한다면 음수사이클을 가진다는 뜻이다.

