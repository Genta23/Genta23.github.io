---
title: map
layout: default
parent: cpp
---
# map

mapはkeyとvalueを1セットとしてデータを保存することができる。

```cpp
#include <map>
```

## 全てのキー・値の取得方法
```cpp
//方法1
for(auto itr = mp.begin(); itr != mp.end(); ++itr) {
    cout << itr->first << " " << itr->second << endl;
}

//方法2
for (const auto& [k, v] : data) {
    cout << k << " " << v << endl;
}
```