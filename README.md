# Transformer Model Benchmark Report

## 1. Key Concepts Understood

### 1.1 Transformer Architecture
Transformers process text using **self-attention** instead of recurrent connections, which allows them to model long-range dependencies efficiently.  
An **encoder stack** is mainly used for understanding tasks (classification, NER, MLM), while a **decoder** or **encoder–decoder stack** is used for generation or sequence-to-sequence tasks like translation and summarization.

### 1.2 Encoder vs Encoder–Decoder Models
- **BERT** and **RoBERTa** are encoder-only models designed to build rich contextual representations of text for tasks such as masked language modeling and classification.  
- **BART** uses an encoder–decoder architecture where the encoder reads the input and the decoder generates output token by token, making it suitable for text generation and transformation tasks.

### 1.3 Masked Language Modeling (MLM)
In masked language modeling, some tokens in the input are replaced by a special `[MASK]` token and the model is trained to predict the original tokens.  
This objective teaches encoder models like BERT and RoBERTa to fill in missing words in the middle of a sentence, which is why they perform well on fill-mask tasks.

### 1.4 Fine-Tuning for Specific Tasks
Base models learn general language representations, but high performance on tasks like question answering usually requires **fine-tuning** on task-specific datasets such as **SQuAD**.  
Fine-tuned QA models (for example, *distilbert-base-cased-distilled-squad*) can accurately extract answers from context, whereas unfine-tuned base models often give partial or noisy answers.

---

## 2. Solution Implemented

### 2.1 Objective
The goal of the benchmark was to understand how **model architecture affects behavior** when models are used both for the tasks they were designed for and for tasks they were not explicitly designed for.  
By “forcing” models into slightly misaligned tasks, we can observe clear differences in capabilities and limitations.

### 2.2 Models Used
- **BERT (bert-base-uncased):** Encoder-only, optimized for understanding and MLM.  
- **RoBERTa (roberta-base):** Encoder-only, BERT-style architecture with improved training.  
- **BART (facebook/bart-base):** Encoder–decoder, designed for text generation and sequence-to-sequence tasks.

### 2.3 Experiments Designed
Implemented three experiments in a Jupyter notebook (`Unit1_Benchmark.ipynb`):

1. **Text Generation** – Prompt: “The future of Artificial Intelligence is …” to generate a continuation.  
2. **Fill-Mask (MLM)** – Sentence: “The goal of Generative AI is to [MASK] new content.” to predict the masked word.  
3. **Question Answering** –  
   Context: “Generative AI poses significant risks such as hallucinations, bias, and deepfakes.”  
   Question: “What are the risks?”  

For each experiment, the success or failure of the model was recorded, along with observations explaining behavior based on architecture.

---

## 3. Key Findings from Experiments

### 3.1 Text Generation
- **BERT – Failure:** Repeated parts of the prompt or produced low-quality continuations, showing it’s unsuited for autoregressive generation.  
- **RoBERTa – Failure:** Similar to BERT, produced repetitive or incoherent content due to encoder-only design.  
- **BART – Success:** Generated coherent, meaningful continuations. Its encoder–decoder structure and autoregressive decoder make it well-suited for text generation.

### 3.2 Fill-Mask (MLM)
- **BERT – Success:** Predicted appropriate verbs like “create” or “generate,” aligning with its MLM training objective.  
- **RoBERTa – Success:** Produced similarly strong results, consistent with its improved BERT-style architecture.  
- **BART – Partial:** Sometimes correct but less consistent. Its training involves denoising tasks rather than pure token-level MLM, making it less specialized.

### 3.3 Question Answering (QA)
- **BERT – Partial:** Could identify relevant fragments but not complete answers due to lack of QA-specific fine-tuning.  
- **RoBERTa – Partial:** Context-related responses but usually incomplete. Needs fine-tuning for QA precision.  
- **BART – Partial:** Generated answer-like text, though it missed key details. Generative ability doesn't substitute for task-specific optimization.

---

## 4. Overall Insight
The experiments clearly show that **architecture and training objectives strongly determine model capability**:

- **Encoder-only models (BERT, RoBERTa):** Excel at MLM-style understanding tasks but fail at free-form generation.  
- **Encoder–decoder models (BART):** Handle text generation better but may lag behind in strict token prediction tasks.  
- **Fine-tuning:** Crucial for high-quality question answering, regardless of model architecture.

---
