---
layout: post
title:  "gRPC Basics - Python"
categories: grpc
tags:  RPC python
---

By walking through this example you’ll learn how to:

- Define a service in a .proto file.
- Generate server and client code using the protocol buffer compiler.
- Use the Python gRPC API to write a simple client and server for your service.

## Why use gRPC

This example is a simple route mapping application that lets clients get information about features on their route, create a summary of their route, and exchange route information such as traffic updates with the server and other clients.

With gRPC you can define your service once in a .proto file and implement clients and servers in any of gRPC’s supported languages, which in turn can be run in environments ranging from servers inside Google to your own tablet - all the complexity of communication between different languages and environments is handled for you by gRPC. You also get all the advantages of working with protocol buffers, including efficient serialization, a simple IDL, and easy interface updating.

## Example code and setup

```bash
# Install requirement
pip3 install grpcio
## Python’s gRPC tools include the protocol buffer compiler protoc and the special plugin for generating server and client code from .proto service definitions.
pip3 install grpcio-tools


# Clone the repository to get the example code:
$ git clone -b v1.28.1 https://github.com/grpc/grpc

```

## Example: helloworld

```bash
# Navigate to the "hello, world" Python example:
$ cd grpc/examples/python/helloworld

# Run the server
$ python greeter_server.py

# From another terminal, run the client:
$ python greeter_client.py
Greeter client received: Hello, you!
```

### Update a gRPC service

Now let’s look at how to update the application with an extra method on the server for the client to call. Our gRPC service is defined using protocol buffers;

Let’s update this so that the Greeter service has two methods. Edit examples/protos/helloworld.proto and update it with a new SayHelloAgain method, with the same request and response types:

```proto
// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
  // Sends another greeting
  rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}

```

### Generate gRPC code

Next we need to update the gRPC code used by our application to use the new service definition.

From the examples/python/helloworld directory, run:

```bash
python -m grpc_tools.protoc -I../../protos --python_out=. --grpc_python_out=. ../../protos/helloworld.proto
```

This regenerates helloworld_pb2.py which contains our generated request and response classes and helloworld_pb2_grpc.py which contains our generated client and server classes.
