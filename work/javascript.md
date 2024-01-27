---
title: Javascript
description: 
published: true
date: 2024-01-27T12:19:19.405Z
tags: work
editor: markdown
dateCreated: 2024-01-27T12:19:19.405Z
---

- Fetch: handle errors

```javascript
try {
  const response = await fetch('https://restcountries.com/v4.1/all');
  if (response.ok) {
    console.log('Promise resolved and HTTP status is successful');
    // ...do something with the response
  } else {
  // Custom message for failed HTTP codes
  if (response.status === 404) throw new Error('404, Not found');
  if (response.status === 500) throw new Error('500, internal server error');
  // For any other server error
  if (!response.ok) throw new Error(response.status);
  }
} catch (error) {
  console.error('Fetch', error);
  // Output e.g.: "Fetch Error: 404, Not found"
}
```
