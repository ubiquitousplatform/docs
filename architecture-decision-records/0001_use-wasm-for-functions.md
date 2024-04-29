# Architecture Decision Record: Use WebAssembly for Functions

## Context

The Ubiquitous Functions Runtime needs an execution engine to run user code.  This engine needs to meet the following characteristics:

1. User code is as small as possible
2. It is quick to start an instance executing the user code (> 100 ms)
3. The code can be hot-reloaded at runtime (> 1 second to load new version)
4. The bundle is cross-platform, so it can be built on the user's OS and then executed on a server-oriented OS without rebuilding
5. There are adjustable 
6. Strong typing is supported in the host interface

## Decision

Use WebAssembly as the underlying implementation for the Ubiquitous Functions Runtime.


## Justification

One of the key goals of Ubiquitous is minimal dependencies and incredibly fast performance.  Containers require a minimal Kubernetes or Docker to be installed on your system to virtualize the software, which is difficult and not intuitive for beginners (eg. it takes multiple restarts to get going on Windows). Even StackOverflow's Developer Survey, which deals with professional software developers, showed that only 53% of developers are using Docker in 2023.  In fact, even with those learning languages, only 26% used Docker, vs 50% using NPM and 37% using Pip.  That is one of the reasons that Python and Node are so widely-adopted, because you run a single installer and then you can start making projects. They have their own package managers built in.  Python is better than Node because its standard library is very extensive.  We want to be a better Python, where the entire building and running system is all included in a single distributable binary.  Therefore, Containers are incompatible with the vision.

It would be possible to sandbox code without using an agnostic runtime, but then the runtime would not be available to the widest number of users possible.  Ideally, we want to expose a simple and minimally-invasive API surface, but let people use whatever language they are comfortable with - whether that's js, python, c#, f#, rust, go, ruby, or any other language.

Wasm provides the ability to sandbox and cross-platform portability without having the management overhead of a Container.  Additionally, wasm files are small and quick to initialize, meaning they can be quickly compiled and easily reloaded at runtime by an execution engine.

v8 isolates were considered, but it is difficult to safely run v8 isolates in a multitenant environment (Cloudflare is the only company currently doing this, and have had major security vulnerabilities as a result of this choice).

Some other advantages WASM has:

- **Performance**: WebAssembly provides near-native performance, making it suitable for computationally intensive tasks.
- **Portability**: WebAssembly is platform-independent and can run in any modern web browser or server environment.
- **Security**: WebAssembly runs in a sandboxed environment, reducing the risk of malicious code execution.
- **Interoperability**: WebAssembly can be used with multiple programming languages, allowing for code reuse and integration with existing systems.
- **Resource Efficiency**: WebAssembly modules are lightweight and have minimal memory footprint, making them efficient for scaling and deployment.

## Alternatives Considered

- **Containers**: Containers provide isolation and portability, but they come with additional overhead and complexity compared to WebAssembly. Containers require managing dependencies and infrastructure, whereas WebAssembly simplifies deployment and reduces operational overhead.
- **Sandboxing a specific language**: It's possible to strip out all system access and other things from a given language, but this constrains the implementation to a specific language.  For example, Deno is a runtime that is capability-based, so by default you can exclude things like network access from Deno.  However, there are many pitfalls in ensuring this is properly sandboxed, and WebAssembly follows a much more rigid security posture.


## References

