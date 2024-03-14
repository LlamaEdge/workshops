
1 Quickstart!

```
bash <(curl -sSfL 'https://raw.githubusercontent.com/LlamaEdge/LlamaEdge/main/run-llm.sh')
```

Access the chatbot UI from a local web browser:

```
http://localhost:8080/
```

2 Choose a different LLM

```
bash <(curl -sSfL 'https://raw.githubusercontent.com/LlamaEdge/LlamaEdge/main/run-llm.sh') --interactive
```

3 Get sample source code

```
git clone https://github.com/second-state/WasmEdge-WASINN-examples
cd WasmEdge-WASINN-examples
```

4 A basic example

Build the basic example

```
cd wasmedge-ggml/basic
cargo build --target wasm32-wasi --release
cp target/wasm32-wasi/release/wasmedge-ggml-basic.wasm .
```

Download a non-chat model

```
curl -LO https://huggingface.co/second-state/StarCoder2-7B-GGUF/resolve/main/starcoder2-7b-Q5_K_M.gguf
```

Run it!

```
wasmedge --dir .:. \
  --env n_predict=100 \
  --nn-preload default:GGML:AUTO:starcoder2-7b-Q5_K_M.gguf \
  wasmedge-ggml-basic.wasm default
```

Try a few examples.

```
USER:
def print_hello_world():

USER:
fn is_prime(n: u64) -> bool {

USER:
Write a Rust function to check if an input number is prime:
```

5 A portable chat example

Build a chat example

```
cd wasmedge-ggml/llama
cargo build --target wasm32-wasi --release
cp target/wasm32-wasi/release/wasmedge-ggml-llama.wasm .
```

Copy it to another device with a different GPU!

```
scp -i YOUR-KEY.pem wasmedge-ggml-llama.wasm remoteuser@ip-addr:/home/remoteuser
```

Download a chat model

```
curl -LO https://huggingface.co/second-state/Llama-2-7B-Chat-GGUF/resolve/main/Llama-2-7b-chat-hf-Q5_K_M.gguf
```

Run it!

```
wasmedge --dir .:. \
  --nn-preload default:GGML:AUTO:Llama-2-7b-chat-hf-Q5_K_M.gguf \
  wasmedge-ggml-llama.wasm default
```


