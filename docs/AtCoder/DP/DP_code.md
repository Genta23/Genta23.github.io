---
title: 基本問題のコード
layout: default
parent: 動的計画法
grand_parent: AtCoder
---

# 基本問題のコード
動的計画法の基本問題のコード集です。

このサイトに載っていた問題を練習として解きました。
<a href="https://qiita.com/drken/items/a5e6fe22863b7992efdb#%E5%95%8F%E9%A1%8C-8%E6%9C%80%E9%95%B7%E5%85%B1%E9%80%9A%E9%83%A8%E5%88%86%E5%88%97-lcs-%E5%95%8F%E9%A1%8C" target="_blank">典型的な DP (動的計画法) のパターンを整理 Part 1 ～ ナップサック DP 編 ～</a>

## コード集

### ナップザック問題
```cpp
ll dp[107][100007];

int main(){
    int n, w; cin >> n >> w;
    vector<pair<ll, ll>> data(n); rep(i, n) cin >> data[i].first >> data[i].second;

    rep(i, n){
        rep(j, 100007){
            if(j - data[i].first >= 0) dp[i+1][j] = max(dp[i][j - data[i].first] + data[i].second, dp[i][j]);
            else dp[i+1][j] = dp[i][j];
        }
    }

    cout << dp[n][w] << endl;
    return 0;
}
```

### 部分和問題
```cpp
bool flag[103][10003];

int main(){
    int n, m; cin >> n >> m;
    vector<int> w(n); rep(i, n) cin >> w[i];

    flag[0][0] = true;
    rep(i, n){
        rep(j, m+1){
            flag[i+1][j] = flag[i][j];
            if(j - w[i] >= 0 && flag[i][j - w[i]]) flag[i+1][j] = true;
        }
    }

    cout << (flag[n][m] ? "Yes" : "No") << endl;
    return 0;
}
```

### 部分和数え上げ問題
```cpp
int cnt[103][10003];
const int mod = 1000;

int main(){
    int n, m; cin >> n >> m;
    vector<int> a(n); rep(i, n) cin >> a[i];

    cnt[0][0] = 1;
    rep(i, n){
        rep(j, m+1){
            cnt[i+1][j] = cnt[i][j];
            if(j - a[i] >= 0){
                cnt[i+1][j] += cnt[i][j - a[i]];
                cnt[i+1][j] %= mod;
            }
        }
    }

    cout << cnt[n][m] << endl;
    return 0;
}
```

### 最小個数部分和問題
```cpp
int cnt[103][10003];

int main(){
    int n, m; cin >> n >> m;
    vector<int> a(n); rep(i, n) cin >> a[i];

    rep(i, n+1) rep(j, m+1) cnt[i][j] = inf;
    
    cnt[0][0] = 0;
    rep(i, n){
        rep(j, m+1){
            if(j - a[i] >= 0) cnt[i+1][j] = min(cnt[i][j - a[i]] + 1, cnt[i][j]);
            else cnt[i+1][j] = cnt[i][j];
        }
    }

    cout << (cnt[n][m] != inf ? cnt[n][m] : -1) << endl;
    return 0;
}
```

### k個以内部分和問題
```cpp
int cnt[503][10003];

int main(){
    int n, m, k; cin >> n >> m >> k;
    vector<int> a(n); rep(i, n) cin >> a[i];

    rep(i, n+1) rep(j, m+1) cnt[i][j] = inf;
    
    cnt[0][0] = 0;
    rep(i, n){
        rep(j, m+1){
            if(j - a[i] >= 0) cnt[i+1][j] = min(cnt[i][j - a[i]] + 1, cnt[i][j]);
            else cnt[i+1][j] = cnt[i][j];
        }
    }

    cout << (cnt[n][m] <= k ? "Yes" : "No") << endl;
    return 0;
}
```

### 個数制限付き部分和問題
```cpp
ll cnt[503][10003];

int main(){
    int n, m; cin >> n >> m;
    vector<ll> a(n), b(n); rep(i, n) cin >> a[i] >> b[i];

    cnt[0][0] = 1;
    rep(i, n){
        rep(j, m+1){
            cnt[i+1][j] = cnt[i][j];
            if(j - a[i] >= 0) cnt[i+1][j] += cnt[i+1][j - a[i]];
            if(j - a[i]*(b[i]+1) >= 0){
                if(cnt[i][j - a[i]*(b[i]+1)] != 0) cnt[i+1][j]--;
            }
        }
    }

    cout << (cnt[n][m] != 0 ? "Yes" : "No") << endl;
    return 0;
}
```

### 最長共通部分列 (LCS) 問題
```cpp
int cnt[1003][1003];

int main(){
    string s, t; cin >> s >> t;
    int s__ = s.size(), t__ = t.size();

    cnt[0][0] = 0;
    rep(i, s__){
        rep(j, t__){
            if(s[i] == t[j]) cnt[i+1][j+1] = cnt[i][j] + 1;
            else cnt[i+1][j+1] = max(cnt[i][j+1], cnt[i+1][j]);
        }
    }

    cout << cnt[s__][t__] << endl;
    return 0;
}
```

### 最小コスト弾性マッチング問題
```cpp
int cst[103][103];

int main(){
    int n, m; cin >> n >> m;
    vector<vector<int>> c(n, vector<int>(m));
    rep(i, n) rep(j, m) cin >> c[i][j];

    rep(i, n){
        rep(j, m){
            if(i != 0 & j != 0) cst[i+1][j+1] = min({cst[i][j], cst[i+1][j], cst[i][j+1]}) + c[i][j];
            else if(i == 0) cst[i+1][j+1] = cst[i+1][j] + c[i][j];
            else if(j == 0) cst[i+1][j+1] = cst[i][j+1] + c[i][j];
        }
    }

    cout << cst[n][m] << endl;
    return 0;
}
```

### レーベンシュタイン距離 (diffコマンド)
```cpp
int diff[1003][1003];

int main(){
    string s, t; cin >> s >> t;
    int s__ = s.size(), t__ = t.size();

    rep(i, s__) diff[i+1][0] = i+1;
    rep(j, t__) diff[0][j+1] = j+1;

    rep(i, s__){
        rep(j, t__){
            if(s[i] == t[j]) diff[i+1][j+1] = min({diff[i][j], diff[i+1][j] + 1, diff[i][j+1] + 1});
            else diff[i+1][j+1] = min({diff[i][j], diff[i+1][j], diff[i][j+1]}) + 1;
        }
    }

    cout << diff[s__][t__] << endl;
    return 0;
}
```