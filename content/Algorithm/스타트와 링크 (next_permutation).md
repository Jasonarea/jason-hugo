---
title: "Baekjoon_14889 스타트와링크 (next_permutation)"
date: 2018-08-31

featuredImage: "images/start and link.png"
categories: ["BruteForce"]
tags: ["Algorithm", "BruteForce", "Baekjoon"]
author: "Jason Jung"
---
### next_permutation
### Baekjoon 14889 : 스타트와 링크
문제 설명 : 스타트와 링크팀으로 반반을 나누어 능력치를 비교한다. 능력치를 최소화하는 팀을 만든다 --> 모든 팀의 경우의 수를 구하여 그 중 능력치의 차이가 최소화되는 팀을 정한다.
- next_permutation 을 이용하여 문제를 푸는 경우
[Source Code] 

        #include <iostream>
        #include <vector>
        using namespace std;
        int main(void)
        {
            int n;
            cin >> n;
            vector<vector<int>> a(n, vector<int>(n));
            for(int i = 0;i<n;i++) {
                for(int j = 0;j<n;j++)
                    cin >> a[i][j];
            }
            vector<int> b(n);
            for(int i = 0;i<n/2;i++)
                b[i] = 1;

            sort(b.begin(), b.end());
            int ans = 2147483647;

            do {
                vector<int> first, second;
                for(int i = 0;i<n;i++) {
                    if(b[i] == 0)
                        first.push_back(i);
                    else
                        second.push_back(i);
                }
                int one = 0;
                int two = 0;
                for(int i = 0;i<n/2;i++) {
                    for(int j = 0;j<n/2;j++) {
                        if(i==j) continue;
                            one += a[first[i]][first[j]];
                            two += a[second[i]][second[j]];
                    }
                }
                int diff = one - two;
                if(diff <0) diff = -diff;
                if(ans > diff) ans = diff;
            } while(next_permutation(b.begin(), b.end()));

            cout << ans << '\n';
            return 0;
        }
