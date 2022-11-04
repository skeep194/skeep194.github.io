---
title: C++ std::accmulate 함수 주의점
tags: [TIL]
style: fill
color: alert
date: 2022-10-21 00:22:30+0900
description: this is description
external_url: 
---
# 이런
매우 쉬운 코드포스 900난이도 문제를 푸는데 무려 2번이나 틀렸다. 알고보니 accmulate함수는 마지막에 주는 인자가 처음 계산되는 수라서 배열이 long long이어도 0이라고 쓰면 int로 계산한다. 따라서 0대신 0ll이라고 써야 long long으로 계산할 수 있다.

# 해결책
```cpp
//calculate int
accmultae(arr.begin(), arr.end(), 0);
//calculate long long
accmultae(arr.begin(), arr.end(), 0ll);
```

같은 실수는 반복하지만 않으면 되지~