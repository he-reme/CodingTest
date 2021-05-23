# Bound

> Lower Bound / Upper Bound

* 이분 탐색에서 파생

* 이분 탐색
  * 원하는 **값 k를 찾는** 과정

* Lower Bound 
  * 원하는 **값 k 이상이 처음 나오는 위치**를 찾는 과정

* Upper Bound
  * 원하는 **값 k를 초과한 값이 처음 나오는 위치**를 찾는 과정

* 주의사항
  * 값 정렬 먼저 필수 !!!!



---



## 사용

```
vector<int> v  = {10, 20, 30, 40, 50, 60, 70};
```

#### 1. 헤더파일

```
<algorithm>
```

#### 2. lower_bound

```c++
lower_bound(v.begin(), v.end(), k, [정렬함수]) - v.begin();
```

* 값 `k` 이상이 처음 나오는 위치 

* `lower_bound()` 반환값은 주소값이므로, 배열의 처음 값의 주소를 빼줘야 몇번째 인덱스인지 알 수 있음
* `[정렬함수]`
  * 사용자 지정 lower_bound 값 찾고싶을 때 추가
  * return 값이 `bool` 형식
  * 정렬함수 사용시 주의사항
    * `lower_bound()` 하기 전 해당 정렬함수를 가지고 `v`를 먼저 `sort()` 해줘야 함
    * `sort(v.begin, v.end(), [정렬함수])` 

#### 2. upper_bound

```c++
upper_bound(v.begin(), v.end(), k, [정렬함수]) - v.begin();
```

* 값 `k` 초과가 처음 나오는 위치 

* `upper_bound()` 반환값은 주소값이므로, 배열의 처음 값의 주소를 빼줘야 몇번째 인덱스인지 알 수 있음
* `[정렬함수]`
  * 사용자 지정 upper_bound 값 찾고싶을 때 추가
  * return 값이 `bool` 형식
  * 정렬함수 사용시 주의사항
    * `upper_bound()` 하기 전 해당 정렬함수를 가지고 `v`를 먼저 `sort()` 해줘야 함
    * `sort(v.begin, v.end(), [정렬함수])` 



---



## 응용

> int형은 물론, 문자열에서도 유용하게 사용 가능

#### 1. 배열 안에서 k값이상에 해당하는 값 갯수 세기

```c++
#include <algorithm>

vector<int> v  = {10, 20, 30, 40, 50, 60, 70}; // 정렬된 배열
int num = 30;
vector<int>::iterator low = lower_bound(v.begin(), b.end(), 30); // 30 이상이 시작되는 index
cout << v.end()-low;

// v.end()와 low를 단독으로 출력하는 건 불가. 둘 다 반환값이 주소값이기 때문에.
// 연산 후 int형이 되는 것임.
```

#### 2. 쿼리 문자열에 해당하는 문자열 검색

> 프로그래머스 2020 KAKAO BLIND RECRUITMENT - 가사 검색
>
> https://programmers.co.kr/learn/courses/30/lessons/60060

```c++
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
 
 
//길이로 먼저 정렬하고 길이가 같다면 사전순으로 정렬
bool comp(string a, string b) {
    if (a.length() < b.length())
        return true;
    else if (a.length() == b.length())
        if (a < b) return true;
    return false;
}
 
 
vector<int> solution(vector<string> words, vector<string> queries) {
    vector<int> answer;
 
    //rwords 벡터에 단어를 뒤집어서 저장
    vector<string> rwords = words;
    int size = rwords.size();
    for (int i = 0; i < size; i++) reverse(rwords[i].begin(), rwords[i].end());
 
 
    //comp 기준 정렬
    sort(words.begin(), words.end(), comp);
    sort(rwords.begin(), rwords.end(), comp);
 
 
    int len, lower, upper;
    int idx;
    for (string query : queries) {
        len = query.length();
        
        if (query[0] == '?') {
            //?가 접두사에 있는 경우
            reverse(query.begin(), query.end()); //키워드도 뒤집어준다.
            idx = query.find_first_of('?');
 
            for (int i = idx; i < len; i++) query[i] = 'a';
            lower = lower_bound(rwords.begin(), rwords.end(), query, comp) - rwords.begin();
 
            for (int i = idx; i < len; i++) query[i] = 'z';
            upper = upper_bound(rwords.begin(), rwords.end(), query, comp) - rwords.begin();
        }
        else {
            //?가 접미사에 있는 경우
            idx = query.find_first_of('?');
 
            for (int i = idx; i < len; i++) query[i] = 'a';
            lower = lower_bound(words.begin(), words.end(), query, comp) - words.begin();
 
            for (int i = idx; i < len; i++) query[i] = 'z';
            upper = upper_bound(words.begin(), words.end(), query, comp) - words.begin();            
        }
 
        answer.push_back(upper - lower);
    }
 
    return answer;
}
```

