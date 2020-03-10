# 이진 탐색

> 이진 탐색 알고리즘은 간단하면서, 꽤나 많은 문제를 해결할 수 있는 알고리즘이다.
>
> `정렬된 배열`에 특정 원소가 존재하는지 여부를 파악하는 등의 문제를 `O(logN)` 시간에 해결한다.



## 방법 1. 시작점과 끝점을 조정하기

- 탐색은 배열의 특정 부분 배열을 살펴보며 진행되며, 처음에는 배열 전체를 놓고 시작한다.
- 단계별로 알고리즘을 진행해 나가면서 탐색 범위를 절반으로 줄여나간다.
- 단계마다 현재 살펴보고 있는 부분 배열 중앙의 원소를 검사한다.
- 중앙 원소가 목푯값과 같다면 탐색을 끝낸다.

~~~c++
int a = 0, b = n - 1;
while(a <= b) {
  int middle = (a + b) / 2;
  if(array[middle] == x) {
    // 위치 K에서 x를 찾았다.
  }
  if(array[middle] < x) a = middle + 1;
  else b = middle - 1;
}
~~~

> 단계마다 부분 배열의 크기를 절반씩 줄여나가기 때문에 시간 복잡도는 `O(logN)` 이다.



## 방법 2. 건너뛰기

> 배열을 왼쪽에서 오른쪽으로 건너뛰어 가며 살펴보는 것이다.

- 처음에는 n / 2개의 원소를 건너뛴다.
- 라운드마다 건너뛸 원소 수를 n / 4, n / 8, n / 16 과 같은 식으로 1이 될때 까지 절반씩 줄인다.
- 건너뛴 후의 원소가 목푯 값을 벗어난다면 건너뛰지 않고 멈춘다.
- 진행하고나면 찾고자 하는 원소에 도달하게 되고 없다면, 그 배열에 원소가 없다는 뜻이다.

```c++
int k = 0;
for(int b = n / 2; b >= 1; b /= 2) {
	while(k + b < n && array[k + b] <= x) k += b;
}
if(array[k] == x) {
  // 위치 k에서 x를 찾음
}
```



## 문제풀어보기

> 이진탐색에 대한 문제를 한번 풀어보자. 선별한 문제는 백준온라인저지의 `2110번 공유기` 이다.

- 나는 코드를 읽기 편한 방법 1을 선호한다.

~~~c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

vector<int> arr;
int N, C;
int main() {
    cin >> N >> C;
    for(int i = 0; i < N; i++) {
        int num;
        cin >> num;
        arr.push_back(num);
    }
    sort(arr.begin(), arr.end());

    int left = 1; // 가능한 최소 거리
    int right = arr[N - 1] - arr[0]; // 가능한 최대 거리
    int ans = 0, d = 0;
    while(left <= right) {
        int mid = (left + right) / 2;
        int start = arr[0];
        int cnt = 1;

        // 간격을 기준으로 공유기를 설치한다.
        for(int i = 1; i < N; i++) {
            d = arr[i] - start;
            if(mid <= d) {
                cnt++;
                start = arr[i];
            }
        }
        if(cnt >= C) { // 공유기의 수를 줄인다.
            ans = mid;
            left = mid + 1;
        } else { // 공유기가 더 설치되어야한다.
            right = mid - 1;
        }
    }
    cout << ans;
}
~~~

> 문제에서의 범위가 20만개가 넘어가서 전체를 전부 탐색할 경우 시간 초과를 맞게 된다. 
>
> 이진탐색으로 해결한다.

