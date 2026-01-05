# AI Policy & SOP Assistant: Fine-Tuning DeepSeek with QLoRA
## Professional Project Report (Zero Budget)

---

## Executive Summary (The Big Picture)

This project teaches you how to build a smart chatbot for your college/lab that answers questions about **policies and procedures** (SOPs) — like leave approval, reimbursement steps, lab safety rules, and event permissions — all from documents you already have.

**Why build this?**
- Students and staff ask the same policy questions over and over (slow, inconsistent answers).
- A chatbot that knows *your specific rules* gives instant, accurate answers.
- You train it for **$0** using free GPU (Kaggle) and open-source AI.
- The final "brain" (adapters) is tiny — only a few MB to share.

**What you'll learn:**
- How to collect and format real data.
- How to use QLoRA (smart memory-saving trick) to train AI models.
- How to shrink huge models into portable adapters.
- How to run everything for free on Kaggle notebooks.

---

## 1. The Problem We're Solving

### Real-World Pain
Imagine a college lab:
- **Monday, 10 AM**: Student asks, "How do I request a reimbursement for lab supplies?"
- **Lab manager replies**: "Uh, you fill out a form... I think? Let me check the policy."
- **Two days later**: Manager sends a vague email with contradictory info.
- **Wednesday**: Another student asks the same question and gets a different answer.
- **Friday**: Faculty get frustrated. Time wasted.

### What This Project Does
You build an **AI assistant** that:
1. **Reads your lab/college SOPs** (PDF, document, handbook).
2. **Learns** how to answer policy questions from examples you provide.
3. **Answers instantly** with structured, consistent replies (e.g., "Eligibility → Steps → Timeline → Who to contact").
4. **Says "I don't know"** when unsure (doesn't make up fake policies).

### Who Benefits
- **Students**: Instant answers without waiting for office hours.
- **Lab managers/Office staff**: Fewer repetitive questions, more time for real work.
- **Faculty**: Consistent policy messaging across the department.

---

## 2. What is QLoRA? (Beginner Explanation)

Imagine you have a **huge encyclopedia** (7 billion pages = DeepSeek-R1-Distill-Qwen-7B model) and want to teach it about *your college's specific rules*.

### The Problem with Normal Training
- **Full fine-tuning**: Rewrite every page. Needs 100 GB of memory. Costs $$$. Takes weeks.

### The QLoRA Solution (Smart Memory Trick)
QLoRA = **4-bit Quantization + Low-Rank Adapters**. Think of it like this:

```
Original Model (7 billion parameters, full precision)
     ↓
[COMPRESS to 4-bit] ← Uses 8× less memory! But frozen (read-only).
     ↓
[ADD tiny trainable "adapter" layers]
     ↓
Train ONLY the adapters (≈ 1% of parameters)
     ↓
Result: Your college-specific knowledge in tiny adapters (5–50 MB)
```

**Real analogy:**
- Full fine-tuning = Rewriting a 1,000-page encyclopedia with new college rules on every page.
- QLoRA = Writing a 5-page **"College Addendum"** that explains the *differences*. Attach it when needed.

### What Gets Trained?
- ✅ Small adapter weights (trainable)
- ✅ Model learns to blend base knowledge + your SOP examples
- ❌ Original model weights stay frozen (saves memory)

**Memory savings**: 7B model normally needs ~26 GB → With QLoRA needs ~7–10 GB (fits Kaggle free T4 GPU)

---

## 3. What Are Adapters?

An **adapter** is a tiny add-on to the base model. It's like a USB stick:
- **Base model** = Your laptop (can't modify).
- **Adapter** = USB stick with college rules.
- **Loading both together** = Laptop now acts like "college-rule-laptop."

**Why adapters are great:**
| Feature | Full Model | Adapter Only |
|---------|-----------|--------------|
| **File size** | 14–16 GB | 5–50 MB |
| **Sharing** | Slow (hours to upload) | Fast (minutes) |
| **Storage cost** | Expensive | Free (Hugging Face) |
| **Load time** | Minutes | Seconds |
| **Can switch tasks?** | No (one purpose) | Yes (keep base, swap adapters) |

**Real example:**
- Base: DeepSeek (general knowledge)
- Adapter 1: College policies
- Adapter 2: Lab safety (same base, different adapter)
- You can swap adapters depending on what you need.