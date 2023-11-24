# Go reference

### TOC

<!-- TOC -->
* [Go reference](#go-reference)
    * [TOC](#toc)
  * [Env](#env)
  * [Package management](#package-management)
* [Syntax](#syntax)
    * [References (* and &)](#references---and--)
    * [Strings](#strings)
    * [Maps](#maps)
    * [Arrays](#arrays)
    * [Conditions](#conditions)
  * [Logging](#logging)
    * [Fatal](#fatal)
  * [Type assertions](#type-assertions)
  * [Functions](#functions)
  * [Misc](#misc)
    * [Gin](#gin)
    * [Profiling (for gin)](#profiling--for-gin-)
<!-- TOC -->

## Env

```bash
# Print
go env

# Edit
go env -w GONOPROXY=myurl
```
[readme.md](..%2F..%2F_OTHER%2F_DOCS%2Flanguages%2FPython%2Freadme.md)
## Package management
```bash
go get githu.com/path-to-package # Download. Add `@v1.2.8` for specific version
go mod init # Init project
go mod tidy  # Solve dependencies
````

Reload with local module

In go .mod put before require

	replace github.com/example/path-to-module => local/path/to/module


# Syntax

### References (* and &)

[https://gist.github.com/josephspurrier/7686b139f29601c3b370](https://gist.github.com/josephspurrier/7686b139f29601c3b370)

### Strings

Unescaped raw strings

```go
	a := "\n" // This is one character, a line break.
	b := `\n` // These are two characters, backslash followed by letter n.
```

### Maps

```go
// Create map (dict) of misc types
data :=  map[string]interface{}  // Can be {'key': 'value', 'key2': 64}
```

### Arrays
```go
// Create array (list) of misc types
data := map[string]interface{}  // Can be {'key': 'value', 'key2': 64}

// Array unpacking, e.g. insert array as separate parameters
my_func(arr...)
```

### Conditions

Define variable only for if scope

```go
if err := thisCouldFail(); err != nil { 
    log.Fatal("oh no")
}
```

## Logging

### Fatal
This not only log, but also do sys exit

	log.Fatalf("Oh no: %s", err)


## Type assertions

```go

_, ok := x.(string)  // Now i can use all functions accessible to string
if !ok {
    // ...
}

switch v.(type) {
    case Comment: return true
    // ...
}
```

## Functions

Variadic functions - Accept unknown number of arguments
```go
func sum(nums ...int) {
    ...
}
```

## Misc

### Gin

Gin is a library that simplify creation of http servers


```go
// Expose gin on other port
err := router.Run(":8081")

// Get request body to json
var response map[string]interface{}
data, err := ioutil.ReadAll(res.Body)
if err != nil {
    return nil, err
}

err = json.Unmarshal(data, &response)
if err != nil {
    return nil, err
}
```

### Profiling (for gin)

Add to import

	"github.com/gin-contrib/pprof"

Add to router

	pprof.Register(router)

Run profiler with some profile

	curl http://localhost:8080/debug/pprof/heap > heap.out
	curl http://localhost:8080/debug/pprof/goroutine > goroutine.out
	curl http://localhost:8080/debug/pprof/block > block.out


Display on web

	go tool pprof -http=:8081 -nodefraction=0 heap.out
