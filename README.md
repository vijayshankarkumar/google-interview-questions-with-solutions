# Google Interview Questions with Solutions
[source](https://leetcode.com/discuss/post/6185127/2024-google-interview-questions-compilat-mjrf/)

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

##### 2. There will be some tasks, and each task can have one or more subtasks. By default, all tasks have a completion time of 1 unit. However, the completion time of a parent task is x if all its subtasks have a completion time of x; otherwise, it is the sum of their completion times. We need to find the total completion time.

##### Solution
```cpp
unordered_map<int, vector<int>> adj;
int computeCompletionTime(int node) {
    if (adj[node].empty()) return 1;
    vector<int> subtaskTimes;
    for (int child : adj[node]) {
        subtaskTimes.push_back(computeCompletionTime(child));
    }
    bool allSame = true;
    for (int i = 1; i < subtaskTimes.size(); i++) {
        if (subtaskTimes[i] != subtaskTimes[0]) {
            allSame = false;
            break;
        }
    }
    return allSame ? subtaskTimes[0] : accumulate(subtaskTimes.begin(), subtaskTimes.end(), 0);
}
```

##### 3. You’re given a string which may contain "bad pairs".
A bad pair is defined as a pair of adjacent characters, where they are the same letter in different cases. For example, "xX" is a bad pair, but "xx" or "XX" are not.
Implement a solution to remove the bad pairs from the string.<br />
Sample:<br />
	• Input: abxXw<br />
	• Output: abw<br />
    • Input: abxXXxw<br />
	• Output: abw<br />
Additional Case:<br />
If the input string is empty:<br />
	• Input: ""<br />
	• Output: ""<br />

##### Solution
```cpp
bool isBadPair(const char& a, const char& b) {
    return (tolower(a) == tolower(b)) && 
           ((isupper(a) && islower(b)) || (islower(a) && isupper(b)));
}

string removeBadPair(const string& str) {
    string res = str.substr(0, 1);
    for (int i = 1; i < str.size(); i++) {
        if (isBadPair(str[i - 1], str[i])) res.pop_back();
        else res.push_back(str[i]);
    }
    return res;
}
```

##### 4. 
