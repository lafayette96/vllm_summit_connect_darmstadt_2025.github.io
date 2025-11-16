This page contains notes from the end of the PDF, which appear to be for setting up a local environment (not the shared lab VM).What we didWe used two Red Hat-provided, already optimized Mistral variants (FP8 and W8A8).Deployed them with Red Hat AI Inference Server on RHEL AI.Interacted with them via an OpenAI-compatible API.Evaluated and compared their behavior and quality.What we did not doWe did not run a full quantization or compression pipeline live.Reason: it's compute-heavy, can take many hours, and isn't reliable in a 90-minute lab.We did not train or fine-tune models.We did not modify model weights ourselves.Local Environment PrepThese steps appear to be for preparing a local RHEL machine, not the provided lab VM.1. Red Hat Account & LoginSetup Red Hat Developer account: https://developers.redhat.com/  podman login registry.redhat.io
2. Pull Image & Prep Filesystem  podman pull registry.redhat.io/rhaiis/vllm-cuda-rhel9:3.0.0
  sudo setsebool -P container_use_devices 1
  mkdir -p rhaiis-cache-mistral
  chmod g+rwX rhaiis-cache-mistral
3. Hugging Face Token  echo "export HF_TOKEN=<your_HF_token>" > private.env
  source private.env
4. Check GPU  podman run --rm -it --security-opt=label=disable --device [nvidia.com/gpu=all](https://nvidia.com/gpu=all) nvcr.io/nvidia/cuda:12.4.1-base-ubi9 nvidia-smi
