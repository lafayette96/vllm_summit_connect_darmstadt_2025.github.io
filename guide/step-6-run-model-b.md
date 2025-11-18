**Step 5 \- Run Model B (W8A8 variant)**

Now we serve the other variant of model using the same Red Hat AI Inference Server image.

Model B: RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-quantized.w8a8  
This is another Red Hat-provided quantized version of the same base model, using a different scheme (W8A8/INT8-style). 

"w8” means the weights of the model are quantized to 8-bit integers (int8) rather than full precision floating point.

“a8” means the activations (the intermediate outputs during inference) are also quantized to 8-bit integers.

Put together: w8a8 = weights in 8-bit, activations in 8-bit (often called INT8 quantisation)

Let's run it!

Example (using port 8001; adjust if running sequentially):

```bash
podman run --rm -it \
  --device nvidia.com/gpu=all \
  --security-opt=label=disable \
  --shm-size=4g \
  -p 8001:8000 \
  --userns=keep-id:uid=1001 \
  --env HUGGING_FACE_HUB_TOKEN="$HF_TOKEN" \
  --env HF_HUB_OFFLINE=0 \
  --env VLLM_NO_USAGE_STATS=1 \
  --env NVIDIA_VISIBLE_DEVICES=2,3 \
  --env CUDA_VISIBLE_DEVICES=2,3 \
  --env TMPDIR=/tmp \
  --tmpfs /tmp:rw,exec,nosuid,nodev,size=2g,mode=1777 \
  --tmpfs /var/tmp:rw,exec,nosuid,nodev,size=1g,mode=1777 \
  -v ./rhaiis-cache-mistral:/opt/app-root/src/.cache:Z \
  registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0 \
  --model RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-quantized.w8a8 \
  --tensor-parallel-size 2 \
  --gpu-memory-utilization 0.8 \
  --max-model-len 8192 \
  --max-num-seqs 32
```

**If you're running sequentially on the same GPUs:**

1. Stop Model A.  
2. Run the command above, but change the ports and devices back:  
   * \-p 8000:8000  
   * NVIDIA\_VISIBLE\_DEVICES=0,1  
   * CUDA\_VISIBLE\_DEVICES=0,1