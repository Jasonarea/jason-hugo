---
title: "Baekjoon_14889 스타트와링크 (bitmask)"
date: 2018-08-31

featuredImage: "images/start and link.png"
categories: ["BruteForce"]
tags: ["Algorithm", "BruteForce", "Baekjoon"]
author: "Jason Jung"
---
### Bitmask
### Baekjoon 14889 : 스타트와 링크
문제 설명 : 스타트와 링크팀으로 반반을 나누어 능력치를 비교한다. 능력치를 최소화하는 팀을 만든다 
모든 팀의 경우의 수를 구하여 그 중 능력치의 차이가 최소화되는 팀을 정한다. (이 때 나올 수 있는 부분집합을 bitmask 를 이용하여 구한다.)

Next Permutation을 이용하여 문제를 해결하는 방법과 동일하나 나올 수 있는 부분집합을
next_permutation으로 처리하냐, bitmask를 이용하여 처리하냐의 차이다.
원리는 동일하다.

- Bitmask를 이용하여 문제를 푸는 경우
[Source Code] 

        #include <iostream>
        #include <vector>
        #include <algorithm>
        using namespace std;

        int s[20][20];
        int main(void)
        {
            int n;
            cin >> n;
            for(int i = 0;i<n;i++)
                for(int j = 0;j<n;j++)
                    cin >> s[i][j];

            int ans = -1;
            for(int i = 0;i<(1<<n);i++) {
                vector<int> first, second;
                for(int j = 0;j<n;j++) {
                    if(i&(1<<j))
                        first.push_back(j);
                    else
                        second.push_back(j);
                }
                if(first.size() != n/2) continue;
                int t1 = 0, t2 = 0;
                for(int l1 = 0;l1<n/2;l1++) {
                    for(int l2 = 0;l2<n/2;l2++) {
                        if(l1==l2) continue;
                        t1 += s[first[l1]][first[l2]];
                        t2 += s[second[l1]][second[l2]];
                    }
                }
                int diff = t1 - t2;
                if(diff < 0) diff = -diff;
                if(ans == -1 || ans > diff)
                    ans = diff;
            }
            cout << ans << '\n';
            return 0;
        }

