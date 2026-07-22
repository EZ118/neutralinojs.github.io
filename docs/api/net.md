---
title: Neutralino.net
toc_max_heading_level: 2
---

`Neutralino.net` namespace contains methods for making HTTP/HTTPS requests.

## net.request(url, method, options)

Sends an HTTP/HTTPS request with the given method and options. This is the core method used by all other shorthand methods. All responses are returned as a complete payload; streaming is not supported. The promise rejects with one of the following error codes (see [Error Codes](./error-codes.md) for details):

- `NE_NW_SSLCONN` – SSL connection failed.
- `NE_NW_SSLLOAD` – SSL certificate loading failed.
- `NE_NW_SSLVERI` – SSL server verification failed.
- `NE_NW_SSLHOST` – SSL hostname verification failed.
- `NE_NW_HTTPERR` – other HTTP errors.

### Parameters

- `url` String: The target URL.
- `method` String: Request method. Only the following values are accepted:
  - `GET`
  - `POST`
  - `HEAD`
  - `OPTIONS`
  - `PUT`
  - `PATCH`
  - `DELETE`

### Options

- `timeout` Number (optional): Request timeout in milliseconds.
- `params` Object (optional, cannot be used with `body`): Form parameters to be sent. Only effective for `GET` and `POST` requests. For `POST`, these are sent as `application/x-www-form-urlencoded` in the request body. For `GET`, they are appended to the URL as a query string. For other methods, this option is ignored.
- `headers` Object (optional): Custom HTTP headers.
- `auth` Object (optional): Basic authentication credentials.
  - `username` String
  - `password` String
- `body` String (optional, cannot be used with `params`): Raw request body. When provided, `contentType` must also be set.
- `allowRedirects` Boolean (optional): Follow redirects automatically. Default is `false`.
- `encodePath` Boolean (optional): Encode special characters in the URL path. Default is `true`.
- `keepAlive` Boolean (optional): Use a Keep-Alive connection.
- `contentType` String (required when `body` is used): Value of the `Content-Type` header (e.g., `application/json`).

### Return Object (awaited):

- `statusCode` Number: HTTP status code.
- `text` String: Raw response body as text.
- `reason` String: Status reason phrase (e.g., "OK", "Object moved").
- `headers` Object: Response headers as key-value pairs.
- `cookies` String: Set‑Cookie header value(s) as a single string.
- `version` String: HTTP version used (e.g., `"HTTP/1.1"`).

```js
const res = await Neutralino.net.request(
    'https://neutralino.js.org/',
    'POST',
    {
        timeout: 5000,
        params: {
            foo: 'bar'
        },
        auth: {
            username: 'NeutralinoJS',
            password: '12345678'
        },
        headers: {
            Origin: 'https://neutralino.js.org/'
        },
        allowRedirects: false,
        encodePath: true,
        keepAlive: true
    }
);

// Alternatively, send a JSON body:
const res2 = await Neutralino.net.request(
    'https://neutralino.js.org/',
    'POST',
    {
        timeout: 5000,
        body: '{ "foo": "bar" }',
        contentType: 'application/json'
    }
);

console.log('Result:', res);
```

## net.get(url, options)

Performs a `GET` request. Works exactly as `net.request(url, 'GET', options)`.

### Parameters

- `url` String: The target URL.
- `options` Object: Same options as [`net.request`](#netrequesturl-method-options) (except `method`).

### Return Object (awaited):

Same as [`net.request`](#return-object-awaited).

```js
const res = await Neutralino.net.get('https://neutralino.js.org/', { timeout: 5000 });
console.log('Result:', res);
```

## net.head(url, options)

Performs a `HEAD` request. Works exactly as `net.request(url, 'HEAD', options)`.

### Parameters

- `url` String: The target URL.
- `options` Object: Same options as [`net.request`](#netrequesturl-method-options) (except `method`).

### Return Object (awaited):

Same as [`net.request`](#return-object-awaited).

```js
const res = await Neutralino.net.head('https://neutralino.js.org/');
console.log('Result:', res);
```

## net.options(url, options)

Performs an `OPTIONS` request. Works exactly as `net.request(url, 'OPTIONS', options)`.

### Parameters

- `url` String: The target URL.
- `options` Object: Same options as [`net.request`](#netrequesturl-method-options) (except `method`).

### Return Object (awaited):

Same as [`net.request`](#return-object-awaited).

```js
const res = await Neutralino.net.options('https://neutralino.js.org/');
console.log('Result:', res);
```

## net.put(url, options)

Performs a `PUT` request. Works exactly as `net.request(url, 'PUT', options)`.

### Parameters

- `url` String: The target URL.
- `options` Object: Same options as [`net.request`](#netrequesturl-method-options) (except `method`).

### Return Object (awaited):

Same as [`net.request`](#return-object-awaited).

```js
const testData = {
    id: 123,
    username: 'updateduser',
    status: 'active'
};
const options = {
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(testData)
};
const res = await Neutralino.net.put('https://neutralino.js.org/', options);
console.log('Result:', res);
```

## net.patch(url, options)

Performs a `PATCH` request. Works exactly as `net.request(url, 'PATCH', options)`.

### Parameters

- `url` String: The target URL.
- `options` Object: Same options as [`net.request`](#netrequesturl-method-options) (except `method`).

### Return Object (awaited):

Same as [`net.request`](#return-object-awaited).

```js
const testData = { age: 30, status: 'updated' };
const options = {
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(testData)
};
const res = await Neutralino.net.patch('https://neutralino.js.org/', options);
console.log('Result:', res);
```

## net.del(url, options)

Performs a `DELETE` request. Works exactly as `net.request(url, 'DELETE', options)`.

### Parameters

- `url` String: The target URL.
- `options` Object: Same options as [`net.request`](#netrequesturl-method-options) (except `method`).

### Return Object (awaited):

Same as [`net.request`](#return-object-awaited).

```js
const testData = { id: 123, reason: 'For testing' };
const options = {
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(testData)
};
const res = await Neutralino.net.del('https://neutralino.js.org/', options);
console.log('Result:', res);
```