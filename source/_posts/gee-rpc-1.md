---
title: gee-rpc
date: 2024-09-13 10:39:55
tags:
---

### gee-rpc

```c++
/*

a request

| Option{MagicNumber: xxx, CodecType: xxx} | Header{ServiceMethod ...} | Body interface{} |
| <------      固定 JSON 编码      ------>  | <-------   编码方式由 CodeType 决定   ------->|

one connection many requests

| Option | Header1 | Body1 | Header2 | Body2 | ...

*/

```

---

#### rpc全流程

![rpc](https://ACCLE123.github.io/picx-images-hosting/image.2rv3bggfz7.webp)


#### rpc条件

`func (t *T) MethodName(argType T1, replyType *T2) error`


---

### 数据结构

---

#### server

```go
type Option struct {
	MagicNumber int
	CodecType   codec.Type
}

type Header struct {
	ServiceMethod string
	Seq           uint64
	Error         string
}

type request struct {
	h            *codec.Header
	argv, replyv reflect.Value
}

type Server struct{}
```

---

#### client

```go
type Call struct {
	Seq           uint64
	ServiceMethod string
	Args          interface{}
	Reply         interface{}
	Error         error
	Done          chan *Call
}

func (call *Call) done() {
	call.Done <- call
}

type Client struct {
	cc       codec.Codec
	opt      *Option
	sending  sync.Mutex // protect cc
	header   codec.Header
	mu       sync.Mutex
	seq      uint64
	pending  map[uint64]*Call
	closing  bool // user has called Close
	shutdown bool // server has told us to stop
}
```


---

#### 编码解码


```go

type GobCodec struct {
	conn io.ReadWriteCloser
	buf  *bufio.Writer
	dec  *gob.Decoder
	enc  *gob.Encoder
}

type Codec interface {
	io.Closer
	ReadHeader(*Header) error
	ReadBody(interface{}) error
	Write(*Header, interface{}) error
}

```

---

### 处理细节

---

#### server流程

1. Accepct 并行接受链接 生成conn
2. conn 和 Option 生成对应的 codec
3. codec 并行处理 request
4. request处理后响应client


---

#### 协程

协程处理conn

```go
func Accpet(lis net.Listen) {
    for {
        conn := lis.Accept()
        go handleConn(conn)
    }
}
```

一个conn使用协程处理多个request

```go
func handleCodec(cc codec.Codec) {
    for {
        request := readRequest(cc)
        sending := &sync.Mutex{}
        wg := &sync.WaitGroup{}

        wg.Add(1)
        go handleRequest(request, wg, sending)
    }

    wg.Wait()
    cc.Close()
}


func handleRequest(request Request, wg sync.WaitGroup, sending sync.Mutex) {
    defer wg.Done()
    
    sending.Lock()
    defer sending.Unlock()
    somethind := struct{}{}
    cc.Write(something)
}
```






