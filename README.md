🚀 Text-to-SQL Engine using Fine-Tuned LLM
📌 Overview
**Ignore the sql_tune_gguf(1).ipynb file
This project demonstrates an end-to-end pipeline for building a domain-specific Text-to-SQL generator using a fine-tuned Large Language Model (LLM). The system converts natural language queries into accurate SQL queries tailored for utility domain datasets (e.g., feeder and complaints tables).

The model is fine-tuned using LoRA (Low-Rank Adaptation) on a custom dataset and deployed locally using Ollama for real-time inference.

🎯 Features
🔹 Natural Language → SQL Query conversion
🔹 Domain-specific training (utility data)
🔹 Efficient fine-tuning using LoRA
🔹 4-bit quantization for optimized performance
🔹 GGUF model conversion for lightweight inference
🔹 Local deployment using Ollama
🔹 Prompt engineering to reduce hallucinations
🧠 Model Details
Base Model: TinyLlama-1.1B-Chat
Fine-Tuning Method: LoRA (PEFT)
Trainer: SFTTrainer (TRL)
Quantization: 4-bit (BitsAndBytes)
🛠️ Tech Stack
Python
PyTorch
Hugging Face Transformers
TRL (SFTTrainer)
PEFT (LoRA)
BitsAndBytes
Ollama
GGUF (llama.cpp)
JSON (dataset format)
⚙️ Training Pipeline
Step 1: Install Dependencies
pip install torch torchvision torchaudio
pip install transformers datasets peft trl bitsandbytes accelerate
Step 2: Fine-Tune Model
python train.py
Step 3: Merge LoRA Weights
from peft import PeftModel
from transformers import AutoModelForCausalLM

base_model = AutoModelForCausalLM.from_pretrained("base_model")
model = PeftModel.from_pretrained(base_model, "adapter_model")
model = model.merge_and_unload()
model.save_pretrained("merged_model")
Step 4: Convert to GGUF
python convert_hf_to_gguf.py merged_model --outfile sql_model.gguf
🚀 Running with Ollama

Step 1: Create Modelfile
FROM ./sql_model.gguf

PARAMETER temperature 0

SYSTEM """
You are a SQL query generator.
Use only provided table names.
Do not hallucinate.
Return only SQL queries.
"""
Step 2: Create Model
ollama create sql-model -f Modelfile
Step 3: Run Model
ollama run sql-model

🧪 Example
Input:
Get all complaints for today
Output:
SELECT * FROM complaints_fluentgrid WHERE date = CURRENT_DATE;
