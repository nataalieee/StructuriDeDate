
## Tema 5 SD - Pogar Denisa Natalia 151 

### Problem 1.1 

Am folosit varianta cu vector de noduri.

```cpp
struct Node
{
    int final_cnt, inner_cnt, fiu[60];

    Node():final_cnt{0}, inner_cnt{0}
    {
        for(int i=0; i<60; i++)
            fiu[i]=-1;
    }

};
class Trie
{
    vector<Node> trie;
    int char_to_int(char c)
    {
        if (c>='a' && c<='z') return c-'a';
        if (c>='A' && c<='Z') return c-'A'+26;
        return 0;
    }
public:
    Trie()
    {
        trie.push_back(Node());
    }
    void insert(string cuv)
    {
        int poz=0;
        trie[poz].inner_cnt++;

        for(int i=0; i<cuv.size(); i++)
        {
            int val=char_to_int(cuv[i]);
            if(trie[poz].fiu[val]==-1)
            {
                trie.push_back(Node());
                trie[poz].fiu[val]=trie.size()-1;
            }
            int dest=trie[poz].fiu[val];
            trie[dest].inner_cnt++;
            poz=dest;

        }
        trie[poz].final_cnt++;
    }
    void erase(string cuv)
    {
        int poz=0;
        trie[poz].inner_cnt--;
        for(int i=0; i<cuv.size(); i++)
        {
            int val=char_to_int(cuv[i]);
            int dest=trie[poz].fiu[val];
            trie[dest].inner_cnt--;
            if(trie[dest].inner_cnt==0)
                trie[poz].fiu[val]=-1;

            poz=dest;

        }
        trie[poz].final_cnt--;
    }
    int longest_com_prefix(string cuv)
    {
        int poz=0;
        int lung=0;
        for(int i=0; i<cuv.size(); i++)
        {
            int val=char_to_int(cuv[i]);
            if(trie[poz].fiu[val]==-1)
            {
                return lung;
            }
            lung++;
            int dest=trie[poz].fiu[val];
            poz=dest;

        }
        return lung;
    }
    int count(string cuv)
    {
        int poz=0;

        for(int i=0; i<cuv.size(); i++)
        {
            int val=char_to_int(cuv[i]);
            if(trie[poz].fiu[val]==-1)
            {
                return 0;
            }
            int dest=trie[poz].fiu[val];
            poz=dest;

        }
        return trie[poz].final_cnt;
    }
};
```
### Problem 1.2
### Problem 1.3

### Problem 1.5
### Problem 1.6
## Bonus
### Problem 1.4