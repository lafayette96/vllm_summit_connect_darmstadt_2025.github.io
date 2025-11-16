## **Step 5 \- Run Model B (W8A8 variant)**

Now we serve the other variant of model using the same Red Hat AI Inference Server image.

Model B: RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-quantized.w8a8  
This is another Red Hat-provided quantized version of the same base model, using a different scheme (W8A8/INT8-style).  
Example (using port 8001; adjust if running sequentially):

podman run \--rm \-it \\  
 \--device \[nvidia.com/gpu=all\](https://nvidia.com/gpu=all) \\  
 \--security-opt=label=disable \\  
 \--shm-size=4g \\  
 \-p 8001:8000 \\  
 \--userns keep-id:uid=1001 \\  
 \--env HUGGING\_FACE\_HUB\_TOKEN="$HF\_TOKEN" \\  
 \--env HF\_HUB\_OFFLINE=0 \\  
 \--env VLLM\_NO\_USAGE\_STATS=1 \\  
 \--env NVIDIA\_VISIBLE\_DEVICES=2,3 \\  
 \--env CUDA\_VISIBLE\_DEVICES=2,3 \\  
 \--env TMPDIR=/tmp \\  
 \--tmpfs /tmp:rw,exec,nosuid,nodev,size=2g,mode=1777 \\  
 \--tmpfs /var/tmp:rw,exec,nosuid,nodev,size=1g,mode=1777 \\  
 \-v ./rhaiis-cache-mistral:/opt/app-root/src/.cache:Z \\  
 registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0 \\  
 \--model RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-quantized.w8a8 \\  
 \--tensor-parallel-size 2 \\  
 \--gpu-memory-utilization 0.8 \\  
 \--max-model-len 8192 \\  
 \--max-num-seqs 32

**If you're running sequentially on the same GPUs:**

1. Stop Model A.  
2. Run the command above, but change the ports and devices back:  
   * \-p 8000:8000  
   * NVIDIA\_VISIBLE\_DEVICES=0,1  
   * CUDA\_VISIBLE\_DEVICES=0,1