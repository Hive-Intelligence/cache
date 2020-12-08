# Cache gin's middleware

[![Build Status](https://travis-ci.org/Hive-Intelligence/cache.svg)](https://travis-ci.org/Hive-Intelligence/cache)
[![codecov](https://codecov.io/gh/Hive-Intelligence/cache/branch/master/graph/badge.svg)](https://codecov.io/gh/Hive-Intelligence/cache)
[![Go Report Card](https://goreportcard.com/badge/github.com/Hive-Intelligence/cache)](https://goreportcard.com/report/github.com/Hive-Intelligence/cache)
[![GoDoc](https://godoc.org/github.com/Hive-Intelligence/cache?status.svg)](https://godoc.org/github.com/Hive-Intelligence/cache)

Gin middleware/handler to enable Cache.

## Usage

### Start using it

Download and install it:

```sh
$ go get github.com/Hive-Intelligence/cache
```

Import it in your code:

```go
import "github.com/Hive-Intelligence/cache"
```

### Canonical example:

See the [example](example/example.go)

```go
package main

import (
	"fmt"
	"time"

	"github.com/Hive-Intelligence/cache"
	"github.com/Hive-Intelligence/cache/persistence"
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()

	store := persistence.NewInMemoryStore(time.Second)
	
	r.GET("/ping", func(c *gin.Context) {
		c.String(200, "pong "+fmt.Sprint(time.Now().Unix()))
	})
	// Cached Page
	r.GET("/cache_ping", cache.CachePage(store, time.Minute, func(c *gin.Context) {
		c.String(200, "pong "+fmt.Sprint(time.Now().Unix()))
	}))

	// Listen and Server in 0.0.0.0:8080
	r.Run(":8080")
}
```
