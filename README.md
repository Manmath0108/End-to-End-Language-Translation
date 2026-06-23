# End-to-End Language Translation (English → Hindi)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Docker](https://img.shields.io/badge/Docker-Enabled-2496ED.svg?logo=docker)
![FastAPI](https://img.shields.io/badge/FastAPI-Powered-009688.svg?logo=fastapi)
![PyTorch](https://img.shields.io/badge/PyTorch-Powered-EE4C2C.svg?logo=pytorch)
![Python](https://img.shields.io/badge/Python-3.9+-3776AB.svg?logo=python)

A production-oriented deep learning system for English → Hindi sequence-to-sequence translation. Built from scratch using a custom LSTM Encoder-Decoder architecture in PyTorch, served via a FastAPI inference engine, and fully containerized with Docker — with GPU runtime support.

---

## What This Project Demonstrates

Most translation projects fine-tune a pretrained model and wrap it in Flask. This one doesn't.

Every layer here is hand-built:
- A **custom LSTM encoder** that maps padded token batches into context states
- A **custom auto-regressive decoder** that generates Hindi tokens step-by-step until `<EOS>`
- A **production FastAPI serving layer** with Pydantic v2 validation and async I/O
- A **Dockerized deployment** supporting both CPU and CUDA GPU runtimes

The goal was to understand what happens inside a translation model — not just call one.

---

## Architecture

```
Input (English Text)
        │
        ▼
[ T5-base Tokenizer ]        ← Subword tokenization via Hugging Face
        │
        ▼
[ Encoder: Embedding → LSTM ]   ← Produces hidden state + cell state
        │
        ▼
[ Decoder: LSTM → Linear Head ] ← Auto-regressive generation, step-by-step
        │
        ▼
Output (Hindi Translation)
```

### Encoder
- **Embedding layer** maps source tokens to a high-dimensional latent space
- **LSTM core** processes padded mini-batches and produces hidden + cell states passed to the decoder at every timestep

### Decoder
- Consumes encoder's final hidden state as initial context
- Generates target tokens one at a time (**auto-regressive**) until `<EOS>` is produced
- **Linear projection head** maps LSTM outputs to the Hindi target vocabulary

---

## Tech Stack

| Layer | Technology |
|---|---|
| Deep Learning | PyTorch, LSTM, CUDA |
| Tokenization | Hugging Face Transformers, T5-base |
| API | FastAPI, Pydantic v2, Uvicorn |
| Deployment | Docker (CPU + GPU), Uvicorn |

---

## Project Structure

```
End-to-End-Language-Translation/
│
├── app/
│   ├── main.py          # FastAPI app entrypoint
│   ├── routes.py        # /translate endpoint
│   ├── schemas.py       # Pydantic v2 request/response models
│   └── inference.py     # Model loading + decoding logic
│
├── src/
│   ├── encoder.py       # LSTM Encoder
│   ├── decoder.py       # Auto-regressive LSTM Decoder
│   ├── seq2seq.py       # Seq2Seq wrapper
│   └── tokenizer.py     # T5-base tokenizer integration
│
├── notebooks/           # Training + experimentation
├── Dockerfile
├── requirements.txt
├── README.md
└── LICENSE
```

---

## API Reference

### `POST /translate`

Accepts English text, returns Hindi translation.

**Request**
```json
{
  "text": "Hello, how are you?"
}
```

**Response**
```json
{
  "hindi_translation": "नमस्ते, आप कैसे हैं?"
}
```

Interactive docs available at:
- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

---

## Getting Started

### Prerequisites
- Python 3.9+
- Docker Engine
- CUDA Drivers *(optional, for GPU acceleration)*

### Local Development

```bash
git clone https://github.com/Manmath0108/End-to-End-Language-Translation.git
cd End-to-End-Language-Translation

python -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate

pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

### Docker (CPU)

```bash
docker build -t language-translation .
docker run -p 8000:8000 language-translation
```

### Docker (GPU)

```bash
docker run --runtime nvidia --gpus all -p 8000:8000 language-translation
```

---

## Key Design Decisions

**Why LSTM and not a Transformer?**
The goal was to learn sequence modeling from first principles — understanding hidden states, cell states, and auto-regressive decoding before abstracting them away. Transformers hide this under attention layers; LSTM makes the information flow explicit.

**Why T5-base tokenizer on an LSTM model?**
Using a subword tokenizer prevents the OOV (out-of-vocabulary) problem that plagues character or word-level tokenization, without requiring a full Transformer backbone. It's the right tool for the tokenization job independent of the model architecture.

**Why separate `encoder.py`, `decoder.py`, `seq2seq.py`?**
Mirrors production ML codebases where components are independently testable and swappable. Swapping the decoder from LSTM to a Transformer attention decoder requires touching only one file.

---

## License

Distributed under the **MIT License**. See [LICENSE](LICENSE) for details.

---

## Author

**Manmath Tiwari**
- GitHub: [Manmath0108](https://github.com/Manmath0108)
- LinkedIn: [manmath-tiwari](https://linkedin.com/in/manmath-tiwari)
- Medium: [manmathtewaridps](https://medium.com/@manmathtewaridps)

---

*If this project was useful, consider giving it a star ⭐*
