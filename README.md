# Triton Server
## Run docker image
`docker run --rm -it --net host --shm-size=2g \
    --ulimit memlock=-1 --ulimit stack=67108864 --gpus all \
    -v $PWD/llama2vllm:/opt/tritonserver/model_repository/llama2vllm \
    nvcr.io/nvidia/tritonserver:23.11-vllm-python-py3` 

# Pull model from Hugging Face
`huggingface-cli download meta-llama/Llama-2-7b-hf --local-dir /home/mahadev/llama2vllm/1/model
`

# Create model JSON
`cat ./1/model.json                                                                             
{
  "model": "/opt/tritonserver/model_repository/llama2vllm/1/model",
  "dtype": "float16",
  "tensor_parallel_size": 1,
  "gpu_memory_utilization": 0.90,
  "max_model_len": 4096
}
`

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



