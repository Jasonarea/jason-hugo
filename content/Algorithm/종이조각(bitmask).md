---
title: "Baekjoon_14391 종이조각 (bitmask)"
date: 2018-08-31

featuredImage: "images/paper.png"
categories: ["BruteForce"]
tags: ["Algorithm", "BruteForce", "Baekjoon"]
author: "Jason Jung"
---
### next_permutation
### Baekjoon 14391 종이조각
문제 설명 : n * m으로 이루어진 숫자 테이블이 있다. 이 테이블을 여러 조각으로 나누어 나온 숫자들의 합이 가장 최대가 되는 경우를 구하는 문제이다.
예를 들어 3 * 3으로 이루어진 숫자 테이블의 경우
1   2   1      
2   3   4      
4   5   2     
이런식으로 구성될 수 있다.
이는 결국 가로로 쪼갤 것이냐, 세로로 쪼갤 것이냐 두가지의 경우로 나뉠 수 있고 가로로 쪼개는 경우를 0, 세로로 쪼개는 경우를 1이라 정한 다음
 비트마스크로 풀 수 있다.

000000000부터 111111111까지의 모든 경우의 수를 따져봐야 하므로 

    for(int i = 0;i<(1<<n*m);i++) 식을 이용해야한다.
    
[Source Code] 

    #include <cstdio>
    #include <iostream>
    #include <algorithm>
    using namespace std;
    int a[4][4];
    int main(void)
    {
        int n, m;
        scanf("%d %d", &n, &m);
        for(int i = 0;i<n;i++)
            for(int j = 0;j<m;j++)
                scanf("%1d", &a[i][j]);

        int ans = 0;
        // 0:-, 1:|

        for(int s = 0;s<(1<<n*m);s++) {
            int sum = 0;
            for(int i = 0;i<n;i++) {    //가로의 경우
                int cur = 0;
                for(int j = 0;j<m;j++) {
                    int k = i*m + j;
                    if((s & (1<<k)) == 0)
                        cur = cur * 10 + a[i][j];
                    else {
                        sum += cur;
                        cur = 0;
                    }
                }
                sum += cur;
            }

            for(int j = 0;j<m;j++) {    //세로의 경우
                int cur = 0;
                for(int i = 0;i<n;i++) {
                    int k = i*m + j;
                    if((s&(1<<k)) != 0)
                        cur = cur * 10 + a[i][j];
                    else {
                        sum += cur;
                        cur = 0;
                    }
                }
                sum += cur;
            }
            ans = max(ans, sum);
        }
        cout << ans << '\n';
        return 0;
    }

