# Google Secret Manager Emulator

> [!NOTE]
> Forked from charlesgreen/gsm until charlesgreen/gsm#1 is merged. Otherwise several
> bits of functionality don't actually work, especially for Go clients.

## Usage in tests

```go
package main

import (
    "testing"
    "github.com/coxley/gsm/gsmtest"
)

func TestTCP(t *testing.T) {
    gsm, err := gsmtest.New(t)
    if err != nil {
        t.Fatal(err)
    }

    ctx, cancel := context.WithCancel(context.Background())
    defer cancel()
    go gsm.Start(ctx)

    client, err := gsm.Client(ctx)
    if err != nil {
        t.Fatal(err)
    }
    defer client.Close()
    // Use normally
}

func TestMem(t *testing.T) {
    // Uses local buffer instead of network sockets. Enables use with new packages like
    // testing/synctest
    gsm, err := gsmtest.New(t, gsmtest.InMemory())
    if err != nil {
        t.Fatal(err)
    }
    // Same as other test
}
```
