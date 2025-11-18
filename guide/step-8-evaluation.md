**Step 7 \- Use an evaluation tool to compare them**

Now we use a lightweight evaluation tool (llm-eval-test) to send a small benchmark to each model and compare.

Model A

```bash
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-FP8-dynamic",
    "messages": [
      {
        "role": "system",
        "content": "You are an expert on Red Hat and enterprise open source. Answer clearly and concisely."
      },
      {
        "role": "user",
        "content": "What does Red Hat do as a company, and name three key products or technologies it provides?"
      }
    ],
    "max_tokens": 150,
    "temperature": 0.2
  }' | python3 -m json.tool
  ```

  Model B

  ```bash
  curl http://localhost:8001/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-quantized.w8a8",
    "messages": [
      {
        "role": "system",
        "content": "You are an expert on Red Hat and enterprise open source. Answer clearly and concisely."
      },
      {
        "role": "user",
        "content": "What does Red Hat do as a company, and name three key products or technologies it provides?"
      }
    ],
    "max_tokens": 150,
    "temperature": 0.2
  }' | python3 -m json.tool
  ```