**Hands-on Lab: Host your own LLM with the Red Hat vLLM Inference Server**

**Welcome!👋**

```bash
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
```

In this lab, you'll run two optimized versions of the same Mistral model on RHEL AI **(1)** with GPUs and see how their quality and behavior compare.

We'll use:

* A standard Red Hat AI Inference Server (RHAIIS) container
* Two pre-optimized Mistral variants (already compressed/quantized by Red Hat)
* A small evaluation tool to compare them

Below you can see a diagram of an environment used in the workshop:

![Demo diagram](/_media/demo-diagram-vllm.png)

1. Accessing the RHEL AI environment via SSH and verifying GPU, Podman and registries
2. Log in to the Red Hat Registry and pull the Red Hat AI Inference Server container image (vllm-cuda-rhel9:3.0.0)
3. Start the server running Model A to enable model download and inference
4. Call the model using the OpenAI-style HTTP API (curl to port 8000).
5. Run a second, differently optimized variant of the model (quantized W8A8)
6. Use the llm-eval-test tool 
7. (Optional) Deploy a simple Web UI container to interact with the model via a browser.


No model training, no long-running compression jobs - everything you do should be visible within 60 minutes.

You'll work in teams of 2-3 in a shared environment. Below you can find credentials to the workshop where you can sign up:

Workshop URL: https://catalog.demo.redhat.com/workshop/wfw7ya

Password: summit2025

Now please gather in groups and get credentials from the facilitators. Once you've met your partners in crime and you have your environment ready, feel free to go to Lab Guide -> Introduction.

**(1)** If you're asking yourself, what on earth RHEL AI is, here's a quick explanation:

In the simplest terms, RHEL AI is Red Hat Enterprise Linux. More specifically, RHEL AI is an optimized and highly-tuned version of RHEL that provides a foundation model platform for consistently developing, refining, testing and running large language models (LLM) to power enterprise AI applications.

RHEL AI also takes advantage of image mode for Red Hat Enterprise Linux, a new approach to operating system deployment that lets users build, deploy and manage RHEL as a bootable container (bootc) image. This means RHEL AI can be deployed, managed and scaled the same way you deploy, manage and scale any containerized application.

In essence, RHEL AI is a fully-equipped LLM development, tuning and hosting platform that can be deployed and managed with Red Hat OpenShift AI anywhere across the hybrid cloud, from on-prem to edge.

