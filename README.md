# End-to-End Language Translation (English → Hindi)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Docker](https://img.shields.io/badge/Docker-Enabled-2496ED.svg?logo=docker)
![FastAPI](https://img.shields.io/badge/FastAPI-Powered-009688.svg?logo=fastapi)

A production-oriented deep learning framework for translating text between languages. This repository implements a Seq2Seq Encoder-Decoder architecture using LSTM building blocks, served via a high-performance FastAPI interface with full containerization support.

---

# Overview

This system is designed to handle sequence-to-sequence translation tasks. Unlike naive implementations, this project separates the three core concerns of ML engineering:

1. **Model Architecture**
   - Specialized LSTM units for capturing long-range dependencies in source and target languages.

2. **Inference Engine**
   - Optimized auto-regressive decoding with vocabulary mapping and state management.

3. **Application Layer**
   - A production-grade API layer that handles request validation, asynchronous I/O, and multi-threaded serving.

---

# Technical Architecture

The system follows a Seq2Seq (Sequence-to-Sequence) design.

## The Encoder

- **Embedding Layer**
  - Maps source tokens to a high-dimensional latent space.

- **LSTM Core**
  - Processes padded mini-batches of embeddings to produce hidden states and cell states.

- **Output**
  - Provides the context required by the decoder for every time step.

---

## The Decoder

- **Auto-regressive Generation**
  - Generates tokens one-by-one until the `<EOS>` token is produced.

- **Context Awareness**
  - At each step, the decoder consumes the encoder's final hidden state and the previous step's cell state.

- **Prediction Head**
  - A Linear projection layer maps LSTM outputs back to the target vocabulary (Hindi).

---

# Tech Stack

### Deep Learning
- PyTorch
- LSTM Architecture
- CUDA Acceleration
- Tensor Management

### NLP
- Hugging Face Transformers
- T5-base Tokenizer

### API Layer
- FastAPI
- Pydantic v2
- JSON Serialization
- Schema Validation

### Deployment
- Docker
- Uvicorn

---

# Getting Started

## Prerequisites

- Python 3.9+
- CUDA Drivers (Optional, for GPU acceleration)
- Docker Engine

---

## Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/Manmath0108/End-to-End-Language-Translation.git
cd End-to-End-Language-Translation
```

### 2. Create Virtual Environment

```bash
python -m venv venv

# Linux / macOS
source venv/bin/activate

# Windows
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

# Running the System

You can run the project in two modes:

## Mode A: Development (Local CLI)

Run the FastAPI server directly:

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Access the API documentation:

- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

---

## Mode B: Production (Dockerized)

### Build the Docker Image

```bash
docker build -t language-translation .
```

### Run the Container

#### CPU

```bash
docker run -p 8000:8000 language-translation
```

#### GPU

```bash
docker run --runtime nvidia --gpus all -p 8000:8000 language-translation
```

---

# API Specification

## Endpoint

```http
POST /translate
```

Accepts a `TranslateRequest` object and returns the Hindi translation.

### Request Body

```json
{
  "text": "Hello, how are you?"
}
```

### Response

```json
{
  "hindi_translation": "नमस्ते, आप कैसे हैं?"
}
```

---

# Project Structure

```text
End-to-End-Language-Translation/
│
├── app/
│   ├── main.py
│   ├── routes.py
│   ├── schemas.py
│   └── inference.py
│
├── src/
│   ├── encoder.py
│   ├── decoder.py
│   ├── seq2seq.py
│   └── tokenizer.py
│
├── notebooks/
├── Dockerfile
├── requirements.txt
├── README.md
└── LICENSE
```

---

# License

This project is distributed under the **MIT License**.

See the [LICENSE](LICENSE) file for details.

---

# Author

**Manmath Tiwari**

- GitHub: [Manmath0108](https://github.com/Manmath0108)
- LinkedIn: [manmath-tiwari](https://linkedin.com/in/manmath-tiwari)

---

If you found this project useful, consider giving it a star.
