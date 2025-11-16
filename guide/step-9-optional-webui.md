**Step 9 \- Use web UI to interact with a model (optional)**

To run a WebUI, we need to enable container communication.

1. Create a podman network:  
   podman network create chat

2. Stop your running model container (Ctrl+C).  
3. Relaunch the vLLM container (Model A) with \--name and \--net flags:  
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
    \--name rhaiis \\  
    \--net=chat \\  
    registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0 \\  
    \--model RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-FP8-dynamic \\  
    \--tensor-parallel-size 2 \\  
    \--gpu-memory-utilization 0.8 \\  
    \--max-model-len 8192 \\  
    \--max-num-seqs 32 \\  
    \--kv-cache-dtype fp8 \\  
    \--api-key handsonday

4. In another terminal, start the WebUI:  
   podman run \-d \--name chatbot \--net=chat \-p 8080:8080 \\  
    quay.io/mklaasse/openai-chat-ng:latest

5. Check that both containers are running:  
   podman ps

6. In a **new local terminal on your own laptop**, forward the ports:  
   \# Forward the WebUI port  
   ssh \-L 8080:localhost:8080 cloud-user@bastion.9kb2z.sandbox3231.opentlc.com sleep 3600

   \# Forward the LLM API port  
   ssh \-L 8000:localhost:8000 cloud-user@bastion.9kb2z.sandbox3231.opentlc.com sleep 3600

   Keep these terminals open.  
7. Open your local browser and go to http://localhost:8080  
   * **API URL Field:** http://localhost:8000/v1  
   * **Bearer Token / API Key:** handsonday  
   * Press "Fetch Available Models".  
   * Select a model and start chatting\!  
8. Still have time?  
   Try to start an alternative WebUI, OpenWebUI.  
   (Hint: the container image is ghcr.io/open-webui/open-webui:main and remember docker \-\> podman\!)