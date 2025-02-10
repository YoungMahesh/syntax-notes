# example
```python
# -q = quiet; minimize console output
!pip install -q datasets requests torch peft bitsandbytes tranformers trl accelerate sentencepiece

import os
import re
import math
from tqdm import tqdm
from google.colab import userdata
from huggingface_hub import login
import torch
import transformers
from transformers import AutoModelForCausalLM, AutoTokenizer, BitsAndBytesConfig, TrainingArguments, set_seed
from peft import LoraConfig, PeftModel
from datetime import datetime

# constants
BASE_MODEL = "meta-llama/Meta-Llama-3.1-8B"
FINETUNED_MODEL = f"ed-donner/pricer-2024-09-13_13.04.39"

# Hyperparameters for OLoRA Fine-Tuning

# r - the inner dimension of the low-rank matrices
# started with 8 then double to 16, then good results with 32
LORA_R = 32


LORA_ALPHA = 64
```

# word meanings
- parameters
  - broder term that encompasses all learnable values in a neural network, including weights and biases
  - think of parameters as the entire collection of values that define the network's configuration
- weights
  - specific type of parameter; all weights are parameters
  - represent the strength of connections between neurons in different layers
  - determine how much influence one neuron has on another
  - typically organized in matrices
  - updated during training to minimize the loss function 
  - in technique like LoRA, you're specifically manipulating weights while keeping other parameters frozen
- weight and parameters in Llama 3.1 8B model
  - 8B == 8 billion parameters
  - majority of the 8 billion parameters are weight parameters
    - weights include: attention weights, projection matrices, embedding weights
    - not all 8 billion parameters are equally important, LoRA allows fine-tuning by manipulating a small subset of weights 
    - typically, you might only train 1-10% of the total parameters during fine-tuning

# LoRA (Low-Rank Adaptation)
- a popular technique for efficiently adapting large language models (LLMs) 
- it is an innovative parameter-efficient fine-tuning method designed to reduce the computational and memory costs of adapting 
large neural networks
- problem it solves
  - traditional fine-tuning requires updating all parameters of a large model, which is computationally expensive and memory intensive. 
  For massive models like GPT-3, LLaMA with billions of parameters, full fine-tuning is often impractical
- core principle
  - instead of updating all model weights, LoRA introduces small, trainable "rank decomposition" matrices. These matrices are added to
  the original pre-trained model weights during fine-tuning.
- how it works
  - for a weight matrix W, LoRA introduces two smaller matrices: A (low-rank) and B.
  - the update to the original weights becomes: W' = W + BA
  - A and B are much smaller than W, significantly reducing trainable parameters
- limitations
  - performance can be slightly lower than full fine-tuning 
  - requires careful selection of rank and adaptation strategy 
- best practices
  - start with a low rank (4-16) and increase if needed
  - experiment with different adaptation techniques
  - monitor performance metrics during fine-tuning

## QLoRA - Quantization Low-Rank Adaptation
- combines two powerful techniques: Quantization and Low-Rank Adaptation (LoRA)
- designed to enable fine-tuning of large language models with even more limited computational resources
- keep the number of weights but reduce their precision
- Quantization
  - Reduces model precision from 32-bit floating-point to lower bit-width (e.g. 4-bit)
    - 4 bits are interpreted as float, not int
    - the adapter matrices are still 32-bit
  - dramatically reduces memory requirements
  - minimal performance degradation compared to full-precision models
- LoRA
  - introduces small, trainable LoRA matrices
  - train only these low-rank matrices
  - minimal computational and memory overhead
- advantages
  - enables fine-tuning on consumer-grade hardware
  - reduces memory footprint by 4-10x
  - maintains near-original model performance
  - allows adapation of massive models with limited resources
- best practices
  - start with 4-bit quantization
  - use adaptive optimizers
  - monitor performance metrics
  - experiment with different low-rank matrix sizes

## Hyperparameters
- configuration settings that are set before the learning process begins and control how a machine learning model is trained
- unlike model parameters (like weights and biases), that are learning during training, hyperparameters are set by the machine
learning engineer to guide the training process
- characteristics
  - set before training begins
  - cannot be directly learned from the data
  - significantly impact model performance
  - typically require manual tuning or automated search techniques
- common types
  - learning rate
    - controls how much the model changes in response to the learned error
    - too high: model might overshoot optimal solution
    - too low: model learns very slowly
  - batch size
    - number of training examples used in each iteration
    - affects training speed and model generalization
    - smaller batches: more noise in gradient estimation
    - larger batches: more stable gradient estimation
  - number of epochs
    - how many times the entiner training dataset is passed through the model
    - too few: underfitting; too many: overfitting
  - network architecture 
    - number of layers, number of neurons per layer, activation functions, dropout rates
  - regularization
    - L1/L2 regularization strength, dropout rate, early stopping patience
- key hyperparameters for LoRA fine-tuning
  - 3 essential: rank, alpha, target modules
  - rank (r)
    - the inner dimension of the low-rank matrices
    - low-rank matrices: 
    - typical range: 4-16
    - impact: lower rank (4-8): less computational cost; higher-rank(32-64): more capacity to capture task-specific nuances
    - rule of thumb: start with 8 then double to 16, then 32, until diminishing returns
  - alpha (scaling factor)
    - scaling factor that multiplies the lower rank matrices
    - controls the magnitude of LoRA updates
    - typical range: 8 to 32
    - rule of thumb: 2 times the value of r
    - recommendation: start with 16, adjust based on task complexity
  - target modules
    - which model layers to apply LoRA to
    - common choices
      - for transformer models: `q_proj`, `v_proj`, `k_proj`
      - sometimes include `o_proj`, `down_proj`
    - rule of thumb: target the attention head layers
  - learning rate (α)
    - scaling factor for the low-rank adaptation matrices
    - typical range: 0.1 to 2.0
    - recommendation: start with 1.0, use learning rate scheduling (e.g., cosine decay)
  - dropout rate
    - regularization to prevent overfitting
    - typical range: 0.1 to 0.3
    - recommendation: 0.1 is often a good starting point
  - batch size
    - typical range: 8 to 128
    - depends on: available GPU memory, task complexity, dataset size
  - epochs/training steps
    - typical range: 1 to 5 epochs
    - recommendation: use early stopping and validation monitoring
  ```py
  # example psuedocode configuration
  lora_config = {
    'r': 16, # randk of adaptation
    'lora_alpha': 16, # scaling factor
    'lora_dropout': 0.1, # dropout rate
    'target_modules': ['q_proj', 'v_proj'],
    'learning_rate': 1e-4, # adam learning rate
  }
  ```

## Llama fine-tuning with LoRA
- Llama 3.1 8B architecture consists of 32 groups of modules stacked on top of each other, called 'Llama Decoder Layers'
- Each has self-attention layers, multi-layer perception layers, SiLU activation and layer norm
- These parameters take up 32GB memory (3.1 8B model takes up 32GB, 8B refers to 8-billion parameters), training of it will take way more
than 32GB memory
- But with LoRA, we can do fine-tuning with limited resources with following steps
  1. freeze the weights, we will not optimize them
  2. select some layers to target, called 'Target Modules'
  3. create new 'adapter' metrices with lower dimensions, fewer parameters
  4. apply these adapters to the Target Modules to adjust them - and these get trained
    - to be more precise, there are infact two LoRA metrices A and B that get applied



