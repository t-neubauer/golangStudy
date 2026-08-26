# Concepts

This file tracks Go concepts learned in this repository.

## How Entries Are Stored

Each concept entry should include:

- Concept
- Summary
- Go Example
- C#/.NET Comparison
- Kubernetes/Edge Relevance

## Basic Context

- C#/.NET Comparison: Similar to starting with a console app in C#, but Go emphasizes simple tooling and lightweight binaries.
- Kubernetes/Edge Relevance: Small static binaries and fast startup are useful for containerized and edge-deployed workloads.

## Concepts Log

### `panic`

- Summary: Stops normal execution and begins stack unwinding when Go encounters an unrecoverable condition. Use `error` values for expected failures; use `recover()` inside a deferred function only when a panic must be handled.
- Go Example:
    ```go
    func safeOperation() {
            defer func() {
                    if recovered := recover(); recovered != nil {
                            fmt.Println("recovered:", recovered)
                    }
            }()

            panic("unexpected failure")
    }
    ```
- C#/.NET Comparison: Similar to throwing an exception and handling it with `catch`, but Go normally returns explicit `error` values for expected failures. Go recovery uses `defer` and `recover()` and must happen in the same goroutine.
- Kubernetes/Edge Relevance: A startup panic can terminate a container so Kubernetes can restart it, while request-level or operational failures should usually be returned as errors so the service remains available.


