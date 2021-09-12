---
title: "插件系统"
date: 2021-09-12T19:38:14+08:00
draft: false
weight: 1
---

# Go的插件系统

Go的底层使用c的插件系统实现了一套go的插件系统

支持生成`.so`格式的动态链接库以供C/C++和golang调用

## 引入库

```go
import "plugin"
```

### 核心api

plugin库只有两个核心接口

`plugin.Open()` 用于打开一个路径下的动态链接

`plugin.Lookup()` 用于寻找动态链接中的符号

```go
// Lookup searches for a symbol named symName in plugin p.
// A symbol is any exported variable or function.
// It reports an error if the symbol is not found.
// It is safe for concurrent use by multiple goroutines.
func (p *Plugin) Lookup(symName string) (Symbol, error) {
	return lookup(p, symName)
}
```

返回值为一个插件的`Symbol`接口用于接收来自动态链接的符号

```go
type Symbol interface{}
```



### 插件结构体

```go
// Plugin is a loaded Go plugin.
type Plugin struct {
	pluginpath string
	err        string        // set if plugin failed to load
	loaded     chan struct{} // closed when loaded
	syms       map[string]interface{}
}
```

插件的结构体定义中，`pluginpath`即动态链接的打开路径

`syms`为一个符号字典

### 生成动态链接

```bash
go build -buildmode=plugin -o xxx.so
```



## 示例

`test.so`为一个go编写的静态链接

```go
/*
Name: playgroundx
File: t.go
Author: Landers
*/

package main

import "fmt"

// TTInterface 测试用的动态链接
type TTInterface interface {
	Name(s string)
}

type TT struct {

}

// 插件初始化函数
func init()  {
	fmt.Println("plugin loaded")
}

func (t *TT) Name(s string) {
	fmt.Printf("Name is : %s\n", s)
}

// Simple 常规函数
func Simple() {
	fmt.Println("this is a simple func")
}

// NewTT 暴露出结构体
var NewTT = TT{}
```

```bash
go build -buildmode=plugin -o test.so t.go 
```

### 测试加载simple函数

```go
/*
Name: playgroundx
File: plugin_test.go
Author: Landers
*/

package _go

import (
	"plugin"
	"testing"
)

// 插件系统
func TestGoPlugin(t *testing.T) {
	p, e := plugin.Open("test.so")
	if e != nil {
		t.Fatal("plugin load failed")
	}

	// 读取常规函数Simple
	s, e := p.Lookup("Simple")
	if e != nil {
		t.Fatal("failed to look up symbol")
	}
	simpleFunc := s.(func())
	simpleFunc()
}
```

### 输出

```bash
➜ go test -v plugin_test.go                 
=== RUN   TestGoPlugin
plugin loaded
this is a simple func
--- PASS: TestGoPlugin (0.27s)
PASS
ok      command-line-arguments  0.670s
```

### 测试使用TT结构体

```go
/*
Name: playgroundx
File: plugin_test.go
Author: Landers
*/

package _go

import (
	"plugin"
	"testing"
)

// TTInterface 测试用的动态链接
type TTInterface interface {
	Name(s string)
}

// 插件系统
func TestGoPlugin(t *testing.T) {
	p, e := plugin.Open("test.so")
	if e != nil {
		t.Fatal("plugin load failed")
	}

	// 读取常规函数Simple
	s, e := p.Lookup("Simple")
	if e != nil {
		t.Fatal("failed to look up symbol")
	}
	simpleFunc := s.(func())
	simpleFunc()

	// 读取TT
	s, e = p.Lookup("NewTT")
	tt := s.(TTInterface)
	tt.Name("hello")
}
```

### 输出

```bash
➜ go test -v plugin_test.go
=== RUN   TestGoPlugin
plugin loaded
this is a simple func
Name is : hello
--- PASS: TestGoPlugin (0.00s)
PASS
ok      command-line-arguments  0.366s
```

可以看到对于`Symbol`的推断使用了反射机制

## 限制

只有**linux**和**mac**系统目前支持了插件系统

go编译的插件代码必须是`main`包
