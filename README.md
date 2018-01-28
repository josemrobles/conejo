# Conejo [![Go Report Card](https://goreportcard.com/badge/github.com/josemrobles/conejo)](https://goreportcard.com/report/github.com/josemrobles/conejo) [![GoDoc](https://godoc.org/github.com/josmrobles/conejo?status.svg)](https://godoc.org/github.com/josemrobles/conejo) [![Sourcegraph](https://sourcegraph.com/github.com/josemrobles/conejo/-/badge.svg)](https://sourcegraph.com/github.com/josemrobles/conejo?badge)

Golang lib for RabbitMQ Connecting, Consuming, and Publishing. Needs a great deal of refining, but it's quick & dirty and gets the job done for my current project. WIll definitely refactor in the near future.

## Status

Currently on hold. I use this lib for several personal projects so all should be working as intended. 

## Goals

* [ ] Unit Tests
  * [ ] main.go
  * [ ] channel.go
  * [ ] producer.go
  * [ ] queue.go
  * [ ] exchange.go

## Usage

### Sample Producer
```go
package main

import (
  "github.com/josemrobles/conejo"
)

var (
  rmq      = conejo.Connect("amqp://guest:guest@localhost:5672")
  workQueue = make(chan string) 
  queue    = conejo.Queue{Name: "queue_name", Durable: false, Delete: false, Exclusive: false, NoWait: false}
  exchange = conejo.Exchange{Name: "exchange_name", Type: "topic", Durable: true, AutoDeleted: false, Internal: false, NoWait: false}
)

func main() {
  err := conejo.Publish(rmq, queue, exchange, "{'employees':[{'firstName':'John','lastName':'Doe'}]}")
  if err != nil {
    print("fubar")
  }
}
```

### Sample Consumer
```go
package main

import (
  "github.com/josemrobles/conejo"
)

var (
  rmq       = conejo.Connect("amqp://guest:guest@localhost:5672")
  queue     = conejo.Queue{Name: "queue_name", Durable: false, Delete: false, Exclusive: false, NoWait: false}
  exchange  = conejo.Exchange{Name: "exchange_name", Type: "topic", Durable: true, AutoDeleted: false, Internal: false, NoWait: false}
  
)

func main() {
  err := conejo.Consume(rmq, queue, exchange, "consumer_tag", workQueue)
  if err != nil {
    print("ERROR: %q", err)
  }
}
```
