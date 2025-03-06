# Google Interview Questions with Solutions

##### 1. You are given maximum initial energy k and array A of length n denoting wind speed on n days. You stuck on a boat and each day you can either choose to move or stay put. Each day you decide to travel you will move ahead A[i] Dist and your energy decreases by 1 and if you decide to stay the your energy increases by 1.
Find the maximum distance you can travel without your energy dropping below zero after n days.

##### Solution
It is a classic 0-1 knapsack problem, on each day i decide whether to travel or stay, after i have done processing for the ith day, for (i + 1)th onwards i have the same problem to solve with another set of energy value where on the ith i have to stay or travel, which is sub problem

```f(i, A, k) = max(A[i] + f(i, A, k - 1), f(i, A, k + 1)```<br />

A[i] + f(i, A, k - 1) => I have decided to travel<br />
f(i, A, k + 1) => I have decided to stay<br />

```cpp
vector<vector<int>> dp;
int maxDist(int day, const vector<int>& speed, int energy) {
    if (day >= speed.size() || energy <= 0) return 0;
    if (dp[day][energy] != -1) return dp[day][energy];
    return dp[day][energy] = max(speed[day] + maxDist(day + 1, speed, energy - 1),  maxDist(day + 1, speed, energy + 1));
}
```
