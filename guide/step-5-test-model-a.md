**Step 4 \- Talk to the model A**

Open a **second terminal** to the same VM (feel free to use tmux).

```bash
ssh cloud-user@bastion.9kb2z.sandbox3231.opentlc.com
```

Send a test request:

```bash
curl -s -X POST http://LocalIPAddress:8000/v1/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-FP8-dynamic",
    "prompt": "What is the capital of France?",
    "max_tokens": 50
  }' | jq
 ```

You should see JSON with a sensible answer.

Now try your own prompts as a team, e.g.:

* "Explain RHEL AI in 3 bullet points."  
* "Compare containers vs VMs in two sentences."  
* "Write a short disclaimer about AI-generated content."

Observe:

* Response quality  
* Latency

We'll use the same prompts later on Model B.

When you're done exploring, your facilitator will guide you on the next step. You will either:

1. Keep Model A running and use a different port/GPU set for Model B.  
2. Stop Model A (Ctrl+C) and run Model B next on the same port.