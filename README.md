# DP問題整理

## [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)
- Recursive 解法
```cpp=
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
```cpp=
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
```cpp=
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
