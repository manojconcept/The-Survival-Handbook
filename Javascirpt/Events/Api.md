### 1. XMLHttpRequest
The `XMLHttpRequest` object is used to make HTTP requests to a server.

#### Events:
- `load`: Fired when the request is completed successfully.
- `error`: Fired when the request fails.
- `abort`: Fired when the request is aborted.
- `loadstart`: Fired when the request starts.
- `progress`: Fired periodically as the request is in progress.
- `loadend`: Fired when the request has completed, whether successfully (after `load`) or not (after `error` or `abort`).

#### Example:
```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.example.com/data', true);

xhr.onload = function () {
    if (xhr.status >= 200 && xhr.status < 300) {
        console.log('Response:', xhr.responseText);
    } else {
        console.log('Error:', xhr.statusText);
    }
};

xhr.onerror = function () {
    console.log('Request failed');
};

xhr.send();
```

### 2. Fetch API
The `fetch` API provides a more modern way to make HTTP requests and works with Promises.

#### Using Promises:
While `fetch` itself doesn't have event listeners, it uses Promises which can handle asynchronous events.

#### Example:
```javascript
fetch('https://api.example.com/data')
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok ' + response.statusText);
        }
        return response.json();
    })
    .then(data => {
        console.log('Data:', data);
    })
    .catch(error => {
        console.log('There was a problem with the fetch operation:', error);
    });
```

### 3. WebSocket
The `WebSocket` API is used to create a persistent connection between a client and a server.

#### Events:
- `open`: Fired when the connection is established.
- `message`: Fired when data is received from the server.
- `error`: Fired when an error occurs.
- `close`: Fired when the connection is closed.

#### Example:
```javascript
var socket = new WebSocket('ws://example.com/socket');

socket.addEventListener('open', function (event) {
    console.log('Connected to WebSocket');
    socket.send('Hello Server!');
});

socket.addEventListener('message', function (event) {
    console.log('Message from server:', event.data);
});

socket.addEventListener('error', function (event) {
    console.log('WebSocket error:', event);
});

socket.addEventListener('close', function (event) {
    console.log('WebSocket closed:', event);
});
```

### 4. EventSource (Server-Sent Events)
The `EventSource` interface is used to receive server-sent events.

#### Events:
- `open`: Fired when the connection to the server is opened.
- `message`: Fired when a message is received from the server.
- `error`: Fired when an error occurs.

#### Example:
```javascript
var eventSource = new EventSource('https://example.com/events');

eventSource.addEventListener('open', function (event) {
    console.log('Connection to server opened');
});

eventSource.addEventListener('message', function (event) {
    console.log('New message:', event.data);
});

eventSource.addEventListener('error', function (event) {
    if (event.eventPhase == EventSource.CLOSED) {
        console.log('Connection was closed');
    } else {
        console.log('Error:', event);
    }
});
```
