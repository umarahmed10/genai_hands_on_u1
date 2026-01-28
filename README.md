# Transformer Model Benchmark Report

## 1. Key Concepts Understood

### 1.1 Transformer Architecture  
Transformers process text using **self-attention** instead of recurrent connections, which allows them to model long-range dependencies efficiently. An encoder stack is mainly used for understanding tasks (classification, NER, MLM), while a decoder or encoder–decoder stack is used for generation or sequence-to-sequence tasks like translation and summarization.

### 1.2 Encoder vs Encoder–Decoder Models  
- **BERT** and **RoBERTa** are encoder-only models designed to build rich contextual representations of text for tasks such as masked language modeling and classification.  
- **BART** uses an encoder–decoder architecture where the encoder reads the input and the decoder generates output token by token, making it suitable for text generation and transformation tasks.

### 1.3 Masked Language Modeling (MLM)  
In masked language modeling, some tokens in the input are replaced by a special `[MASK]` token and the model is trained to predict the original tokens. This objective teaches encoder models like BERT and RoBERTa to fill in missing words in the middle of a sentence, which is why they perform well on fill-mask tasks.

### 1.4 Fine-Tuning for Specific Tasks  
Base models learn general language representations, but high performance on tasks like question answering usually requires fine-tuning on task-specific datasets such as SQuAD. Fine-tuned QA models (for example, `distilbert-base-cased-distilled-squad`) can accurately extract answers from context, whereas unfine-tuned base models often give partial or noisy answers.

---

## 2. Solution Implemented

### 2.1 Objective  
The goal of the benchmark was to understand how model architecture affects behavior when models are used both for the tasks they were designed for and for tasks they were not explicitly designed for. By "forcing" models into slightly misaligned tasks, it becomes possible to observe clear differences in capabilities and limitations.

### 2.2 Models Used  
- **BERT (`bert-base-uncased`)** – Encoder-only, optimized for understanding and MLM.  
- **RoBERTa (`roberta-base`)** – Encoder-only, BERT-style architecture with improved training.  
- **BART (`facebook/bart-base`)** – Encoder–decoder, designed for text generation and sequence-to-sequence tasks.

### 2.3 Experiments Designed  
Three experiments were implemented in a Jupyter notebook (`Unit1_Benchmark.ipynb`).

1. **Text Generation**  
   - Prompt: "The future of Artificial Intelligence is …"  
   - Task: Ask each model to generate a continuation.

2. **Fill-Mask (MLM)**  
   - Sentence: "The goal of Generative AI is to [MASK] new content."  
   - Task: Predict the masked word.

3. **Question Answering (QA)**  
   - Context: "Generative AI poses significant risks such as hallucinations, bias, and deepfakes."  
   - Question: "What are the risks?"

For each experiment, the success or failure of the model was recorded, along with observations explaining behavior based on its architecture.

---

## 3. Key Findings from Experiments

### 3.1 Text Generation  

- **BERT – Failure**  
  BERT repeated parts of the prompt or produced low-quality continuations, showing that it is not suited for autoregressive next-token generation. This matches its encoder-only design and MLM objective.

- **RoBERTa – Failure**  
  RoBERTa behaved similarly to BERT, producing repetitive or incoherent content due to its encoder-only architecture.

- **BART – Success**  
  BART generated coherent, meaningful continuations of the prompt. Its encoder–decoder architecture and autoregressive decoder make it well-suited for text generation tasks.

### 3.2 Fill-Mask (MLM)  

- **BERT – Success**  
  BERT predicted appropriate verbs such as "create" or "generate," which fit the sentence grammatically and semantically, aligning with its MLM training objective.

- **RoBERTa – Success**  
  RoBERTa also produced strong predictions for the masked word, consistent with its BERT-style encoder-only architecture and MLM training.

- **BART – Partial**  
  BART sometimes produced reasonable replacements but was less consistent than BERT and RoBERTa on strict [MASK] prediction, as its training relies on denoising rather than pure token-level MLM.

### 3.3 Question Answering (QA)  

- **BERT – Partial**  
  The base BERT model could identify fragments related to the risks but did not consistently output a clean, complete answer. This reflects the absence of QA-specific fine-tuning.

- **RoBERTa – Partial**  
  RoBERTa produced responses loosely grounded in the context but typically incomplete, requiring QA-specific fine-tuning for better results.

- **BART – Partial**  
  BART generated answer-like sentences but sometimes missed important details or was not tightly anchored to the provided context.

---

## 4. Overall Insight  

The experiments show that architecture and training objectives strongly determine model capability.

- Encoder-only models such as BERT and RoBERTa excel at MLM-style understanding tasks but perform poorly on free-form generation.  
- Encoder–decoder models like BART handle text generation significantly better but may not match dedicated MLM models on strict token prediction.  
- Fine-tuning on task-specific datasets is critical for high-quality question answering, regardless of the underlying architecture.

---

## 5. Limitations and Future Work  

This benchmark uses only base versions of BERT, RoBERTa, and BART without task-specific fine-tuning, so results do not represent maximum achievable performance. The scope is limited to a small set of prompts and three tasks.

Possible future extensions include:  
- Comparing base models to fine-tuned variants on QA and other downstream tasks.  
- Adding decoder-only GPT-style models to the benchmark.  
- Introducing quantitative metrics (accuracy, F1 score, human ratings) instead of qualitative observations.

---

## 6. Phase 4 Project: Legal Clause Risk Analyzer  

**Domain:** Law  
**Project Idea:** Legal Clause Risk Analyzer  

Build a tool that takes a contract clause as input and highlights potential risks such as ambiguous liability, unfair termination conditions, or missing data protection language.

**System Components:**  
- Extract key legal entities (parties, dates, obligations) from the clause.  
- Classify clauses into categories: confidentiality, termination, payment, intellectual property, and liability.  
- Generate a short natural-language explanation of why a clause might be risky.

This project demonstrates how encoder and encoder–decoder models can support contract review and legal compliance workflows in real-world applications.
