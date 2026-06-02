## Tema 6 SD - Pogar Denisa Natalia 151 

### Problem 2.1

https://www.infoarena.ro/job_detail/3356521?action=view-source

```cpp
#include <bits/stdc++.h>
#define nmax 100002
using namespace std;
ifstream f("arbint.in");
ofstream g("arbint.out");
int n,m,arr[nmax],sol,aint[nmax*4],op,x,y;
void build(int nod, int st, int dr) {
    if (st==dr) {
        aint[nod]=arr[st];
        return;
    }
    int mid=(st+dr)/2;
    build(2*nod,st,mid);
    build(2*nod+1,mid+1,dr);
    aint[nod]=max(aint[2*nod],aint[2*nod+1]);
}
void update(int nod, int st, int dr, int poz, int val) {
    if (st==dr) {
        aint[nod]=val;
        return;
    }
    int mid=(st+dr)/2;
    if (poz<=mid) update(2*nod,st,mid,poz,val);
    if (poz>mid) update(2*nod+1,mid+1,dr,poz,val);
    aint[nod]=max(aint[2*nod],aint[2*nod+1]);
}
void query(int nod, int st, int dr, int left, int right) {
    if (left>dr || right<st) return;
    if (left<=st && dr<=right) {
        sol=max(sol,aint[nod]); return ;
    }
    int mid=(st+dr)/2;
    query(2*nod,st,mid,left,right);
    query(2*nod+1,mid+1,dr,left,right);
}
int main() {
    f>>n>>m;
    for (int i=1;i<=n;i++) f>>arr[i];
    build(1,1,n);
    for (int i=1;i<=m;i++) {
        f>>op>>x>>y;
        if (op==0) {
            query(1,1,n,x,y);
            g<<sol<<'\n';
            sol=0;
        }
        else update(1,1,n,x,y);
    }
}
```
### Problem 2.2
https://infoarena.ro/job_detail/3356603?action=view-source

```cpp
#include <bits/stdc++.h>
#define nmax 100002
using namespace std;
ifstream f("inv.in");
ofstream g("inv.out");
int n,m,sol,aint[nmax*4],arr[nmax];
vector<pair<int,int>> v;
long long sum;

void update(int nod, int st, int dr, int poz, int val) {
    if (st==dr) {
        aint[nod]=val;
        return;
    }
    int mid=(st+dr)/2;
    if (poz<=mid) update(2*nod,st,mid,poz,val);
    if (poz>mid) update(2*nod+1,mid+1,dr,poz,val);
    aint[nod]=aint[2*nod]+aint[2*nod+1];
}
int query(int nod, int st, int dr, int left, int right) {
    if (left>dr || right<st) return 0;
    if (left<=st && dr<=right) {
        return aint[nod];
    }
    int mid=(st+dr)/2;
    return query(2*nod,st,mid,left,right)+query(2*nod+1,mid+1,dr,left,right);
}
int main() {
    f>>n;
    v.resize(n+5);
    for (int i=1;i<=n;i++) {
        f>>v[i].first;
        v[i].second=i;///pozitia = rangul
    }

    ///sortam crescator dupa valoare, desc dupa pozitie
    sort(v.begin()+1, v.begin()+n+1, [](pair<int, int> a, pair<int, int> b) {
        if (a.first==b.first) return a.second<b.second;
        return a.first<b.first;
    });

    for (int i=1; i<=n; i++) {
        arr[v[i].second]=i;
    }
    ///build(1,1,n);
    for (int i=1;i<=n;i++) {
        sum+=query(1,1,n,arr[i]+1,n);
        sum%=9917;
        update(1,1,n,arr[i],1);
    }
    g<<sum%9917<<'\n';
}
```

### Problem 2.3

https://www.infoarena.ro/job_detail/3356587?action=view-source

```cpp
#include <bits/stdc++.h>
#define nmax 200002
using namespace std;
ifstream f("maxq.in");
ofstream g("maxq.out");
int n,m,arr[nmax],op,x,y;
struct Node {
    long long sum, pref,suff,ans;
}aint[nmax*4];
Node merge_nodes(Node left, Node right) {
    Node parent;
    parent.sum=left.sum+right.sum;
    parent.pref=max(left.pref, left.sum+right.pref);
    parent.suff=max(right.suff, right.sum+left.suff);
    parent.ans=max(left.ans,max(right.ans, left.suff+right.pref));
    return parent;
}

void build(int nod, int st, int dr) {
    if (st==dr) {
        aint[nod].sum=arr[st];
        aint[nod].pref=arr[st];
        aint[nod].suff=arr[st];
        aint[nod].ans=arr[st];
        return;
    }
    int mid=(st+dr)/2;
    build(2*nod,st,mid);
    build(2*nod+1,mid+1,dr);
    aint[nod]=merge_nodes(aint[2*nod],aint[2*nod+1]);
}
void update(int nod, int st, int dr, int poz, int val) {
    if (st==dr) {
        aint[nod].sum=val;
        aint[nod].pref=val;
        aint[nod].suff=val;
        aint[nod].ans=val;
        return;
    }
    int mid=(st+dr)/2;
    if (poz<=mid) update(2*nod,st,mid,poz,val);
    if (poz>mid) update(2*nod+1,mid+1,dr,poz,val);
    aint[nod]=merge_nodes(aint[2*nod],aint[2*nod+1]);
}
Node query(int nod, int st, int dr, int left, int right) {
    if (left<=st && dr<=right) {
       return aint[nod];
    }
    int mid=(st+dr)/2;
    if (right <= mid) return query(2*nod, st, mid, left, right);
    if (left > mid) return query(2*nod+1, mid+1, dr, left, right);
    Node st_node=query(2*nod, st, mid, left, right);
    Node dr_node=query(2*nod+1, mid+1, dr, left, right);
    return merge_nodes(st_node,dr_node);
}
int main() {
    f>>n;
    for (int i=1;i<=n;i++) f>>arr[i];
    build(1,1,n);
    f>>m;
    for (int i=1;i<=m;i++) {
        f>>op>>x>>y;
        if (op==1) {
            Node sol=query(1,1,n,x+1,y+1);
            if (sol.ans<=0) g<<0<<'\n';
            else g<<sol.ans<<'\n';

        }
        else update(1,1,n,x+1,y);
    }
}
```
### Problem 2.4
https://www.infoarena.ro/job_detail/3356591?action=view-source
```cpp
#include <bits/stdc++.h>
#define nmax 100002
using namespace std;
ifstream f("mit.in");
ofstream g("mit.out");
int n,m,arr[nmax],sol,aint[nmax*4],op,x,y,a,b,lazy[4*nmax];
void propagare(int nod, int st, int dr) {
    if (lazy[nod]==0) return;
    aint[nod]+=lazy[nod];
    if (st!=dr) {
        lazy[2*nod]+=lazy[nod];
        lazy[2*nod+1]+=lazy[nod];
    }
    lazy[nod]=0;

}
void build(int nod, int st, int dr) {
    if (st==dr) {
        aint[nod]=arr[st];
        return;
    }
    int mid=(st+dr)/2;
    build(2*nod,st,mid);
    build(2*nod+1,mid+1,dr);
    aint[nod]=max(aint[2*nod],aint[2*nod+1]);
}
void update(int nod, int st, int dr, int left, int right, int val) {
    propagare(nod,st,dr);
    if (left>dr || right<st) return;
    if (left<=st && dr<=right) {
        lazy[nod]+=val;
        propagare(nod,st,dr);
        return;
    }
    int mid=(st+dr)/2;
    update(2*nod,st,mid,left, right,val);
    update(2*nod+1,mid+1,dr,left,right,val);
    aint[nod]=max(aint[2*nod],aint[2*nod+1]);
}
void query(int nod, int st, int dr, int left, int right) {
    if (left>dr || right<st) return;
    propagare(nod,st,dr);
    if (left<=st && dr<=right) {
        sol=max(sol,aint[nod]); return ;
    }
    int mid=(st+dr)/2;
    query(2*nod,st,mid,left,right);
    query(2*nod+1,mid+1,dr,left,right);
}
int main() {
    f>>n>>m;
    for (int i=1;i<=n;i++) f>>arr[i];
    build(1,1,n);
    for (int i=1;i<=m;i++) {
        f>>op;
        if (op==1) {
            f>>x>>y;
            query(1,1,n,x,y);
            g<<sol<<'\n';
            sol=0;
        }
        else {
            f>>a>>b>>x;
            update(1,1,n,a,b,x);
        }
    }
}
```

## Bonus
### Problem 2.5
