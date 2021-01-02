---
layout: post
title:  1211. [S/W 문제해결 기본] 2일차 - Ladder2
category: Algorithm
tags: [Algorithm, SW Expert Academy]
comments: true  
---

#### SW Expert Academy 문제
##### [1211. [S/W 문제해결 기본] 2일차 - Ladder2](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14ABYKADACFAYh)

점심 시간에 산책을 다니는 사원들은 최근 날씨가 더워져, 사다리 게임을 통하여 누가 아이스크림을 구입할지 결정하기로 한다.<br>

김 대리는 사다리타기에 참여하지 않는 대신 사다리를 그리기로 하였다.<br>

사다리를 다 그리고 보니 김 대리는 어느 사다리를 고르면 X표시에 도착하게 되는지 궁금해졌다. 이를 구해보자.<br>

아래 <그림 1>의 예를 살펴보면, 출발점 x=0 및 x=9인 세로 방향의 두 막대 사이에 임의의 개수의 막대들이 랜덤 간격으로 추가되고(이 예에서는 2개가 추가됨) 이 막대들 사이에 가로 방향의 선들이 또한 랜덤하게 연결된다.<br>

X=0인 출발점에서 출발하는 사례에 대해서 화살표로 표시한 바와 같이, 아래 방향으로 진행하면서 좌우 방향으로 이동 가능한 통로가 나타나면 방향 전환을 하게 된다.<br>

방향 전환 이후엔 다시 아래 방향으로만 이동하게 되며, 바닥에 도착하면 멈추게 된다.<br>

문제의 X표시에 도착하려면 X=4인 출발점에서 출발해야 하므로 답은 4가 된다. 해당 경로는 별도로 표시하였다.<br>

***문제에 대한 자세한 설명은 출처를 참조***<br>

**[제약사항]**<br>

한 막대에서 출발한 가로선이 다른 막대를 가로질러서 연속하여 이어지는 경우는 없다.<br>

**[입력]**<br>

각 테스트 케이스의 첫 번째 줄에는 테스트 케이스의 번호가 주어지며, 바로 다음 줄에 테스트 케이스가 주어진다.<br>

총 10개의 테스트 케이스가 주어진다.<br>

**[출력]**<br>

#부호와 함께 테스트 케이스의 번호를 출력하고, 공백 문자 후 도착하게 되는 출발점의 x좌표를 출력한다.<br>

``` cpp
#include <iostream>
#include <stdio.h>

#define TEST_CASE   10
#define MAT_SIZE    100
#define LEFT   -1
#define RIGHT   1
#define DOWN    0

using namespace std;

int search_dir(int mat[MAT_SIZE][MAT_SIZE], int r, int c);
void move(int mat[MAT_SIZE][MAT_SIZE], int dir, int *r, int *c, int *cnt);

int main(int argc, char** argv)
{
    
#ifdef USE_FREOPEN
    freopen("input.txt", "r", stdin);
#endif
    
    int test_case;
    for (test_case = 1; test_case <= TEST_CASE; ++test_case)
    {
        int test_num;
        int mat[MAT_SIZE][MAT_SIZE];
        int min_path = 0;
        int min_path_index;
        
        cin >> test_num;
        
        for (int r = 0; r < MAT_SIZE; r++) {
            for (int c = 0; c < MAT_SIZE; c++) {
                cin >> mat[r][c];
            }
        }
        
        for (int c = 0; c < MAT_SIZE; c++) {
            // ** find the answer from top to bottom
            if (!mat[MAT_SIZE-1][c]) {
                continue;
            }
            int curr_c = c;
            int curr_r = 0;
            int dir;
            int path = 0;
            
            while (curr_r < MAT_SIZE - 1) {
                dir = search_dir(mat, curr_r, curr_c);
                move(mat, dir, &curr_r, &curr_c, &path);
            }
            
            if (!min_path) {
                min_path = path;
                min_path_index = c;
            }
            else if (min_path > path) {
                min_path = path;
                min_path_index = c;
            }
        }
        
        cout << "#" << test_num << " " << min_path_index << endl;
    }
    
    return 0;
}

int search_dir (int mat[MAT_SIZE][MAT_SIZE], int r, int c)
{
    // ** search left
    if (c > 0) {
        if (mat[r][c - 1]) {
            return LEFT;
        }
    }
    
    // ** search right
    if (c < MAT_SIZE - 1) {
        if (mat[r][c + 1]) {
            return RIGHT;
        }
    }
    
    return DOWN;
}

void move (int mat[MAT_SIZE][MAT_SIZE], int dir, int *r, int *c, int *cnt)
{
    if (dir == LEFT) {
        while ((mat[*r][(*c)-1]) && ((*c) > 0)) {
            (*c)--;
            (*cnt)++;
        }
    }
    else if (dir == RIGHT) {
        while ((mat[*r][(*c)+1]) && ((*c) < MAT_SIZE - 1)) {
            (*c)++;
            (*cnt)++;
        }
    }
    (*r)++;
    (*cnt)++;
}
```

##### 출처:
- [SW Expert Academy](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14ABYKADACFAYh)