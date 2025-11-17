**Hands-on Lab: Host your own LLM with the Red Hat vLLM Inference Server**

**Welcome!👋**

In this lab, you'll run two optimized versions of the same Mistral model on RHEL AI **(1)** with GPUs and see how their quality and behavior compare.

We'll use:

* A standard Red Hat AI Inference Server (RHAIIS) container
* Two pre-optimized Mistral variants (already compressed/quantized by Red Hat)
* A small evaluation tool to compare them

No model training, no long-running compression jobs - everything you do should be visible within the 90 minutes.

You'll work in teams of 2-3 in a shared environment.

**(1)**If you're asking yourself, what on earth RHEL AI is, here's an explanation:

In the simplest terms, RHEL AI is Red Hat Enterprise Linux. More specifically, RHEL AI is an optimized and highly-tuned version of RHEL that provides a foundation model platform for consistently developing, refining, testing and running Granite family large language models (LLM) to power enterprise AI applications.

RHEL AI also takes advantage of image mode for Red Hat Enterprise Linux, a new approach to operating system deployment that lets users build, deploy and manage RHEL as a bootable container (bootc) image. This means RHEL AI can be deployed, managed and scaled the same way you deploy, manage and scale any containerized application.

In essence, RHEL AI is a fully-equipped LLM development, tuning and hosting platform that can be deployed and managed with Red Hat OpenShift AI anywhere across the hybrid cloud, from on-prem to edge.