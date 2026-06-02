## Tema 4 SD - Pogar Denisa Natalia 151 

### Problem 1.1 - BST

```cpp
struct Node{
    int key, priority;
    Node *left, *right;
    Node(int val):key{val},left{nullptr},right{nullptr}{
        static std::random_device rd;
        static std::mt19937 mt(rd());

        priority = mt();
    }
};
class Treap{
    Node* root;

    ///fiul stang urca
    Node* rotate_right(Node* current)
    {
        Node* left_child=current->left;
        current->left=left_child->right;
        left_child->right=current;
        return left_child;
    }

    ///fiul drept urca
    Node* rotate_left(Node* current)
    {
        Node* right_child=current->right;
        current->right=right_child->left;
        right_child->left=current;
        return right_child;
    }
    Node* insert(Node* node, int val)
    {
        ///daca e gol
        if(node==nullptr)
            return new Node(val);

        if(val<node->key)
        {
            node->left=insert(node->left, val);
            ///reechilibram la nevoie -> prop de heap
            if(node->left->priority > node->priority)
                {
                    node=rotate_right(node);
                }
        }
        else if(val>node->key)
        {
            node->right=insert(node->right, val);
            ///reechilibram la nev ->prop de heap
            if(node->right->priority > node->priority)
            {
                node=rotate_left(node);
            }
        }
        return node;
    }
    void private_construct(vector<int>& vals)
    {
        for(int x:vals)
            root=insert(root, x);
    }

    Node* erase(Node* node, int val)
    {
        ///nu exista
        if(node==nullptr) return nullptr;

        if(val < node->key)
            node->left=erase(node->left, val);
        else if(val > node->key)
            node->right=erase(node->right, val);

        else ///l-am gasit
        {
            ///0 copii
            if(node->left==nullptr && node->right==nullptr)
            {
                delete node; return nullptr;
            }

            /// 1 copil -> stang
            if(node->right==nullptr)
            {
                Node *aux=node->left;
                delete node; return aux;
            }
            ///1 copil -> dreapta
            else if(node->left==nullptr)
            {
                Node *aux=node->right;
                delete node; return aux;
            }

            ///2 fii
            else if (node->left->priority > node->right->priority)
            {
                node=rotate_right(node);
                node->right=erase(node->right, val);
            }
            else
            {
                node=rotate_left(node);
                node->left=erase(node->left, val);
            }

        }
        return node;
    }
    ///cel mai mic nod dintr-un subarbore
    Node* minimum(Node* node)
    {
        while(node->left != nullptr)
            node=node->left;
        return node;
    }

    ///cel mai mare nod dintr-un subarbore
    Node* maximum(Node* node)
    {
        while(node->right != nullptr)
            node=node->right;
        return node;
    }
    Node* successor(Node *node, int val)
    {
        Node *succ=nullptr;

        while(node!=nullptr)
        {
            if(val<node->key)
            {
                succ=node;
                node=node->left;
            }
            else if (val>node->key)
            {
                node=node->right;
            }
            else ///am agsit fix valoarea cautata
            {
                if(node->right!=nullptr)
                    succ=minimum(node->right);
                break;
            }
        }
        return succ;
    }

    Node* predecessor(Node *node, int val)
    {
        Node *pred=nullptr;

        while(node!=nullptr)
        {
            if(val<node->key)
            {
                node=node->left;
            }
            else if (val>node->key)
            {
                pred=node;
                node=node->right;
            }
            else ///am agsit fix valoarea cautata
            {
                if(node->left!=nullptr)
                    pred=maximum(node->left);
                break;
            }
        }
        return pred;
    }
public:
    Treap():root{nullptr}{}
    void insert(int x)
    {
        root=insert(root, x);
    }
    void construct(vector<int> vals)
    {
        private_construct(vals);
    }
    bool exists(int x)
    {
        Node* current=root;
        while(current!=nullptr)
        {
            if(x==current->key) return true;
            else if(x < current->key) current=current->left;
            else current=current->right;
        }
        return false;
    }
    void erase(int x)
    {
        root=erase(root,x);
    }
    void successor(int x)
    {
        Node* result=successor(root, x);
        if(result==nullptr)
            cout<<"Nu exista succesor"<< endl;
        else
            cout<<result->key<<endl;
    }
    void predecessor(int x)
    {
        Node* result=predecessor(root, x);
        if(result==nullptr)
            cout<<"Nu exista predecesor"<< endl;
        else
            cout<<result->key<<endl;
    }
};
```

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
### Problem 1.3 - Recover a BST
### Problem 1.4 – Serialize and Deserialize a BST

https://leetcode.com/problems/serialize-and-deserialize-bst/submissions/1980821842

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
  };

class Codec {
public:
    void dfs(TreeNode *current, string &code) {
        if (current == NULL) {return;}
        code+=to_string(current->val);
        code+=" ";
        dfs(current->left,code);
        dfs(current->right,code);
    }
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string rasp="";
        TreeNode* rad = root;
        dfs(root,rasp);
        return rasp;
    }
    TreeNode* dfs2(queue<int> &q, int st, int dr) {
        if (q.empty())return NULL;
        int val=q.front();

        if (val<st || val>dr) {
            return NULL;
        }
        q.pop();
        TreeNode* current = new TreeNode(val);
        current->left = dfs2(q, st, val);   // In stanga: valoare < radacina
        current->right = dfs2(q, val, dr); // In dreapta: valoare > radacina

        return current;

    }
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if (data.empty()) return NULL;
        stringstream ss(data);
        queue<int> q;
        int val;
        while (ss >> val) {
            q.push(val);
        }

        return dfs2(q, INT_MIN, INT_MAX);
    }
};
```

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

https://www.infoarena.ro/job_detail/3351151?action=view-source

```cpp
#include <vector>
#include <fstream>
#include <queue>
#include <iostream>
#include <climits>
#define nmax 50003
using namespace std;
ifstream f("dijkstra.in");
ofstream g("dijkstra.out");
typedef pair<int,int> pii;
int dist[nmax],n,m;
vector<pii> adj[nmax];
void dijkstra(int start) {
    dist[start] = 0;
    priority_queue<pii,vector<pii>,greater <pii> >pq;

    pq.push({dist[start],start});
    while(!pq.empty()) {
        int cost=pq.top().first;
        int node=pq.top().second;
        pq.pop();

        if (cost<=dist[node]) {
            for(int i=0;i<adj[node].size();i++) {
                int new_node=adj[node][i].first;
                int new_cost=adj[node][i].second;

                if (dist[new_node]>dist[node]+new_cost) {
                    dist[new_node]=dist[node]+new_cost;
                    pq.push({dist[new_node],new_node});
                }

            }
        }
    }
}
int main() {
    ///citire
    f>>n>>m;
    for (int i=0;i<m;i++) {
        int x,y,w;
        f>>x>>y>>w;
        adj[x].push_back({y,w});
    }
    for (int i=2;i<=n;i++) dist[i]=INT_MAX;
    dijkstra(1);
    for (int i=2;i<=n;i++) {
        if (dist[i]==INT_MAX) g<<0<<' ';
        else g<<dist[i]<<' ';
    }
    return 0;
}
```
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

https://leetcode.com/problems/find-median-from-data-stream/submissions/1980788053

Am folosit un max-heap pentru jumatatea de numere mai mici, si un min-heap pentru jumatatea de numere mai mari.

```cpp

class MedianFinder {
    priority_queue<int,vector<int>,greater<int>> minHeap;
    priority_queue<int> maxHeap;
public:
    MedianFinder() {

    }

    void addNum(int num) {
        if (maxHeap.empty() || num <= maxHeap.top()) {
            maxHeap.push(num);
        }
        else {
            minHeap.push(num);
        }

        if (maxHeap.size()>minHeap.size()+1) {
            int curent=maxHeap.top();
            maxHeap.pop();
            minHeap.push(curent);
        }
        else if (minHeap.size()>maxHeap.size()+1) {
            int current=minHeap.top();
            minHeap.pop();
            maxHeap.push(current);

        }
    }

    double findMedian() {
        if (minHeap.size()==0 && maxHeap.size()==0) {
            return 0.0;
        }
        if (minHeap.size()==maxHeap.size()) {
            double mediana=0;
            int c1=minHeap.top();
            int c2=maxHeap.top();
            mediana=(c1+c2)/2.;
            return mediana;
        }
        else if (maxHeap.size()>minHeap.size()) {
            int curent=maxHeap.top();
            return (double)(curent);
        }
        else {
            int curent=minHeap.top();
            return (double)(curent);
        }
    }
};


```


## Bonus 
### Problem 1.5 – RSBT – Bonus
### Problem 2.6 – Sufle
### Problem 2.7