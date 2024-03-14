## 1 Quickstart!

```
bash <(curl -sSfL 'https://raw.githubusercontent.com/LlamaEdge/LlamaEdge/main/run-llm.sh')
```

Access the chatbot UI from a local web browser:

```
http://localhost:8080/
```

## 2 Choose a different LLM

```
bash <(curl -sSfL 'https://raw.githubusercontent.com/LlamaEdge/LlamaEdge/main/run-llm.sh') --interactive
```

## 3 Get sample source code

```
git clone https://github.com/second-state/WasmEdge-WASINN-examples
cd WasmEdge-WASINN-examples
```

## 4 A basic example

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

## 5 A portable chat example

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

SSH into the remote machine above and download a chat model.

```
curl -LO https://huggingface.co/second-state/Llama-2-7B-Chat-GGUF/resolve/main/Llama-2-7b-chat-hf-Q5_K_M.gguf
```

Run it!

```
wasmedge --dir .:. \
  --nn-preload default:GGML:AUTO:Llama-2-7b-chat-hf-Q5_K_M.gguf \
  wasmedge-ggml-llama.wasm default
```

## 6 Multimodal example

Build it

```
cd wasmedge-ggml/llava
cargo build --target wasm32-wasi --release
cp target/wasm32-wasi/release/wasmedge-ggml-llava.wasm .
```

Download models.

```
curl -LO https://huggingface.co/cmp-nct/llava-1.6-gguf/resolve/main/vicuna-7b-q5_k.gguf
curl -LO https://huggingface.co/cmp-nct/llava-1.6-gguf/resolve/main/mmproj-vicuna7b-f16.gguf
```

Download an image to analyze.

```
curl -LO https://llava-vl.github.io/static/images/monalisa.jpg
```

Run it!

```
wasmedge --dir .:. \
  --env mmproj=mmproj-vicuna7b-f16.gguf \
  --env image=monalisa.jpg \
  --env ctx_size=4096 \
  --nn-preload default:GGML:AUTO:vicuna-7b-q5_k.gguf \
  wasmedge-ggml-llava.wasm default
```

Questions to ask:

```
USER:
What is in that picture?

USER:
Who painted it?
```

## 7 Multimodal example with chatbot UI

https://www.secondstate.io/articles/llava-v1.6-vicuna-7b/

Use this image: https://www.planetware.com/wpimages/2022/02/spain-barcelona-top-attractions-basilica-sagrada-familia.jpg

Ask: 
* What is in that image?
* When is it supposed to complete?

## 8 Integrated RAG example

Preview: https://github.com/LlamaEdge/Example-LlamaEdge-RAG/blob/feat-server-multi-models/curl.md

Coming soon!

## 9 Fine-tune and RAG

Complete tutorial: https://github.com/YuanTony/chemistry-assistant


