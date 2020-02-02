# STL C++ 표준라이브러리

> 빠르게 문제해결할때, 로직을 전부 구현하는 것도 좋지만, 사고방식을 안다면 C++의 표준 라이브러리의 힘을 빌리자.



## 벡터

~~~c++
vector<int> v;
v.push_back(5);
v.pop_back();

vector<int> vp(6) // 크기 6, 초기값 0
vector<int> vq(6, 2) // 크기 6, 초기값 2
~~~

- 위 방식으로 사용이 가능하고, 평소에 알고리즘문제풀이를 위해 자주 사용한다.

> 순회

~~~c++
for(int i = 0; i < v.size(); i++) {
  cout << v[i] << endl;
} // 이런식의 순회를 자주사용한다.

for(auto x : v) {
  cout << x << endl; 
} // 이렇게 auto를 사용하여 더 짧게 순회도 가능하다.
~~~

> 두번째 방식을 따를땐, v가 기본 자료형(int 등)이 아닌경우, 시간이 오래걸릴 수 있다.
>
> v의 각 원소를 순회하면서 값을 x로 복사하기 때문이다.
>
> 그럴땐, for(const auto& x : v) 참조 복사를 이용하여 해결한다.

- Push_back(), pop_back()은 평균적으로 O(1)의 시간복잡도를 가진다.
- 보통 벡터와 배열의 실행속도는 같다.



### 반복자의 범위

> 반복자는 자료 구조의 원소를 가리키는 변수다.
>
> begin 반복자는 자료구조의 첫 번째 원소를 가리킨다.
>
> end 반복자는 마지막 원소 다음을 가리킨다



> Lower_bound, upper_bound 는 우선 정렬이 되어있어야 사용가능하다.
>
> 이때, `이진 탐색` 을 이용하여, 조건에 맞는 원소를 로그시간에 구한다.

- Lower_bound(v.begin(), v.end(), 5); 
  - 5보다 같거나 큰 원소를 반환
- Upper_bound(v.begin(), v.end(), 5);
  - 5보다 큰 원소를 반환



## 덱

> 덱(Deque)은 양쪽 끝 원소를 효율적으로 처리할 수 있는 동적 배열이다.

~~~c++
deque<int> d;
d.push_back(5); // [5];
d.push_back(2); // [5, 2];
d.push_front(3); // [3, 5, 2];
d.pop_back(); // [3, 5];
d.front_back(); // [5];
~~~

- 덱은 평균 O(1)의 연산시간이 소요된다.
- 배열의 양 끝 모두에서 원소를 추가하거나 삭제할 필요가 있을 때만 사용하는 것이 좋다.

- But, 덱은 벡터보다 실행 시간의 상수항이 크다.



## 집합 자료 구조

### 셋(Set)

> 원소의 집합을 관리하는 자료 구조이다.
>
> 셋의 기본 연산 `원소 추가`, `원소 탐색`, `원소 삭제`

- Set : 균형 잡힌 이진 탐색 트리를 기반으로 만들어짐
  - 연산 O(logN)
- Unordered_set : 해시 테이블을 기반으로 만들어짐
  - O(1), 최악의 경우 O(N)이지만, 경우가 드물다.

~~~c++
set<int> s;
s.insert(3);
s.insert(2);
s.insert(5);
cout << s.count(3); // 1
cout << s.count(4); // 0
s.erase(3);
s.insert(4);
cout << s.count(3); // 0
cout << s.count(4); // 1
~~~

> count함수가 반환하는 값은 무조권 0(원소가 없는경우) 1(원소가 있는 경우) 둘 중에 하나.
>
> insert함수는 있는 값은 더이상 원소를 추가하지 않는다.

~~~c++
cout << s.size();
for(auto x : s) {
  cout << x;
}
// 셋은 벡터와 비슷한 방식으로 사용할 수 있지만, []기호로 원소에 접근할 수 없다.
// 위와 같은 방식으로 모든 원소를 출력할 수 있다.

// find
auto it = s.find(x);
if(it == s.end()) {
  // x가 s에 없다.
}
~~~



#### 정렬된 집합

- set  : 정렬되어있다.
- Unordered_set : 정렬되어있지 않다.

##### 문제!

> 원소를 중복을 허용하지않고, 가장 큰 원소와 가장 작은 원소를 구하라.

~~~c++
auto first = s.begin();
auto last = s.end();
last--; // 영역 밖이므로 하나를 빼줘야한다.
cout << *first << " " << *last << '\n';
~~~

- Lower_bound(), upper_bound() 함수도 사용가능하다.





### 맵

> 맵은 키와 값을 쌍으로 저장하는 집합이다.

- map - 균형 잡힌 이진 탐색 트리를 기반으로 한다. O(logN)
- Unordered_map - 해시를 이용한다. O(1)

~~~c++
map<string, int> m;
m["abc"] = 4;
m["qwe"] = 3;
cout << m["qwe"]; // 3
cout << m["asdgasd"]; // 0 없는 경우 0으로 추가된다.

// 맵에 키가 존재하는지 확인
if(m.count("asdf")) {
  // key exists
}

// 맵의 키와 값을 출력
for(auto x : m) {
  cout << x.first << " " << x.second << '\n';
}
~~~



### 우선순위 큐

> 최대 힙, 최소 힙에 활용도가 높다.

> 탐색 및 삭제의 대상이 되는 원소는 최소, 또는 최대 원소이다.

- 원소의 추가, 삭제 - O(logN)
- 탐색 - O(1)

> 보통의 우선순위 큐는 힙을 기반으로 만들어진다.

~~~c++
// 보통은 내림차순정렬
priority_queue<int> q;
// 오름차순으로 변경
priority_queue<int, vector<int>, greater<int>> q;
~~~



































































