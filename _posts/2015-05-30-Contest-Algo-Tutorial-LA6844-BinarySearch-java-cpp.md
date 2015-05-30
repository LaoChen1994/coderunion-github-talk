---
layout: post
title: "一波三折而又苦尽甘来的数学题上的二分查找的算法竞赛应用"
date: 2015-05-30 17:00:00
tag: 
- ACM-ICPC
- semprathlon
categories: acm
---

* content
{:toc}

<embed width="100%" height="600" name="plugin" src="https://icpcarchive.ecs.baylor.edu/external/68/6844.pdf" type="application/pdf" internalinstanceid="9"/>  
[原题页面 ACM-ICPC Live Archive Regionals 2014 >> Asia - Bangkok 6844 - Combination](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&category=637&page=show_problem&problem=4856)   
====
[PDF题面](https://icpcarchive.ecs.baylor.edu/external/68/6844.pdf)   
[Vjudge提交地址](http://acm.hust.edu.cn/vjudge/problem/viewProblem.action?id=79610)   

*艰辛坎坷的探索历程*  
 
* 设 $ {f(n)=C_{n}^{r}(n∈N^{ * },r∈[0,n])} $中 奇数的个数。  
* 题意是说，求出 $ \sum f(k),k∈[low,high] $  
* 庞大的数据，$ n≤16 * {10^{11}}. $  

step1 
----
判断给定$ n∈N^{ * } $时$ C_n^r(r∈[0,n]) $的奇偶性。  不难往n,r的二进制表示方向考虑。
> 查阅他人博客获得结论：$ n{ \& }r==r $时$ C_n^r $为奇数。  

然而本题中无需直接应用这一“定理”，所要做的是统计。

> 拿出纸笔推算发现，若n的二进制表示中有d个1，则$ C_n^r(r∈[0,n]) $中存在$ 2^d $个奇数。

step2 
----
研究 $ f(n) $ 的递推关系及通项。  编写暴力程序 $ O(n^2) $ ，打表观察 $ n∈[0,64) $ 时 $ f(n) $ 的取值：  
   1   
   2   
   2    4   
   2    4    4    8   
   2    4    4    8    4    8    8   16   
   2    4    4    8    4    8    8   16    4    8    8   16    8   16   16   32   
   2    4    4    8    4    8    8   16    4    8    8   16    8   16   16   32    4    8    8   16    8   16   16   32    8   16   16   32   16   32   32   64   
第一行 $ f(0)=1 $ 作为边界条件， 第二行 $ f(1)=2 $ ，随后各行的第k行的 $ 2^{k-2} $ 个数分别表示 $ f(2^{k-2}),f(2^{k-2}+1),...,f(2^{k-1}-1) $ 。
通过不懈的努力，观察发现从第3行开始，每行的前半部分与前一行完全一致，每行的后半部分的各个f值恰为前半部分对应位置的f值的2倍。

step3
----
出于数列直觉，我们尝试求解各行的和构成的数列的递推以及通项。  
$ a_1=1 $  
$ a_2=2 $  
$ a_3=a_2+2 * {a_2}=3 * {a_2}=6 $  
...  
$ a_n=a_{n-1}+2 * {a_{n-1}}=3 * {a_{n-1}}(n\geq 3) $（递推公式）  
$ \begin{equation} a_n= \begin{cases} 1 &\mbox{$n=1$}\newline 2 &\mbox{$n=2$}\newline 2 * 3^{n-2} &\mbox{$n\geq 3$} \end{cases} \end{equation}（通项公式） $  
$ 
\begin{equation} S_n=\sum\limits_{i=1}^{n} {a_i}= \begin{cases} 1 &\mbox{$n=1$}\newline 3 &\mbox{$n=2$}\newline 3+6 * \frac{1-3^{n-2}}{1-3}=3+3 * {(3^{n-2}-1)}=3^{n-1} &\mbox{$n\geq 3$} \end{cases} \end{equation}（a_n的前n项和的公式）  
$  
化简后上面这个公式样子够好看了吧？ 


step4
----
再看一下第2步发现的规律，应用“二分”手段高效地求解，而且真的可以二分求解。
具体地说，就是先置求解区间 $ [l,r) $ (左闭右开)的端点于每行的头尾( $ l=2^{i-2},r=2^{i-1} $ )，利用上一步所得公式计算该区间的 $ 左端点前缀和U=\sum\limits_{k=1}^{2^{i-2}} {f(k)} $ 与 $ 右端点前缀和V=\sum\limits_{k=1}^{2^{i-1}-1} {f(k)} $ 后，不断地对区间折半，总可以确定新区间的左端点前缀和与右端点前缀和。  
当区间缩至一点时，$ U==V==f(k) $，即能得到我们所求，而无需计算出[1..n]的每个函数值。

step5
----
以上所解决的并不是最终答案，而是前缀和 $ \sum\limits_{k=1}^{n} {f(k)} $ ；
不过走到这一步已经很好办了，$ ans=\sum\limits_{k=low}^{high} {f(k)}=\sum\limits_{k=1}^{high} {f(k)}-\sum\limits_{k=1}^{low-1} {f(k)} $  

> PS:写完代码后，用极大的n值测试一下，事实上$ \sum\limits_{k=1}^{n} {f(k)} $已经超出了int64的范围。其他的细节不必多说了。

Java:
```
/**
 * @date 2015-05-30
 * @author Semprathlon
 */
import java.util.*;
import java.math.*;

public class Main {
    static int maxn=42;
    static BigInteger[] sum=new BigInteger[maxn];
    
    static int found_pow2(long n){
        return (int)(Math.log((double)n)/Math.log(2.0));
    }

    static void init(){
        for(int i=0;i<maxn;i++)
            sum[i]=new BigInteger("3").pow(i).add(new BigInteger("2"));
    }
    static BigInteger Bisearch(long l,long r,BigInteger ls,BigInteger rs,long key){
        while(l+1L<r){
            long mid=(l+r)>>1L;
            if(key<mid){
                r=mid;
                rs=ls.add(rs.subtract(ls).divide(new BigInteger("3")));
            }
            else{
                l=mid;
                ls=ls.add(rs.subtract(ls).divide(new BigInteger("3")));
            }
        }
        return ls;
    }
    static BigInteger solve(long n){
        if (n<0L) return BigInteger.ZERO;
        if (n==0L) return BigInteger.ONE;
        int k=found_pow2(n+1);
        BigInteger ls=sum[k];
        BigInteger rs=sum[k+1];
        return Bisearch(1L<<k,1L<<(k+1),ls,rs,n+1).subtract(BigInteger.valueOf(2));
    }
    public static void main(String[] args) {
        init();
        Scanner scan=new Scanner(System.in);
        while(scan.hasNextLong()){
            long n=scan.nextLong();
            long m=scan.nextLong();
            if (n==0L&&m==0L) break;
            System.out.println(solve(m).subtract(solve(n-1)));
        }
    }
}
```

0.279s AC cpp代码：  
```
#include<cmath>
#include<cstdio>
#include<cstring>
#include<iostream>

using namespace std;

typedef long long LL;
typedef unsigned long long ULL;
const int maxn = 42;
const LL mod = 1e8;
const int digit = 8;
struct LongInt
{
    LL data[3];
    LongInt()
    {
        data[0] = data[1] = data[2] = 0;
    }
    LongInt(LL n)
    {
        data[0] = n % mod;
        n /= mod;
        data[1] = n % mod;
        n /= mod;
        data[2] = n;
    }
    LongInt operator= (const LL num)
    {
        LL n = num;
        data[0] = n % mod;
        n /= mod;
        data[1] = n % mod;
        n /= mod;
        data[2] = n;
        return *this;
    }
    LongInt operator+ (const LL& num)
    {
        LL n = num;
        LongInt tmp = *this;
        tmp.data[0] += n;
        tmp.data[1] += tmp.data[0] / mod;
        tmp.data[0] %= mod;
        tmp.data[2] += tmp.data[1] / mod;
        tmp.data[1] %= mod;
        return tmp;
    }
    LongInt operator+ (const LongInt& b)
    {
        LongInt tmp = *this;
        tmp.data[0] += b.data[0];
        tmp.data[1] += b.data[1];
        tmp.data[2] += b.data[2];
        tmp.data[1] += tmp.data[0] / mod;
        tmp.data[0] %= mod;
        tmp.data[2] += tmp.data[1] / mod;
        tmp.data[1] %= mod;
        return tmp;
    }
    LongInt operator- (const LongInt& b)
    {
        LongInt tmp = *this;
        tmp.data[0] -= b.data[0];
        if (tmp.data[0] < 0)
        {
            tmp.data[1]--;
            tmp.data[0] += mod;
        }
        tmp.data[1] -= b.data[1];
        if (tmp.data[1] < 0)
        {
            tmp.data[2]--;
            tmp.data[1] += mod;
        }
        tmp.data[2] -= b.data[2];
        return tmp;
    }
    LongInt operator* (const LL num)
    {
        LL n = num;
        LongInt tmp = *this;
        tmp.data[0] *= num;
        tmp.data[1] *= num;
        tmp.data[2] *= num;
        tmp.data[1] += tmp.data[0] / mod;
        tmp.data[0] %= mod;
        tmp.data[2] += tmp.data[1] / mod;
        tmp.data[1] %= mod;
        return tmp;
    }
    LongInt operator*= (const LL num)
    {
        *this = *this * num;
        return *this;
    }
    LongInt operator/ (const LL num)
    {
        LongInt tmp = *this;
        tmp.data[1] += tmp.data[2] % num * mod;
        tmp.data[2] /= num;
        tmp.data[0] += tmp.data[1] % num * mod;
        tmp.data[1] /= num;
        tmp.data[0] /= num;
        return tmp;
    }
    void fillzero(int n)
    {
        char str[digit * 2];
        sprintf(str, "%d", n);
        for (int i = 1; i <= digit - strlen(str); i++)
        {
            printf("%d", 0);
        }
        printf("%s", str);
    }
    void print()
    {
        if (data[2] > 0)
        {
            printf("%d", data[2]);
            fillzero(data[1]);
            fillzero(data[0]);
        }
        else if (data[1] > 0)
        {
            printf("%d", data[1]);
            fillzero(data[0]);
        }
        else
        {
            printf("%d", data[0]);
        }
    }
};

const LL pow2[maxn] = {1, 2, 4, 8, 16, 32, 64, 128,
                       256, 512, 1024, 2048, 4096, 8192, 16384, 32768,
                       65536, 131072, 262144, 524288, 1048576, 2097152, 4194304, 8388608,
                       16777216, 33554432, 67108864, 134217728, 268435456, 536870912, 1073741824, 2147483648,
                       4294967296, 8589934592, 17179869184, 34359738368, 68719476736, 137438953472, 274877906944, 549755813888,
                       1099511627776, 2199023255552
                      };
/*const ULL sum[maxn] = {3, 5, 11, 29, 83, 245, 731, 2189,
                       6563, 19685, 59051, 177149, 531443, 1594325, 4782971, 14348909,
                       43046723, 129140165, 387420491, 1162261469, 3486784403, 10460353205, 31381059611, 94143178829,
                       282429536483, 847288609445, 2541865828331, 7625597484989, 22876792454963, 68630377364885, 205891132094651, 617673396283949,
                       1853020188851843, 5559060566555525, 16677181699666571, 50031545098999709, 150094635296999123, 450283905890997365, 1350851717672992091, 4052555153018976269,
                       12157665459056928803
                      };*/
LongInt sum[maxn];

int found_pow2(ULL n)
{
    return int(log(n) / log(2));
}

LongInt Bisearch(LL l, LL r, LongInt ls, LongInt rs, LL key) // [ l , r ) 
{
    while (l + 1 < r)
    {
        ULL mid = (l + r) >> 1;
        if (key < mid)
        {
            r = mid;
            rs = (rs - ls) / 3 + ls;
        }
        else
        {
            l = mid;
            ls = (rs - ls) / 3 + ls;
        }
    }
    return ls;
}

LongInt solve(LL n)
{
    if (n < 0)
    {
        return 0;
    }
    if (!n)
    {
        return 1;
    }
    int k = found_pow2(n + 1);
    LongInt ls = sum[k];
    LongInt rs = sum[k + 1];
    return Bisearch(pow2[k], pow2[k + 1], ls, rs, n + 1) - 2;
}

void init()
{
    LongInt tmp = 1;
    for (int i = 0; i < maxn; i++)
    {
        sum[i] = tmp + 2;
        tmp *= 3;
    }
}

int main()
{
    init();
    LL n, m;
    while (cin >> n >> m)
    {
        if (!n && !m)
        {
            break;
        }
        LongInt ans = solve(m) - solve(n - 1);
        ans.print();
        puts("");
    }
    return 0;
}
```
