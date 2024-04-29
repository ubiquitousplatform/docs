# Architecture Decision Record: Use WebAssembly for Functions

## Context



## Decision

Use Extism as the WASM host.


## Justification

Extism is a thin wrapper around webassembly, but crucially, it provides SDKs and PDKs in many languages that make it easy to build software in many guest languages.  It is not super opinionated and is thus a great base to build our platform on.

Some important criteria in making this decision:

1. Host Functions must be fully supported in .NET as an SDK language and in C/C++, JS, Rust, Go, and .NET as guest languages, as these are the top most used languages that are easily supportable by WASM.  Python will be closely watched as it's also a top used language

## Downsides

There is not currently a PDK for Ruby or Python, and ideally these languages would be supported soon.  There is work within the Python community to bring WASM up to a level 1 runtime system, but Python's module loader depends on a lot of OS functionality being available.  Python also needs to have support for tree-shaking or the runtime needs to be able to be loaded as a separate dynamic dependency.

The JS PDK doesn't currently support externalizing the runtime.

## Alternatives Considered

- **Wasmtime**: Wasmtime was investigated as a direct way to wrap functions up.  However, Wasmtime.NET is not incredibly mature, and Extism is a much smoother way to implement webassembly calls from .NET, including an easy Host Functions implementation.
- **Wasmer**: Wasmer is the "move fast and break things" runtime, which is not compatible with this project's vision of having a stable and consistent implementation.
- **Suborbital**:
- **Cosmonic**: Cosmonic is its own ecosystem and follows the distributed actor framework.  It's cool, but not really relevant to Ubiquitous
- **Spin**: Spin by Fermyon is very opinionated and is being built in a particular direction that doesn't align with the Ubiquitous project's goals.  It's intended to be scaled out by a separate orchestration layer and is very low-level, meaning it gives direct access to PGSQL sockets and Redis interfaces to your code.  Ubiquitous is intended to be a high level runtime more akin to something like Rails.

## References

