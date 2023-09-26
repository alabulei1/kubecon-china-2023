# Run CodeLLaMa 2 on your own compuer with WasmEdge

[See it in action!](https://x.com/juntao/status/1705588244602114303)

## Requirement

### For macOS (apple silicon)

Install WasmEdge 0.13.4+WASI-NN ggml plugin(Metal enabled on apple silicon) via installer

```bash
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash -s -- --plugin wasi_nn-ggml
# After install the wasmedge, you have to activate the environment.
# Assuming you use zsh (the default shell on macOS), you will need to run the following command
source $HOME/.zshenv
```

### For Ubuntu (>= 20.04)

Because we enabled OpenBLAS on Ubuntu, you must install `libopenblas-dev` by `apt update && apt install -y libopenblas-dev`.


Install WasmEdge 0.13.4+WASI-NN ggml plugin(OpenBLAS enabled) via installer

```bash
apt update && apt install -y libopenblas-dev # You may need sudo if the user is not root.
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash -s -- --plugin wasi_nn-ggml
# After install the wasmedge, you have to activate the environment.
# Assuming you use bash (the default shell on Ubuntu), you will need to run the following command
source $HOME/.bashrc
```

### For General Linux

Install WasmEdge 0.13.4+WASI-NN ggml plugin via installer

```bash
curl -sSf https://raw.githubusercontent.com/WasmEdge/WasmEdge/master/utils/install.sh | bash -s -- --plugin wasi_nn-ggml
# After install the wasmedge, you have to activate the environment.
# Assuming you use bash (the default shell on Ubuntu), you will need to run the following command
source $HOME/.bashrc
```

## Prepare WASM application

### (Recommend) Use the pre-built one bundled in this repo

We built a wasm of this example under the folder, check `wasmedge-ggml-llama-interactive.wasm`

### (Optional) Build from source

If you want to do some modifications, you can build from source.

Compile the application to WebAssembly:

```bash
cargo build --target wasm32-wasi --release
```

The output WASM file will be at `target/wasm32-wasi/release/`.

```bash
cp target/wasm32-wasi/release/wasmedge-ggml-llama-interactive.wasm ./wasmedge-ggml-llama-interactive.wasm
```

## Get Model

In this example, we are going to use codellama2 7b chat model in GGUF format.

Download llama model:

```bash
curl -LO https://huggingface.co/TheBloke/CodeLlama-7B-Instruct-GGUF/resolve/main/codellama-7b-instruct.Q5_K_M.gguf
```

## Execute

Execute the WASM with the `wasmedge` using the named model feature to preload large model:

```bash
wasmedge --dir .:. \              
  --nn-preload default:GGML:CPU:codellama-7b-instruct.Q5_K_M.gguf \
  wasmedge-ggml-llama-interactive.wasm default
```

After executing the command, you may need to wait a moment for the input prompt to appear.
You can enter your question once you see the `Question:` prompt:

```console
Question:

编写一个Wasmedge Hello World 程序

Answer:

#include <wasmedge.h>

int main() {
    // Create a new WasmEdge instance
    WasmEdge::WasmEdge wasmedge;

    // Load a Wasm module from a file
    wasmedge.loadModule("hello.wasm");

    // Instantiate the Wasm module
    wasmedge.instantiate();

    // Call the `hello` function
    wasmedge.callFunction("hello");

    // Return 0 to indicate success
    return 0;
}

This program creates a new WasmEdge instance, loads a Wasm module from a file, instantiates the module, and then calls the `hello` function. The `hello` function is defined in the Wasm module and is called using the `callFunction` method. The program returns 0 to indicate success.
```

## Credit

The WASI-NN ggml plugin embedded [`llama.cpp`](git://github.com/ggerganov/llama.cpp.git@b1217) as its backend.
