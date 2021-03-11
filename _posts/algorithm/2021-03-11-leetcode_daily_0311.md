---
layout: post
title:  "[LeetCode] Coin Change"
category: Algorithm
tags: [Algorithm, LeetCode]
comments: true  
---

#### [LeetCode] Daily 문제
##### [Coin Change](https://leetcode.com/problems/coin-change/)

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

 

Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1
Example 3:

Input: coins = [1], amount = 0
Output: 0
Example 4:

Input: coins = [1], amount = 1
Output: 1
Example 5:

Input: coins = [1], amount = 2
Output: 2
 

Constraints:

- 1 <= coins.length <= 12
- 1 <= coins[i] <= 231 - 1
- 0 <= amount <= 104


``` cpp
#define MAX_COINS 10000

class Solution {
private:
    int length;
public:
    Solution() {
        length = 0;
    }
    
    int coinChange(vector<int>& coins, int amount) {
        // ** amounts[i]: the minimum number of used coins when the amount is i.
        int *amounts = new int [amount + 1];
        int length = coins.size();
        
        sort(coins.begin(), coins.end());
        
        // ** Abort if minimum coin is larger than amount
        if ((amount < coins[0]) && (amount != 0)) {
            return -1;
        }
        else if (amount == 0) {
            return 0;
        }
        
        // ** Just initialization
        for (int i = 0; i <= amount; i++) {
            amounts[i] = MAX_COINS;
        }

        // ** the minimum number of used coin is 1 if amount is equal to coin
        for (int i = 0; i < length; i++) {
            if (coins[i] <= amount) {
                amounts[coins[i]] = 1;
            }
        }
        
        for (int i = 0; i <= amount; i++) {
            for(int j = 0; j < length; j++) {
                if (i - coins[j] >= 0) {
                    // ** Find the minimum case when the amount is i.
                    amounts[i] = std::min(amounts[i - coins[j]] + 1, amounts[i]);
                }
            }
        }
        
        // ** If the amount is not calculable with coins, then return -1
        if (amounts[amount] == MAX_COINS) {
            return -1;
        }
        else {
            return amounts[amount];
        }
    }
    
};
```

논문 보다가 머리아플 때 머리 식힐 목적으로 LeetCode 풀어보는건데, 되려 머리가 터질거같다. 본인은 알고리즘 공부에 매우 취약하므로, 문제가 잘 풀리지 않을 시 블로그 같은 곳에서 힌트를 살짝 보면서 알고리즘 공부를 하는 중이다.

이런 문제는 BFS를 사용하여 풀기도 하는데, 본인은 BFS를 잘 모르므로 Dynamic Programming이라는 방식을 사용하여 문제를 풀었다. 함수를 Recurssive하게 사용하지 않고 데이터에 직접 넣어 시간을 절약하는 방식이다. (이 방식을 사용한 이유는 실제로 함수 콜 때문에 Time Out이 발생하였기 때문이다.)

현재 가지고 있는 코인들을 통해 표현할 수 있는 모든 코인들의 집합을 찾는 방식이다. 그리고 amount가 0일 때라던지, coin이 amount보다 많을 때와 같은 예외처리를 적용해주었다.

##### 출처:
- [Coin Change](https://leetcode.com/problems/coin-change/)
