#  퇴각검색

> 퇴각검색, 백트래킹은 비어있는 해로를 탐색을 시작하여 단계마다 해를 확장해 나가는 방식의 알고리즘이다.





### 문제, 크기가 NxN 체스판에 n개의 퀸을 서로 공격할 수 없도록 배치하는 방법의 수?

~~~c++
#include <iostream>
#include <vector>
#include <algorithm>

#define FASTSTD ios::sync_with_stdio(0)
#define FASTCIN cin.tie(0)
using namespace std;

int n;
int cnt = 0;
vector<int> col(50); // 열추적
vector<int> diag1(50); // 대각선
vector<int> diag2(50); // 대각선추적
void search(int y) {
    if(y == n) {
        cnt++;
        return;
    }
    for(int x = 0; x < n; x++) {
        if(col[x] || diag1[x + y] || diag2[x - y + n - 1]) continue;
        col[x] = diag1[x + y] = diag2[x - y + n - 1] = 1;
        search(y + 1);
        col[x] = diag1[x + y] = diag2[x - y + n - 1] = 0;
    }
}
int main() {
    FASTSTD;
    FASTCIN;
    cin >> n;
    search(0);
    cout << cnt;
    return 0;
}
~~~

- 조건
  - 퀸은 같은 열에 있으면 안된다.
  - 퀸은 같은 대각선에 있으면 안된다.

