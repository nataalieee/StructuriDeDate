## Tema 1 SD - Pogar Denisa Natalia 151 

### Problem 1.1 - Nearest Smaller Values

<img width="450" height="150" alt="image" src="https://github.com/user-attachments/assets/ccb4e094-30ba-4f0d-982e-e5e7e8883b11" />

```cpp
#include <bits/stdc++.h>
using namespace std;

int n,x;
vector<int> v,rasp;
stack<pair<int,int>> s;
int main()
{
    cin>>n;
    v.push_back(0); ///ca sa mearga indexarea de la 1
    for(int i=0;i<n;i++)
    {
        cin>>x;
        v.push_back(x);
    }
    s.push({v[0],0});
    for(int i=1;i<=n;i++)
    {
        if(v[i]>s.top().first)
            rasp.push_back(s.top().second);

        else
        {
            while(s.top().first>=v[i] && !s.empty())
            {
                s.pop();
            }
            if(s.empty()) rasp.push_back(0);
            else rasp.push_back(s.top().second);
        }
        s.push({v[i],i});
    }
    for(int i=0;i<n;i++) cout<<rasp[i]<<' ';
    return 0;
}
```
### Problem 1.2 - Strabunica
### Problem 1.3 - Fadema
### Problem 1.4 
### Problem 1.5 - Remove Duplicates
### Problem 1.7 
### Problem 2.1 - Alee
### Problem 2.2 - Vase
### Problem 3.1 - Deque

## Bonus 
### Problem 1.6 
### Problem 3.2
### Problem 4.1

