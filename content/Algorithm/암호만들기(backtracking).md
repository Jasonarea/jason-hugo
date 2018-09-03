---
title: "Baekjoon_1759 암호만들기 (Backtracking)"
date: 2018-09-03

featuredImage: "images/password.png"
categories: ["Backtracking"]
tags: ["Algorithm", "Backtracking", "Baekjoon"]
author: "Jason Jung"
---
### Baekjoon 1759 암호만들기
문제 설명 : L길이의 암호를 만들고 싶다. 주어진 알파벳은 6개의 알파벳이 주어진다. 가능한 암호를 모두 출력하는 문제
**암호의조건** 
1. 암호는 반드시 정렬이 되어있어야한다.
   - 입력을 받을 때 벡터로 입력받아 sorting을 한다.
2. 암호에는 반드시 자음 2개와 모음 1개가 포함되어 있어야한다.
   - check 함수를 만들어 password에 자음 2개와 모음 1개가 포함되어있는지 확인한다.
   
**Backtracking 재귀문제 풀이**
- 가능한경우 , 불가능한경우, 다음경우 이 세가지만 기억하자
1. 가능한 경우 : password.length() == L 이 되었을 때 password를 출력 후 return 해준다.
2. 불가능한 경우 : alpha.size() <= i (i는 현재까지 만들어진 password의 길이) 가 되었을 때 return 해준다.
3. 다음 경우 : go(n, alpha, password + alpha[i], i+1) --> 다음 알파벳을 포함하는 경우
                     go(n, alpha, password, i+1) --> 다음 알파벳을 포함하지 않는 경우
                     
    
[Source Code] 

    #include <iostream>
    #include <algorithm>
    #include <string>
    #include <vector>
    using namespace std;

    bool check(string &password) {
        int ja = 0;
        int mo = 0;
        for(char x: password) {
            if(x=='a' || x=='e' || x=='i' || x=='o'||x=='u')
                mo++;
            else
                ja++;
        }
        return (ja==2 && mo==1);
    }

    void go(int n, vector<char> &alpha, string password, int i) {
        if(password.length() == n){ // 정답을 찾은 경우
            if(check(password)){
                cout << password<< '\n';
            }
            return;
        }
        if(i==alpha.size()) return; // 정답을 못찾은 경우
        go(n, alpha, password+alpha[i], i+1);
        go(n, alpha, password, i+1);
    }

    int main(void)
    {
        int n, m;
        cin >> n >> m;
        vector<char> a(m);
        for(int i = 0;i<m;i++)
            cin >> a[i];

        sort(a.begin(), a.end());
        go(n, a, "", 0);

        return 0;
    }

