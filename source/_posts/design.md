---
title: design
date: 2024-09-19 10:36:07
tags:
---


### 选项模式

```go
package main

import (
	"fmt"
	"time"
)

type Server struct {
	Addr         string
	Port         int
	ReadTimeout  time.Duration
	WriteTimeout time.Duration
	Timeout      time.Duration
}

type ServerOption func(*Server)

func WithAddr(addr string) ServerOption {
	return func(s *Server) {
		s.Addr = addr
	}
}

func WithPort(port int) ServerOption {
	return func(s *Server) {
		s.Port = port
	}
}

func NewServer(options ...ServerOption) *Server {
	server := &Server{
		Addr:         "0.0.0.0",
		Port:         8080,
		ReadTimeout:  10 * time.Second,
		WriteTimeout: 10 * time.Second,
		Timeout:      10 * time.Second,
	}

	for _, option := range options {
		option(server)
	}

	return server
}

func main() {
	s1 := NewServer()
	fmt.Println(s1)

	s2 := NewServer(WithAddr("127.0.0.1"))
	fmt.Println(s2)

	s3 := NewServer(WithAddr("127.0.0.1"), WithPort(6379))
	fmt.Println(s3)
}

```


### 装饰器模式

```go

package main

import (
	"fmt"
	"net/http"
	"time"
)

type Handler func(http.ResponseWriter, *http.Request)

func Logger(next Handler) Handler {
	return func(w http.ResponseWriter, r *http.Request) {
		now := time.Now()
		next(w, r)
		fmt.Printf("url: %s, method: %s, time: %v\n", r.URL.Path, r.Method, time.Since(now))
	}
}

func HelloWorld(w http.ResponseWriter, req *http.Request) {
	w.WriteHeader(http.StatusOK)
	w.Write([]byte("Hello World"))
}

func HowAreYou(w http.ResponseWriter, r *http.Request) {
	w.WriteHeader(http.StatusOK)
	w.Write([]byte("I am fine"))
}

func main() {
	mux := http.NewServeMux()

	mux.HandleFunc("GET /hello", Logger(HelloWorld))
	mux.HandleFunc("GET /how", Logger(HowAreYou))

	svc := http.Server{
		Addr:    ":8088",
		Handler: mux,
	}

	svc.ListenAndServe()
}

```