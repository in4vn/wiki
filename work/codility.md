---
title: Codility
description: 
published: true
date: 2024-01-28T08:26:29.363Z
tags: work
editor: markdown
dateCreated: 2024-01-28T08:26:29.363Z
---

# TheWidestPath
```javascript
function solution(X, Y) {
    if (X.length === 0) return 1;
  
    X.sort((a, b) => a - b);

    const missing = X.reduce((res, num, ind) => {
        const next = X[ind + 1];
        if (next && next - num > 1) {
            res.push(next - num);
        }
        return res;
    }, []);

    if (missing.length === 0) return 1;

    return Math.max(...missing);
}
```