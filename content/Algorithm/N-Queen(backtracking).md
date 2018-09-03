---
title: "Baekjoon_9663 N-Queen 을 O(1)의 빠르기로 풀기 (Backtracking)"
date: 2018-09-03

featuredImage: "images/nqueen.png"
categories: ["Backtracking"]
tags: ["Algorithm", "Backtracking", "Baekjoon"]
author: "Jason Jung"
---
Backjoon 9663 N-Queen
문제 설명 : row와 column 의 체스판 크기가 주어지고 해당 체스판에 서로 영향을 받지 않는 Queen의 갯수를 출력하는 문제

- 물론 모든 경우의 수를 고려하여 각각의 row마다 올수 있는지 없는지 check 해주면 된다.
- 하지만 O(1)의 빠르기로 푸는 방법이 존재한다. 
- check_col, check_dig, check_dig2 배열을 만들어 Queen 이 (row, col)에 있게 되는 경우 해당 위치의 가로줄, 대각선줄을 모두 true로 바꿔주어 다음 방문 시 접근하지 못하게 하는 방법이다.


[Source Code] 

    #include <iostream>
    using namespace std;
    bool a[15][15];
    int n;
    bool check_col[15];
    bool check_dig[15];
    bool check_dig2[15];
    bool check(int row, int col) {
        // |
        if(check_col[col])
            return false;

        //왼쪽 위 대각선
        if(check_dig[col])
            return false;

        // 오른쪽 위 대각선
        if(check_dig2[col])
            return false;

        return true;
    }
    int calc(int row) {
        if(row==n) //정답을 찾은경우
            return 1;
        int cnt = 0;

        for(int col = 0;col<n;col++) {
            if(check(row, col)) {
                check_dig[row+col] = true;
                check_dig2[row-col+n] = true;
                check_col[col] = true;
                a[row][col] = true;
                cnt += calc(row+1);
                check_dig[row+col] = false;
                check_dig2[row-col+n] = false;
                check_col[col] = false;
                a[row][col] = false;
            }
        }
        return cnt;
    }

    int main(void)
    {
        cin >> n;
        cout << calc(0) << '\n';
        return 0;
    }
