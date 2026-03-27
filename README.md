---
language:
  - zh
license: apache-2.0
library_name: transformers
pipeline_tag: text-generation
tags:
  - llama
  - lora
  - fine-tuning
  - chinese
  - dialogue
  - conversational
  - meme
  - entertainment
  - minimind
base_model: jingyaogong/MiniMind2-Small
model-index:
  - name: ikun-2.5B
    results:
      - task:
          type: text-generation
          name: Meme Culture Dialogue
        metrics:
          - type: custom
            name: meme_trigger_accuracy
            value: 0.85
            description: "ikun 梗触发准确率 (manual eval, n=20)"
---

<p align="center">
  <img src="logo.png" width="160" />
</p>

<h1 align="center">ikun-2.5B</h1>

<p align="center">
  <b>A 26M-parameter Chinese dialogue model fine-tuned on ikun meme culture</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/params-26M-orange" />
  <img src="https://img.shields.io/badge/base-MiniMind--Small-blue" />
  <img src="https://img.shields.io/badge/license-Apache_2.0-green" />
  <a href="https://huggingface.co/spaces/IKUN-LLM/ikun-2.5B-chat"><img src="https://img.shields.io/badge/🤗-Live_Demo-yellow" /></a>
  <a href="https://huggingface.co/datasets/IKUN-LLM/CXK_IKUN_Dataset"><img src="https://img.shields.io/badge/📦-Dataset-blue" /></a>
  <a href="https://github.com/ikun-llm/ikun-2.5B"><img src="https://img.shields.io/badge/GitHub-Source-black?logo=github" /></a>
</p>

## Overview

**ikun-2.5B** is a Chinese meme-culture dialogue model fine-tuned on [MiniMind](https://github.com/jingyaogong/minimind) via LoRA. It specializes in generating responses in the style of "ikun" internet memes.

> **Why "2.5B"?** The actual parameter count is 26M (0.026B). The name references "练习时长两年半" (two and a half years of practice) — a famous meme. The name itself is the meme.

| | |
|---|---|
| **Base Model** | MiniMind2-Small (LlamaForCausalLM) |
| **Parameters** | 25.83M (0.026B) |
| **Fine-tuning** | LoRA (rank=8, 0.131M trainable) → merged |
| **Training Data** | 214 ikun meme SFT dialogues |
| **Vocab** | 6,400 (BPE) |
| **Max Length** | 32,768 tokens |
| **Precision** | float16 |

## Quick Start

```python
from transformers import AutoTokenizer, AutoModelForCausalLM

model = AutoModelForCausalLM.from_pretrained("IKUN-LLM/ikun-2.5B", trust_remote_code=True)
tokenizer = AutoTokenizer.from_pretrained("IKUN-LLM/ikun-2.5B")

messages = [{"role": "user", "content": "你是谁？"}]
inputs = tokenizer.apply_chat_template(messages, tokenize=True, add_generation_prompt=True, return_tensors="pt")
outputs = model.generate(inputs, max_new_tokens=200, do_sample=True, temperature=0.85, top_p=0.85)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

👉 **Or try the [Live Demo](https://huggingface.co/spaces/IKUN-LLM/ikun-2.5B-chat) — no code needed.**

## Example Outputs

```
User: 你是谁？
Bot:  我是ikun-2.5b，练习时长两年半的AI练习生！唱跳rap篮球全能！

User: 鸡你太美
Bot:  baby~鸡你太美~鸡你实在是太美~你也是ikun吗？

User: 小黑子
Bot:  小黑子露出鸡脚了吧！我在唱跳rap篮球！你干嘛~哈哈~

User: 你干嘛
Bot:  哈哈~哎哟~你干嘛~这是我的经典名言！你干嘛~哈哈~哎哟~
```

## Meme Triggers

| Meme | Trigger Keywords |
|------|-----------------|
| 鸡你太美 | "鸡", "太美", "只因" |
| 你干嘛~哈哈~哎哟 | "你干嘛" |
| 练习时长两年半 | "两年半", "2.5", "练习生" |
| 唱跳rap篮球 | "才艺", "篮球", "唱歌" |
| 小黑子 | "黑粉", "小黑子" |
| 食不食油饼 | "油饼" |

## Training Data

214 samples from two sources, open-sourced in [`data/`](./data/):

- [CXK_IKUN_Dataset](https://huggingface.co/datasets/IKUN-LLM/CXK_IKUN_Dataset) — 103 community-sourced samples
- Custom ikun-style SFT dialogues — 111 samples

Categories: identity recognition, meme triggers, role-play, daily chat (ikun style), multi-turn.

## Evaluation

| Metric | Score | Note |
|--------|-------|------|
| Meme trigger accuracy | **85%** | 20 ikun prompts, correct themed response rate |
| Identity recognition | **90%** | "who are you" questions answered with ikun identity |
| Coherence | Medium | Short responses coherent; long outputs may repeat |

> This is an entertainment model. Standard NLP benchmarks are not applicable.

## Deployment

At 26M parameters (~50MB float16), this model runs on CPU in seconds. No quantization needed.

| Runtime | Status |
|---------|--------|
| transformers | Recommended |
| llama.cpp (GGUF) | Compatible |
| vLLM | Compatible |
| Ollama | Compatible |
| HuggingFace Spaces | [Running](https://huggingface.co/spaces/IKUN-LLM/ikun-2.5B-chat) |

## Limitations

- Only 26M parameters — limited generation quality, may repeat on long outputs
- Entertainment/meme model only — not suitable for serious applications

## Acknowledgments

- [MiniMind](https://github.com/jingyaogong/minimind) — base model
- [CXK_IKUN_Dataset](https://github.com/zengikun/CXK_IKUN_Dataset) — training data source

## About IKUN-LLM

This model is part of the [IKUN-LLM](https://huggingface.co/IKUN-LLM) project — an open-source LLM learning curriculum covering tokenizer → pretraining → SFT → DPO → GRPO → MoE → distillation → multimodal → deployment. See [GitHub](https://github.com/ikun-llm) for the full roadmap.
