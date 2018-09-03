---
title: "Baekjoon_1248 맞춰봐 (Backtracking)"
date: 2018-09-03

featuredImage: "images/solve.png"
categories: ["Backtracking"]
tags: ["Algorithm", "Backtracking", "Baekjoon"]
author: "Jason Jung"
---
### Baekjoon 1248 맞춰봐
문제 설명 : 멍청한 규현이는 이제야 겨우 -10에서 10까지의 21개의 숫자를 알았다. 규현이가 n개의 숫자를 적었을때 s[i][j]의 의미는 i번째 수에서 j번째 수까지의 합의 부호를 의미한다. (합이 양수면 +, 음수면 - , 0이면 0). s 배열이 주어졌을 때 규현이가 쓴 숫자 n개의 가능한 경우 하나를 출력하는 문제

- 모든 경우의 수를 고려할 경우 21^10 이라는 어마어마한 경우의 수가 나올 수 있다. 그러므로 시간복잡도를 최소화하는 경우를 생각해야한다.

**풀이**

- Backtracking을 이용하여 문제를 푼다.
- 정답이 가능한경우 : index == n 
- sign[index][index] 의 경우 해당 숫자의 부호를 따라가는 걸 이용한다.

        if(sign[index][index]==0) ans[index] = 0;
- 무조건 for문을 -10부터 10까지 돌지 않고 1부터 10까지 돌면서 sign[index][index]값의 부호에 따라 i를 곱해준다.
- 그렇게 해주면 자동으로 -10부터 찾아보지 않아도 해당 숫자의 부호의 경우를 살펴볼 수 있다.

        for(int i = 1;i<=10;i++){
                ans[index] = sign[index][index] * i; //부호화의 연관성 고려
                if(go(index+1)) return true;
        }
- index번째 수를 결정하면 0~index번째 수는 변하지 않는다.
- 따라서 모든 sign[k][index] (0<=k<index)를 go(index)에서 검사할 수 있다.

        for (int i=1; i<=10; i++) {
            ans[index] = sign[index][index]*i;
            if (check(index) && go(index+1)) return true;
        }



[Source Code] 

    #include <iostream>
    #include <string>
    using namespace std;
    int n;
    int sign[10][10];
    int ans[10];
    bool check(int index) {
        int sum = 0;
        for (int i=index; i>=0; i--) {
            sum += ans[i];
            if (sign[i][index] == 0) {
                if (sum != 0) return false;
            } else if (sign[i][index] < 0) {
                if (sum >= 0) return false;
            } else if (sign[i][index] > 0) {
                if (sum <= 0) return false;
            }
        }
        return true;
    }
    bool go(int index) {
        if (index == n) {
            return true;
        }
        if (sign[index][index] == 0) {
            ans[index] = 0;
            return check(index) && go(index+1);
        }
        for (int i=1; i<=10; i++) {
            ans[index] = sign[index][index]*i;
            if (check(index) && go(index+1)) return true;
        }
        return false;
    }
    int main() {
        cin >> n;
        string s;
        cin >> s;
        int cnt = 0;
        for (int i=0; i<n; i++) {
            for (int j=i; j<n; j++) {
                if (s[cnt] == '0') {
                    sign[i][j] = 0;
                } else if (s[cnt] == '+') {
                    sign[i][j] = 1;
                } else {
                    sign[i][j] = -1;
                }
                cnt += 1;
            }
        }
        go(0);
        for (int i=0; i<n; i++) {
            cout << ans[i] << ' ';
        }
        cout << '\n';
        return 0;
    }