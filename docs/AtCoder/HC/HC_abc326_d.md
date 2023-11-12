---
title: 1371 文字パズル
layout: default
parent: 実装系
grand_parent: AtCoder
---
<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# ABC322 D問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc326/tasks/abc326_d" target="_blank">D - ABC Puzzle</a>

## 解説

ABC..の並び替えは全部で60通りなので、ナイーブな実装を行うと盤面を作成するのに\\(60^5\\)程度の計算量が必要になり、さらに判定で\\(5^2\\)回くらい必要となります。
これでは制限時間何に解くことが困難に思えます。

そこで60通りの内訳を考察してみます。.を除いて考えたときにAが最も左にくるもの、Bが最も左にくるもの、Cが最も左にくるものという3つのグループに分けます。

そうすることで、rの文字によってどのグループから選択すべきかを決定することができ、盤面の作成部分が\\(20^5\\)程度の計算となり、判定の部分と合わせてこれは制限時間に動作します。

以下コードになります。

```cpp
bool check(vector<string>& ans, string& c, int n){
    string str = "ABC";
    while(str.size() < n) str += '.';
    sort(str.begin(), str.end());
    rep(i, n){
        // 文字の個数判定
        string col = "";
        rep(j, n) col += ans[j][i];
        sort(col.begin(), col.end());
        if(col != str) return false;

        // 最も上にある文字が条件を満たしているか
        char c_check;
        rep(j, n) if(ans[j][i] != '.'){
            c_check = ans[j][i];
            break;
        }
        if(c_check != c[i]) return false;
    }
    return true;
}
int main(){
    int n; cin >> n;
    string r, c; cin >> r >> c;
    
    string str = "ABC";
    while(str.size() < n) str += '.';
    sort(str.begin(), str.end());

    vector<vector<string>> row(3);
    do{
        char tmp = '.';
        for(auto x : str){
            if(x != '.'){
                tmp = x;
                break;
            }
        }
        row[tmp - 'A'].push_back(str);
    }while(next_permutation(str.begin(), str.end()));

    int p = n*(n-1)*(n-2)/3;
    int max = 1;
    rep(i, n) max *= p;
    
    int cnt = 0;
    while(cnt < max){
        vector<int> dig(n);
        int tmp = cnt;
        rep(i, n){
            dig[i] = tmp % p;
            tmp /= p;
        }

        vector<string> ans(n);
        rep(i, n) ans[i] = row[r[i] - 'A'][dig[i]];

        if(check(ans, c, n)){
            cout << "Yes" << endl;
            rep(i, n) cout << ans[i] << endl;
            return 0;
        }
        cnt++;
    }
    cout << "No" << endl;
    return 0;
}
```

20通りのうち利用するものを指定する配列はdigで、20進数によって管理しています。
