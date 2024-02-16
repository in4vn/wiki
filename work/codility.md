---
title: Codility
description: 
published: true
date: 2024-02-16T09:05:27.596Z
tags: work
editor: markdown
dateCreated: 2024-01-28T08:26:29.363Z
---

[CodeCheck > Mandatory](https://app.codility.com/dashboards/campaigns/#231674)

- đọc và hiểu đề 
- Luôn validate input: empty, 1 element, min và max của từng item trong array
- Nếu kết quả không phụ thuộc kết quả tính toán, không cần thực sự tính toán, lưu phép tính là được 
- Set to array: Array.from(set)
- Array to set: new Set(array)

# AsphaltPatches

Test cases
- .X...XX
- ...XXX.XX.XXX...
- ..........X...............X.........X...

```javascript
function solution(S) {
    const len = S.length;
    if (len < 3 || len > 100000) {
        return 0;
    }

    let count = 0;
    // remove ... chars
    let string = S.replace(/\.\.\./g, '');
  	// remove .. chars at start of string
    string = string.substring(string.indexOf('X'));
    const segments = string.match(/.{1,3}/g) ?? [];
    for (let i = 0; i < segments.length; i++) {
        if (segments[i].indexOf('X') !== -1) {
            count++;
        }
    }

    return count;
}
```

# ValueOccurrences

```javascript
function solution(A) {
    const len = A.length;
    if (len === 0 || len > 100000) {
        return 0;
    }

    const map = {};
    for (let i = 0; i < len; i++) {
        const num = A[i];
        if (map[num]) {
            map[num] ++;
        } else {
            map[num] = 1;
        }
    }

    let move = 0;
    for (const key in map) {
        if (key !== map[key]) {
            if (key < map[key]) {
                move += map[key] - key;
            } else {
                const removeCount = map[key];
                const addCount = key - map[key];
                move += Math.min(removeCount, addCount);
            }
        }
    }

    return move;
}
```

# SmallestDigitSum

```javascript
function solution(N) {
    if (N < 1 || N > 50) {
        return 0;
    }

    if (N < 10) {
        return N;
    }

    const getDigitSum = num => {
        let sum = 0;
        while (num !== 0) {
            sum += num % 10;
            num = Math.floor(num / 10);
        }
        return sum;
    }

    let i = 0;
    while (1) {
        if (getDigitSum(i) === N) {
            return i;
        }
        i++;
    }
}
```

# ShortestUniqueSubstring

```javascript
function solution(S) {
    const length = S.length;
    if (length < 1 || length > 200) {
        return 0;
    }

    for (let len = 1; len < length; len++) {
        for (let pos = 0; pos < length; pos++) {
            const substring = S.substring(pos, pos + len);
            const subStringIndex = S.indexOf(substring);
            if (S.indexOf(substring, subStringIndex + 1) === -1) {
                return len;
            }
        }
    }

    return length;
}
```

# SameDigitMerge

58%

```javascript
function solution(numbers) {
    const len = numbers.length;
    if (len < 1) {
        return 0;
    }

    const texts = numbers.map(number => number + "");

    let found = 0;
    for (let i = 0; i < len; i ++) {
        const string = texts[i];
        if (string.length < 2) {
            return 0;
        }

        const lastDigit = string.charAt(string.length - 1);
        for (let j = 0; j < len; j ++) {
            if (i !== j && texts[j].startsWith(lastDigit)) {
                found ++;
            }
        }
    }

    return found;
}
```

# MonitorsDelivery

Test cases
- Same distance, but with different monitors => we sort by both distance and monitors

```javascript
function solution(D, C, P) {
    if (D.length < 1 || C.length < 0 || P < 0) {
        return 0;
    }

    const mapped = D.reduce((res, distance, index) => {
        res.push({
            distance,
            monitors: C[index]
        });
        return res;
    }, []);

    const sorted = mapped.sort((a, b) => a.distance - b.distance || a.monitors - b.monitors);
    let fulfilledOrders = 0;
    let fulfilledMonitors = 0;

    for (let i = 0; i < sorted.length; i ++) {
        const { monitors } = sorted[i];
        if (monitors <= P - fulfilledMonitors) {
            fulfilledMonitors += monitors;
            fulfilledOrders ++;
        } else {
            break;
        }
    }

    return fulfilledOrders;
}
```

# FormatArray

Test cases:
- 1 element
- 1 column
- same length

```javascript
function solution(A, K) {
    const len = A.length;
    if (len < 0) {
        return '';
    }

    const buildCell = (num, cellLength) => {
        const numLength = cellLength - num.length;

        const body = [];
        body.push('|');
        for (let i = 0; i < cellLength; i++) {
            if (i < numLength) {
                body.push(' ');
            } else if (i === numLength) {
                body.push(num);
            }
        }
        return body.join("");
    }

    const buildRow = (array, cellLength) => {
        let body = [];
        for (let i = 0; i < array.length; i++) {
            body.push(buildCell(array[i], cellLength))
        }
        return body.join("") + '|';
    }

    const buildHeaderFooter = (cellCount, cellLength) => {
        const hf = [];
        for (let i = 0; i < cellCount; i++) {
            hf.push('+');
            for (let j = 0; j < cellLength; j++) {
                hf.push('-');
            }
            if (i === cellCount - 1) {
                hf.push('+');
            }
        }
        return hf.join('');
    }

    const buildTable = (array, cellCount) => {
        const cellLength = array.reduce((res, num) => {
            return Math.max(res, num.length);
        }, 0);
        const table = [];
        const row = [];
        // header
        table.push(buildHeaderFooter(cellCount, cellLength));
        for (let i = 0; i < len; i++) {
            row.push(array[i]);
            if ((array.length && i % cellCount === cellCount - 1) || i === len - 1) {
                // body
                table.push(buildRow(row, cellLength));
                // footer
                table.push(buildHeaderFooter(row.length, cellLength));
                row.length = 0;
            }
        }
        return table.join('\n');
    }

    const arrayString = A.map(num => num + "");
    const cellCount = arrayString.length > K ? K : arrayString.length;
    const res = buildTable(arrayString, cellCount);

    process.stdout.write(res);
    // process.stderr.write(res);
}
```

# ForbiddenTriosSwaps

```javascript
function solution(S) {
    const len = S.length;
    
    // at least 3 chars are required to do a move
    if (len < 3 || len > 200000) {
        return 0;
    }

    const array = S.split("");

    let count = 0;
    for (let i = 0; i < len; i ++) {
        const current = array[i];
        if (current === array[i + 1] && current === array[i + 2]) {
            const moved = current === 'a' ? 'b' : 'a';
            if (current === array[i + 3]) {
                array[i + 2] = moved;
            } else {
                array[i + 1] = moved;
            }
            count ++;
        }
    }

    return count;
}
```

# EvenPairsOnCycle

```javascript
function solution(A) {
    const len = A.length;
  	// we need at least 2 numbers to make a pair
    if (len < 2 || len > 100000) {
        return 0;
    }

    const doCount = (startIndex) => {
        let count = 0;
        let skipNext = false;
        let skipFirst = false;
        let skipLast = false;

        for (let i = startIndex; i < len; i += skipNext ? 2 : 1) {
            if (A[i + 1] !== undefined && (A[i] + A[i + 1]) % 2 === 0) {
                count ++;
                skipNext = true;
                if (i === 0) {
                    skipFirst = true;
                }
                if (i === len - 1) {
                    skipLast = true;
                }
            } else {
                skipNext = false;
            }
        }

        if (skipFirst === false && skipLast === false) {
            if ((A[0] + A[len - 1]) % 2 === 0) {
                count ++;
            } 
        }

        return count;
    }

    return Math.max(doCount(0), doCount(1));
}
```

# EraseOneLetter

50%

```javascript
function solution(S) {
    const len = S.length;
    if (len < 2 || len > 100000) {
        return '';
    }

    const removed = [];
    for (let i = 0; i < len; i++) {
        const string = S.substr(0, i) + S.substr(i + 1);
        removed.push(string);
    }

    const sorted = removed.sort((a, b) => a.localeCompare(b));
    return sorted[0];
}

function solution(S) {
    const len = S.length;
    if (len < 2 || len > 100000) {
        return '';
    }

    const removed = [];
    const original = S.split("");
    for (let i = 0; i < len; i++) {
        removed.push(original.filter((char, index) => index !== i).join(""));
    }

    const sorted = removed.sort((a, b) => a.localeCompare(b));
    return sorted[0];
}

function solution(S) {
    const len = S.length;
    if (len < 2 || len > 100000) {
        return '';
    }

    const removed = [];
    for (let i = 0; i < len; i++) {
        const string = S.split("");
        string[i] = "";
        removed.push(string.join(""));
    }

    const sorted = removed.sort((a, b) => a.localeCompare(b));
    return sorted[0];
}
```

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

# CastleBuilding
```javascript
function solution(A) {
    if (A.length === 0) {
        return 0;
    }

    A = A.reduce((res, num, ind) => {
        if (num !== A[ind + 1]) {
            res.push(num);
        }
        return res;
    }, []);

    let count = 0;
    const len = A.length;
    for (let i = 0; i < len; i++) {
        const current = A[i];
        const prev = A[i - 1] || current;
        const next = A[i + 1] || current;
        if (Math.min(prev, current, next) === current || Math.max(prev, current, next) === current) {
            count ++;
        }
    }

    return count;
}
```

# CommonLetter
```javascript
function solution(S) {
    const len = S.length;
    if (len === 0) {
        return [];
    }

    for (let i = 0; i < len; i++) {
        const current = S[i].split("");
        for (let j = i + 1; j < len; j++) {
            const next = S[j].split("");
            for (let k = 0; k < current.length; k++) {
                if (current[k] === next[k]) {
                    return [i, j, k];
                }
            }
        }
    }

    return [];
}
```

# CountBananas
```javascript
function solution(S) {
    if (S.length === 0) {
        return 0;
    }

    let string = S;
    const toBeRemoved = ['B', 'A', 'N', 'A', 'N', 'A'];
    const len = toBeRemoved.length;

    const move = () => {
        let count = 0;
        for (let i = 0; i < len; i++) {
            const charToRemove = toBeRemoved[i];
            if (string.indexOf(charToRemove) !== -1) {
                count++;
                string = string.replace(charToRemove, '');
            }
        }
        return count === 6;
    }

    let removeCount = 0;
    while (move()) {
        removeCount++;
    }

    return removeCount;
}
```

# CreatePalindrome
Test cases
```
"?a?"
"ab?"
"?ba"
"???"
```

```javascript
function solution(S) {
    const Slen = S.length;
    if (Slen === 0 || Slen > 1000) {
        return "NO";
    }

    let string = S.split("");
    const len = string.length;
    const last = len - 1;
    const middle = Math.floor(len / 2);

    for (let i = 0; i < middle; i++) {
        const current = string[i];
        const opposite = string[last - i];
        if (current === '?' && opposite !== '?') {
            string[i] = opposite;
        } else if (current !== '?' && opposite === '?') {
            string[last - i] = current;
        } else if (current === "?" && opposite === "?") {
            string[i] = "a";
            string[last - i] = "a";
        } else if (current !== opposite) {
            return "NO";
        }
    }

    if (string[middle] === "?") {
        string[middle] = "a";
    }
    
    return string.join("");
}
```

# DiversityString 

80%

```javascript
function solution(N) {
    if (N < 1 || N > 200000) {
        return "";
    }

    const chars = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'];
    const len = chars.length;
    let repeat = 0;
    
    const getRepeatTimes = () => {
        for (let i = len - 1; i >= 0; i--) {
            if (N % i === 0) {
                return N / i;
            }
        }
    }

    if (N <= len) {
        repeat = 1;
    } else {
        repeat = getRepeatTimes();
    }

    const result = [];
    for (let i = 0; i < len; i++) {
        for (let j = 0; j < repeat; j++) {
            if (result.length < N) {
                result.push(chars[i]);
            }
        }
    }

    return result.join("");
}
```