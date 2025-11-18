**Step 8 \- Use web UI to interact with a model (optional)**

In order to run an additional WebUI in another container, we need create a communication network to enable the containers to communicate with each other.

```bash
podman network create chat
```

Now we have to reconfigure the vllm container to receive a name and join the pod network, by adding the common line switches ```bash --name=rhaiis ```  and  ```bash --net=chat ```

Now we stop the running container and change the command line as follows:

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
  --name=rhaiis \
  --net=chat \
  registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0 \
  --model RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-FP8-dynamic \
  --tensor-parallel-size 2 \
  --gpu-memory-utilization 0.8 \
  --max-model-len 8192 \
  --max-num-seqs 32 \
  --kv-cache-dtype fp8
  --api-key=handsonday
```

In another terminal we can now start the WebUI:

```bash
podman run -d --name=chatbot --net=chat -p 8080:8080 quay.io/mklaasse/openai-chat-ng:latest
```

Command ```bash podman ps ``` now should show at least two containers: 

```bash
[cloud-user@bastion ~]$ podman ps
CONTAINER ID  IMAGE                                            COMMAND               CREATED         STATUS         PORTS                   NAMES
52edda8a49ff  registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0  --model RedHatAI/...  5 minutes ago   Up 5 minutes   0.0.0.0:8000->8000/tcp  rhaiis
0212a7711f78  quay.io/mklaasse/openai-chat-ng:latest           /usr/sbin/httpd -...  43 seconds ago  Up 43 seconds  0.0.0.0:8080->8080/tcp  chatbot
```

Now we need to forward the WebUI port 8080 back to your local machine. For this invoke in a new terminal a new ssh connection:
```bash
ssh  -L 8080:localhost:8080 cloud-user@bastion.<your_id>.opentlc.com sleep 3600
```

Additionally, we will need a tunnel to the LLM, so that the browser can send the API request and get answers back:

```bash
ssh  -L 8000:localhost:8000 cloud-user@bastion.<your_ID>.opentlc.com sleep 3600
```

And keep it open.

Now you can point your browser to ` http://localhost8080 `
You should see a browser window with the title “OpenAI Chat Frontent”. A very simple Web Frontend to the OpenAI API.

Now enter in the API URLField: ` http://localhost:8000/v1 `

In the Bearer Token / API Key: ` bash handsonday `

And press the button: **“Fetch Available Models”**.

Select a model and be ready to chat! If you still have the podman vLLM container open, you can follow the flow in the output.

You still have time left?

Try to start an alternative, more comprehensive WebUI, OpenWebUI. 

Hint: the container image can be found here: ` ghcr.io/open-webui/open-webui:main `
More  hints in this getting-started Guide: https://docs.openwebui.com/getting-started/quick-start
(and remember docker -> podman \;\-\)\) 