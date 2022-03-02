---
id: yskq69lbyw4kpr8y5sdx9lu
title: API Architecture
desc: ""
updated: 1646263119676
created: 1646047028554
---

### RPC, REST and GRAPHQL

## RPC - Remote Procedure Call

Earliest API architecture. RPC is to call a remote "procedure" or a function e.g. from a client application to a server with the running API. For example

```javascript
// this procedure is invoked and the response is returned
function sayHello(name) {
  return `Hello, ${name}!`;
}
```

How the procedure is invoked on the server depends on the framework and choice of technology adopted. RPC is just a concept, and in a simple scenario, the method could be determined by simply parsing the url coming from the client.

A contrived example:

```Javascript
const express = require('express')
const app = express()
const port = 3000

app.get('/',(req, res) => {
  // extract the method being called from the query params
  const { method } = req.query;
  const a = parseInt(req.query.a);
  const b = parseInt(req.query.b);

  let methodResponse = "";

  // call the method requested
  switch(method) {
     case "add":
     	methodResponse = add(a,b);
        break;
     case "multiply":
     	methodResponse = multiply(a,b);
        break;
     case "minus":
     	methodResponse = minus(a,b);
	break;
     default:
        methodResponse = "method doesn't exist";

  }

  res.send({ result: methodResponse });
})

function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

function minus(a, b) {
  return a - b;
}
app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
})

```

Calls to the above as follows:

```bash
POST http://localhost:3000/?method=multiply&a=400&b=2
{output: 8}

```

### Disadvantages

**Tight coupling** - RPCs client and server are tightly coupled since function calls need to match.

**Security issues** - It may be considered a flaw that the inner workings of the API are somewhat revealed to the client as the method name may imply.

### Advantages

** Lightweight ** RPC APIs are expected not to return a lot of meta data and the most popular frameworks such as gRPC are built in such a way that allows them to be more performant than their REST counterpart.

### gRPC

One of the most notable examples of RPC frameworks in modern day is [gRPC](https://grpc.io/). gRPC is an open-source Google framework used for high performance microservice architecture styles. It's supported by multiple languages/frameworks such as Go, Node, Rust Python and more. It's used by many well known tech companies such as Square, Netlfix and [Cloudflare](https://blog.cloudflare.com/road-to-grpc/).

gRPC is considered highly performant due to:

- Using HTTP/2 for transport
- Allowing both clients and servers to stream data as opposed to polling (_Note: I believe this is available due to HTTP/2_)
- Encouraging Protobuf as the messaging format

Protobuf (protocol buffers) are a message format that allow for serialization of structured data in a language neutral way. They are an alternative messaging format to others such as JSON or XML and are considered more compact to achieve low latency.

Example of a Protobuf message:

```Protobuf
// Person.proto
message Person {
  required string name = 1;
  required int32 age = 2;
  optional string email = 3;
}
```

gRPC can be implemented using a variety of languages or frameworks, however it seems performance varies - multiple language performance tests are [performed often](https://grpc.io/docs/guides/benchmarking/) on the code base. Note that the [performance dashboard](https://grafana-dot-grpc-testing.appspot.com/?kiosk=tv) is missing Node however, it has been [benchmarked by 3rd parties](https://www.nexthink.com/blog/comparing-grpc-performance/) and _appears_ to be less performant than alternatives such as Go.

gRPC currently cannot natively communicate with the web via HTML/2. Browsers do not support protobuf payloads and hence, grpc-web was introduced in order to act as a proxy to serialize web payload and give developers the ability to use protobufs across the full stack:

![https://grpc.io/blog/state-of-grpc-web/](/assets/images/2022-03-03-10-13-17.png)

<caption>Source: https://grpc.io/blog/state-of-grpc-web/</caption>

</br>
</br>
Prior to this, developers were forced to use a REST API with HTTP to communicate with the gRPC backend.

#### References

1. https://blog.jscrambler.com/rpc-style-vs-rest-web-apis
2. https://grpc.io/
