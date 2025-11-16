## **Step 7 \- Talk to Model B**

In another terminal, send a request to Model B.  
(Note the port 8001 \- change to 8000 if you're running sequentially).  
curl \-s \-X POST http://localhost:8001/v1/completions \\  
 \-H "Content-Type: application/json" \\  
 \-d '{  
    "model": "RedHatAI/Mistral-Small-3.1-24B-Instruct-2503-quantized.w8a8",  
    "prompt": "Explain what RHEL AI is in 3 bullet points.",  
    "max\_tokens": 80  
 }' | jq

Use the **same prompts** you sent to Model A.

As a team, discuss:

* Are the answers similar?  
* Any differences in style, detail, or correctness?