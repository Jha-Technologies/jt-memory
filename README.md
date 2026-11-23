# 🧠 jt-memory

[[PyPI version]](https://pypi.org)
[[Python 3.8+]](https://python.org)
[[License: AGPL v3]](https://gnu.org)
[[Organization]](https://github.io)

**Emotion-Guided Continual Memory Engine for AI Chatbots & Autonomous Agents.**

Modern AI chatbots and LLM agents suffer from two critical bottlenecks: **catastrophic forgetting** (new context overwrites critical past facts) and **memory bloat** (storing every trivial message increases token costs and latency). 

Developed by **Jha Technologies**, `jt-memory` solves both problems using a neurobiology-inspired **Importance Gate** and **Offline Sleep Pruning**—with **zero external dependencies**.

---

## 🔬 How It Works: The Importance Gate

Inspired by human cognitive consolidation, `jt-memory` evaluates every incoming interaction using a prediction-error and emotional-intensity formula:

\[\text{Memory Importance} = \vert{}\text{Expected Outcome} - \text{Actual Outcome}\vert{} \times \text{Emotional Intensity}\]

1. **Short-Term Buffer (Low Importance):** Routine messages (e.g., *"Hello"*, *"Okay"*) score below the threshold and stay temporarily in short-term memory.
2. **Long-Term Consolidation (High Importance):** Unexpected events, user corrections, or critical facts trigger a high prediction-error spike and are permanently locked into long-term memory without overwriting unrelated contexts.
3. **Offline Sleep Pruning (`sleep_and_prune`):** Periodic cleanup cycles purge trivial short-term noise, keeping memory footprint strictly bounded.

---

## ⚡ Installation

```bash
pip install jt-memory
```

---

## 🚀 Quickstart Example

```python
from jt_memory import JTMemoryEngine

# Initialize engine with an importance threshold and max memory bound
memory = JTMemoryEngine(importance_threshold=5.0, max_long_term_nodes=100)

# 1. Routine interaction (Low surprise -> goes to Short-Term Memory)
memory.observe(
    content="User said good morning",
    expected=1.0,
    actual=1.0,
    intensity=1.0,
    context="general_chat"
)

# 2. Critical user preference (High surprise -> locked into Long-Term Memory)
memory.observe(
    content="User has a severe peanut allergy",
    expected=0.0,
    actual=1.0,
    intensity=9.8,
    context="medical_safety"
)

# 3. Run offline sleep cycle to purge trivial short-term noise
purged = memory.sleep_and_prune()
print(f"Purged {purged} trivial memories.")

# 4. Recall highest-importance memories for your LLM prompt
print("Consolidated Memories:", memory.recall(top_k=5))

# 5. Persist memory to disk across restarts
memory.save("agent_memory.json")
```

---

## 🤖 Integrating with LLM Chatbots (OpenAI / Local LLMs)

Use `jt-memory` as a lightweight, zero-token-overhead memory layer before calling any LLM API:

```python
from jt_memory import JTMemoryEngine

mem = JTMemoryEngine(importance_threshold=4.5)
mem.load("agent_memory.json")  # Load past user memories

# Retrieve top relevant facts to inject into system prompt
past_facts = "\n".join([f"- {m['content']}" for m in mem.recall(top_k=5)])
system_prompt = f"You are a helpful assistant. Remember these critical facts:\n{past_facts}"

# Pass `system_prompt` to your LLM...
```

---

## 📚 Research Foundation

This SDK implements the cognitive memory architecture introduced in:
* **Jha, S. (2026).** *Emotion-Guided, Low-Energy Continual Learning Architecture for Artificial General Intelligence (AGI).* Jha Technologies Research Labs. [Read Paper]((https://github.com/shrishantjha4-byte/Emotion-Guided-AGI-Continual-Learning))

---

## ⚖️ Dual Licensing & Commercial Use

`jt-memory` is dual-licensed by **Jha Technologies**:

1. **Open-Source License (AGPL-3.0):** Free for academic research, personal projects, and open-source software compliant with GNU AGPL-3.0 terms.
2. **Commercial License:** If you wish to integrate `jt-memory` into proprietary, closed-source commercial software, SaaS products, or enterprise systems without AGPL source-disclosure obligations, you must obtain a **Jha Technologies Commercial License & Revenue-Share Agreement**.

For licensing inquiries, custom enterprise integrations, or research collaborations, visit **[(https://jha-technologies.github.io/)]**.

---

<p align="center">
  Engineered by <strong>Shrishant Jha</strong> • Founder, <a href="(https://jha-technologies.github.io/)">Jha Technologies</a>
</p>

