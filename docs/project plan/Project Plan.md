

## Get WebAssembly Host Functions working

* Use an existing Rust example to read stdin/stdout using wasmtime on both sides.
* Then try putting a host function call and use dependent data so we know that it worked on both sides (just ints for simplicity)
* Then get it working with strings as a different method
* Then get wasmtime.net project set up with the first Rust example (just stdin/stdout)
* Then test the host function with just the ints
* Then the host function with strings

* Once that's working, make an example tinygo and javy version that calls the same string host function
* Then wrap these into single command compilers that are cross-platform.
* Figure out how to get async/await working in javy
* Fork javy and make a version that uses the module system and the patched host function, so you just pass in the js code
* investigate the possibility of doing the same shared runtime with tinygo like javy does

Host Functions examples:
https://www.secondstate.io/articles/extend-webassembly/
- Host is Go, Rust is the plugin language
- Example .wasm file already precompiled
- Passes string from plugin to host via pointer and length
- To get string return value from function, the following process happens:
	- Host function returns just a response length in bytes
	- Plugin allocates length bytes
	- Plugin calls ANOTHER host function, write_mem, passing a pointer to the memory location that was allocated
- This is complicated, and would be a lot simpler if the host function could allocate memory in the address space, put the response in that memory space, and then return a pointer to that memory space instead.
	- Is this not possible? does the host not have access to allocate memory in the plugin?



## Phase 1

* JavaScript Only
* Implement Logging module, Config module, Data module, and writing to DB



Website
Discord invite: https://discord.gg/Uvtaq6NQ3M