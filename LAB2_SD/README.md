## Tema 1 SD - Pogar Denisa Natalia 151 

TESTARE PULL REQUEST nr 2
### Problem 1.1 - Nearest Smaller Values

https://cses.fi/paste/445b2c8a3ced649efd0d75/

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

https://www.infoarena.ro/job_detail/3347524?action=view-source

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

https://www.infoarena.ro/job_detail/3349056?action=view-source

```cpp
#include <bits/stdc++.h>
#define nmax 1005
using namespace std;
ifstream f("fadema.in");
ofstream g("fadema.out");

int n,m,dp[nmax][nmax],a[nmax][nmax], st[nmax][nmax], dr[nmax][nmax],rasp,zid_st,zid_dr;
vector<int> s;
int main()
{
    f>>n>>m;
    for(int i=1; i<=n; i++)
        for(int j=1; j<=m; j++) f>>a[i][j];

    ///calculam inaltimea maxima ca la histograma
    for(int j=1; j<=m; j++) dp[1][j]=1;
    for(int i=2; i<=n; i++)
        for(int j=1; j<=m; j++)
        {
            if(a[i][j]!=a[i-1][j]) dp[i][j]=dp[i-1][j]+1;
            else dp[i][j]=1;
        }

    for(int i=1; i<=n; i++)
    {
        zid_st=0;
        for(int j=1; j<=m; j++)
        {
            ///se strica din cauza vecinilor egali
            if(j>1 && a[i][j]==a[i][j-1])
            {
                zid_st=j-1;
                s.clear();
            }
            ///calculam granita dupa inaltime
            while(!s.empty() && dp[i][s.back()]>=dp[i][j]) s.pop_back();
            ///setam stanga
            if(s.empty()) st[i][j]=zid_st;
            else st[i][j]=s.back();

            s.push_back(j);
        }

        s.clear();

        zid_dr=m+1;
        for(int j=m; j>=1; j--)
        {
            ///se strica din cauza vecinilor egali
            if(j<m && a[i][j]==a[i][j+1])
            {
                zid_dr=j+1;
                s.clear();
            }
            ///calculam granita dupa inaltime
            while(!s.empty() && dp[i][s.back()]>=dp[i][j]) s.pop_back();
            ///setam dreapta
            if(s.empty()) dr[i][j]=zid_dr;
            else dr[i][j]=s.back();

            s.push_back(j);
        }
        s.clear();
        for(int k=1; k<=m; k++)
        {
            int dif=dr[i][k]-st[i][k]-1;
            int crt=dif*dp[i][k];
            rasp=max(rasp,crt);
        }
    }
    g<<rasp;
    return 0;
}

```

### Problem 1.4 

(update si aici)
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
    
    cout<<x; return 0;
}
```
### Problem 1.5 - Remove Duplicates

https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/submissions/1958679813

Am tinut intr-un stack fiecare litera si numarul de aparitii pentru a putea sa scot fiecare calup de k litere consecutive. 

```cpp
class Solution {
public:
    string removeDuplicates(string s, int k) {
        vector<pair<char,int>> v;
        v.push_back({s[0],1});
        for(int j=1;j<s.size();j++)
        {
            if(!v.empty() && v.back().first==s[j])
            {
                int fr=v.back().second;
                v.pop_back();
                if(fr+1!=k) v.push_back({s[j],fr+1});
            }
            else v.push_back({s[j],1});   
        }
        string rasp;
        for(int i=0;i<v.size();i++)
        {
            char x=v[i].first;
            for(int k=0;k<v[i].second;k++)  rasp.push_back(x);
        }
        return rasp;
    }
};
```

### Problem 1.7 

Am gandit problema ca "subsirul crescator care sa contina toate literele cu nr de aparitii impare o singura data". Daca o litera are nr par de aparitii poate fi ignorata complet. Pentru literele "de pus" le adaugam in stiva numai daca nu sunt deja, le scadem frecventa cand trecem peste o pozitie si scoatem toate literele de dinaintea ei mai mari, daca ele vor mai aparea in sir in viitor. La final bagam litera curenta in stiva si o marcam ca e bagata.

```cpp
#include <bits/stdc++.h>
using namespace std;

string s;
vector<int> v[35],stiva;
vector<char> rasp;
bool de_pus[40],pus[40];
int fr[40];

int main()
{
    cin>>s;
    for(int i=0; i<s.size(); i++)
    {
        char x=s[i];
        int p=int(x-'a');
        v[p].push_back(i);
    }
    for(int i=0; i<35; i++)
    {
        if(v[i].size()%2==0)
        {
            v[i].clear();
            de_pus[i]=0;
        }
        else de_pus[i]=1,fr[i]=v[i].size();
    }

    for(int i=0; i<s.size(); i++)
    {
        if(de_pus[s[i]-'a'])
        {
            fr[s[i]-'a']--;
            if(pus[s[i]-'a']==1) continue;

            while(!stiva.empty() && stiva.back()>(s[i]-'a') && fr[stiva.back()]>0)
            {
                pus[stiva.back()]=0;
                stiva.pop_back();
            }

            stiva.push_back(s[i]-'a');
            pus[s[i]-'a']=1;

        }
    }
    while(stiva.size())
    {
        int x=stiva.back();
        rasp.push_back(x+'a');
        stiva.pop_back();
    }
    reverse(rasp.begin(), rasp.end());
    for(int i=0; i<rasp.size(); i++) cout<<rasp[i];
    return 0;
}

}

```
### Problem 2.1 - Alee

https://infoarena.ro/job_detail/3349246?action=view-source
```cpp
#include <bits/stdc++.h>
#define nmax 177
using namespace std;
ifstream f("alee.in");
ofstream g("alee.out");

class coada
{
public:
    int i,j,val;
    coada(int i, int j, int v)
    {
        this->i=i;
        this->j=j;
        val=v;
    }
};
queue<coada> q;
int vi[4]={-1,0,1,0},vj[4]={0,1,0,-1};
int n,m,x1,x2,y11,y2,a[nmax][nmax],rasp;
bool gasit,viz[nmax][nmax];
int main()
{
    f>>n>>m;
    for(int i=0;i<=n+1;i++)a[i][0]=a[i][n+1]=-1;
    for(int j=0;j<=n+1;j++)a[0][j]=a[n+1][j]=-1;
    for(int i=1;i<=m;i++)
    {
        int x,y;
        f>>x>>y; a[x][y]=-1; ///marcam copac
    }
    f>>x1>>y11>>x2>>y2;

    viz[x1][y11]=1;
    q.push(coada(x1,y11,1));
    while (q.size() && !gasit)
    {
        coada crt=q.front();
        q.pop();
        for(int k=0;k<4;k++)
        {
            int new_i=crt.i+vi[k];
            int new_j=crt.j+vj[k];
            if(new_i==x2 && new_j==y2)
            {
                rasp=crt.val+1;
                gasit=1;
                break;
            }
            if(viz[new_i][new_j]==0 && a[new_i][new_j]!=-1)
            {
                viz[new_i][new_j]=1;
                q.push(coada(new_i,new_j,crt.val+1));
            }
        }
    }
    g<<rasp;
    return 0;
}

```
### Problem 2.2 - Vase

http://pbinfo.ro/detalii-evaluare/63924175

Pentru ca se garanteaza ca avem solutie avem 2 scenarii: turnari repetate din A in B sau din B in A. Am ales metoda asta, deoarece mi s-a parut mai usor sa memorez drumul decat cea cu BFS. (la final am ales scenariul cu pasi mai putini)

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> r1,r2;
int a,b,c,crt_a,crt_b;
void cod(int x)
{
    if(x==1) cout<<"R A\n";
    else if(x==2) cout<<"R B\n";
    else if(x==3) cout<<"A C\n";
    else if(x==4) cout<<"B C\n";
    else if(x==5) cout<<"A B\n";
    else  cout<<"B A\n";
}
int main()
{
    cin>>a>>b>>c;
    ///testam scenariul 1: turnam a->b
    while(crt_a!=c && crt_b!=c)
    {
        if(crt_a==0)
        {
            r1.push_back(1);///cod pt "umplem a"
            crt_a=a;
        }
        else if(crt_b==b)
        {
            r1.push_back(4);///cod "varsam b"
            crt_b=0;
        }
        else if (crt_a+crt_b<=b)
        {
            r1.push_back(5);///cod "turnam a in b"
            crt_b+=crt_a;
            crt_a=0;
        }
        else if(crt_a+crt_b>b)
        {
            r1.push_back(5);///cod "turnam a in b"
            crt_a=crt_a-(b-crt_b);
            crt_b=b;
        }
    }
    crt_a=0; crt_b=0;
    ///testam scenariul 2: turnam b->a in mod repetat
    while(crt_a!=c && crt_b!=c)
    {
        if(crt_b==0)
        {
            r2.push_back(2);///cod pt "umplem b"
            crt_b=b;
        }
        else if(crt_a==a)
        {
            r2.push_back(3);///cod "varsam a"
            crt_a=0;
        }
        else if (crt_a+crt_b<=a)
        {
            r2.push_back(6);///cod "turnam b in a"
            crt_a+=crt_b;
            crt_b=0;
        }
        else if(crt_a+crt_b>a)
        {
            r2.push_back(6);///cod "turnam b in a"
            crt_b=crt_b-(a-crt_a);
            crt_a=a;
        }
    }
    if(r1.size()<=r2.size())
    {
        cout<<r1.size()<<'\n';
        for(int i=0;i<r1.size();i++)
            cod(r1[i]);
    }
    else
    {
        cout<<r2.size()<<'\n';
        for(int i=0;i<r2.size();i++)
            cod(r2[i]);
    }
    return 0;
}

```
### Problem 3.1 - Deque

https://www.infoarena.ro/job_detail/3347534?action=view-source

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

Am calculat cu bucketuri de cate k functia f aplicata elementelor mele pe sufix si pe prefix. La final am facut f(rez_st,rez_dr).

```cpp
#include <iostream>
using namespace std;

int f(int a, int b)
{
    if(a<b) return a;
    return b;
}
int n,k,v[1000],pref[1000],suff[1000],crt;
int main()
{
    cin>>n>>k;
    for(int i=1;i<=n;i++) cin>>v[i];
    
    for(int i=1;i<=n;i++)
    {
        if(i%k==1) crt=v[i];
        else crt=f(crt,v[i]);
        pref[i]=crt;
    }
    for(int i=n;i>=1;i--)
    {
        if(i==n || i%k==0) crt=v[i];
        else crt=f(crt,v[i]);
        suff[i]=crt;

    }
    for(int i=k;i<=n;i++)
        if(i%k==0) cout<<pref[i]<<' ';
        else cout<<f(pref[i],suff[i-k+1])<<' ';
    return 0;
}

```
### Problem 3.2

Am implementat un deque cu o lista dublu inlantuita si un array care memoreaza adresele fiecarui nod pentru a ramane iteratorul valid. Logica a fost sa folosesc arrayul in mod circular, astfel "endd" tine minte prima pozitie libera pe care se poate adauga direct, iar "start" prima pozitie ocupata. Iar la resize m-am folsoit de logica de la vectorul stl, intai ii dublez capacitatea, mut elementele, sterg vechiul array si pun pointerul unde trebuie. 

```cpp
#include <iostream>
using namespace std;
class Node
{
public:
    int value;
    Node *next,*prev;
};
class Deque
{
    Node *head=nullptr, *tail=nullptr, *prev=nullptr, **array_ads=nullptr;///array de adrese
    int length=0,capacity=0,start,endd;
public:
    Deque(int s=16)
    {
        capacity=s;
        array_ads=new Node*[capacity];
        for(int i=0; i<capacity; i++) array_ads[i] = nullptr;
        start=endd=0;

    }
    void my_resize()
    {
        int old_cp=capacity;
        if(old_cp==0) capacity=2;
        capacity*=2;

        Node **new_array=new Node*[capacity];
        for(int i=0; i<capacity; i++) new_array[i] = nullptr;
        int cnt=0;

        ///mutam elementele
        if(start<endd)
            for(int i=start; i<endd; i++) new_array[cnt++]=array_ads[i];

        else
        {

            for(int i=start; i<old_cp; i++) new_array[cnt++]=array_ads[i];
            for(int i=0; i<endd; i++) new_array[cnt++]=array_ads[i];;
        }
        start=0;
        endd=length;

        delete[]array_ads;
        array_ads=new_array;
    }
    ~Deque()
    {
        delete[] array_ads;
        while(head!=nullptr)
        {
            Node *current=head;
            head=head->next;
            delete current;
        }
    }
    void push_back(int v)
    {
        Node *nod_nou=new Node();
        nod_nou->value=v;
        nod_nou->next=nullptr;
        nod_nou->prev=nullptr;
        ///daca lista era goala
        if (head==nullptr)
        {
            head=nod_nou;
            tail=nod_nou;
        }
        else ///avem elemente
        {
            nod_nou->prev=tail;
            tail->next=nod_nou;///legam nodurile
            tail=nod_nou;
        }
        if(length==capacity) my_resize();

        ///adaugam in arrayul de adrese
        array_ads[endd]=nod_nou;
        endd=(endd+1)%capacity;
        length++;

    }
    void push_front(int v)
    {
        Node *nod_nou=new Node();
        nod_nou->value=v;
        nod_nou->next=head;///legam nodurile
        nod_nou->prev=nullptr;

        ///daca lista era goala
        if (head==nullptr)
        {
            head=nod_nou;
            tail=nod_nou;
        }
        else ///avem elemente
        {
            head->prev=nod_nou;
            head=nod_nou;
        }
        if(length==capacity) my_resize();

        ///adaugam in arrayul de adrese
        start--;
        if(start<0) start=capacity-1;
        array_ads[start]=nod_nou;

        length++;
    }
    void pop_end()
    {
        if(head==nullptr) return ;
        length--;

        ///stergem din arrayul de adrese
        endd--;
        if(endd<0) endd=capacity-1;
        array_ads[endd]=nullptr;

        if (head==tail)
        {
            delete tail;
            tail=nullptr;
            head=nullptr;
            return;
        }

        Node *current=tail->prev;
        current->next=nullptr;
        delete tail;
        tail=current;
    }
    void pop_front()
    {
        if (head==nullptr) return ;

        ///stergem din arrayul de adrese
        array_ads[start]=nullptr;
        start=(start+1)%capacity;

        if (length==1)
        {
            delete head;
            head=nullptr;
            tail=nullptr;
            length=0;
            return;
        }
        length--;
        Node *current=head->next;
        current->prev=nullptr;
        delete head;
        head=current;
    }

};
int main()
{
    return 0;
}

```

### Problem 4.1

