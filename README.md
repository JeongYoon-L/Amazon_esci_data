# Amazon ESCI Data Project

Dataset: [Amazon ESCI Dataset](https://github.com/amazon-science/esci-data)

## Project Objective

The objective of this project is to build a vector index for the product dataset and evaluate the quality of this index against the provided search queries.

## Dataset Summary

The dataset is **multilingual**, including queries in English, Japanese, and Spanish. It contains:
- **1,802,772products**
- **48,300 unique queries**
- A list of query-result pairs annotated with **E/S/C/I labels** (Exact, Substitute, Complement, Irrelevant).

## Approach

### System Diagram
![System Diagram](https://github.com/user-attachments/assets/9c424e30-15f3-4256-8094-8b47a1869fd2)

### Technical Environment
- **Platform**: Google Colab
- **Hardware**: A100 GPU

Due to resource limitations, 10000 samples are used when running the model.

Vector Index: **FAISS** (Facebook AI Similarity Search)  
Embedding Model: **SentenceTransformer('all-MiniLM-L6-v2')**  
Distance Metric: **Cosine Similarity**

### Product Embeddings

For generating product embeddings, the following product attributes were concatenated into a single string:
- Product Title : {Product Title}, Product Brand : {Product Brand}, Description : {Description}, Bullet Points: {Bullet Points}

The rationale behind this approach is to enrich the product representation with more descriptive information, potentially leading to better results. This concatenation strategy would also be beneficial for a prompt-based model like RAG (Retrieval-Augmented Generation), which thrives on rich context.

### Assumptions

The model assumes that all products are available in all countries and that queries in one language can return product recommendations in other languages.

### Why Use SentenceTransformer over Vanilla BERT?

The model **SentenceTransformer('all-MiniLM-L6-v2')** was chosen for its speed, strong performance in understanding sentence similarities, and pre-fine-tuned high-level capabilities, which makes it easy and fast to implement. It offers a well-balanced trade-off between performance and computational efficiency.

More details about the performance comparison can be found in the official documentation:  
[Pretrained SentenceTransformer Models](https://www.sbert.net/docs/sentence_transformer/pretrained_models.html)

## Results

All products were embedded using the model, and for the queries, 10,000 samples were randomly selected using a fixed seed. The evaluation metrics used were **Hit@K** and **MRR**.

### Evaluation Results (10,000 Query Samples Across All Languages)

- **Hit@1**: 0.0220
- **Hit@3**: 0.0595
- **Hit@10**: 0.1493
- **MRR**: 0.0525

## How Can I Improve the Performance? / What Other Approaches Can I Try?

To further improve performance, I plan to explore the following strategies:

1. **Using `bert-base-multilingual-cased`**: This could potentially handle multilingual queries and products more effectively.
2. **Separate Embeddings for Each Language**: Encode products separately for each language and evaluate the performance on language-specific subsets.
3. **Preprocess Product Descriptions**: Check for any noise or inconsistencies in product descriptions and clean the data to improve embeddings.
4. **Apply a RAG Approach**: Implementing Retrieval-Augmented Generation (RAG) could enhance results by understanding context of queries and product descriptions.
