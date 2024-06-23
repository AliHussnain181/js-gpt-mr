### JavaScript: BOM, Async/Await, Event Loop, and Error Handling

#### Beginner Level

**1. What is the Browser Object Model (BOM)?**

**Answer:**
The Browser Object Model (BOM) provides interaction with the browser. It includes objects like `window`, `navigator`, `screen`, `history`, `location`, and `document`.

```javascript
// Example: Using BOM to get the URL of the current page
console.log(window.location.href);
```

**2. How do you use `async` and `await` in JavaScript?**

**Answer:**
`async` and `await` are used to handle asynchronous operations in a more readable way. `async` makes a function return a Promise, and `await` pauses the execution of the function until the Promise is resolved.

```javascript
async function fetchData() {
  let response = await fetch('https://api.example.com/data');
  let data = await response.json();
  console.log(data);
}

fetchData();
```

**3. What is the Event Loop in JavaScript?**

**Answer:**
The Event Loop is a mechanism that handles the execution of multiple pieces of code over time, making JavaScript asynchronous and non-blocking.

```javascript
console.log('Start');
setTimeout(() => console.log('Timeout'), 0);
console.log('End');
// Output: Start, End, Timeout
```

**4. How do you handle errors in JavaScript?**

**Answer:**
Errors can be handled using `try...catch` blocks. The `try` block contains code that may throw an error, and the `catch` block handles the error.

```javascript
try {
  let result = riskyOperation();
  console.log(result);
} catch (error) {
  console.error('An error occurred:', error);
}
```

#### Intermediate Level

**5. What are some common BOM methods for interacting with the browser window?**

**Answer:**
Common BOM methods include `alert`, `confirm`, `prompt`, `open`, `close`, and `setInterval`.

```javascript
// Example: Using `alert` and `confirm`
alert('This is an alert message.');
if (confirm('Do you want to proceed?')) {
  console.log('User chose to proceed.');
} else {
  console.log('User canceled.');
}
```

**6. How do you handle multiple asynchronous operations with `async` and `await`?**

**Answer:**
You can handle multiple asynchronous operations using `Promise.all`, `Promise.race`, or by chaining `await` calls.

```javascript
async function fetchMultipleData() {
  const urls = [
    'https://api.example.com/data1',
    'https://api.example.com/data2',
    'https://api.example.com/data3'
  ];

  let [data1, data2, data3] = await Promise.all(urls.map(url => fetch(url).then(res => res.json())));
  console.log(data1, data2, data3);
}

fetchMultipleData();
```

**7. Can you explain how the Event Loop works with microtasks and macrotasks?**

**Answer:**
The Event Loop processes macrotasks (e.g., setTimeout, setInterval) and microtasks (e.g., Promises). Microtasks have higher priority and are executed before the next macrotask.

```javascript
console.log('Start');

setTimeout(() => console.log('Timeout'), 0);

Promise.resolve().then(() => console.log('Promise'));

console.log('End');
// Output: Start, End, Promise, Timeout
```

**8. How do you create custom errors in JavaScript?**

**Answer:**
Custom errors can be created by extending the `Error` class.

```javascript
class CustomError extends Error {
  constructor(message) {
    super(message);
    this.name = 'CustomError';
  }
}

try {
  throw new CustomError('This is a custom error');
} catch (error) {
  console.error(error.name, error.message);
}
```

#### Advanced Level

**9. How can you manipulate the browser history using BOM?**

**Answer:**
The `history` object provides methods to manipulate the browser history, such as `back`, `forward`, `go`, `pushState`, and `replaceState`.

```javascript
// Example: Navigating the browser history
history.pushState({ page: 1 }, 'title 1', '?page=1');
history.pushState({ page: 2 }, 'title 2', '?page=2');
history.back(); // Goes back to ?page=1
history.forward(); // Goes forward to ?page=2
```

**10. How do you handle async/await errors and ensure proper error handling in async functions?**

**Answer:**
Errors in async functions can be handled using `try...catch` blocks. Additionally, you can handle rejected promises using `.catch`.

```javascript
async function fetchData() {
  try {
    let response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Fetch error:', error);
  }
}

fetchData();
```

**11. Explain the difference between microtasks and macrotasks in the context of the Event Loop.**

**Answer:**
Microtasks (e.g., `Promise.then`, `MutationObserver`) are executed immediately after the currently executing script, before any rendering or I/O operations. Macrotasks (e.g., `setTimeout`, `setInterval`) are executed in the next iteration of the Event Loop after rendering.

```javascript
console.log('Script start');

setTimeout(() => console.log('setTimeout'), 0);

Promise.resolve().then(() => console.log('Promise'));

console.log('Script end');
// Output: Script start, Script end, Promise, setTimeout
```

**12. How do you handle uncaught errors in JavaScript?**

**Answer:**
Uncaught errors can be handled globally using `window.onerror` or `process.on('uncaughtException')` in Node.js.

```javascript
// Browser
window.onerror = function(message, source, lineno, colno, error) {
  console.error('Global error handler:', message);
};

// Node.js
process.on('uncaughtException', (error) => {
  console.error('Uncaught exception:', error);
});
```

#### FAANG Level

**13. How can you optimize asynchronous operations to improve performance in JavaScript applications?**

**Answer:**
Optimize asynchronous operations by using:
- `Promise.all` for concurrent operations.
- Lazy loading for resources.
- Web Workers for parallel processing.
- Efficiently managing network requests to reduce latency.

```javascript
async function optimizedFetch(urls) {
  const responses = await Promise.all(urls.map(url => fetch(url)));
  const data = await Promise.all(responses.map(res => res.json()));
  return data;
}

const urls = ['https://api.example.com/data1', 'https://api.example.com/data2'];
optimizedFetch(urls).then(data => console.log(data));
```

**14. Describe how you would implement a retry mechanism for failed network requests using async/await.**

**Answer:**
A retry mechanism can be implemented by wrapping the fetch call in a loop that retries the operation a specified number of times.

```javascript
async function fetchWithRetry(url, options = {}, retries = 3) {
  for (let i = 0; i < retries; i++) {
    try {
      let response = await fetch(url, options);
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      return await response.json();
    } catch (error) {
      if (i === retries - 1) throw error;
      console.log(`Retrying... (${i + 1}/${retries})`);
    }
  }
}

fetchWithRetry('https://api.example.com/data')
  .then(data => console.log(data))
  .catch(error => console.error('Failed to fetch data:', error));
```

**15. How do you handle race conditions in JavaScript using async/await?**

**Answer:**
Race conditions can be managed by using locks or semaphores, ensuring that only one operation accesses the shared resource at a time.

```javascript
class Mutex {
  constructor() {
    this.queue = [];
    this.locked = false;
  }

  async lock() {
    const unlock = () => {
      const next = this.queue.shift();
      if (next) {
        next(unlock);
      } else {
        this.locked = false;
      }
    };

    if (this.locked) {
      return new Promise(resolve => this.queue.push(resolve)).then(() => lock());
    } else {
      this.locked = true;
      return unlock;
    }
  }
}

const mutex = new Mutex();

async function criticalSection() {
  const unlock = await mutex.lock();
  try {
    // Critical section
  } finally {
    unlock();
  }
}
```

**16. What are the best practices for error handling in large-scale JavaScript applications?**

**Answer:**
Best practices include:
- Using consistent error handling patterns (`try...catch`, `.catch` for Promises).
- Logging errors and using monitoring tools (e.g., Sentry, LogRocket).
- Providing user-friendly error messages.
- Categorizing and handling different types of errors (network, application, user errors).
- Implementing fallback strategies (retry mechanisms, default values).

```javascript
async function fetchData(url) {
  try {
    let response = await fetch(url);
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return await response.json();
  } catch (error) {
   

 console.error('Error fetching data:', error);
    // Implement fallback strategy
    return { data: 'default data' };
  }
}
```

These questions and answers cover a wide range of concepts related to BOM, Async/Await, the Event Loop, and Error Handling in JavaScript, suitable for various levels of expertise from beginner to FAANG-level.