# [Trang chủ](https://ppap-1264589.github.io/interesting-solution)

## Bài Toán
[DQUERY](https://oj.vnoi.info/problem/dquery)

## Tài liệu

[VNOI - Persistent Segment Tree](https://vnoi.info/wiki/algo/data-structures/persistent-data-structures.md#2-persistent-it)

## Thuật toán

[QUORA](https://www.quora.com/Is-there-any-way-to-solve-the-dquery-problem-on-SPOJ-using-persistent-segment-trees-an-online-solution)

# [Code](https://ideone.com/qlu6x0)
```c++
#include <bits/stdc++.h>
#define up(i,a,b) for (int i = (int)a; i <= (int)b; i++)
#define ep emplace_back
using namespace std;

const int maxn = 2e5 + 10;
const int LOG = log2(maxn)+2;
struct Node{
    int l = 0, r = 0;
    int sum = 0;
};
Node T[maxn*LOG];

int n,q;
int cnt = 0;
int ROOT[maxn];
int a[maxn];

void build(int& nod, int l, int r){
    nod = ++cnt;
    if (l == r){
        T[nod].sum = 0;
        return;
    }
    int mid = (l+r) >> 1;
    build(T[nod].l, l, mid);
    build(T[nod].r, mid+1, r);
}

void push_up(int nod){
    T[nod].sum = T[T[nod].l].sum + T[T[nod].r].sum;
}

void update(int& nod, int prev_nod, int l, int r, int pos, int val){
    nod = ++cnt;
    T[nod] = T[prev_nod];
    if (l == r){
        T[nod].sum += val;
        return;
    }

    int mid = (l+r) >> 1;
    if (pos <= mid) update(T[nod].l, T[prev_nod].l, l, mid, pos, val);
    else update(T[nod].r, T[prev_nod].r, mid+1, r, pos, val);
    push_up(nod);
}

int query(int nod, int l, int r, int u, int v){
    if (l > v || r < u) return 0;
    if (l >= u && r <= v) return T[nod].sum;
    int mid = (l+r) >> 1;
    int L = query(T[nod].l, l, mid, u, v);
    int R = query(T[nod].r, mid+1, r, u, v);
    return L+R;
}

int pre[1000001];
signed main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    #define Task "DQUERY"
    if (fopen(Task".inp", "r")){
        freopen(Task".inp", "r", stdin);
        freopen(Task".out", "w", stdout);
    }

    cin >> n;
    up(i,1,n) cin >> a[i];
    build(ROOT[0], 1, n);

    up(i,1,n){
        if (pre[a[i]]){
            update(ROOT[i], ROOT[i-1], 1, n, pre[a[i]], -1);
            update(ROOT[i], ROOT[i], 1, n, i, 1);
        }
        else{
            update(ROOT[i], ROOT[i-1], 1, n, i, 1);
        }
        pre[a[i]] = i;
    }

    cin >> q;
    while (q--){
        int l, r;
        cin >> l >> r;
        cout << query(ROOT[r], 1, n, l, r) << "\n";
    }
}
```
