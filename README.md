# getty

 *a netty like asynchronous network I/O library*

[![Build Status](https://travis-ci.org/AlexStocks/getty.svg?branch=master)](https://travis-ci.org/AlexStocks/getty)
[![codecov](https://codecov.io/gh/AlexStocks/getty/branch/master/graph/badge.svg)](https://codecov.io/gh/AlexStocks/getty)
[![go.dev reference](https://img.shields.io/badge/go.dev-reference-007d9c?logo=go&logoColor=white&style=flat-square)](https://pkg.go.dev/github.com/AlexStocks/getty?tab=doc)
[![Go Report Card](https://goreportcard.com/badge/github.com/AlexStocks/getty)](https://goreportcard.com/report/github.com/AlexStocks/getty)
![license](https://img.shields.io/badge/license-Apache--2.0-green.svg)

## INTRO

Getty is a asynchronous network I/O library in golang. Getty works on tcp/udp/websocket network protocol and supplies [a uniform interface](https://github.com/alexstocks/getty/blob/master/getty.go#L45).

In getty there are two goroutines in one connection(session), one reads tcp stream/udp packet/websocket package, the other handles logic process and writes response into network write buffer. If your logic process may take a long time, you should start a new logic process goroutine by yourself in codec.go:(Codec)OnMessage.

You can also handle heartbeat logic in codec.go:(Codec):OnCron. If you use tcp/udp, you should send hearbeat package by yourself, and then invoke session.go:(Session)UpdateActive to update its active time. Please check whether the tcp session has been timeout or not in codec.go:(Codec)OnCron by session.go:(Session)GetActive.

Whatever if you use websocket, you do not need to care about hearbeat request/response because Getty do this task in session.go:(Session)handleLoop by sending/received websocket ping/pong frames. You just need to  check whether the websocket session has been timeout or not in codec.go:(Codec)OnCron by session.go:(Session)GetActive.

You can get code example in https://github.com/AlexStocks/getty-examples.

## RPC

An open source, Go based, RPC framework.

Feature list:

- 1 Transport: TCP(√), UDP(√), Websocket(√)
- 2 Codec: ProtoBuf(√), JSON(√)
- 3 Strategy: Failover(√), Failfast(√)
- 4 Metrics: Invoke Statistics(X), User Auth(X)

Code example:

The subdirectory rpc of [getty-examples](https://github.com/alexstocks/getty-examples/) shows how to build rpc client/rpc server.

## Micro

An micro service framework based on getty/rpc.

Feature list:

- 1 Registry: ZooKeeper(√), Etcd(√)
- 2 Load Balance: Random(X), RoundRobin(√), [Self-Defined(√)](https://github.com/alexstocks/getty-examples/blob/master/micro/client/app/main.go#L86)
- 3 Service Discovery: Service Publish(√), Service Watch(√), Service Notify(√)

Code example:

The subdirectory micro of [getty-examples](https://github.com/alexstocks/getty-examples/) shows how to build micro client/rpc server.

## LICENCE

Apache License 2.0

