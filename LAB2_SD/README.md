## Tema 1 SD - Pogar Denisa Natalia 151 

TESTARE PULL REQUEST nr 2
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
        if(v[i]>s.top().first) rasp.push_back(s.top().second);
        else
        {
            while(!s.empty() && s.top().first>=v[i] ) s.pop();
            
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
<img width="350" height="150" alt="image" src="https://github.com/user-attachments/assets/27696b9a-6c11-4c5b-ac9a-0ad86dfbf87e" />

```cpp
#include <bits/stdc++.h>
using namespace std;
ifstream f("strabunica.in");
ofstream g("strabunica.out");

int n,v[200005],st[200005],dr[200005];
stack<pair<int,int>> s;
long long rasp;
long long max(long long a, long long b)
{
    if(a>b) return a;
    return b;
}
int main()
{
    f>>n;
    for(int i=0;i<n;i++) f>>v[i];

    s.push({-1,-1});
    for(int i=0;i<n;i++)
    {
        if(v[i]>s.top().first) st[i]=s.top().second;
        else
        {
            while(!s.empty() && s.top().first>=v[i]) s.pop();
            
            if(s.empty()) st[i]=-1;
            else st[i]=s.top().second;
        }
        s.push({v[i],i});
    }

    while(s.size()) s.pop();
    s.push({0,n});
    for(int i=n-1;i>=0;i--)
    {
        if(v[i]>s.top().first) dr[i]=s.top().second;
        else
        {
            while(!s.empty() && s.top().first>=v[i] ) s.pop();
            
            if(s.empty()) dr[i]=n;
            else dr[i]=s.top().second;
        }
        s.push({v[i],i});
    }
    for(int i=0;i<n;i++)
    {
        long long dif;
        dif=dr[i]-st[i]-1;
        long long crt=dif*v[i];
        rasp=max(rasp,crt);
    }
    g<<rasp;  return 0;
}

```

### Problem 1.3 - Fadema
### Problem 1.4 
Ne gandim la prob ca la un graf => a[i][j]=1 <=> i il cunoste pe j. Un nod este celebritate daca linia a[nod][] este plina de 0, si coloana lui a[][nod] de 1, cu exceptia a[nod][nod]. Astfel, noi incepem din coltul stanga-sus si ne uitam pe coloana.
```cpp
#include <bits/stdc++.h>
using namespace std;

int n,a[1000][1000],x,y;
int main()
{
    cin>>n;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
            cin>>a[i][j];
    x=1; y=1; ///coltul stanga-sus
    ///gandim maticea de adiacenta a unui graf orientat <-> a[i][j]=1 <=> i il cunoste pe j
    ///un nod este celebritate daca linia a[nod][] este plina de 0,
    ///si coloana lui a[][nod] de 1, cu exceptia a[nod][nod]

    while(y<n)
        {
            if(a[x][y+1]==1) x=y+1; y++;
        }
    ///daca ne oprim pe linia x, x+1 va fi raspunsul
    cout<<x; return 0;
}
```
### Problem 1.5 - Remove Duplicates
### Problem 1.7 
### Problem 2.1 - Alee
### Problem 2.2 - Vase
### Problem 3.1 - Deque

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/8ff5ccba-3edf-4570-b2f7-77d5b15210cf" />

```cpp
#include <bits/stdc++.h>
using namespace std;
ifstream f("deque.in");
ofstream g("deque.out");

int n,k,x,cnt,v[5000002];
long long suma;
deque <int> q;
int main()
{
    cin>>n>>k;
    for(int i=1;i<=n;i++)cin>>v[i];
    for(int i=1;i<=n;i++)
    {
        x=v[i];
        while(q.size() && q.front()>x) q.pop_front();
        q.push_front(x);
        if(i>=k)
        {
            if(v[i-k]==q.back()) q.pop_back();
            suma+=q.back();
            ///cout<<q.back()<<' ';
        }
    }
    cout<<suma; return 0;
}

```

## Bonus 
### Problem 1.6 
### Problem 3.2
### Problem 4.1

