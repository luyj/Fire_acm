<script type="text/javascript"
       src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
# UVA11752-Super Power

### Problem
We all know the Super Powers of this world and how they manage to get advantages in political warfare or even in other sectors. But this is not a political platform and so we will talk about a different kind of super powers — “The Super Power Numbers”. A positive number is said to be super power when it is the power of at least two different positive integers. For example 64 is a super power as 64 = ${8^2}$ and 64 = ${4^3}$. You have to write a program that lists all super powers within 1 and $2^{64} − 1$ (inclusive).

### Input
This program has no input.

### Output
Print all the Super Power Numbers within 1 and $2^{64} − 1$. Each line contains a single super power
number and the numbers are printed in ascending order.
Note: Remember that there are no input for this problem. The sample output is only a partial solution.

### Sample Input

### Sample Output
1
16
64
81
256
512
.
.
.

## 思路：
- **合数幂的数是Super Power**
- **要求求的是$2^{64}-1$内的所有Super Power**
    - **64内最小的合数是4，所以底数最大为$2^{16}$  因为$(({2^{16}})^4)$**
    - **设底数为a,指数为x,则$x^a = 2^{64}-1 \Rightarrow alogx = log(2^{64}-1) \Rightarrow a =\frac{log(2^{64}-1)}{logx}$ 所以在确定了底数之后可以确定最大的指数**
## 代码
```c
#include <iostream>
#include <string>
#include <cstring>
#include <algorithm>
#include <cmath>
#include <vector>
#include <set>
#include <cstdio>

using namespace std;
#define ll unsigned long long
vector<int>imprime;
set<ll>ans;
int vis[105];
//求出64以内的合数
void Prime()
{
    memset(vis,0,sizeof(vis));
    vis[0]=vis[1]=1;
    for(int i=2;i<=64;i++)
    {
        if(vis[i]==0)
        {
            for(int j = i*i;j<=64;j+=i)
            {
                if(vis[j]==0)
                   {
                        imprime.push_back(j);
                        vis[j]=1;
                   }
            }
        }
    }
}

int main()
{

    Prime();
    //最大的底数
    int maxd = 1<<16;
    ans.insert(1);
    sort(imprime.begin(),imprime.end());
    
    for(ll i=2;i<maxd;i++)
    {
        //在底数为i时最大的指数
        int maxz = ceil(64*log(2)/log(i))-1;
        ll t = i*i*i*i;
        ans.insert(t);
        for(int j=1;imprime[j]<=maxz;j++)
        {
            //相同指数直接在上一次的结果上乘
            for(int k = imprime[j-1]+1;k<=imprime[j];k++)
            {
                t*=i;
            }
            ans.insert(t);
        }

    }
    //输出结果
    for(set<ll>::iterator i=ans.begin();i!=ans.end();i++)
    {
        printf("%llu\n",*i);
    }
    return 0;


}
```