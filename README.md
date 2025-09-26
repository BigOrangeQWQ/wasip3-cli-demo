
## Script

```sh
moon build --no-strip -C cli --target wasm
wasm-tools component embed --encoding utf16 -w command wit-0.3.0-draft cli/target/wasm/release/build/gen/gen.wasm -o cli/target/wasm/release/build/gen/gen.wasm
wasm-tools component new cli/target/wasm/release/build/gen/gen.wasm -o cli/target/wasm/release/build/gen/gen.wasm
wasmtime -W component-model-async-builtins -W component-model-async -S p3 cli/target/wasm/release/build/gen/gen.wasm
```

## Code 

```moonbit
///|
/// Run the program.
pub async fn run() -> Result[Unit, Unit] {
  let task = @ffi.current_task()
  let (reader, writer) = @ffi.new_stream(
    @stdin.static_ffi_stream_reader_byte_stream_table,
  )
  let hello = "hello world\n".to_bytes().to_fixedarray()
  task.spawn(fn() {
    let _ = writer.write(hello)
    task.drop_waitable(writer)
  })
  task.spawn(fn() {
    let _ = @stdout.write_via_stream(reader)

  })
  Ok(())
}
```