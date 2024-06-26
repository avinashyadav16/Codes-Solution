# 2933. High-Access Employees

[![High-Access Employees](https://img.shields.io/badge/-Leetcode-grey?style=for-the-badge&logo=Leetcode&logoColor=Gray)](https://leetcode.com/problems/high-access-employees/description/) ![Medium](https://img.shields.io/badge/-Medium-green?style=for-the-badge&logoColor=green)


<details>
Author: Avinash Yadav<br>
Date: 02-01-2024
</details><br>

You are given a **2D 0-indexed** array of strings, `access_times`, with size `n`. For each `i` where `0 <= i <= n - 1`, `access_times[i][0]` represents the name of an employee, and `access_times[i][1]` represents the access time of that employee. All entries in `access_times` are within the same day.

The access time is represented as **four digits** using a **24-hour** time format, for example, `"0800"` or `"2250"`.

An employee is said to be **high-access** if he has accessed the system **three or more** times within a **one-hour period**.

Times with exactly one hour of difference are not considered part of the same one-hour period. For example, `"0815"` and `"0915"` are not part of the same one-hour period.

Access times at the start and end of the day are **not** counted within the same one-hour period. For example, `"0005"` and `"2350"` are not part of the same one-hour period.

Return *a list that contains the names of **high-access** employees with any order you want*.

 

### Example 1:
```
Input: access_times = [["a","0549"],["b","0457"],["a","0532"],["a","0621"],["b","0540"]]
Output: ["a"]

Explanation: "a" has three access times in the one-hour period of [05:32, 06:31] which are 05:32, 05:49, and 06:21.
But "b" does not have more than two access times at all.
So the answer is ["a"].
```


### Example 2:
```
Input: access_times = [["d","0002"],["c","0808"],["c","0829"],["e","0215"],["d","1508"],["d","1444"],["d","1410"],["c","0809"]]
Output: ["c","d"]

Explanation: "c" has three access times in the one-hour period of [08:08, 09:07] which are 08:08, 08:09, and 08:29.
"d" has also three access times in the one-hour period of [14:10, 15:09] which are 14:10, 14:44, and 15:08.
However, "e" has just one access time, so it can not be in the answer and the final answer is ["c","d"].
```


### Solution:

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution
{
public:
    bool hasHighAccessFrequency(vector<int> &accessTimes)
    {
        for (int i = 0; i < accessTimes.size() - 2; i++)
        {
            if (accessTimes[i + 2] - accessTimes[i] < 60)
            {
                return true;
            }
        }
        return false;
    }

    vector<string> findHighAccessEmployees(vector<vector<string>> &access_times)
    {
        vector<string> highAccessEmployees;
        unordered_map<string, vector<int>> employeeAccessMap;

        for (auto access : access_times)
        {
            string hours = access[1].substr(0, 2);
            string minutes = access[1].substr(2, 2);
            int totalMinutes = stoi(hours) * 60 + stoi(minutes);

            employeeAccessMap[access[0]].push_back(totalMinutes);
        }

        for (auto employee : employeeAccessMap)
        {
            if (employee.second.size() < 3)
            {
                continue;
            }

            sort(employee.second.begin(), employee.second.end());

            if (hasHighAccessFrequency(employee.second))
            {
                highAccessEmployees.push_back(employee.first);
            }
        }

        return highAccessEmployees;
    }
};

```