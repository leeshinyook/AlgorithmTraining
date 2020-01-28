# 부분집합 생성

> 재귀적방법을 이용하여 부분집합을 만들어보자.

```c++
#include <iostream>
#include <vector>

#define FASTSTD ios::sync_with_stdio(0)
#define FASTCIN cin.tie(0)
using namespace std;

vector<int> subset;
int n;
void search(int k);
int main() {
    FASTSTD;
    FASTCIN;
    cin >> n; // n은 원소의 개수
    search(1);
    cout << subset.size();
    return 0;
}
void search(int k) {
    if(k == n + 1) { // 출력부
        for(int i = 0; i < subset.size(); i++) {
            cout << subset[i] << " ";
        }
        cout << '\n';
    } else {
        // k를 부분집합에 포함시킨다.
        subset.push_back(k);
        search(k + 1);
        subset.pop_back();
        // k를 부분집합에 포함시키지 않는다.
        search(k + 1);
    }
}
```

> 재귀적이다. 라는 말은 분기를 생성한다라고 생각하자.
>
> 지금 보면, search함수 안에 두번의 재귀적 함수가 실행된다. 이는 `재귀 호출 트리` 를 만든다.
>
> 예를 들어 n=2라면, 4개 n=3이라면 8개가 생성.

  

