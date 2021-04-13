# 순위 검색

> 프로그래머스
>
> 2021 KAKAO BLIND RECRUITMENT

* `C++`

* [문제링크](https://programmers.co.kr/learn/courses/30/lessons/72412)

---



## Key Point

* `lower_bound`

  * 벡터에서 기준 숫자보다 큰 지점으로 넘어가기 바로 전의 작은 지점을 도출해줌
  * 기준 숫자보다 크거나 같은 숫자 갯수 구할 때 사용

  ```c++
  #include <algorithm>
  
  vector<int> v  = {10, 20, 30, 40, 50, 60, 70};
  int num = 30;
  vector<int>::iterator low = lower_bound(v.begin(), b.end(), 30); 
  cout << v.end()-low;
  
  // v.end()와 low를 단독으로 출력하는 건 불가. 연산 후 int형이 되는 것임.
  ```

* `stringstream`

  * 특정 문자 기준 문자열 나누기
  * 방법1 (공백 기준일 때)

  ```c++
  #include <sstream>
  string str = "안녕 혜림아 잘지내지 호호 123"
  stringstream ss(str);
  string token[4];
  int score;
  ss >> token[0] >> token[1] >> token[2] >> token[3] >> score;
  ```

  * 방법2 (공백 기준일 때)

  ```c++
  #include <sstream>
  string str = "java and backend and junior and pizza 150"
  stringstream ss(str);
  string query[4];
  string tmp; // 필요없는 부분은 이렇게 처리할 수 있다.
  int score;
  ss >> query[0] >> tmp >> query[1] >> tmp >> query[2] >> tmp >> query[3] >> score;
  ```

  * 방법3 (특정 문자 기준일 때)

  ```c++
  #include <sstream>
  string str = "java-backend-junior-pizza-150"
  stringstream ss(str);
  string token;
  while(getline(ss, token, '-'))
  {
  	// 원하는 코딩
  }
  ```

* 백터의 n차원

  ```c++
  #include <vector>
  vector<int> v[4][3][3][3];
  v[3][3][3][3].push_back(1);
  v[3][3][3][3].push_back(2);
  v[3][3][3][3].size(); // 2
  ```

  

---



## Code

```c++
#include <string>
#include <vector>
#include <map>
#include <sstream>
#include <iostream>
#include <algorithm>
using namespace std;

map<string, int> mp;
vector<int> list[4][3][3][3]; // 경우의 수
vector<int> solution(vector<string> info, vector<string> query) {
    vector<int> answer;
    mp["-"] = 0;
    mp["cpp"] = 1;
    mp["java"] = 2;
    mp["python"] = 3;
    mp["backend"] = 1;
    mp["frontend"] = 2;
    mp["junior"] = 1;
    mp["senior"] = 2;
    mp["chicken"] = 1;
    mp["pizza"] = 2;
    
    string language, job, career, food;
    for(int i=0; i<info.size(); i++) // 한 정보 당 16가지의 경우의 수가 있음
    {
        stringstream ss(info[i]);
        string person[4];
        int score;
        ss >> person[0] >> person[1] >> person[2] >> person[3] >> score;
       
        for(int a=0; a<2; a++)
        {
            if(a==0)
                language = "-";
            else
                language = person[0];
            for(int b=0; b<2; b++)
            {
                 if(b==0)
                    job = "-";
                else
                    job = person[1];
                for(int c=0; c<2; c++)
                {
                    if(c==0)
                        career = "-";
                    else
                        career = person[2];
                    for(int d=0; d<2; d++)
                    {
                        if(d==0)
                            food = "-";
                        else
                            food = person[3];
                        
                        list[mp[language]][mp[job]][mp[career]][mp[food]].push_back(score);
                    }
                }
            }
        }
    }
    for(int a=0; a<4; a++)
        for(int b=0; b<3; b++)
            for(int c=0; c<3; c++)
                for(int d=0; d<3; d++)
                    sort(list[a][b][c][d].begin(), list[a][b][c][d].end());

    for(int i=0; i<query.size(); i++)
    {
        stringstream ss(query[i]);
        string token;
        string q[4], tmp;
        int score;
        ss >> q[0] >> tmp >> q[1] >> tmp >> q[2] >> tmp >> q[3] >> score;
        
        vector<int> List = list[mp[q[0]]][mp[q[1]]][mp[q[2]]][mp[q[3]]];
        vector<int>::iterator low = lower_bound(List.begin(), List.end(), score);
        answer.push_back(List.end()-low);

    }
    return answer;
}
```



---



## Solution

* `map`을 사용하여 경우의 수를 `list` 벡터 변수에 다 넣는다.
* 이 문제는 효율성 있는 문제로, lower_bound 사용해야 효율성 통과 가능.