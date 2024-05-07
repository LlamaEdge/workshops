## 0 All software and docs are open source on GitHub

Follow these repos:

https://github.com/LlamaEdge/LlamaEdge

https://github.com/GaiaNet-AI/gaianet-node

https://github.com/wasmedge/wasmedge

## 1 Quickstart

```
bash <(curl -sSfL 'https://raw.githubusercontent.com/LlamaEdge/LlamaEdge/main/run-llm.sh') --interactive
```

Select the Llama 3 8B model. Start the API server. Access the chatbot UI from a local web browser:

```
http://localhost:8080/
```

Example QA

```
Me: Where is Estes Park?

Bot: Estes Park is a town located in Larimer County, Colorado, USA. It's situated at the entrance to Rocky Mountain National Park and is known for its natural beauty and outdoor recreation opportunities.

Me: What's its elevation?

Bot: Estes Park's elevation is 7,800 feet (2,377 meters) above sea level.

Me: Should I be worried about altitude sickness when traveling there?

Bot: Yes, it's recommended to take precautions for altitude sickness. Estes Park is at a high elevation, and visitors who are not accustomed to it may experience symptoms like headaches, nausea, and fatigue. It's suggested to drink plenty of water, ascend gradually, and avoid strenuous activities in the first 24 hours.

Me: Can you recommend some medicine?

Bot: For altitude sickness, I recommend talking to your doctor or a pharmacist about medications like acetazolamide (Diamox) or dexamethasone (Decadron). However, it's essential to consult with a medical professional before taking any medication. They can help determine the best course of treatment for you.
```

## 2 Portable binary app across GPUs

Copy the compiled `wasm` binary app to my Azure VM (with Nvidia T4 and CUDA 12). Notice that we have demonstrated that the `wasm` app works on a Mac with Apple Silicon and Metal framework in the previous step. We just `scp` here. There is no code change nor even re-compiling.

```
scp -i path/to/server.pem llama-api-server.wasm azureuser@ip.addr:/home/azureuser/demo/
```

Go to that machine

```
ssh -i path/to/server.pem azureuser@ip.addr
cd demo
```

Start the `wasm` API server in the background.

```
nohup wasmedge --dir .:. --nn-preload default:GGML:AUTO:Meta-Llama-3-8B-Instruct-Q5_K_M.gguf llama-api-server.wasm --prompt-template llama-3-chat --model-name Meta-Llama-3-8B-Instruct --socket-addr 0.0.0.0:8080 --log-prompts --log-stat &
```

Test the API!

```
curl -X POST http://localhost:8080/v1/chat/completions \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"user", "content": "Where is Estes Park?"}]}'
```

Example answer:

```
{"id":"chatcmpl-8121b961-18b5-4c01-859d-da0621350d36","object":"chat.completion","created":1715091449,"model":"Meta-Llama-3-8B-Instruct","choices":[{"index":0,"message":{"role":"assistant","content":"Estes Park is a town in Colorado, USA. It's located at the entrance of Rocky Mountain National Park."},"finish_reason":"stop"}],"usage":{"prompt_tokens":44,"completion_tokens":25,"total_tokens":69}}
```

## 3 Run an RAG example

Install GaiaNet. You may be prompted for your password on the commnd line.

```
curl -sSfL 'https://raw.githubusercontent.com/GaiaNet-AI/gaianet-node/main/install.sh' | bash
```

Initialize the node.

```
gaianet init
```

Start the node.

```
gaianet start
```

Load the browser at http://localhost:8080/

Or, try the API endpoint:

```
curl -X POST http://localhost:8080/v1/chat/completions \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"messages":[{"role":"system", "content": "You are a helpful assistant."}, {"role":"user", "content": "Where is Paris?"}]}'
```

Checkout the file `~/gaianet/start-log.txt` to see the prompt supplementation and manipulation.

```
sh <(curl -sSfL 'https://raw.githubusercontent.com/GaiaNet-AI/gaianet-node/main/demo/stop.sh')
```

## 4 Get sample source code

```
git clone https://github.com/second-state/WasmEdge-WASINN-examples
cd WasmEdge-WASINN-examples
```
