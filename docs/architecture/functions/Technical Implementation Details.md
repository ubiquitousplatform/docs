

The Functions runtime is implemented on top of [Extism](https://extism.org).



## Scheduling

#TODO Functions provides the ability to schedule


### Overprovisioning

#TODO Functions has customizable overprovisioning thresholds 


## Host Functions

Under the covers, [Extism](https://extism.org) is used as the runtime, allowing for both host and plugin development. However, [Extism](https://extism.org) is a more general plugin model, intended for direct dependencies on the plugin development kits.

Ubiquitous seeks to wrap and normalize all the plugin implementations with an understanding of the Ubiquitous host environmment out-of-the-box, so things like Logs, Cache, Events, etc. will "just work".

Some language runtimes, such as the Javascript runtime, require the host function implementation to be embedded within the compiler.  This means that, as host functions are added to the Ubiquitous runtime, all downstream Ubiquitous wrappers around Extism would have to change.

To obviate that issue, there is a single interop host function that provides all of the downstream host function functionality.  That single host function can be added statically to the PDK compilers once, and as long as that interface doesn't change, there can be an ongoing evolution of host functionality without breaking compatibility with the existing compiled PDKs or their compilers.

### Host Functions specification

The specification for a host function call between the Ubiquitous Functions layer and the Ubiquitous Functions Runtime layer is defined below.

#### Ubiquitous Functions Runtime

The runtime exposes a single method, `InvokeHost`, that takes 2 parameters.  The first is an `i32` enum defining the string format, and the second is a single C-String. #TODO is the C-String 3 params or 2? is it null-terminated or is there a length passed?)

In pseudocode, the following would be the method signature:
```
enum Format {
    RawJson = 0,
    RawMessagePack = 1
}

public (Format format, String response) InvokeHost(Format format, String response) {

}
```

A good mental model of how to think about this is that this simple protocol is a carrier to adapt all sorts of plugins to hosts in a static way, so that they dynamic nature can come from the Ubiquitous Functions Runtime and the Functions code itself, without having to rely on any underlying changes in the [Extism](https://extism.org) runtime.

On top of this simple interop pattern, each library will be able to call all of the definitions that are exposed by the API.

The IPC call itself is stringly-typed, but on each end, the parser ensures that the parameters are properly in line with expectations.

TODO: use http / REST over Host Functions instead of making custom implementations.  


Theory to test:

TypeSpec file generates OpenAPI spec
OpenAPI spec is fed into openapi-generator CLI
client libraries are generated for TypeScript / Go / Rust / C#
the client libraries are hooked into / adapted to build the HTTP request but then send it across the IPC boundary to the WASM host instead. headers, route, query parameters, and body are all parsed via JSON.

openapi-generator supports customizing of the generated code so it can be adapted for this situation.



## WebAssembly
### Web Assembly Component Model
We do not currently use the [Component Model](https://www.fermyon.com/blog/webassembly-component-model), because it is not yet mature (as of 05/18/2023, it is still in the first phase of proposal development).  However, the library that `ubiquitous` presents to each application is built with a clear separation between the types that are needed for IPC and the API for exposing those methods to the applications.  Therefore, in the future, a Component Model version of the core `ubiquitous` library should be fairly straightforward.

The Component Model and dynamic linking also allow the user-generated code to be much smaller (because a single bundled runtime can be shared by all users who target it) and boot faster (the shared runtime can be copied more efficiently into wasm memory because it is known beforehand and can be cached, and user code can depend on an already-scaled "runtime" instance for pre-scaling).

Javy is the only runtime that is currently exploring this concept, but as this approach gets more refined, we have plans to move in that direction as well.

As a starting point - [wit-bindgen](https://github.com/bytecodealliance/wit-bindgen#host-runtimes-for-components) can be used to generate components, and they can be injected into runtimes with `wasmtime`, although this is currently experimental.
