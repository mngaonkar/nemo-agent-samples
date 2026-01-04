# Triton Server
## Run docker image
`docker run --rm -it --net host --shm-size=2g \
    --ulimit memlock=-1 --ulimit stack=67108864 --gpus all \
    -v $PWD/llama2vllm:/opt/tritonserver/model_repository/llama2vllm \
    nvcr.io/nvidia/tritonserver:23.11-vllm-python-py3` 

## Pull model from Hugging Face
`huggingface-cli download meta-llama/Llama-2-7b-hf --local-dir /home/mahadev/llama2vllm/1/model
`

## Create model JSON
`cat ./1/model.json                                                                             
{
  "model": "/opt/tritonserver/model_repository/llama2vllm/1/model",
  "dtype": "float16",
  "tensor_parallel_size": 1,
  "gpu_memory_utilization": 0.90,
  "max_model_len": 4096
}
`

## Directory structure
```
root@mahadev-ms7d28:/opt/tritonserver/model_repository/llama2vllm/1/model# ls -l
total 26325768
-rw-r--r-- 1 triton-server triton-server       7020 Jan  4 00:16 LICENSE.txt
-rw-r--r-- 1 triton-server triton-server      22295 Jan  4 00:16 README.md
-rw-r--r-- 1 triton-server triton-server    1253223 Jan  4 00:18 Responsible-Use-Guide.pdf
-rw-r--r-- 1 triton-server triton-server       4766 Jan  4 00:18 USE_POLICY.md
-rw-r--r-- 1 triton-server triton-server        609 Jan  4 00:18 config.json
-rw-r--r-- 1 triton-server triton-server        188 Jan  4 00:18 generation_config.json
-rw-r--r-- 1 triton-server triton-server 9976578928 Jan  4 00:25 model-00001-of-00002.safetensors
-rw-r--r-- 1 triton-server triton-server 3500297344 Jan  4 00:21 model-00002-of-00002.safetensors
-rw-r--r-- 1 triton-server triton-server      26788 Jan  4 00:18 model.safetensors.index.json
-rw-r--r-- 1 triton-server triton-server 9976634558 Jan  4 00:25 pytorch_model-00001-of-00002.bin
-rw-r--r-- 1 triton-server triton-server 3500315539 Jan  4 00:22 pytorch_model-00002-of-00002.bin
-rw-r--r-- 1 triton-server triton-server      26788 Jan  4 00:18 pytorch_model.bin.index.json
-rw-r--r-- 1 triton-server triton-server        414 Jan  4 00:18 special_tokens_map.json
-rw-r--r-- 1 triton-server triton-server    1842767 Jan  4 00:18 tokenizer.json
-rw-r--r-- 1 triton-server triton-server     499723 Jan  4 00:18 tokenizer.model
-rw-r--r-- 1 triton-server triton-server        776 Jan  4 00:18 tokenizer_config.json

```

## Run Triton server
`tritonserver --model-repository /opt/tritonserver/model_repository/`

## Test from a client
`curl -X POST http://mahadev-ms7d28:8000/v2/models/llama2vllm/generate -d '{"text_input": "What is Linux?", "parameters": {"stream": false, "temperature": 0, "max_tokens":256}}`  

# NeMo Agent
## Installation
`pip install nvidia-nat nvidia-nat-langchain
`

## Create a sample workflow
Check repository yml files

## Run agent
`
nat run --config_file basic-agent/workflow-nvidia-cloud.yml --input "What is quantum computing"
`



