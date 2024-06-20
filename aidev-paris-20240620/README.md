# AI_DEV Paris

## Demo #1: Build a web chatbot for open-source LLMs

**Quick start:**

```
bash <(curl -sSfL 'https://raw.githubusercontent.com/LlamaEdge/LlamaEdge/main/run-llm.sh') --interactive
```

Follow the prompts on the screen to run an open-source LLM on your own machine.

**Change a more fancy WebUI**

The API base URL of LlamaEdge is `http://localhost:8080/v1`. Since LlamaEdge will build an OpenAI API compatible with the open-source LLM, you can easily integrate LlamaEdge with other OpenAI-based frameworks.

See details here: https://llamaedge.com/docs/user-guide/openai-api/lobechat

## Demo #2: Build a RAG based LLM applications


LlamaEdge provides a Rust-based development platform that enables developers to combine RAG logic into the chat API server.

See the tutorial here: https://llamaedge.com/docs/user-guide/server-side-rag/quick-start

```
wasmedge --dir .:. \
   --nn-preload default:GGML:AUTO:Meta-Llama-3-8B-Instruct-Q5_K_M.gguf \
   --nn-preload embedding:GGML:AUTO:all-MiniLM-L6-v2-ggml-model-f16.gguf \
   rag-api-server.wasm -p llama-3-chat --web-ui ./chatbot-ui \
     --model-name Meta-Llama-3-8B-Instruct-Q5_K_M,all-MiniLM-L6-v2-ggml-model-f16 \
     --ctx-size 4096,384 \
     --rag-prompt "Use the following context to answer the question.\n----------------\n" \
     --log-prompts --log-stat
```


Or you can quickly start with the gaianet CLI tool.

```
curl -sSfL 'https://raw.githubusercontent.com/GaiaNet-AI/gaianet-node/main/install.sh' | bash
```


**Use your knowledge embeddings**

See details: https://llamaedge.com/docs/category/server-side-rag

## Demo #3: Integrate the RAG API server to other frameworks

Replace the OpenAI URL with the base URL `http://localhost:8080/v1`.

See details: TBD

## Use Kubernetes to manage your LLM workloads

See details: https://github.com/WasmEdge/docs/pull/234




