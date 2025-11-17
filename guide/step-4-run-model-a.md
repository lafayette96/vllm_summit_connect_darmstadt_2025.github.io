**Step 4 \- Run the Mistral model**

Create a volume and mount it into the container. Adjust the container permissions so that the container can use it.

```bash
mkdir \-p rhaiis-cache-mistral  
chmod g+rwX rhaiis-cache-mistral
```

Create or append your HF\_TOKEN Hugging Face token to the private.env file. Source the private.env file.

```bash
echo "export HF\_TOKEN=\<your\_HF\_token\>" \> private.env  
source private.env
```

Start the model server with the original model:

```bash
podman run \--rm \-it \\  
 \--device \[nvidia.com/gpu=all\](https://nvidia.com/gpu=all) \\  
 \--security-opt=label=disable \\  
 \--shm-size=4g \-p 8000:8000 \\  
 \--userns keep-id:uid=1001 \\  
 \--env HUGGING\_FACE\_HUB\_TOKEN="$HF\_TOKEN" \\  
 \--env HF\_HUB\_OFFLINE=0 \\  
 \--env VLLM\_NO\_USAGE\_STATS=1 \\  
 \--env NVIDIA\_VISIBLE\_DEVICES=0,1 \\  
 \--env CUDA\_VISIBLE\_DEVICES=0,1 \\  
 \--env TMPDIR=/tmp \\  
 \--env TRITON\_CACHE\_DIR=/opt/app-root/src/.cache/triton \\  
 \--tmpfs /tmp:rw,exec,nosuid,nodev,size=2g,mode=1777 \\  
 \--tmpfs /var/tmp:rw,exec,nosuid,nodev,size=1g,mode=1777 \\  
 \-v ./rhaiis-cache-mistral:/opt/app-root/src/.cache:Z \\  
 registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0 \\  
 \--model RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-FP8-dynamic \\  
 \--tensor-parallel-size 2 \\  
 \--gpu-memory-utilization 0.8 \\  
 \--max-model-len 8192 \\  
 \--max-num-seqs 32 \\  
 \--kv-cache-dtype fp8
 ```

Leave this running in that terminal.

If it exits with an error → let us know.