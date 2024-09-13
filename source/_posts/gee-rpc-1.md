---
title: gee-rpc 1
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


### 数据结构

---

#### 数据

```go
type Option struct {
	MagicNumber int
	CodecType   codec.Type
}
```

```go
type Header struct {
	ServiceMethod string
	Seq           uint64
	Error         string
}
```

```go
type request struct {
	h            *codec.Header
	argv, replyv reflect.Value
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




