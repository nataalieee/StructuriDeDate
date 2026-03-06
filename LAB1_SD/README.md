## Tema 1 SD - Pogar Denisa Natalia 151 

### Problem 1.1 - SumeSecv 
Am facut sume partiale.

<img width="400" height="250" alt="image" src="https://github.com/user-attachments/assets/bb3304d1-3be8-49c3-8a3b-ea82b555189e" />

``` #include <iostream>
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

```class Solution {
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

```
class Solution {
public:
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
```
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

### Problem 3.2 - ciclu in lista 

### Problem 4.1 - Candies

### Problem 4.2 - Square root of n

## Bonus 

### 1. LRU Cache

### 2. Find Any Duplicate

Am primit hintul de la colegii mei cu iepurele si testoasa :))
