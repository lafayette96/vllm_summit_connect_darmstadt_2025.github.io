**Step 7 \- Use an evaluation tool to compare them**

Now we use a lightweight evaluation tool (llm-eval-test) to send a small benchmark to each model and compare.

1. Create a venv:  

```bash
python3 -m venv eval-venv
source eval-venv/bin/activate

pip install --upgrade pip
pip install requests
```

2. Download a tiny evaluation dataset:  

```bash
mkdir -p datasets
llm-eval-test download \
  --datasets ./datasets \
  --tasks arc_challenge
```

3. Run against Model A (FP8):  

```bash
llm-eval-test run \
  --endpoint http://127.0.0.1:8000/v1/completions \
  --model RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-FP8-dynamic \
  --datasets ./datasets \
  --tasks arc_challenge \
  --output results_fp8.json \
  --format summary
```

4. Run against Model B (W8A8):  

```bash
llm-eval-test run \
  --endpoint http://127.0.0.1:8001/v1/completions \
  --model RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-quantized.w8a8 \
  --datasets ./datasets \
  --tasks arc_challenge \
  --output results_w8a8.json \
  --format summary
```

Look at the summaries:

* Did one model perform slightly better?  
* Are they close?

This is how you quantify trade-offs between different optimized variants.