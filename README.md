# Node.js HTTP over TCP

Examples of working with HTTP in Node.js on two levels: with the built-in `http` module, and by hand over a raw TCP socket with the `net` module. The raw version builds the HTTP request byte by byte to show what actually goes over the wire.

## Files

| File | Description |
| --- | --- |
| `http/simple-server.js` | HTTP server on port `8050` that logs the method, URL, headers and body of each request and responds with JSON |
| `http/simple-client.js` | HTTP client with a keep-alive agent that sends a `POST /create-post` request |
| `http/net-sender.js` | TCP client that sends a hand-built HTTP request (headers and body as hex buffers) and prints the raw response |

## Usage

Start the server in one terminal:

```bash
node http/simple-server.js
```

Then run either client in another:

```bash
node http/simple-client.js
node http/net-sender.js
```

Both send the same `POST /create-post` request with a JSON body, and the server responds with:

```json
{"message":"Post with title Title of my post was created by Joe"}
```

## How the raw request works

An HTTP request is plain text sent over TCP: a request line, headers separated by `\r\n`, an empty line, and then the body. `net-sender.js` writes these bytes directly to the socket, including the `Content-Length` header, so the server parses it as a normal HTTP request.

## Requirements

- Node.js 16 or newer
