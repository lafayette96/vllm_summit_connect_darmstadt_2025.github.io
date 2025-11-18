**Step 3 \- Run Model A (FP8 dynamic variant)**

We will be using **RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-FP8-dynamic** model.

This model was obtained by quantizing activations and weights of Mistral-Small-3.1-24B-Instruct-2503 to FP8 data type

Traditionally, FP32 (32-bit floating point) and FP16 (16-bit floating point) have been the go-to formats for machine learning models. However, as LLMs grow larger and more complex, there's an increasing need for more efficient formats that can maintain accuracy while reducing computational and memory requirements.

FP8, or 8-bit floating point, is a modern quantization format that strikes a balance between precision and efficiency. It provides a non-uniform range representation and per-tensor scaling factors with hardware acceleration on modern GPUs, allowing for significant performance gains and 2x reduced memory usage without sacrificing model quality.

"dynamic" at the end of the name means, that quantization does support dynamic scaling or switches between precisions to balance accuracy and efficiency. This is achieved by adapting the model's precision to the importance of the data, keeping critical information in higher precision (like BF16/FP16) while reducing precision for less critical data (like FP8/FP4) to improve performance.

Alright, let's get going!

Create a volume and mount it into the container. Adjust the container permissions so that the container can use it.

```bash
mkdir -p rhaiis-cache-mistral  
chmod g+rwX rhaiis-cache-mistral
```

Create or append your HF\_TOKEN Hugging Face token to the private.env file. Source the private.env file.

```bash
echo "export HF_TOKEN=<your_HF_token>" > private.env  
source private.env
```

Start the model server with the original model:

```bash
podman run --rm -it \
  --device nvidia.com/gpu=all \
  --security-opt=label=disable \
  --shm-size=4g -p 8000:8000 \
  --userns=keep-id:uid=1001 \
  --env HUGGING_FACE_HUB_TOKEN="$HF_TOKEN" \
  --env HF_HUB_OFFLINE=0 \
  --env VLLM_NO_USAGE_STATS=1 \
  --env NVIDIA_VISIBLE_DEVICES=0,1 \
  --env CUDA_VISIBLE_DEVICES=0,1 \
  --env TMPDIR=/tmp \
  --env TRITON_CACHE_DIR=/opt/app-root/src/.cache/triton \
  --tmpfs /tmp:rw,exec,nosuid,nodev,size=2g,mode=1777 \
  --tmpfs /var/tmp:rw,exec,nosuid,nodev,size=1g,mode=1777 \
  -v ./rhaiis-cache-mistral:/opt/app-root/src/.cache:Z \
  registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0 \
  --model RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-FP8-dynamic \
  --tensor-parallel-size 2 \
  --gpu-memory-utilization 0.8 \
  --max-model-len 8192 \
  --max-num-seqs 32
  --kv-cache-dtype fp8
 ```

Leave this running in that terminal.

If it exits with an error → let us know.