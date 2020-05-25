# Cache gin's middleware

[![Build Status](https://travis-ci.org/gin-contrib/cache.svg)](https://travis-ci.org/gin-contrib/cache)
[![codecov](https://codecov.io/gh/gin-contrib/cache/branch/master/graph/badge.svg)](https://codecov.io/gh/gin-contrib/cache)
[![Go Report Card](https://goreportcard.com/badge/github.com/gin-contrib/cache)](https://goreportcard.com/report/github.com/gin-contrib/cache)
[![GoDoc](https://godoc.org/github.com/gin-contrib/cache?status.svg)](https://godoc.org/github.com/gin-contrib/cache)

Gin middleware/handler to enable Cache.

## Usage

### Start using it

Download and install it:

```sh
$ go get github.com/dennypenta/cache
```

Import it in your code:

```go
import "github.com/dennypenta/cache"
```

### Canonical example:

See the [example](example/example.go)

```go
package main

import (
	"fmt"
	"time"

	"github.com/dennypenta/cache"
	"github.com/dennypenta/cache/persistence"
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()

	store := persistence.NewInMemoryStore(time.Second)
	
	r.GET("/ping", func(c *gin.Context) {
		c.String(200, "pong "+fmt.Sprint(time.Now().Unix()))
	})
	// Cached Page
    // CachePage在并发时会引起缓存数据错误，推荐使用CachePageAtomic
	r.GET("/cache_ping", cache.CachePageAtomic(store, time.Minute, func(c *gin.Context) {
		c.String(200, "pong "+fmt.Sprint(time.Now().Unix()))
	}))

	// Listen and Server in 0.0.0.0:8080
	r.Run(":8080")
}
```
