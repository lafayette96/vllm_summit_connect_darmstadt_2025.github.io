# Hands-on Lab: Compare Optimized Mistral Models on RHEL AI

Welcome! 👋

In this 90-minute hands-on lab, you will:

- Run two optimized variants of the **Mistral Small 3.1 24B** model on **RHEL AI**  
- Interact with them through an **OpenAI-compatible API**  
- Compare their quality and behavior  
- Use a lightweight evaluation tool to score their differences  
- (Optional) Launch a simple Web UI and chat with the models

This workshop focuses on **practical model serving**, not training or quantization.

---

## 💡 What You Will Learn

By the end of the lab, you will be able to:

1. Launch Red Hat AI Inference Server (vLLM) on RHEL AI with Podman  
2. Serve an optimized FP8 Mistral model (Model A)  
3. Serve a W8A8 quantized variant of the same model (Model B)  
4. Compare them interactively  
5. Run a small automated evaluation suite  
6. (Optional) Expose a local Web UI and chat with your model

---

## 🧰 Environment Provided

Each team receives:

- 1× VM with RHEL AI
- 4× NVIDIA GPUs (visible via container runtime)
- Podman installed
- Internet access to:
  - `registry.redhat.io`
  - `huggingface.co`
- Credentials:
  - Red Hat registry login
  - Hugging Face `HF_TOKEN`

If something breaks, raise your hand immediately — that’s part of the workshop. 😉

---

**Let’s begin →**  
Use the sidebar to navigate between steps.
