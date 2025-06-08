# Recipe Vector Embeddings Creation

This project creates vector database for recipes using a fine-tuned sentence transformer model. The embeddings are used for semantic search and recipe recommendations.

## Overview

The process involves:
1. Loading the fine-tuned model from Hugging Face Hub
2. Processing recipe data to create embeddings
3. Storing embeddings in Redis for efficient similarity search
4. Testing the embeddings with example queries

## Prerequisites

- Python 3.12+
- Redis instance
- Hugging Face account with access to the fine-tuned model
- CUDA-capable GPU (recommended)

## Dependencies

Install the required packages:
```bash
pip install sentence-transformers transformers[torch] tqdm s3fs redis peft
```

## Setup

Update Redis credentials and Hugging Face model IDs in the notebook:
```python
REDIS_HOST="your-redis-host"
REDIS_PORT="your-redis-port"
REDIS_USERNAME="your-redis-username"
REDIS_PASSWORD="your-redis-password"
BASE_MODE_ID = "base-model-id"
HF_MODEL_ID = "your-username/your-model-name"
```

## Process Overview

1. **Data Processing**
   - Reads recipe data from local zip file
   - Combines recipe titles and ingredients
   - Creates unique identifiers for each recipe
   - Prepares data for embedding generation

2. **Model Loading**
   - Loads the fine-tuned model from Hugging Face Hub using PEFT (Parameter-Efficient Fine-Tuning) to load LoRA weights
   - Moves model to GPU if available

3. **Embedding Generation**
   - Generates embeddings with 768 dimentions for each recipe
   - Uses batch processing for efficiency
   - Handles GPU memory management

4. **Redis Storage**
   - Connects to Redis instance
   - Creates Redis index for vector search
   - Stores embeddings with recipe metadata
   - Configures similarity search parameters

## Testing

The notebook includes example queries to test the embeddings:
1. "chocolate cake"
2. "spaghetti carbonara"
3. "chicken curry"

Each query will return:
- Similar recipes with their titles
- Links to the recipes
- Similarity scores

## Notes

- The process requires significant GPU memory for embedding generation
- Redis should have enough memory to store all embeddings
- The vector search index is optimized for cosine similarity

## Troubleshooting

Common issues and solutions:
1. **GPU Memory Errors**
   - Reduce batch size
   - Clear GPU memory between batches
   - Use CPU if GPU memory is insufficient

2. **Redis Connection Issues**
   - Verify Redis credentials
   - Check Redis memory limits
   - Ensure Redis is running and accessible

3. **Model Loading Errors**
   - Verify Hugging Face credentials
   - Make sure base model is the correct model for the LoRA config
   - Ensure all dependencies are installed
