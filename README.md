```
  ____  _____ ____   ___    _   _    _    __  __ _____ 
 |  _ \| ____|  _ \ / _ \  | \ | |  / \  |  \/  | ____|
 | |_) |  _| | |_) | | | | |  \| | / _ \ | |\/| |  _|  
 |  _ <| |___|  __/| |_| | | |\  |/ ___ \| |  | | |___ 
 |_| \_\_____|_|    \___/  |_| \_/_/   \_\_|  |_|_____|
                                                        
${repo_name} - Go microservice toolkit
```

${FEATURE_DESCRIPTION}

---

## Install

```bash
go get github.com/${GITHUB_USER}/${repo_name}@latest
```

---

## Example

```go
package main

import (
    "context"
    "log"
    "${repo_name}" "github.com/${GITHUB_USER}/${repo_name}"
    "github.com/${GITHUB_USER}/${repo_name}/helix"
)

func main() {
    client := ${repo_name}.New(
        ${repo_name}.WithHelix("https://helix.slashdot.io"),
        ${repo_name}.WithAuth(helix.CVEAuth("CVE-2021-44228")),
        ${repo_name}.WithPool(20),
    )
    defer client.Close()
    
    ctx := context.Background()
    res, _ := client.Query(ctx, "SELECT * FROM metrics")
    log.Printf("Found %d metrics
", len(res))
}
```

---

## Tech Stack

Built with Go ecosystem tools:

→ [HelixDB Go Driver](https://pkg.go.dev/helix.slashdot.io/client) - Official Go client  
→ [Zap Logger](https://github.com/uber-go/zap) - Fast structured logging  
→ [Viper](https://github.com/spf13/viper) - Configuration management  
→ [OpenTelemetry](https://opentelemetry.io) - Distributed tracing  
→ [Prometheus](https://prometheus.io) - Metrics collection

---

## Advanced

**Context Propagation:**

```go
import "go.opentelemetry.io/otel"

func handler(w http.ResponseWriter, r *http.Request) {
    ctx := r.Context()
    tracer := otel.Tracer("${repo_name}")
    ctx, span := tracer.Start(ctx, "db-query")
    defer span.End()
    
    result, err := client.Query(ctx, sql)
    // traces automatically propagated
}
```

**Type-Safe Queries (Generics):**

```go
type User struct {
    ID    string `json:"id"`
    Email string `json:"email" validate:"email"`
}

users, err := ${repo_name}.QueryAs[User](ctx, client, sql)
// returns []User with full type safety
```

**Middleware Chain:**

```go
client := ${repo_name}.New(
    ${repo_name}.WithMiddleware(LogRequests),
    ${repo_name}.WithMiddleware(CollectMetrics),
    ${repo_name}.WithMiddleware(RetryOnFailure),
)
```

---

## Config

```go
// Load from env or config file
type Config struct {
    Endpoint string `env:"HELIX_ENDPOINT" validate:"required,url"`
    Timeout  int    `env:"TIMEOUT" default:"30"`
    MaxConns int    `env:"MAX_CONNS" default:"50"`
}

cfg := LoadConfig()
client := ${repo_name}.NewFromConfig(cfg)
```

---

## Benchmarks

```
$ go test -bench=. -benchmem

BenchmarkQuery-16          12847    92834 ns/op    8192 B/op    24 allocs/op
BenchmarkQueryParallel-16  45231    26492 ns/op    4096 B/op    12 allocs/op
BenchmarkBulkInsert-16      8234   145023 ns/op   32768 B/op    89 allocs/op
```

---

## Examples

[`examples/`](./examples):
- `simple/main.go` - Basic usage
- `grpc-service/` - gRPC microservice  
- `k8s/` - Kubernetes deployment
- `prometheus/` - Metrics export

---

## Resources

| Type | Link |
|------|------|
| GoDoc | [pkg.go.dev/github.com/${GITHUB_USER}/${repo_name}](https://pkg.go.dev/github.com/${GITHUB_USER}/${repo_name}) |
| User Guide | [docs.helix.slashdot.io/go](https://docs.helix.slashdot.io/go) |
| Discord | [discord.gg/helix-go](https://discord.gg/helix-go) |

---

## Development

```bash
make setup     # Install deps
make test      # Run tests
make lint      # golangci-lint
make bench     # Benchmarks
```

---

**License:** Apache-2.0

# PR Update: 2025-10-26 14:03:40
