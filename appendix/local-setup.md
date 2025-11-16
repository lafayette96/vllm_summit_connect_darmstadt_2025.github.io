## What we did

* We used two Red Hat-provided, already optimized Mistral variants (FP8 and W8A8).
* Deployed them with Red Hat AI Inference Server on RHEL AI.
* Interacted with them via an OpenAI-compatible API.
* Evaluated and compared their behavior and quality.

## What we did not do

* We did not run a full quantization or compression pipeline live.
  * **Reason:** it's compute-heavy, can take many hours, and isn't reliable in a 90-minute lab.
* We did not train or fine-tune models.
* We did not modify model weights ourselves.