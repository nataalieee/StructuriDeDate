
## Tema 3 SD - Pogar Denisa Natalia 151 

### Problem 1.1 - Hash_map
M-am folosit de lista dublu-inlantuita ca sa tin un array de obiecte de tip LinkedList. Am folosit politica de resize la alfa>0.75, functia de hash a lui wang bazata de shiftari si xoruri. 

```cpp
#include <iostream>
class Node
{
public:
    int value;
    Node *next=nullptr,*prev=nullptr;
    Node(int val):value{val}{}
};
class LinkedList{
    Node *head=nullptr, *tail=nullptr, *prev=nullptr, **array_ads=nullptr;///array de adrese
    unsigned int length=0;
public:
    LinkedList() {}
    ~LinkedList() {
        while (head!=nullptr) {
            pop_front();
        }
    }
    void erase(Node* current) {
        if (length==0) return;
        ///un elem
        length--;
        if (head==tail) {
            delete current;
            head=tail=nullptr;
            return;
        }
        ///e primul elem
        if (current==head) {
            head=current->next;
            head->prev=nullptr;
            delete current;
            return;
        }
        ///e ultimul elem
        if (current==tail) {
            tail->prev->next=nullptr;
            tail=tail->prev;
            delete current;
            return;
        }
        ///mijloc
        current->prev->next=current->next;
        current->next->prev=current->prev;
        delete current;
    }
    void insert_after(Node *current,int val) {
        Node *new_node=new Node(val);
        length++;
        if (head==nullptr) {
            head=tail=new_node;
            return;
        }
        if (current==tail) {
            current->next=new_node;
            new_node->prev=tail;
            tail=new_node;
            return;
        }
        new_node->next=current->next;
        new_node->prev=current;
        new_node->next->prev=new_node;
        new_node->prev->next=new_node;
    }
    void insert_before(Node *current, int val) {
        length++;
        Node *new_node=new Node(val);
        if(head==nullptr)
        {
            head=tail=new_node;
            return;
        }
        if (current==head) {
            head->prev=new_node;
            new_node->next=head;
            head=new_node;
            return;
        }
        new_node->next=current;
        new_node->prev=current->prev;
        new_node->next->prev=new_node;
        new_node->prev->next=new_node;
    }
    void push_back(int val) {
        insert_after(tail,val);
    }
    void push_front(int val) {
        insert_before(head,val);
    }
    void pop_front() {
        erase(head);
    }
    void pop_back() {
        erase(tail);
    }
    Node* get_front() {
        return head;
    }
    Node* get_back() {
        return tail;
    }
    /*int get_key_back() {
        return tail->key;
    }*/
};

class Hash_map {
    int size,capacity;
    LinkedList *array;
    unsigned int wang_hash_32(int old_x) {
        unsigned int x=(unsigned int)old_x;
        x = ((x >> 16) ^ x) * 0x45d9f3b;
        x = ((x >> 16) ^ x) * 0x45d9f3b;
        return x;
    }
    void resize() {
        int new_cap=capacity*4;
        if (capacity==0) new_cap=8;
        LinkedList *new_array=new LinkedList[new_cap];

        for (int i=0;i<capacity;i++) {
            Node *current=array[i].get_front();
            while (current!=nullptr) {
                int cur_value=current->value;
                new_array[wang_hash_32(cur_value)%new_cap].push_back(cur_value);
                current=current->next;
            }
        }

        delete[]array;
        array=new_array;
        capacity=new_cap;
    }
public:
    Hash_map(int cap=8):capacity{cap},size{0} {
        array=new LinkedList[capacity];
    }
    ~Hash_map() {
        delete[] array;
    }
    void insert(int x) {
        if (exists(x)) return;
        int index=wang_hash_32(x)%capacity;
        array[index].push_back(x);
        size++;
        double alfa=(double)size/capacity;
        if (alfa>=0.75) resize();
    }
    void erase(int x) {
        if (!exists(x)) {
            throw std::out_of_range("Nu exista cheia!");
        }
        int index=wang_hash_32(x)%capacity;
        LinkedList * lista=&array[index];
        Node *current=lista->get_front();
        while (current!=nullptr) {
            if (current->value==x) {
                lista->erase(current);
                size--;
                break;
            }
            current=current->next;
        }
    }
    bool exists(int x) {
        int index=wang_hash_32(x)%capacity;
        LinkedList * lista=&array[index];
        Node *current=lista->get_front();
        while (current!=nullptr) {
            if (current->value==x) {
                return true;
            }
            current=current->next;
        }
        return false;
    }
};
```
### PROBLEM 1.2 - Open adressing

Am folosint liniar probing cu Robin Hood method. La erase am mutat valoritle din dreapta cu o pozitie mai in stanga. (am vazut ca e mai rapid decat sa pun -1?).
```cpp
class Open_hash_map {
    int size,capacity,*array,*PSL;
    unsigned int wang_hash_32(int old_x) {
        unsigned int x=(unsigned int)old_x;
        x = ((x >> 16) ^ x) * 0x45d9f3b;
        x = ((x >> 16) ^ x) * 0x45d9f3b;
        return x;
    }
    void resize() {
        int new_cap=capacity*2;
        if (capacity==0) new_cap=8;

        int *new_array=new int[new_cap];
        int *new_PSL=new int[new_cap];

        for (int i=0; i<new_cap; i++) {
            new_array[i]=0;
            new_PSL[i]=0;
        }
        size=0;
        for (int i=0;i<capacity;i++) {
            int x=array[i];
            if (x!=0)
            {
                int val=wang_hash_32(x)%new_cap;
                int pasi=0;
                while (true) {
                    ///am gasit loc liber
                    if (new_array[val]==0) {
                        new_array[val]=x;
                        new_PSL[val]=pasi;
                        size++;
                        break;
                    }
                    ///am gasit element mai bogat ->trebuie mutat
                    if (pasi>new_PSL[val]) {
                        std::swap(x,new_array[val]);
                        std::swap(pasi,new_PSL[val]);
                    }

                    val=(val+1)%new_cap;
                    pasi++;
                }
            }
        }
        delete[]array;
        delete[]PSL;
        array=new_array;
        PSL=new_PSL;
        capacity=new_cap;
    }
public:
    Open_hash_map(int cap=8):capacity{cap},size{0} {
        array=new int[capacity];
        PSL=new int[capacity];
        for (int i=0; i<capacity; i++) {
            array[i]=0; PSL[i]=0;
        }
    }
    ~Open_hash_map() {
        delete[] array;
        delete[] PSL;
    }
    void insert(int x) {
        int val=wang_hash_32(x)%capacity;
        int pasi=0;
        if (exists(x)) return ;
        if (capacity==0 || (double)size/capacity>=0.75) {
            resize();
        }
        while (true) {
            ///am gasit loc liber
            if (array[val]==0 || array[val]==-1) {
                array[val]=x;
                PSL[val]=pasi;
                size++;
                return;
            }
            ///am gasit element mai bogat ->trebuie mutat
            if (pasi>PSL[val]) {
                std::swap(x,array[val]);
                std::swap(pasi,PSL[val]);
            }

            val=(val+1)%capacity;
            pasi++;
        }
    }
    void erase(int x) {
        int val=wang_hash_32(x)%capacity;
        int pasi=0;
        while (array[val]!=0 && pasi<=PSL[val]) {
            if (array[val]==x) {
                move(val); return;
            }
            val=(val+1)%capacity;
            pasi++;
        }
        throw std::out_of_range("Nu exista cheia!");
    }
    void move(int st) {
        int curr=st;
        int next=(st+1)%capacity;

        while (array[next]!=0 && PSL[next]>0) {
            array[curr]=array[next];
            PSL[curr]=PSL[next]-1;
            curr=next;
            next=(next+1)%capacity;
        }

        array[curr]=0;
        PSL[curr]=0;
        size--;
    }
    bool exists(int x) {
        int val=wang_hash_32(x)%capacity;
        int old_val=val,pasi=0;
        while (true) {
            if (array[val]==x) return true;
            if (array[val]==0 || PSL[val]<pasi) return false;
            val=(val+1)%capacity;
            pasi++;
            if (val==old_val) return false;
        }
    }
};
```

### PROBLEM 1.3 - hash n cuvinte de lungimi egale

```cpp
///am folosit zobrist hashing
#include <iostream>
#include <string>
#include <random>
#include <vector>
#define nmax 100000
using namespace std;

string s[nmax];
int n,m;
long long a[nmax][256]; ///facem pe caracterele din tabelul ascii
vector<long long> v;
long long zobrist_hashing(string &s) {
    long long h=0;
    for (int i=0;i<s.size();i++) {
        cout<<(unsigned char)(s[i])<<' ';
        h=h^a[i][(unsigned char)s[i]];
    }
    return h;
}
void init_hashing() {
    random_device rd;
    mt19937 gen(rd());
    long long maxx=LLONG_MAX;
    int len=s[0].size();
    uniform_int_distribution<long long> distr(1, maxx);
    for (int i=0;i<nmax;i++) {
        for (int j=0;j<256;j++) {
            a[i][j]=distr(gen);
        }
    }
}
int main() {
    cin>>n;
    for (int i=0;i<n;i++) {
        cin>>s[i];
    }
    init_hashing();
    for (int i=0;i<n;i++) {
        v.push_back(zobrist_hashing(s[i]));
    }

    for (int i=0;i<n;i++) {
        cout<<v[i]<<" ";
    }
    return 0;
}
```

### PROBLEM 2.1 - 3SUM

https://leetcode.com/problems/3sum/submissions/1980324194

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> rasp;
        int len=nums.size(),j,k,suma;
        sort(nums.begin(),nums.end());
        for(int i=0; i<len-2;i++)
        {
            if(i>0 && nums[i]==nums[i-1]) continue;

            j=i+1;
            k=len-1;
            while(j<k)
            {
                suma=nums[i]+nums[j]+nums[k];
                if(suma==0 && i!=j && i!=k && j!=k)
                {
                    vector<int> crt={nums[i],nums[j],nums[k]};
                    rasp.push_back(crt);
                    while (j<k && nums[j]==nums[j+1]) j++;
                    while (j<k && nums[k]==nums[k-1]) k--;

                    j++;
                    k--;
                }
                else if(suma>0)k--;
                else j++;


            }

        }
        return rasp;
    }
};
```

### Problem 2.2

https://www.infoarena.ro/job_detail/3356515
```cpp
#include <bits/stdc++.h>
using namespace std;

ifstream f("secv5.in");
ofstream g("secv5.out");

int n,l,u,cnt=1;
vector<long long> arr;
vector<int> arr_norm;
unordered_map<long long,int> id;
long long cauta_secventa(int x) {
    vector<int> frecv(cnt+1, 0);
    int dist_cnt=0;
    long long final_ans=0;
    int st=0;
    for (int i=0;i<n; i++) {
       // cout<<final_ans<<" ";
        if (!frecv[arr_norm[i]]) dist_cnt++;
        frecv[arr_norm[i]]++;

        while (dist_cnt>=x && st<=i) {
            final_ans+=(n-i);
            frecv[arr_norm[st]]--;
            if (frecv[arr_norm[st]]==0) dist_cnt--;
            st++;
        }
    }
    return final_ans;
}
int main() {
    f>>n>>l>>u;
    arr.resize(n);
    arr_norm.resize(n);
    for(int i=0;i<n;i++) {
        f>>arr[i];
        if (id[arr[i]]==0) id[arr[i]]=cnt++;
        arr_norm[i]=id[arr[i]];
    }

    g<<cauta_secventa(l)-cauta_secventa(u+1);
}
```

### Problem 2.4

```cpp 
#include <iostream>
#include <vector>
#include <random>
#include <chrono>
using namespace std;

mt19937_64 rng(chrono::steady_clock::now().time_since_epoch().count());
vector<long long>a;
vector<unsigned long long> h,pref_a,pref_perm;
int main() {
    int n, q;
    cin>>n>>q;

    a.resize(n);
    long long max_val = 0;
    for (int i=0; i<n; i++) {
        cin >> a[i];
        if (a[i]>max_val) {
            max_val=a[i];
        }
    }


    long long limit=max(max_val, (long long)n) + 1;
    h.resize(limit);
    for (int i=0; i<limit; i++) {
        h[i]=rng();
    }


    pref_a.resize(n+1, 0);
    for (int i=0; i<n; i++) {
        pref_a[i+1]=pref_a[i]+h[a[i]];
    }


    pref_perm.resize(limit+1, 0);
    for (int i=0; i<limit; i++) {
        pref_perm[i+1]=pref_perm[i]+h[i];
    }

    for (int i=0; i<q; i++) {
        int l, r;
        cin>>l>>r;

        int k=r-l;
        unsigned long long current_hash=pref_a[r+1]-pref_a[l];
        unsigned long long target_hash=pref_perm[k+1];

        if (current_hash==target_hash) {
            cout << "DA\n";
        } else {
            cout << "NU\n";
        }
    }

    return 0;
}```

### Problem 2.5 - subsecvente 

https://www.infoarena.ro/job_detail/3351121?action=view-source

```cpp
#include <iostream>
#include <string>
#include <fstream>
#include <vector>
#include <algorithm>
#define nmax 6
using namespace std;
ifstream f("subsecvente.in");
ofstream g("subsecvente.out");
int n;
string s,v[6];

bool check(int nr) {
    vector<unsigned long long > array[5];
    unsigned long long mask=(1ULL<<nr)-1;
    for (int i=0;i<n;i++) {
        if (nr>v[i].size()) return false;
            
        unsigned long long val=0;
        for (int k=0;k<nr;k++)
            val=(val<<1) | (v[i][k]-'0');
        array[i].push_back(val);

        for (int k=nr;k<v[i].size();k++) {
            val=(val<<1) & mask;
            val=val | (v[i][k]-'0');
            array[i].push_back(val);
        }
        sort(array[i].begin(),array[i].end());
    }
    for (int i=0;i<array[0].size();i++) {
        bool ok_in_toate=true;
        for (int k=1;k<n;k++) {
            if (!binary_search(array[k].begin(), array[k].end(), array[0][i])) {
            ok_in_toate=false; break;
            }
        }
        if (ok_in_toate) { return true;}
    }
    return false;
}
int main() {
    f>>n;
    for (int i = 0; i < n; i++) {
        f>>s;
        for (int j=0;j<s.size();j++)
            if (s[j]=='a') v[i]+='0';
            else v[i]+='1';
    }
    ///cautam binar raspunsul
    int ans=0;
    int st=1,dr=62;
    while (st<=dr) {
        int mid=(st+dr)/2;
        if (check(mid)) {
            ans=mid;
            st=mid+1;
        }
        else dr=mid-1;
    }
    g<<ans;
    return 0;
}
```
## Bonus

### Problem 1.4 - perfect hashing
### Problem 2.3 - LFU cache





