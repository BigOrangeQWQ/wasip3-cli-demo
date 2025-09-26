```sh
moon build --no-strip -C cli --target wasm
wasm-tools component embed --encoding utf16 -w command wit-0.3.0-draft cli/target/wasm/release/build/gen/gen.wasm -o cli/target/wasm/release/build/gen/gen.wasm
wasm-tools component new cli/target/wasm/release/build/gen/gen.wasm -o cli/target/wasm/release/build/gen/gen.wasm
RUST_LOG=trace WASMTIME_LOG=wasmtime_wasi=trace wasmtime -W component-model-async-builtins -W component-model-async -S p3 -D debug-info -D logging cli/target/wasm/release/build/gen/gen.wasm
```