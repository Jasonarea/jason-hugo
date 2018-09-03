---
title: "Baekjoon_14889 스타트와 링크 (Backtracking)"
date: 2018-09-03

featuredImage: "images/start and link.png"
categories: ["Backtracking"]
tags: ["Algorithm", "Backtracking", "Baekjoon"]
author: "Jason Jung"
---
### Baekjoon 14889 스타트와 링크
문제 설명 : 스타트와 링크팀으로 반반을 나누어 능력치를 비교한다. 능력치를 최소화하는 팀을 만든다 --> 모든 팀의 경우의 수를 구하여 그 중 능력치의 차이가 최소화되는 팀을 정한다.
- Backtracking을 이용하여 문제를 푸는 경우


**Backtracking 재귀문제 풀이**

1. 가능한 경우 : index == n
2. 불가능한 경우 : first.size() > n/2 or second.size() > n/2
3. 다음 경우 : (first에 팀원이 속한 경우) go(index, first, second)
(second에 팀원이 속한 경우) go(second, first, second)


[Source Code] 

    #include <iostream>
    #include <vector>
    #include <algorithm>
    using namespace std;
    int s[20][20];
    int n;

    int go(int index, vector<int> &first, vector<int> &second) {
        if(index == n) {    // 정답을 찾은 경우
            if(first.size() != n/2) return -1;
            if(second.size() != n/2) return -1;
            int t1 = 0;
            int t2 = 0;
            for(int i = 0;i<n/2;i++){
                for(int j = 0;j<n/2;j++) {
                    if(i==j) continue;
                    t1 += s[first[i]][first[j]];
                    t2 += s[second[i]][second[j]];
                }
            }
            int diff = t1 - t2;
            if(diff < 0) diff = -diff;
            return diff;
        }

        if(first.size() > n/2) return -1; // 정답을 못찾은 경우
        if(second.size() > n/2) return -1;

        int ans = -1;
        first.push_back(index);
        int t1 = go(index + 1, first, second);
        if(ans == -1 || (t1 != -1 && ans > t1))
            ans = t1;
        first.pop_back();

        second.push_back(index);
        int t2 = go(index + 1, first, second);
        if(ans == -1 || (t2 != -1 && ans > t2))
            ans = t2;
        second.pop_back();
        return ans;
    }

    int main(void)
    {
        cin >> n;
        for(int i = 0;i<n;i++)
            for(int j = 0;j<n;j++)
                cin >> s[i][j];
        vector<int> first, second;

        cout << go(0, first, second) << '\n';
        return 0;
    }
