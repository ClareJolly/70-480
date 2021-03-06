# Module 6 - JavaScript Advanced Topics <!-- omit in toc -->

- [**Exception Handling**](#Exception-Handling)
  - [try - catch - finally](#try---catch---finally)
  - [Throw](#Throw)
- [**Promises**](#Promises)
  - [Syntax](#Syntax)
  - [Demo 1](#Demo-1)
  - [Demo 2 - uses](#Demo-2---uses)
    - [**The main promise**](#The-main-promise)
    - [**Consuming a promise**](#Consuming-a-promise)
    - [**Passing on a promise**](#Passing-on-a-promise)
    - [**Creating a promise**](#Creating-a-promise)
- [**TypeScript**](#TypeScript)
- [**Example questions**](#Example-questions)

---

[🔼](#readme)

## **Exception Handling**

- Managing failure in code
- Minimising failures in code

### try - catch - finally

For trying something that could potentially cause a problem.  API call, network call.

Something that would throw a runtime error rather than a syntax error.  Will it succeed in connecting for example

```js
try {
  // line(s) of suspicious code
} catch (err) {
  // .description
  // .message
  // .name
  // .number
  // .stack
} finally {
  // success or failure, code executes here
}
```

`try` the code
`catch` the `err` (use any name here) and can access the listed properties of the error
`finally` run the code.

---

### Throw

```js
function throwHelper() {
  var err = new Error(123, "Error in helper");
  throw err;
}

// elsewhere in code
try {
  throwHelper();
} catch (ex) {
  var msg = ex.number + ": " + ex:message;
  // log msg
}
```

Demo

```js
function isTypeOf(o, t) {
  if (t && t.charAt && t.charAt(0)) {
    return (Object.prototype.toString.call(o) === "[object " + t + "]");
  } else {
    throw new Error(123, "Error in helper")
    // throw new Error ("Args", "'t' required and must be a string.");
  }
}

console.log(isTypeOf({test: 'test'},'Object'))  // true
console.log(isTypeOf({test: 'test'},999)) //

```

---

[🔼](#readme)

## **Promises**

If there's a possibility that a function may take more that 50 ms then this is an async function. And must be called as such.

Can also create asynchronous functions ourselves

- Consuming a promise
- Chaining then... then... done
- Passing a promise
- Creating a promise
- Storing a promise

---

### Syntax

from [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

```js
var promise1 = new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('foo');
  }, 300);
});

promise1.then(function(value) {
  console.log(value);
  // expected output: "foo"
});

console.log(promise1);
// expected output: [object Promise]
```

---

### Demo 1

[Demo](./demo/6-demo-promises.html)

![promises](../images/promises-full.png)

**Long Sync Task**

Runs a while loop for 3 seconds.  Synchronous - can't do anything until complete.  ZERO concurrency

![p1](../images/promises-ex-1.png)

**Long Async (no Promise)**

Implemented using `setInterval` which immediately returns but maybe that's not what you want as it will continue onto the next piece of code (maybe too soon)

Progress bar finished which is not representing a true state of the process

![p2](../images/promise2.png)

3rd option doesn't actually work properly due to set up of code in the source demo - will try to fix manually

---

### Demo 2 - uses

[Demo](./demo/6-demo-promises-uses.html)

![uses](../images/prom-uses.png)

#### **The main promise**

```js
unction longTaskAsyncPromise(method) {
    return new Promise(function (c, e, p) {
        var seconds = 0;
        var intervalId = window.setInterval(function () {
            seconds++;
            prog(seconds, method);
            if (seconds >= SECONDS_DELAY) {
                window.clearInterval(intervalId);
                complete();
                c()
            }
        }, 1000);
    });
}
```

#### **Consuming a promise**

```js
// onclick event
method1.querySelector("button.start").onclick = function (e) {
    longTaskAsyncPromise(method1).then(function () {
        method1.querySelector("progress").value = 100;
        console.log('promise complete')
        method1.querySelector("button.start").innerHTML = 'DONE'
    });
};
```

When button is clicked we are calling the `longTaskAsyncPromise`

A promise has a `.then` function which we can use to do something once the promise finishes.  In all these cases I have added a console log and button text change.  Can chain `.then`s together

![uses](../images/prom-uses-1.png)

#### **Passing on a promise**

```js
// onclick event
method2.querySelector("button.start").onclick = function (e) {
    function myAsyncFunction() {
        return longTaskAsyncPromise(method2);
    }

    myAsyncFunction().then(function () {
        method2.querySelector("progress").value = 100;
        console.log('promise complete')
        method2.querySelector("button.start").innerHTML = 'DONE'
    });
};
```

When button is clicked we are declaring an async function, which then calls the `longTaskAsyncPromise` function/promise.  `myAsyncFunction` returns the `longTaskAsyncPromise` (which is a promise) to it's ultimately also returning a promise.

Can then run `.then` on `myAsyncFunction` as it's now a promise

![uses](../images/prom-uses-2.png)

#### **Creating a promise**

```js
// onclick event
method3.querySelector("button.start").onclick = function (e) {
    function myAsyncFunction() {
        return new Promise(function (c, e, p) {
            //do something that might take longer than 50ms
            try {
                longTaskAsyncPromise(method3).then(function () {
                    console.log('initial promise complete')
                    c();
                });
            }
            catch (err) {
                e(err);
            }
        });
    }

    myAsyncFunction().then(function () {
        method3.querySelector("progress").value = 100;
        console.log('main promise complete')
        method3.querySelector("button.start").innerHTML = 'DONE'
    });
};
```

`myAsyncFunction` creates it's own Promise inside

`return new Promise(function (c, e, p) {`

- `c` = complete
- `e` - error
- `p` - progress - think this is winJS specific

Can access all these from within the Promise

```js
try {
    longTaskAsyncPromise(method3).then(function () {
        console.log('initial promise complete')
        c();
    });
}
```

Tests that the `longTaskAsyncPromise` function is completing, `.then` calls the `myAsyncFunction` `c` (complete) to complete our promise.

```js
catch (err) {
    e(err);
}
```

Does a similar thing in the case of an error.  Not throwing the error, just reporting the error

<!-- ```js
longTaskAsyncPromise(method3).then(function () {
    console.log('initial promise complete')
    //10%
    p();
    c();
});
```

Could add in `p` (progress) and use this  -->

![uses](../images/prom-uses-3.png)

#### **Storing a promise**

```js
// onclick event
method4.querySelector("button.start").onclick = function (e) {

    var storedPromise = longTaskAsyncPromise(method4);

    //much time passes here

    storedPromise.then(function () {
        method4.querySelector("progress").value = 100;
        console.log('promise complete')
        method4.querySelector("button.start").innerHTML = 'DONE'
    });
};
```

Calling the `longTaskAsyncPromise` and instead of using `.then` it's being stored as `storedPromise`

Other actions may be done but can get back to the stored Promise to get an update

![uses](../images/prom-uses-4.png)

---

### Error Handling with Promises

Particularly with Promise chaining, each Promise can have it's own error reporting and handling.

example

```js
boxPromiseChainDone.onclick = function () {
  doAsync("chain[1]", 2)
    .then(function (msg) {
      doComplete(msg);
      return doAsync("chain[2]", 2);
    }, null, doProgress)
    .then(function (msg) {
      doComplete(msg);
      return doAsync("fail");
    }, null, doProgress)
    .then(function (msg) {
      doComplete(msg);
      return doAsync("chain[4]", 4);
    }, null, doProgress)
    .done(null, doError);
};

function doAsync(msg, limit) {
        return new Promise(
          //etc
          //etc


```

The `.done` is essentially a `finally`

```js
boxPromiseChainJoined.onclick = function () {
    var promises = [];
    var i = 0;
    // each promise has an error handler
    promises[i++] = doAsync("2seconds", 2)
        .then(doComplete, doError);
    promises[i++] = doAsync("fail", 8)
        .then(doComplete, doError);
    promises[i++] = doAsync("3seconds", 3)
        .then(doComplete, doError);
    // join promises
    Promise.join(promises)
        .done(function () {
            doProgress("all promises completed");
        }, doError);
};
```

Difference between joining and chaining a promises is that in chaining, one is done, then the next, then the next, etc.  Joining you are saying do all Promises at the same time, and the longest one will be the one we end up waiting on.

May want to handle each error individually (with chaining you may just do at end)

---

[🔼](#readme)

## **Web worker**

Only means of concurrency in JavaScript (as of 2012)

[Demo](./demo/6-demo-web-worker.html)

```js
//first we create a web worker and define the js file that defines it
var worker = new Worker("/demos/webworker/task.js");

function log(text) {
    document.querySelector("#log").innerHTML = document.querySelector("#log").innerHTML + '\n' + text
}

//this is how the web worker talks to us (the main thread)
worker.onmessage = function(e) {
    log("The worker said " + e.data);
};

//and this is how we talk to the web worker
worker.postMessage("Hey there, worker.");
```

Note that chrome doesn't like the above when you run it locally - so I did this to get it working - [workaround](https://stackoverflow.com/a/33432215)

What it's doing.....

- Creates the worker
- subscribes to the workers message function so that it can do something when the worker sends a message
- posts a message to the worker

Here's the worker (`task.js` in the example)

```js
 //this is how the main thread talks to us (the web worker)
  self.onmessage = function (e) {
  //here's how we talk to the main thread
  self.postMessage("\"Hey, boss. I heard you say '" + e.data + "\"");
};
```

Can pass messages between the 2 but they are on different threads.  True concurrency.

![webworker](../images/webworker.png)

---

[🔼](#readme)

## **Web sockets**

The ability to talk at a lower level to http.  Send a request and open channel of communication in near real time.  e.g. live chat

[Demo](./demo/6-demo-web-sockets.html)

```js
var websocket = new WebSocket("ws://echo.websocket.org/");

websocket.onopen = function (evt) {
    writeToScreen("CONNECTED");
    var message = "Web socket test";
    writeToScreen("SENT: " + message);
    websocket.send(message);
};

websocket.onclose = function (evt) {
    writeToScreen("DISCONNECTED");
};

websocket.onmessage = function (evt) {
    writeToScreen('<span style="color: blue;">RESPONSE: ' + evt.data + '</span>');
    websocket.close();
};
websocket.onerror = function (evt) { writeToScreen('<span style="color: red;">ERROR:</span> ' + evt.data); };

function writeToScreen(message) {
    var pre = document.createElement("p");
    pre.style.wordWrap = "break-word";
    pre.innerHTML = message;
    output.appendChild(pre);
}

function unload () {
websocket.close();
```

![websockets](../images/websockets.png)

---

<!-- ## Architecture Patterns



--- -->

[🔼](#readme)

## **TypeScript**

Superset of Javascript but adds in:

- types
- strong typing
- and more

---

[🔼](#readme)

## **Example questions**

[Module 6](./example-questions/6-example-questions.pdf)
