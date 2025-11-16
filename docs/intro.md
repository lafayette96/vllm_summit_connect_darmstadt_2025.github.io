# 1. Introduction

In this workshop, we compare **two Red Hat–optimized variants** of the same Mistral model:

| Model | Variant | Notes |
|-------|---------|-------|
| **Model A** | FP8 dynamic | High quality, GPU-optimized |
| **Model B** | W8A8 quantized | Smaller, faster, slightly reduced precision |

Both models run using the **Red Hat AI Inference Server** (vLLM-based).

---

## Why We Compare Models

Optimization and quantization techniques affect:

- Latency  
- Throughput  
- Memory usage  
- Response quality  

This lab shows how to quickly test multiple variants — a common workflow in enterprise AI.

---

## What We *Do Not* Cover Today

- Model training  
- Fine-tuning or LoRA adapters  
- Running long quantization jobs  
- Modifying internal model weights  

We focus on **serving**, **querying**, and **evaluating** models efficiently.
