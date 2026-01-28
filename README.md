# Transformer Model Benchmark Report

## 1. Key Concepts Understood

### 1.1 Transformer Architecture  
Transformers process text using **self-attention** instead of recurrent connections, which allows them to model long-range dependencies efficiently.[file:2] An encoder stack is mainly used for understanding tasks (classification, NER, MLM), while a decoder or encoder–decoder stack is used for generation or sequence-to-sequence tasks like translation and summarization.[file:2]

### 1.2 Encoder vs Encoder–Decoder Models  
- **BERT** and **RoBERTa** are encoder-only models designed to build rich contextual representations of text for tasks such as masked language modeling and classification.[file:2]  
- **BART** uses an encoder–decoder architecture where the encoder reads the input and the decoder generates output token by token, making it suitable for text generation and transformation tasks.[file:2]

### 1.3 Masked Language Modeling (MLM)  
In masked language modeling, some tokens in the input are replaced by a special `[MASK]` token and the model is trained to predict the original tokens.[file:2] This objective teaches encoder models like BERT and RoBERTa to fill in missing words in the middle of a sentence, which is why they perform well on fill-mask tasks.[file:2]

### 1.4 Fine-Tuning for Specific Tasks  
Base models learn general language representations, but high performance on tasks like question answering usually requires fine-tuning on task-specific datasets such as SQuAD.[file:2] Fine-tuned QA models (for example, `distilbert-base-cased-distilled-squad`) can accurately extract answers from context, whereas unfine-tuned base models often give partial or noisy answers.[file:2]

---

## 2. Solution Implemented

### 2.1 Objective  
The goal of the benchmark was to understand how model architecture affects behavior when models are used both for the tasks they were designed for and for tasks they were not explicitly designed for.[file:2] By “forcing” models into slightly misaligned tasks, it becomes possible to observe clear differences in capabilities and limitations.[file:2]

### 2.2 Models Used  
- **BERT (`bert-base-uncased`)** – Encoder-only, optimized for understanding and MLM.[file:2]  
- **RoBERTa (`roberta-base`)** – Encoder-only, BERT-style architecture with improved training.[file:2]  
- **BART (`facebook/bart-base`)** – Encoder–decoder, designed for text generation and sequence-to-sequence tasks.[file:2]

### 2.3 Experiments Designed  
Three experiments were implemented in a Jupyter notebook (`Unit1_Benchmark.ipynb`).[file:2][file:5]

1. **Text Generation**  
   - Prompt: “The future of Artificial Intelligence is …”  
   - Task: Ask each model to generate a continuation.[file:2]

2. **Fill-Mask (MLM)**  
   - Sentence: “The goal of Generative AI is to [MASK] new content.”  
   - Task: Predict the masked word.[file:2]

3. **Question Answering (QA)**  
   - Context: “Generative AI poses significant risks such as hallucinations, bias, and deepfakes.”  
   - Question: “What are the risks?”[file:2]

For each experiment, the success or failure of the model was recorded, along with observations explaining behavior based on its architecture in a summary table in the notebook.[file:5]

---

## 3. Key Findings from Experiments

### 3.1 Text Generation  

- **BERT – Failure**  
  BERT repeated parts of the prompt or produced low-quality continuations, showing that it is not suited for autoregressive next-token generation.[file:5] This matches its encoder-only design and MLM objective, which do not train it to generate text from left to right.[file:2]

- **RoBERTa – Failure**  
  RoBERTa behaved similarly to BERT, often producing repetitive or incoherent content because it is also an encoder-only MLM model without a generative decoder.[file:5][file:2]

- **BART – Success**  
  BART generated coherent, meaningful continuations of the prompt.[file:5] Its encoder–decoder architecture and autoregressive decoder make it well-suited for text generation tasks.[file:2]

### 3.2 Fill-Mask (MLM)  

- **BERT – Success**  
  BERT predicted appropriate verbs such as “create” or “generate,” which fit the sentence both grammatically and semantically.[file:5] This aligns with its core MLM training objective of recovering masked tokens from context.[file:2]

- **RoBERTa – Success**  
  RoBERTa also produced strong predictions for the masked word, consistent with its BERT-style encoder-only architecture and optimized MLM training.[file:5][file:2]

- **BART – Partial**  
  BART sometimes produced a reasonable replacement but was less consistent than BERT and RoBERTa on strict [MASK] prediction.[file:5] Its denoising training objective (span corruption, sentence permutation) makes it less specialized for pure token-level MLM.[file:2]

### 3.3 Question Answering (QA)  

- **BERT – Partial**  
  The base BERT model could identify fragments related to the risks but did not consistently output a clean, complete answer listing hallucinations, bias, and deepfakes.[file:5] This reflects the absence of QA-specific fine-tuning on datasets like SQuAD.[file:2]

- **RoBERTa – Partial**  
  RoBERTa produced responses that were loosely grounded in the context but typically incomplete.[file:5] As with BERT, strong QA performance requires fine-tuning, which the base model does not have.[file:2]

- **BART – Partial**  
  BART generated answer-like sentences but sometimes missed important details or was not tightly anchored to the provided context.[file:5] Its generative strengths do not replace the need for QA-focused fine-tuning.[file:2]

---

## 4. Overall Insight  

The experiments show that architecture and training objectives strongly determine what each model is good at.[file:2][file:5]

- Encoder-only models such as BERT and RoBERTa excel at MLM-style understanding tasks but perform poorly on free-form generation.[file:2][file:5]  
- Encoder–decoder models like BART handle text generation significantly better but may not match dedicated MLM models on strict token prediction.[file:2][file:5]  
- Fine-tuning on task-specific datasets is critical for high-quality question answering, regardless of the underlying architecture.[file:2][file:5]

---

## 5. Limitations and Future Work  

This benchmark uses only base versions of BERT, RoBERTa, and BART, without any task-specific fine-tuning, so the results do not represent the maximum achievable performance for the tasks considered.[file:2][file:5] The scope is also limited to a small set of prompts and three tasks, leaving out other common applications like classification, NER, and long-context reasoning.[file:2]

Possible future extensions include:[file:2]  
- Comparing base models to fine-tuned variants on QA and other downstream tasks.  
- Adding decoder-only GPT-style models to the benchmark for text generation.  
- Introducing quantitative metrics (for example, accuracy for MLM, F1 for QA, human ratings for generation quality) instead of relying only on qualitative observations.[file:2]

---

## 6. Phase 4 Project Direction (Law Domain)  

For the Phase 4 “Build & Innovate” component, the chosen domain is **Law** instead of Finance.[file:2] The proposed project idea is a **Legal Clause Risk Analyzer**, a prototype that takes a contract clause as input and highlights potential risks such as ambiguous liability, unfair termination conditions, or missing data protection language.[file:2]

The system would:[file:2]  
- Extract key legal entities (parties, dates, obligations) from the clause.  
- Classify clauses into categories like confidentiality, termination, payment, intellectual property, and liability.  
- Generate a short natural-language explanation of why a clause might be risky, using generative models to assist legal review and compliance workflows.[file:2]

This connects the architectural insights from the benchmark with a practical legal NLP application, illustrating how encoder and encoder–decoder models can be applied in a real-world law-related use case.[file:2]
