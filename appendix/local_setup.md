## What we did

* We used two Red Hat-provided, already optimized Mistral variants (FP8 and W8A8).
* Deployed them with Red Hat AI Inference Server on RHEL AI.
* Interacted with them via an OpenAI-compatible API.
* Evaluated and compared their behavior and quality.

## What we did not do

* We did not run a full quantization or compression pipeline live.
  * **Reason:** it's compute-heavy, can take many hours, and isn't reliable in a 90-minute lab.
* We did not train or fine-tune models.
* We did not modify model weights ourselves.

---

## Local Environment Prep

> These steps appear to be for preparing a local RHEL machine, *not* the provided lab VM.

### 1. Red Hat Account & Login

* Setup Red Hat Developer account: `https://developers.redhat.com/`

* ```bash
  podman login registry.redhat.io

## **Step 3 \- Setup & Run Model A**

This step combines logging in to the Red Hat Registry, pulling the server image, and running the first model (Model A).

### **3.1 \- Log in to Red Hat Registry**

Login to Red Hat Registry (if your environment is not preconfigured):

podman login registry.redhat.io

Use either your own Red Hat account or the credentials provided in the session.

### **3.2 \- Pull the model server image**

Next, we use Red Hat AI Inference Server (vLLM-based):

podman pull registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0

This container will host both variants of the Mistral model.

### **3.3 \- Run the Mistral model (Model A \- FP8)**

Create a volume and mount it into the container. Adjust the container permissions so that the container can use it.

mkdir \-p rhaiis-cache-mistral  
chmod g+rwX rhaiis-cache-mistral

Create or append your HF\_TOKEN Hugging Face token to the private.env file. Source the private.env file.

echo "export HF\_TOKEN=\<your\_HF\_token\>" \> private.env  
source private.env

Start the model server with the original model:

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

Leave this running in that terminal.

If it exits with an error → let us know.
