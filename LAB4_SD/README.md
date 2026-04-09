## Tema 4 SD - Pogar Denisa Natalia 151 

### Problem 1.1 - BST
### Problem 1.2 - Validate a BST
https://leetcode.com/problems/validate-binary-search-tree/submissions/1973852842

Verificam recursiv daca nodul curent se afla in intervalul [st,dr]. Mereu cand mergem in stanga, dam update la dr cu nodul anterior, iar in dreapta la sst ca sa restrangem intervalul in care se pot gasi nodurile.

```cpp
class Solution
{
public:
    bool valid(TreeNode *current, long long minim, long long maxim)
    {
        if(current==nullptr) return true;
        if(current->val>=maxim || current->val<=minim) return false;

        return valid(current->left, minim, current->val) &&
        valid(current->right, current->val,maxim);
    }
    bool isValidBST(TreeNode* root)
    {
        return valid(root,LLONG_MIN,LLONG_MAX);
    }
};
```
### Problem 1.3 - Reciver a BST
### Problem 1.4 – Serialize and Deserialize a BST

### Problem 2.1 - Heap

```cpp

///vom face max-heap
class heap
{
    vector<int> v;
public:
    heap()
    {
        v.push_back(0);
    }
    void heapify_up(int poz)
    {
        int current=v[poz];
        int tata=v[poz/2];
        if(tata!=0 && tata<current)
        {
            swap(v[poz],v[poz/2]);
            heapify_up(poz/2);
        }


    }
    void insert(int x)
    {
        v.push_back(x);
        heapify_up(v.size()-1);
    }

    int top()
    {
        if (v.size() <= 1) throw runtime_error("Heap-ul este gol! Nu exista elemente pentru operatia top().");
        return v[1];
    }
    void heapify_bottom(int poz)
    {
        int maxim=poz,n=v.size();
        int f1=2*poz,f2=2*poz+1;
        if(f1<n && v[f1]>v[maxim]) maxim=f1;
        if(f2<n && v[f2]>v[maxim]) maxim=f2;
        if(maxim!=poz)
        {
            swap(v[poz],v[maxim]);
            heapify_bottom(maxim);
        }
    }
    void pop()
    {
        if (v.size() <= 1)
        {
            throw runtime_error("Heap-ul este gol! Nu exista elemente pentru operatia pop().");
            return;
        }

        swap(v[1],v[v.size()-1]);
        v.pop_back();
        heapify_bottom(1);

    }

};
```
### Problem 2.2 – Merge k Sorted Lists
https://leetcode.com/problems/merge-k-sorted-lists/submissions/1973808209

Am bagat primul nod din fiecare lista intr-un pq ca mereu sa-l returneze pe cle mai mic. Cand scot unul, il bag pe vecinul lui din drapta(current->next).

```cpp
class Solution
{
public:
    struct compare
    {
        bool operator()(ListNode* a, ListNode* b)
        {
            return a->val > b->val; // Atenție: '>' face ca cel mai mic să fie în vârf
        }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists)
    {
        priority_queue <ListNode*, vector<ListNode*>, compare> pq;
        for(int i=0; i<lists.size(); i++)
            if(lists[i]!=nullptr) pq.push(lists[i]);
        ///cream un nod temporar care va fi headul
        ListNode dummy(0);
        ListNode* tail=&dummy;

        while(pq.size())
        {
            ListNode *current=pq.top();
            pq.pop();
            if(current->next!=nullptr) pq.push(current->next);
            tail->next=current;
            tail=current;
        }
        return dummy.next;
    }
};
```
### Problem 2.3 – Dijkstra
### Problem 2.4 – Interclasari
https://www.pbinfo.ro/detalii-evaluare/55341236

Mereu alegem cele mai mici 2 lungimi ca suma finala sa fie minimala. 

```cpp
#include <iostream>
#include <queue>
using namespace std;

priority_queue <int,vector<int>, greater<int>> pq;
int n,val;
long long rasp;
int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>val; pq.push(val);
    }
    while(pq.size()>1)
    {
        int varf=pq.top();
        pq.pop();
        int varf2=pq.top();
        pq.pop();
        rasp=rasp+varf;
        rasp=rasp+varf2;
        pq.push(varf+varf2);
    }

    cout<<rasp;
    return 0;
}
```
### Problem 2.5 – Find Median from Data Stream



## Bonus 
### Problem 1.5 – RSBT – Bonus
### Problem 2.6 – Sufle
### Problem 2.7