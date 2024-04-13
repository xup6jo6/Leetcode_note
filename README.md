# DP問題整理
<!-- TOC -->

- [DP問題整理](#dp%E5%95%8F%E9%A1%8C%E6%95%B4%E7%90%86)
    - [入門DP](#%E5%85%A5%E9%96%80dp)
        - [70. Climbing Stairs](#70-climbing-stairs)
    - [進階DP](#%E9%80%B2%E9%9A%8Edp)
        - [714. Best Time to Buy and Sell Stock with Transaction Fee](#714-best-time-to-buy-and-sell-stock-with-transaction-fee)

<!-- /TOC -->





## 入門DP

### [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)
- Recursive 解法
```cpp
class Solution {
public:
    int climbStairs(int n) {
        return solve(n);
    }
    int solve(int n){
        if(n<0){
            return 0;
        }
        if(n==0){
            return 1;
        }
        return solve(n-1)+solve(n-2);
    }
};
```
- Top Down 解法
Top Down 其實就是 **Recursive** + **Memoization**
```cpp
class Solution {
public:
    int climbStairs(int n) {
        vector<int> memo(n+1, -1);
        return solve(n, memo);
    }
    int solve(int n, vector<int> &memo){
        if(n<=1){
            return n<0? 0: 1;
        }
        if(memo[n-1]==-1){
            memo[n-1] = solve(n-1, memo);
        }
        if(memo[n-2]==-1){
            memo[n-2] = solve(n-2, memo);
        }
        int move1Step = memo[n-1];
        int move2Step = memo[n-2];
        return move1Step+move2Step;
    }
};
```
- Bottom Up 解法
```cpp
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n+1);
        dp[0] = 1, dp[1] = 1;
        
        // dp[i] 表示，下一步就要到第i階，有幾種走法
        // 所以 dp[i] = dp[i-1]+dp[i-2]
        for(int i=2; i<=n; i++){
            dp[i] = dp[i-2]+dp[i-1];
        }
        return dp[n];
    }
};
```

## 進階DP

### [714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/?envType=study-plan-v2&envId=leetcode-75)
- Recursive 解法
```cpp
class Solution {
public:
    int solve(vector<int>& prices, int fee, int index, bool hold, int buy_price){
        if(index==prices.size()-1){
            if(!hold){
                return 0;
            }
            else{
                return prices[index]-buy_price;
            }
        }
        if(!hold){
            int buyAtToday = -fee+solve(prices, fee, index+1, 1, prices[index]);
            int skipToday = solve(prices, fee, index+1, 0, INT_MAX);
            return max(buyAtToday, skipToday);
        }
        else{
            int sellAtToday = prices[index]-buy_price+solve(prices, fee, index+1, 0, INT_MAX);
            int skipToday = solve(prices, fee, index+1, 1, buy_price);
            return max(sellAtToday, skipToday);
        }
    }
    int maxProfit(vector<int>& prices, int fee) {
        bool hold = 0;
        return solve(prices, fee, 0, hold, 0);
    }
};
```

- Top Down解法
```cpp
class Solution {
public:
    int solve(vector<int>& prices, int fee, int index, vector<vector<int>> &memo, int hold){
        if(index==prices.size()){
            return 0;
        }
        if(memo[hold][index]!=INT_MIN){
            return memo[hold][index];
        }
        int profit;
        if(hold){
            int keepHolding = solve(prices, fee, index+1, memo, 1);
            int SellAtToday = prices[index]-fee+solve(prices, fee, index+1, memo, 0);
            profit = max(keepHolding, SellAtToday);
        }
        else{
            int keepNoAction = solve(prices, fee, index+1, memo, 0);
            int buyAtToday = -prices[index]+solve(prices, fee, index+1, memo, 1);
            profit = max(keepNoAction, buyAtToday);
        }
        //printf("profit=%d\n", profit);
        return memo[hold][index] = profit;
    }
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        vector<vector<int>> memo(2, vector<int>(n, INT_MIN));
        return solve(prices, fee, 0, memo, 0);
    }
};
```