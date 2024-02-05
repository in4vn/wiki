---
title: Codility
description: 
published: true
date: 2024-02-05T07:26:37.714Z
tags: work
editor: markdown
dateCreated: 2024-01-28T08:26:29.363Z
---

[CodeCheck > Mandatory](https://app.codility.com/dashboards/campaigns/#231674)

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

# CardPayments
```javascript
function solution(A, D) {
    const len = A.length;
    if (len < 1 || len > 100) {
        return 0;
    }

    const cache = new Array();
    for (let i = 0; i < 12; i++) {
        cache.push({ cardCount: 0, amount: 0, cardAmount: 0 });
    }

    for (let i = 0; i < len; i++) {
        const amount = A[i];
        if (amount < -1000 || amount > 1000) {
            return 0;
        }

        const [year, month] = D[i].split('-');
        if (parseInt(year, 10) > 2020) {
            return 0;
        }

        const index = parseInt(month, 10) - 1;
        const monthCache = cache[index];
        if (amount < 0) {
            monthCache.cardCount ++;
            monthCache.cardAmount += amount;
        }
        monthCache.amount += amount;
        cache[index] = monthCache;
    }

    const total = cache.reduce((res, monthCache) => {
        res += monthCache.amount;
        if (monthCache.cardCount < 3 || monthCache.cardAmount > -100) {
            res -= 5;
        }
        return res;
    }, 0);

    return total;
}
```