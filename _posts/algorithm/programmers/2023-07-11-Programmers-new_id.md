---
title:  "[프로그래머스] 신규 아이디 추천 (C++)" 

categories:
  - Programmers
tags:
  - [BOJ, 프로그래머스, 신규아이디추천, C++]

toc: true
toc_sticky: true

date: 2023-07-11
last_modified_at: 2023-07-11
---


## 프로그래머스 (신규 아이디 추천)

### 문제 

카카오에 입사한 신입 개발자 네오는 "카카오계정개발팀"에 배치되어, 카카오 서비스에 가입하는 유저들의 아이디를 생성하는 업무를 담당하게 되었습니다. "네오"에게 주어진 첫 업무는 새로 가입하는 유저들이 카카오 아이디 규칙에 맞지 않는 아이디를 입력했을 때, 입력된 아이디와 유사하면서 규칙에 맞는 아이디를 추천해주는 프로그램을 개발하는 것입니다.
다음은 카카오 아이디의 규칙입니다.

아이디의 길이는 3자 이상 15자 이하여야 합니다.
아이디는 알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.) 문자만 사용할 수 있습니다.
단, 마침표(.)는 처음과 끝에 사용할 수 없으며 또한 연속으로 사용할 수 없습니다.
"네오"는 다음과 같이 7단계의 순차적인 처리 과정을 통해 신규 유저가 입력한 아이디가 카카오 아이디 규칙에 맞는 지 검사하고 규칙에 맞지 않은 경우 규칙에 맞는 새로운 아이디를 추천해 주려고 합니다.
신규 유저가 입력한 아이디가 new_id 라고 한다면,

1단계 new_id의 모든 대문자를 대응되는 소문자로 치환합니다.
2단계 new_id에서 알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.)를 제외한 모든 문자를 제거합니다.
3단계 new_id에서 마침표(.)가 2번 이상 연속된 부분을 하나의 마침표(.)로 치환합니다.
4단계 new_id에서 마침표(.)가 처음이나 끝에 위치한다면 제거합니다.
5단계 new_id가 빈 문자열이라면, new_id에 "a"를 대입합니다.
6단계 new_id의 길이가 16자 이상이면, new_id의 첫 15개의 문자를 제외한 나머지 문자들을 모두 제거합니다.
     만약 제거 후 마침표(.)가 new_id의 끝에 위치한다면 끝에 위치한 마침표(.) 문자를 제거합니다.
7단계 new_id의 길이가 2자 이하라면, new_id의 마지막 문자를 new_id의 길이가 3이 될 때까지 반복해서 끝에 붙입니다.


[제한사항]
new_id는 길이 1 이상 1,000 이하인 문자열입니다.
new_id는 알파벳 대문자, 알파벳 소문자, 숫자, 특수문자로 구성되어 있습니다.
new_id에 나타날 수 있는 특수문자는 -_.~!@#$%^&*()=+[{]}:?,<>/ 로 한정됩니다.



### 풀이


단순 구현 문제이다. 

1단계는 모든 for문으로 돌며 대문자를 -32로 소문자로 바꿔주면 될 일이고,

2단계는 정규식을 사용해도 되지만 나는 쌩으로 했다. if문으로 저 기준에 해당하지 않는 글자라면 넘어가면 된다.

3단계는 여기서 고민을 많이 했는데, 결과적으로 for문으로 계속 가다가, .을 만나면 flag를 true로 바꿔준다. 그러다가 .이 아닌 문자를 만났을 때, flag가 true라면 거기다가 . 하나만 붙여넣어주고, 
그대로 다른 문자들은 붙여넣어주면 끝난다. 

4단계는 0번째, size()-1로 끝 자락만 검사해주면 되는 거고. 

5단계도 size() == 0으로 검사해서 없으면 'a'를 붙여넣어주면 되는 일이다.

6단계는. 16보다 큰지 작은지 확인 한 후, 15개 까지만 일단 새로운 string에다가 붙여넣어준다. 그 후, 맨 끝이 .이라면 그걸 제외해준다. 

7단계는 3이하인지 아닌지 확인한 후, 3이하라면 맨 끝에 있는 걸 계속 while문으로 넣어주면 되는 간단한 문제다. 

```
#include <iostream>
#include <algorithm>


using namespace std;

int main() {
#include <string>
#include <vector>
#include <iostream>

using namespace std;

string transone(string id){
    string new_id;
    for(int x= 0; x < id.size(); x++){
        if(id[x] >= 'A' && id[x] <= 'Z') new_id += (id[x] + 32);
        else new_id += id[x];
    }
    return new_id;
}

string transtwo(string id){
    string new_id;
    for(int x = 0; x< id.size(); x++){
        if((id[x] >= 'a' && id[x] <= 'z') || (id[x] >= '0' && id[x] <= '9') || id[x] == '.' || id[x] == '-' || id[x] == '_'){
            new_id += id[x];
        }
    }
    return new_id;
}

string transthree(string id){
    int flag = 0;
    string old_id = id;
    string new_id;
    int s = 0, e = 0, pflag = 0;
    for(int x = 0; x< old_id.size(); x++){
        if(old_id[x] == '.'){
            pflag = 1;
        }
        else{
            if(pflag){
                new_id += '.';
                pflag = 0;
            }
            new_id += old_id[x];
        }
    }
    return new_id;
}

string transfour(string id){
    string old_id;
    for(int x = 0; x< id.size(); x++){
        if(x == 0 && id[x] == '.') continue;
        if(x == (id.size() - 1) && id[x] == '.')continue;
        old_id += id[x];
    }
    return old_id;
}

string transfive(string id){
    if(id.size() == 0) id += 'a';
    return id;
}

string transsix(string id){
    string new_id;
    if(id.size() < 16) return id;
    else{
        for(int x = 0; x < 15; x++){
            new_id += id[x];
        }
    }
    string newnew_id;
    if(new_id[new_id.size()-1] == '.'){
        for(int x = 0; x < new_id.size()-1; x++){
            newnew_id += new_id[x];
        }
        return newnew_id;
    }
    return new_id;
}

string transseven(string id){
    if(id.size() >= 3) return id;
    while(1){
        char ch = id[id.size()-1];
        id += ch;
        if(id.size() >= 3) return id;
    }
}

string solution(string new_id) {
    string id = transone(new_id);
    string id_2 = transtwo(id);
    string id_3 = transthree(id_2);
    string id_4 = transfour(id_3);
    string id_5 = transfive(id_4);
    string id_6 = transsix(id_5);
    string answer = transseven(id_6);
    return answer;
}
```


다소 날림으로 쓴 코드라서 가독성이나 그런 부분이 많이 부족하다. 