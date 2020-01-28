# 순열

> 원소가 n개인 집합의 모든 순열을 생성하는 알고리즘

~~~c++
#include <iostream>
#include <vector>
#include <algorithm>

#define FASTSTD ios::sync_with_stdio(0)
#define FASTCIN cin.tie(0)
using namespace std;

int n;
vector<int> permutation;
bool chosen[1000000];
void search() {
    if(permutation.size() == n) {
        for(int i = 0; i < n; i++) {
            cout << permutation[i] << " ";
        }
        cout << "\n";
    } else {
        for(int i = 1; i <= n; i++) {
            if(chosen[i]) continue;
            chosen[i] = true;
            permutation.push_back(i);
            search();
            chosen[i] = false;
            permutation.pop_back();
        }
    }
}
int main() {
    FASTSTD;
    FASTCIN;
    cin >> n;
    search();
    return 0;
}
~~~

> 함수가 호출될 때 마다, 새로운 원소를 순열에 추가하고, 그 원소가 선택되었음을 chosen배열에 표시한다.



### 추가적인 내용 next_permutation, prev_permutation

```c++
#include <iostream>
#include <vector>
#include <algorithm>

#define FASTSTD ios::sync_with_stdio(0)
#define FASTCIN cin.tie(0)
using namespace std;

vector<int> permutation;
int n, num;
int cnt = 0;
bool check = false;
int main() {
    FASTSTD;
    FASTCIN;
    cin >> n;

    for(int i = 1; i <= n; i++) {
        cin >> num;
        permutation.push_back(num);
    }
    do {
        if(cnt == 1) {
            for(int i = 0; i < n; i++) {
                cout << permutation[i] << " ";
            }
            check = true;
        }
        cnt++;
        if(check) break;
    } while(next_permutation(permutation.begin(), permutation.end()));
    if(!check) cout << -1;
    return 0;
}
```

- `next_permutation()` 이 함수에 순열을 하나 주고 호출하면 , 그 순열부터 다음에 오는 순열을 생성한다.
  - 이를 활용하여, 순열을 생성할 수 있다.
- `prev_permutation()` 는 반대.



### 추가적 잡담

> 백준에 순열알고리즘 파트가 있다. 위 내용을 활용하여 문제도 한 번 풀어보면 좋겠다.









