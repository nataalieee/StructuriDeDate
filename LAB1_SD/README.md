## Tema 1 SD - Pogar Denisa Natalia 151 

### Problem 1.1 - SumeSecv 
Am facut sume partiale.

<img width="400" height="250" alt="image" src="https://github.com/user-attachments/assets/bb3304d1-3be8-49c3-8a3b-ea82b555189e" />

```cpp
#include <iostream>
#include <vector>
using namespace std;
int n,q;
long long maxim;
vector<long long> v;
int main()
{
    cin>>n;
    v.push_back(0);
    for(int i=0;i<n;i++)
    {
        int x; cin>>x;
        v.push_back(x);
    }
    for(int i=1;i<=n;i++) v[i]+=v[i-1];
    cin>>q;
    for(int i=0;i<q;i++)
    {
        int l,r; cin>>l>>r;
        if(l>r) swap(l,r);
        if (i==0) maxim=v[r]-v[l-1];
        else
        {
            long long rasp=v[r]-v[l-1];
            if (rasp>maxim)  maxim=rasp; 
        }
    }
    cout<<maxim<<'\n';
    return 0;
} 
```
### Problem 1.2 -  Two Sum II

Pornim cu i din stanga, cu j din dreapta si cand gasim v[i]+v[j]==s ne oprim. 

<img width="315" height="175" alt="image" src="https://github.com/user-attachments/assets/9d5c384a-158b-4f1f-b3a6-856b005881a7" />

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int i=0,j=numbers.size()-1;
        bool gasit=0;
        while(!gasit)
        {
            if (numbers[i]+numbers[j]<target) i++;
            else if (numbers[i]+numbers[j]>target) j--;
            else
            {
                vector<int> v;
                v.push_back(i+1);
                v.push_back(j+1);
                return v;
            }
        }
        return {}; ///nu mergea fara returnul asta
    }
};
```

###  Problem 1.3 - Search in Matrix II

Ne gandim la cautare ca la un drum in matrice. Datorita sortarii lor, mereu ce e "in jos" e mai mare, ce e "in sus" e mai mic, ce e "in drepata" e mai mare si ce e "in stanga" e mai mic. Asadar, putem porni ori din coltult drepta-sus ori din coltul stanga-jos. Am ales dreapta-sus. La fiecare pas avem de ales ori sa mergem in stanga (daca nuamrul caitat e mai mic decat cel curent) ori in dreapta (daca e mai mare). Astfel, nu ne vom intoarce niciodata si de aici complexitatea O(n). Ne oprim cand "iesim din matrice".

<img width="320" height="175" alt="image" src="https://github.com/user-attachments/assets/e55d5343-ed5a-4eac-b655-8b3fbcc19768" />

```cpp
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m=matrix.size();
        int n=matrix[0].size();
        int i=0;
        int j=n-1;
        while(i<m && j>=0)
        {
            if (target>matrix[i][j]) i++;
            else if (target<matrix[i][j]) j--;
            else return true;
        }
        return false;
    }
};
```
### Problem 1.4 - Statistici de Ordine

###  Problem 2.1 - my_stl_vector 
```cpp
#include <iostream>
using namespace std;

class Vector{
    int sizee, capacity;
    int *array_ads,*new_array;
    void my_resize()
    {
        capacity*=2;
        new_array=new int[capacity];

        for (int i=0;i<sizee;i++)
            new_array[i]=array_ads[i];
        
        delete[] array_ads;
        array_ads=new_array;
    }
public:
    Vector(int cp=2)
    {
        capacity=cp; sizee=0;
        array_ads=new int[cp];
    }
    ///constructor de copiere
    Vector(Vector &other)
    {
        sizee=other.sizee;
        capacity=other.capacity;

        array_ads=new int[capacity];

        for(int i=0;i<sizee;i++)
            array_ads[i]=other.array_ads[i];
    }
    ~Vector()
    {
        delete[] array_ads;
    }
    void my_push_back(int val)
    {
        if (sizee==capacity)
            my_resize();
        array_ads[sizee++]=val;
    }
    void my_pop_back()
    {
        if(sizee>0)
            sizee--;
    }
    int get_size()
    {
        return sizee;
    }
    int get_capacity()
    {
        return capacity;
    }

    ///am supraincarcta operatorul [] ca sa putem accesa elementele
    int &operator [](int i)
    {
        return *(array_ads+i);
    }

    void &operator =(Vector &other)
    {
        ///setam noul size, si noua capacitate
        sizee=other.sizee;
        capacity=other.capacity;

        ///stergem vectorul vechi, alocam spatiu pentru cel nou
        delete[] array_ads;
        array_ads = new int[capacity];

        ///copiem elementele
        for(int i=0; i<sizee; i++)
            array_ads[i]=other.array_ads[i];
    }

};
int main()
{
    ///teste
    Vector v1,v2,v3;
    for(int i=0;i<100;i++)
      v1.my_push_back(i);
    for(int i=1;i<=500;i*=2)
      v2.my_push_back(i);
    cout<<v2.get_capacity()<<' '<<v2.get_size()<<'\n';
    v1=v2;
    int s=v1.get_size();
    for (int i=0;i<s;i++)
        cout<<v1[i]<<' ';
    return 0;
}

```
### Problem 2.2 - kinda unordered_map

### Problem 3.1 - my_linked_list

Am facut o lista simplu inlantuita si i-am adaugat diferite functionalitati. 

```cpp
#include <iostream>
using namespace std;
class Node
{ public: int value;  Node *next;};
class List
{
    Node *head=nullptr, *tail=nullptr; int length=0;
public:
    void push_back(int v)
    {
        Node *nod_nou=new Node();
        nod_nou->value=v; nod_nou->next=nullptr;

        ///daca lista era goala
        if (head==nullptr)
        {
            head=nod_nou;  tail=nod_nou;
        }
        else ///avem elemente
        {
            tail->next=nod_nou; ///legam nodurile
            tail=nod_nou;
        }
        length++;
    }
    void push_front(int v)
    {
        Node *nod_nou=new Node();
        nod_nou->value=v;
        nod_nou->next=head;///legam nodurile

        ///daca lista era goala
        if (head==nullptr)
        {
            head=nod_nou; tail=nod_nou;
        }
        else ///avem elemente
        { head=nod_nou; }
        length++;
    }
    void pop_end()
    {
        if(head==nullptr) return ;
        length--;
        if (head==tail)
        {
            delete tail;
            tail=nullptr; head=nullptr;
            return;
        }

        Node *current= head;
        while(current->next!=tail)  current=current->next;
        delete tail;
        current->next=nullptr;
        tail=current;

    }
    void pop_front()
    {
        if (head==nullptr) return ;

        length--;
        Node *current=head;
        head=current->next;
        delete current;

        if(head==nullptr) tail=nullptr; ///daca ramane lista goala
    }
   ///aici am facut o functie care returneaza efectiv nodul 
    Node *find_by_val(int v)
    {
        Node *current=head;

        while (current!=nullptr)
        {
            if(current->value==v)
                return current;
            current=current->next;
        }
        return nullptr;
    }
    ///aici returnez pozitia valorii cautate
    int find_pos_by_value(int v)
    {
        int poz=1;
        Node *current=head;

        while (current!=nullptr)
        {
            if(current->value==v)
                return poz;
            current=current->next;
            poz++;
        }
        return -1;
    }
    ///aici returnez valoarea pozitiei
    int find_by_index(int p)
    {
        if(p>length || p<=0) return -1;///it was suppposed to be an error

        Node *current=head;
        int cnt=1;
        while(cnt<p)
        {
            current=current->next;
            cnt++;
        }
        return current->value;
    }
   ///aici inseram o valoare pe pozitia p
    void insert_pos(int p, int v)
    {
        if (p<=0 || p>length+1) return;
        if(p==1) push_front(v); return;
        if (p==length+1) push_back(v); return;
        
        int cnt=1;
        Node *current=head;

        while(cnt<p-1)
        {
            current=current->next;
            cnt++;
        }

        Node *nod_nou=new Node();
        nod_nou->value=v;
        nod_nou->next=current->next;
        current->next=nod_nou;
        length++;
    }
    ///aici stergem valoarea de pe pozitia p
    void erase_pos(int p)
    {
        if (p<=0 || p>length) return;
        if(p==1) pop_front(); return; 
        if (p==length) pop_end(); return;

        int cnt=1;
        Node *current,*prev=head;

        while(cnt<p-1)
        {
            prev=prev->next;
            cnt++;
        }
        current=prev->next;
        prev->next=current->next;
        delete current;
        length--;
    }
    ///am facut si o functie de checkuit existenta
    bool found(int v)
    {
        if (head==nullptr) return 0;
        Node *current=head;

        while(current!=nullptr)
        {
            if (current->value==v)
            return 1;
            current=current->next;
        }

        return 0;
    }
   ///si afisat
    void print_list()
    {
        if (head==nullptr) return;

        Node *current=head;
        while(current!=nullptr)
        {
            cout<<current->value<<' ';
            current=current->next;
        }

    }

};
int main()
{
    List l;
    l.push_back(10); l.push_back(20); l.push_front(5);
    l.insert_pos(2, 7);
    cout << "Initial: "; l.print_list(); cout << "\n";
    cout << "Index 3: " << l.find_by_index(3) << "\n";
    cout << "Pozitie 20: " << l.find_pos_by_value(20) << "\n";
    cout << "Gasit 7? " << l.found(7) << "\n";
    cout << "Gasit 99? " << l.found(99) << "\n";
    l.pop_front();
    l.pop_end();
    cout << "Dupa pop capete: "; l.print_list(); cout << "\n";
    l.insert_pos(2, 15);
    l.erase_pos(2);
    cout << "Dupa erase mijloc: "; l.print_list(); cout << "\n";
    Node* adresa = l.find_by_val(7);
    if(adresa) cout << "Adresa nod 7: " << adresa << "\n";
    l.erase_pos(1);
    cout << "Final (ar trebui goala): "; l.print_list(); cout << "\n";
    for(int i = 1; i <= 5; i++) l.push_back(i * 10);
    cout << "Reumplere: "; l.print_list(); cout << "\n";

    return 0;
}
```

### Problem 3.2 - ciclu in lista 

### Problem 4.1 - Candies

### Problem 4.2 - Square root of n

Am gasit algoritmul lui Newton-Raphson care calculeaza radacina patrata in log(log n). Practic noi cautam "latura unui patrat" cu aria <= n si la fiecare pas facem media aritmetica dintre  x-ul ghicit si n/x, astfel apropiindu-ne de rezultat. Partea matematica spune ca la fiecare pas se dubleaza numarul de biti corecti. 
CPU -> aprox 6/10 iteratii pt numere de 64 biti => aprox 70 milioane de square roots/sec
GPU -> prob 15 miliarde/sec 

```cpp
#include <iostream>
using namespace std;
long long n;
double x,y;
int main() {
    long long n;
    cin>>n;
    if (n <= 1 ) { cout << n; return 0; }
    ///facem prima incercare
    x=n; 
    y=(x+n/x)/2.0;
    ///aici mergem pana cand urmatoarea aproximare este strict mai mica decat precedenta
    ///din cauza memoriei limitate, aproximarea nu va merge pana la infinit (avem numar fix de zecimale), iar x si y vor ajunge egale.
    while (y < x)
    { 
        x=y; y=(x+n/x)/2.0;
    }
    cout << (long long)x << "\n";
    return 0;
}
```

## Bonus 

### 1. LRU Cache

### 2. Find Any Duplicate

Am primit hintul de la colegii mei cu iepurele si testoasa :))
