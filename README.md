# cpp  ๐๐ป
- Hash
- Queue, Stack
- Sort
- Vector


----
# Hash

* ์ค๋ณต์ ํ์ฉํ์ง ์๋ ์๋ฃ ๊ตฌ์กฐ

| ์ข๋ฅ               | ๋ผ์ด๋ธ๋ฌ๋ฆฌ ๋ด๋ถ ๊ตฌํ |
| ------------------ | -------------------- |
| std::unordered_map | Hash table           |
| std::map           | binary search tree   |



## 1) unordered_map

* ํค๋

  ```c++
  #include<unordered_map>
  ```

* ์ ์ธ

  ```c++
  std::unordered_map<string, int>d;
  //<key์ type, value์ type> name;
  
  cout << d["leo"] << endl; 
  //์กด์ฌํ์ง ์๋ value๋ ๊ธฐ๋ณธ ๊ฐ -> 0ํน์ " "
  
  d["leo"]=3; //"leo" key์ ๋ํ value ์๋ฐ์ดํธ
  cout << d["leo"] << endl;
  ```

* ์ํ

  ```c++
  for(auto&i:d)
    cout << i.first << "-" << i.second << endl;
  
  //auto๋ฅผ ํตํด i์ type์ ์ปดํ์ผ๋ฌ๊ฐ ์ถ๋ก ํ => ์ฌ๊ธฐ์์๋ ์์์์ธ pair
  //i.first = key , i.second = value
  ```

* ์์ : ํ๋ก๊ทธ๋๋จธ์ค - ์์ฃผํ์ง ๋ชปํ ์ ์

  ```c++
  #include <string>
  #include <vector>
  #include <unordered_map>
  
  using namespace std;
  
  string solution(vector<string> participant, vector<string> completion) {
      string answer = "";
      std::unordered_map<string, int>d;
      for (auto&i : participant) d[i]++;
      for (auto&i : completion) d[i]--;
      for (auto&i : d){
          if (i.second!=0) answer = i.first;
      }
      return answer;
  }
  ```

* ์์ : ํ๋ก๊ทธ๋๋จธ์ค - ์์ฅ

  ```c++
  #include <vector>
  #include <unordered_map>
  #include <string>
  using namespace std;
   
  int solution1(vector<vector<string>>clothes){
      int answer = 1;
      unordered_map <string, int> m;
      for (auto clo : clothes){
          m[clo[1]] += 1;
      }
      for (auto n : m ){
          answer *= (n.second+1);
      }
      return answer-1;
  }
  ```

  

## 2) Map

* ํค๋

  ```c++
  #include <map>
  ```

* ์ ์ธ

  ```c++
  std::map<string, int> d;
  //<key์ type, value์ type> name
  
  d.insert({"Cam", 300}); //"Cam" key์ ๋ํ value 300 ์๋ฐ์ดํธ
  
  if (d.find("Cam") != d.end()){
    cout << "find" << endl;
  }
  else {
    cout << "not find" << endl;
  }
  //๋ฐ์ดํฐ๋ฅผ ๋๊น์ง ์ฐพ์ง ๋ชปํ์ ๊ฒฝ์ฐ, iterator๋ map.end()๋ฅผ ๋ฐํ
  
  map<string, int, greater>d;
  //๋ด๋ฆผ์ฐจ์์ ๊ฒฝ์ฐ 'greater'๋ฅผ ์ถ๊ฐ
  ```

* ์ํ

  * ์ธ๋ฑ์ค ๊ธฐ๋ฐ

    ```c++
    for (auto iter=d.begin(); iter!=d.end(); i++){
      cout << iter->first << " " << iter->second << endl;
    }
    cout << endl;
    ```

  * ๋ฒ์ ๊ธฐ๋ฐ

    ```c++
    d.erase(d.begin()+2); //ํน์  ์์น์ ์์ ์ญ์ d.erase("Cam"); //key ๊ฐ ๊ธฐ์ค์ผ๋ก ์์ ์ญ์ d.erase(d.begin(),d.end()); //erase ํจ์๋ก ๋ชจ๋  ์์ ์ญ์ d.clear(); //clear ํจ์๋ก ๋ชจ๋  ์์ ์ญ์ 
    ```

* ์์ : ํ๋ก๊ทธ๋๋จธ์ค ์์ฅ

  ```c++
  #include <string>#include <vector>#include <map>using namespace std;int solution(vector<vector<string>> clothes) {    int answer = 1;    map<string,int> m;    for(auto clo : clothes){        m[clo[1]]+=1;    }    for(auto iter=m.begin();iter!=m.end();iter++){        answer*=iter->second+1;    }    return answer-1;}
  ```

* ์์ : ํ๋ก๊ทธ๋๋จธ์ค ๋ฒ ์คํธ ์จ๋ฒ

  ```c++
  #include <map>#include <vector>#include <string>using namespace std;vector<int> solution(vector<string> genres, vector<int> plays){  vector <int> answer;  std::map<string, int> music;  std::map<string, map<int, int>> musicalist;  for(int i=0; i<genres.size(); i++){    music[genres[i]] += plays[i];    musicalist[genres[i]][i] = plays[i];  }    while (music.size()>0){    string genre{};    int max{0};    for(auto mu : music){      if(mu.second > max){        max = mu.second;        genre = mu.first;      }    }        for(int i=0; i<2; i++){      int val=0, ind=-1;      for(auto m1 : musicalist[genre]){        if(val<m1.second){          val = m1.second;          ind = m1.first;        }      }            if (ind==-1)break;      answer.push_back(ind);      musicalist[genre].erase(ind);    }    music.erase(genre);  }  return answer;}
  ```

  * vector์์ pushํ๊ธฐ

    ```c++
    vector<int> answer;answer.push_back(ind);
    ```

  * vector์์ eraseํ๊ธฐ

    ```c++
    musicalist[genre].erase(ind);music.erase(genre);//erase(์ญ์ ํ  ์์์ ์์น)
    ```

  * map ์ค์ฒฉํด์ ์ฌ์ฉํ๊ธฐ

    ```c++
    map<string,map<int,int>> musicalist;
    ```

  * ์ต๋ ๊ฐ ๊ตฌํ๊ธฐ

    ```c++
    for(auto mu :music){  int max{0};  string genre{};  if (mu.second > max){    max = mu.second;    genre = mu.first;  }}
    ```
    
 ---
 # 1) Queue

* ์ ์์ ์ถ (First In First Out)
  * front ์์์ back ์์์ ์ ๊ทผ ๊ฐ๋ฅ

* ํค๋

  ```c++
  #include <queue>
  ```

* ์ ์ธ

  ```c++
  queue<int>q;
  //<queue ์์์ type> name;
  ```

* ํจ์

  | ๊ธฐ๋ณธ ํจ์     | ์ค๋ช                               | ์ฌ์ฉ                         |
  | ------------- | ---------------------------------- | ---------------------------- |
  | push(element) | ํ์ ์์๋ฅผ ์ถ๊ฐ(๋งจ ๋ค)            | q.push(1)                    |
  | pop()         | ํ์ ์๋ ์์ ์ญ์ (๋งจ ์)         | q.pop()                      |
  | front()       | ํ ๋งจ ์ ์์ ๋ฐํ                 | int a = q.front()            |
  | back()        | ํ ๋งจ ๋ค ์์ ๋ฐํ                 | int b = q.back()             |
  | empty()       | ๋น์ด์์ผ๋ฉด true, ์๋๋ฉด false ๋ฐํ | while(!q.empty())            |
  | size()        | ์ฌ์ด์ฆ ๋ฐํ                        | for(int i=0; i<q.size();i++) |

* ์์ : ํ๋ก๊ทธ๋๋จธ์ค - ๊ธฐ๋ฅ๊ฐ๋ฐ

  ```c++
  #include<queue>
  #include<queue>
  #include<vector>
  using namespace std;
  
  vector<int>solution(vector<int>progresses, vector<int>speeds){
    vector<int> answer;
    queue<int> rest;
    for(int i=0; i<progresses.size(); i++){
      int re = 100-progresses[i];
      if (re%speeds[i]!=0) re += speeds[i]; //์์ธ์ฒ๋ฆฌ(ํ์คํธ์ผ์ด์ค 11)
  		rest.push(re/speeds[i]);    
    }
    
    while(!rest.empty()){
      int count = 1;
      int now = rest.front();
      rest.pop();
      while(rest.size()>=1){
        if (rest.front<=now){
          rest.pop();
          count++;
        }
        else break;
      }
      answer.push_back(count); //answer๋ vector์ด๊ธฐ ๋๋ฌธ์ push_back ์ฌ์ฉ
    }
    return answer;
  }
  ```



# 1-2) priority_queue

* ํค๋

  ```c++
  #include <queue>
  ```

* ์ ์ธ

  ```c++
  priority_queue<int> pq;
  //<type> ๋ณ์๋ช : ์ ์ธํ ์๋ฃํ ๋ณ์๋ค์ ๋ด๋ฆผ์ฐจ์์ ๋ฐ๋ผ ์ ๋ ฌ
  
  priority)queue<int, container, ๋น๊ตํจ์>pq;
  //<type, container, ๋น๊ตํจ์> ๋ณ์๋ช : ๋น๊ตํจ์์ ๋ฐ๋ผ ์ ๋ ฌ
  ```

* ํจ์

  | ๊ธฐ๋ณธ ํจ์ | ์ค๋ช                     | ์ฌ์ฉ             |
  | --------- | ------------------------ | ---------------- |
  | top()     | ๋งจ ์์ ์๋ ์์๋ฅผ ๋ฐํ | int s = pq.top() |

* ์์ : ํ๋ก๊ทธ๋๋จธ์ค - ํ๋ฆฐํฐ

  ```c++
  #include <string>
  #include <vector>
  #include <queue>
  using namespace std;
  
  int solution(vector<int> priorities, int location){
    int answer = 0;
    queue<pair<int,int>> que //pair ํด๋์ค๋ฅผ ํตํด ๋ ๊ฐ์ฒด๋ฅผ ํ๋๋ก ๊ฐ์ฒด๋ก ๋ฌถ์ 
    priority_queue<int> que2; //priority_queue์ ์ธ
    for(int i=0; i<priorities.size(); i++){
      que.push(make_pair(i,priorities[i])); //pair๋ฅผ ๋ง๋ค์ด์ฃผ๋ make_pairํจ์
      que2.push(priorities[i]);
    }
    while(!que.empty()){
      int f = que.front().first;
      int s = que.front().second;
      if (s==que2.top()){
        if (f==location){
          return answer+1;
        }
        else{
          answer++;
          que.pop();
          que2.pop();
        }
      }
      else{
        que.push(que.front());
        que.pop();
      }
    }
    return answer;
  }
  ```

* ์์ : ํ๋ก๊ทธ๋๋จธ์ค - ๋ ๋งต๊ฒ

  ```c++
  #include <queue>
  #include <string>
  using namespace std;
  
  int solution(vector<int> scoville, int K){
    int answer = 0;
    priority_queue<int, vector<int>, greater<int>> q;
    for(auto&i :scoville){
      q.push(i);
    }
    while(1){
      int min1 = q.top();
      q.pop();
      if(min1>=K) break;
      else if(q.empty){
        answer -=1;
        break;
      }
      int min2 = q.top();
      q.pop();
      q.push(min1+2*min2);
      answer++;
    }
    return answer;
  }
  ```

  

* ์์ : ์ค๋ฆ์ฐจ์

  ```c++
  #include <iostream>
  #include <queue>
  using namespace std;
  
  int main(void){
    priority_queue<int,vector<int>,greater<int>>pq; //๋น๊ตํจ์์ greater<int>๋ฅผ ๋ฃ์ด์ค
    pq.push(5);
    pq.push(1);
    pq.push(7);
    for(int i=0; i<3; i++){
      cout << pq.top() << '';
      pq.pop();
    }
  }
  //1 5 7
  ```

  



# 2) Stack

* ์ ์ํ์ถ (First In Last Out)

* ํค๋

  ```c++
  #include <stack>
  ```

* ์ ์ธ

  ```c++
  stack<int> s;//stack<type> name;stack<int> s2({1,2,3,4,5});s2.top() //5๊ฐ ๋์ด
  ```

  ---
  # Sort

* ํค๋

```c++
#include <algorithm>
```

* ์ ์ธ

```c++
sort(start, end) //[start, end) ๋ฒ์์ ์ธ์๋ฅผ ์ค๋ฆ์ฐจ์์ผ๋ก ์ ๋ ฌ
sort(start, end, compare) //compare ๊ธฐ์ค์ผ๋ก ์ ๋ ฌ
sort(start, end, greater<์๋ฃํ>()) //[start, end) ๋ฒ์์ ์ธ์๋ฅผ ๋ด๋ฆผ์ฐจ์์ผ๋ก ์ ๋ ฌ
```

* ์์(๋ด๋ฆผ์ฐจ์) : ํ๋ก๊ทธ๋๋จธ์ค H-Index

  ```c++
  #include <algorithm>
  #include <string>
  #include <vector>
  
  using namespace std;
  
  int solution(vector<int> citations){
    int answer = 0;
    sort(citations.begin(), citations.end(), greater<int>()); //greater<int>()
    if(!citations[0]) return 0;
    for(int i=0; i<citations.size(); i++){
      if(citations[i]>i) answer++;
      else break;
    }
    return answer;
  }
  ```



## + sort(start, end, compare)

* compare() ํจ์๋ฅผ sort()์ ์ธ ๋ฒ์งธ ์ธ์ ๊ฐ์ผ๋ก ๋ฃ์ผ๋ฉด, ํด๋น ํจ์์ ๋ฐํ ๊ฐ์ ๋ง๊ฒ ์ ๋ ฌ์ด ๋์

* ์์

  ```c++
  #include <iostream>
  #include <algorithm>
  using namespace std;
  
  bool compare(int a, int b){
    return a>b;
  }
  
  int main(void){
    int a[10] = {9,3,5,4,1}
    sort(a, a+10, compare); //์์ compare์ ๋ฐ๋ผ ์ ๋ ฌํจ
    for(int i=0;i<10;i++){
      cout << a[i] << ' ';
    }
  }
  ```

* ์์ : ํ๋ก๊ทธ๋๋จธ์ค - ๊ฐ์ฅ ํฐ ์

  ```c++
  #include <string>
  #include <vector>
  #include <algorithm>
  using namespace std;
  
  bool compare(int a, int b){
    string first = to_string(a) + to_string(b);
    string second = to_string(b) + to_string(a);
    return first > second;
  }
  string solution(vector<int> numbers){
    string answer = "";
    sort(numbers.begin(), numbers.end(), compare);
    for(auto&i:numbers){
      answer+=to_string(i);
    }
    return answer[0]=='0' ? "0" : answer; // a ? b : c -> a์ด๋ฉด b์ด๊ณ , ์๋๋ฉด c
  }
  ```

  

* ํน์ ํ ๋ณ์ ๊ธฐ์ค์ผ๋ก ์ ๋ ฌ

  ```c++
  #include <iosteram>
  #include <algorithm>
  using namespace std;
  
  class Student{
    public :
    string name;
    int score;
    Student(string name, int score){
      this-> name = name;
      this-> score = score;
    }
    bool operator < (Student &student){
      return this->score < student.score;
    }
    
  };
  
  bool compare(int a, int b){
    return a>b;
  }
  
  int main(void){
    Student students[] = {
      Student("๋๋๋น", 90);
      Student("๋๋๋ฐ", 80);
      Student("๋๋๋ฒ", 70);
    };
    sort(students, students+3);
    for(int i=0; i<5; i++){
      cout << students[i].name << ' ';
    }
  }
  ```

  

## + Appendix

* string::substr(์์์ธ๋ฑ์ค, ๋ฌธ์์ด ๊ธธ์ด)

  ```c++
  s1 = s.substr(2,3) //2๋ฒ ์ธ๋ฑ์ค๋ถํฐ ๋ฌธ์์ด 3๊ฐ
  ```

* string::substr(์์์ธ๋ฑ์ค)

  ```c++
  s2 = s.substr(1) //1๋ฒ ์ธ๋ฑ์ค๋ถํฐ ๋ฌธ์์ด ๋๊น์ง
  ```

* ์์

  ```c++
  if (phone_book[i] == phone_book[i+1].substr(0, phone_book[i].size()))
  //1) phone_book์ i๋ฒ์งธ ์์์
  //2) i+1๋ฒ์งธ ์์๋ฅผ 0๋ถํฐ phone_book[i]์ ์ฌ์ด์ฆ๋งํผ์ ๊ธธ์ด๋ก ์๋ฅธ ๊ฒ
  //1)๊ณผ 2)๊ฐ ๊ฐ๋ค๋ฉด!
  ```
---
# Vector

## ํต์ฌ

| ์ข๋ฅ         | ํจ์               | ์ค๋ช                                      |
| ------------ | ------------------ | ----------------------------------------- |
| ์ถ๊ฐ ๋ฐ ์ญ์  | push_back(element) | ๋ฒกํฐ ์ ์ผ ๋ค์ ์์ ์ถ๊ฐ                  |
|              | pop_back()         | ๋ฒกํฐ ์ ์ผ ๋ค์ ์์ ์ญ์                   |
| ์กฐํ         | [i]                | i๋ฒ์งธ ์์ ๋ฐํ                           |
|              | at(i)              | i๋ฒ์งธ ์์ ๋ฐํ                           |
|              | front()            | ์ฒซ๋ฒ์งธ ์์ ๋ฐํ                          |
|              | back()             | ๋ง์ง๋ง ์์ ๋ฐํ                          |
| ๊ธฐํ         | empty()            | ๋ฒกํฐ๊ฐ ๋น์ด์์ผ๋ฉด true, ์๋๋ฉด false ๋ฐํ |
|              | size()             | ๋ฒกํฐ ์์๋ค์ ์ ๋ฐํ                     |



## ํค๋ํ์ผ & ์ ์ธ

```c++
#include <vector>
```

```c++
//vector <์๋ฃํ> ๋ฒกํฐ๋ช;
vector <int> v1;
vector <char> v2;
vector <string> v3;
```

```c++
//int
vector <int> v1;
vector <int> v2(10);

vector <int> v3(7,5);
vector <int> v4{0,1,2,3};
vector <int> v5(v2);

//char
vector <char> v1;
vector <char> v2{'a','b','c'};
vector <char> v3(10);

//string
vector <string> v1{"ab", "xyx"};
vector <string> v1 = {"ab", "xyx"};
```



## ๋ฉค๋ฒํจ์

* v.size()

  v์ ์์์ ๊ฐ์๋ฅผ ๋ฐํ

* v.empty()

  v๊ฐ ๋น์ด์์ผ๋ฉด true(1), ๋น์ด์์ง ์์ผ๋ฉด false(0) ๋ฐํ

* v.front()

  v์ ์ฒซ๋ฒ์งธ ์์ ๋ฐํ

* v.back()

  v์ ๋ง์ง๋ง ์์ ๋ฐํ

* v.at(i) 

  v์ i๋ฒ์งธ ์์ // v[i]์ ๊ฐ์

* v.clear()

  v์ ๋ชจ๋  ์์ ์ ๊ฑฐ

* **v.push_back(5)**

  **v์ ๋งจ ๋ค์ ์์ '5'๋ฅผ ์ถ๊ฐ**

* **v.pop_back()**

  **v์ ๋งจ ๋ค์ ์์ ๊บผ๋ด๊ธฐ**

* v.insert(i,5)

  v์ i๋ฒ์งธ ์์น์์ ์์ '5'๋ฅผ ์ถ๊ฐ

* v.erase(i)

  v์ i๋ฒ์งธ ์์ ์ญ์  (v์ size๊ฐ 1๋งํผ ์ค์ด๋ฌ)

* v1.swap(v2)

  v1๊ณผ v2๋ฅผ swap

### iterator

* v.begin()

  beginning iterator๋ฅผ ๋ฐํ

* v.end()

  end iterator๋ฅผ ๋ฐํ





## ๊ธฐํ

### sort

* sort(arr,arr+n);
* sort(v.begin(),v.end());
* sort(v.begin(),v.end(),compare); //์ฌ์ฉ์ ์ ์ ํจ์ ์ฌ์ฉ
* sort(v.begin(), v.end(), greater<์๋ฃํ>()); //๋ด๋ฆผ์ฐจ์
* sort(v.begin(),v.end(),less<์๋ฃํ>());
  



    
