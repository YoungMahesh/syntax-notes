Large Language Models (LLMs) are advanced artificial intelligence systems designed to understand and generate human language

They are built on deep learning techniques, specifically using a type of neural network architecture known as transformer models

The term 'Large' refers to the extensive datasets on which these models are trained

### techniques to optimize the performance of the model
- prompting: multi-shot; chaining and tools
  - fast to implement; low cost
  - limited by context length; slower, expensive inference
- RAG
  - accuracy improvement with low data needs, scalable, efficient
  - harder than prompt, lack nuance
- fine-tuning
  - deep expertise & specialist knowledge, nuance, learn a different tone/style, faster and cheaper inference
  - need significant effort and resources to implement; high data needs; risk of 'catastrophic forgetting'

### inference

- process of generating responses or predictions based on the input provided by the user

- process
  1. input reception: the model receives a prompt or query
  2. tokenization: the input is broken down into tokens, which are the smallest units of meaning that the model can understand
  3. response generation: using complex probabilistic computations, the model generates a response by selecting the most likely next
     token based on the learned patterns
