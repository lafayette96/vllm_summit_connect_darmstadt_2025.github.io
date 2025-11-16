# **What you will learn**

By the end of the lab, you will:

1. Start a model server on RHEL AI using Podman.  
2. Call the model using an OpenAI-style HTTP API.  
3. Run a second, differently optimized variant of the same model.  
4. Use an evaluation tool to:  
   * Send the same tasks to both models  
   * Compare their answers and behavior

## **The focus**

How to run trusted, optimized models on RHEL AI, and how to compare them using evaluation tools.

## **Your setup**

Each group gets:

* 1x RHEL AI VM  
* 4 NVIDIA GPU(s)  
* podman installed  
* Network access to:  
  * registry.redhat.io (for the serving image)  
  * huggingface.co (for downloading the model)

We are going to connect and use:

* Red Hat registry credentials  
* Hugging Face token (HF\_TOKEN) with read access

**Facilitators:**

* Give you SSH details.  
* Make sure HF\_TOKEN and registry access are ready.

If something doesn't work: **raise your hand immediately**. That's part of the workshop.