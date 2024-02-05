---
title: Codility
description: 
published: true
date: 2024-02-05T06:42:28.947Z
tags: work
editor: markdown
dateCreated: 2024-01-28T08:26:29.363Z
---

# TheWidestPath
```javascript
function solution(X, Y) {
    const length = X.length;
    if (length === 0) return 1;

    X.sort((a, b) => a - b);

    let max = 1;
    for (let i = 1; i < length; i++) {
        max = Math.max(max, X[i] - X[i - 1]);
    }

    return max;
}
```