---
id: redirect
---

# Redirect

Redirection middleware for Fiber.

## Signatures

```go
func New(config ...Config) fiber.Handler
```

## Examples

```go
package main

import (
    "github.com/gofiber/fiber/v3"
    "github.com/gofiber/fiber/v3/middleware/redirect"
)

func main() {
    app := fiber.New()
    
    app.Use(redirect.New(redirect.Config{
      Rules: map[string]string{
        "/old":   "/new",
        "/old/*": "/new/$1",
      },
      StatusCode: fiber.StatusMovedPermanently,
    }))
    
    app.Get("/new", func(c fiber.Ctx) error {
      return c.SendString("Hello, World!")
    })
    app.Get("/new/*", func(c fiber.Ctx) error {
      return c.SendString("Wildcard: " + c.Params("*"))
    })
    
    app.Listen(":3000")
}
```

## Test

```bash
curl http://localhost:3000/old
curl http://localhost:3000/old/hello
```

## Config

| Property   | Type                    | Description                                                                                                                | Default                |
|:-----------|:------------------------|:---------------------------------------------------------------------------------------------------------------------------|:-----------------------|
| Next       | `func(fiber.Ctx) bool` | Filter defines a function to skip middleware.                                                                              | `nil`                  |
| Rules      | `map[string]string`     | Rules defines the URL path rewrite rules. The values captured in asterisk can be retrieved by index e.g. $1, $2 and so on. | Required               |
| StatusCode | `int`                   | The status code when redirecting. This is ignored if Redirect is disabled.                                                 | 302 Temporary Redirect |

## Default Config

```go
var ConfigDefault = Config{
    StatusCode: fiber.StatusFound,
}
```
