# Legal RAG - Retrieval-Augmented Generation for Legal Documents

A powerful Retrieval-Augmented Generation (RAG) system designed to answer questions about legal documents using advanced AI models. This project uses LangChain, Ollama, and FAISS to provide accurate, context-aware answers based on your legal document repository.

## Features

- 🔍 **Document Retrieval**: Efficiently search and retrieve relevant sections from legal documents using FAISS vector database
- 🤖 **AI-Powered Answers**: Generate accurate answers using local Ollama LLMs with context from your documents
- 📄 **PDF Support**: Process and analyze PDF documents automatically
- 🔐 **Privacy-Focused**: Run everything locally with Ollama - no cloud dependencies
- ⚡ **Fast Embeddings**: Uses `nomic-embed-text` for quick and accurate text embeddings
- 📊 **Scalable**: Easily add more documents to your knowledge base

## Architecture

```
Legal Documents (PDF)
        ↓
    PDF Parser (fitz)
        ↓
Text Embeddings (Ollama)
        ↓
FAISS Vector Store
        ↓
Query Processing
        ↓
Context Retrieval
        ↓
LLM Response Generation (Ollama)
```

## Prerequisites

- Python 3.8+
- Ollama (running locally)
- Virtual environment (recommended)

### Ollama Setup

1. Install Ollama from [ollama.ai](https://ollama.ai)
2. Pull required models:
   ```bash
   ollama pull nomic-embed-text:latest
   ollama pull llama3.2:3b
   ```
3. Start Ollama server (usually runs on `http://localhost:11434`)

## Installation

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/legal-rag.git
cd legal-rag
```

### 2. Create virtual environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure environment variables
```bash
cp .env.example .env
```

Edit `.env` with your configuration:
```env
OLLAMA_BASE_URL=http://localhost:11434
EMBEDDING_MODEL=nomic-embed-text:latest
LLM_MODEL=llama3.2:3b
```

## Usage

### Building the Knowledge Base

First, place your PDF documents in the `docs/` folder, then run:

```bash
python build_rag_index.py
```

This will:
- Read all PDFs from `docs/` folder
- Generate embeddings for each document
- Store them in a FAISS vector database (`.rag_faiss_store`)

### Querying the Knowledge Base

```bash
python test.py
```

Or use it programmatically:

```python
from test import retrieve_legal_index, genrate_final_answer

# Retrieve relevant context
query = "What are the cleaning fees?"
context = retrieve_legal_index(query)

# Generate answer using retrieved context
answer = genrate_final_answer(query, context)
print(answer)
```

## Project Structure

```
legal-rag/
├── README.md                      # Project documentation
├── requirements.txt               # Python dependencies
├── .env.example                   # Environment configuration template
├── .gitignore                     # Git ignore rules
├── test.py                        # Main query interface
├── rag_embedding_genrator.py      # Embedding generation (helper)
├── database.db                    # SQLite database
├── docs/
│   └── sample_rental_agreement.pdf # Sample legal document
└── .rag_faiss_store/              # FAISS vector store (auto-generated)
```

## Key Components

### `test.py`
Main module for querying the legal document knowledge base.

**Functions:**
- `retrieve_legal_index(query)`: Retrieves top 3 most relevant document sections
- `genrate_final_answer(query, context)`: Generates AI-powered answer from context

### Embedding Generator
Uses `nomic-embed-text` model for creating dense vector representations of text chunks.

### Vector Database (FAISS)
Stores embeddings locally for lightning-fast similarity search without cloud dependencies.

## Example Queries

```python
queries = [
    "What are cleaning fees?",
    "What is the cancellation policy?",
    "Who is responsible for maintenance?",
    "What are the payment terms?",
]

for query in queries:
    context = retrieve_legal_index(query)
    answer = genrate_final_answer(query, context)
    print(f"Q: {query}\nA: {answer}\n")
```

## Configuration

### Models

- **Embedding Model**: `nomic-embed-text:latest` - Fast, accurate text embeddings
- **LLM Model**: `llama3.2:3b` - Lightweight, fast inference for legal Q&A

To use different models, update `.env`:
```env
EMBEDDING_MODEL=your-embedding-model
LLM_MODEL=your-llm-model
```

### Temperature

Adjust response creativity (default: 0.2 for factual legal answers):
- Lower values (0.0-0.3): More factual and consistent
- Higher values (0.7-1.0): More creative and varied

## Performance Optimization

- **Chunk Size**: Adjust document chunk size for better context retrieval
- **Search K**: Modify `k=3` in `similarity_search()` to retrieve more/fewer chunks
- **Embedding Model**: Switch to a larger model for better accuracy (if computational resources allow)

## Limitations

- Responses are limited to information in provided documents
- Does not perform real-time legal research or updates
- Should not replace professional legal advice
- Model hallucinations are possible - always verify important information

## Future Enhancements

- [ ] Web UI for easier querying
- [ ] Support for additional document formats (DOCX, TXT, etc.)
- [ ] Advanced filtering and metadata support
- [ ] Citation tracking for answer sources
- [ ] Multi-language support
- [ ] Batch processing for multiple queries
- [ ] API endpoint for integration with other systems

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

MIT License - see LICENSE file for details

## Disclaimer

⚠️ **Important**: This tool is for informational purposes only and should not be used as a substitute for professional legal advice. Always consult with a qualified attorney for legal matters.

## Support

For issues, questions, or suggestions, please:
- Open an [Issue](https://github.com/yourusername/legal-rag/issues)
- Start a [Discussion](https://github.com/yourusername/legal-rag/discussions)
- Contact: [your-email@example.com]

## Acknowledgments

- [LangChain](https://langchain.com/) - LLM framework
- [Ollama](https://ollama.ai/) - Local LLM inference
- [FAISS](https://github.com/facebookresearch/faiss) - Vector similarity search
- [PyMuPDF](https://pymupdf.readthedocs.io/) - PDF processing

---

Built with ❤️ for the legal tech community
