# Ollama Project: Localhost Integration with LangChain

## Overview
This project integrates the Ollama language model running locally with LangChain to build a robust and flexible conversational AI application. The project is designed to leverage Ollama's capabilities for question answering and document-based reasoning.

---

## Features
- Local integration with the Ollama language model.
- Document chunking and embedding using LangChain.
- Vector store management with FAISS.
- API interaction for generating responses.
- Tunneled access to localhost using ngrok for external connectivity.

---

## Prerequisites
1. **Ollama Language Model**:
   - Download and install Ollama from its [official website](https://www.ollama.ai/).
   - Ensure it's running on `http://localhost:11434`.
2. **Python**:
   - Python 3.8 or later.
3. **Libraries**:
   Install required Python packages:
   ```bash
   pip install -r requirements.txt
   ```
4. **ngrok** (optional for Colab integration):
   - Install ngrok from [ngrok.com](https://ngrok.com/).

---

## Setup

### Step 1: Clone the Repository
```bash
git clone <repository-url>
cd <repository-folder>
```

### Step 2: Set Up Virtual Environment
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Configure Environment Variables
Create a `.env` file with the following variables:
```plaintext
LANGCHAIN_API_KEY=<your_langchain_api_key>
LANGCHAIN_PROJECT=ChatMyPDF
LANGCHAIN_ENDPOINT=https://api.smith.langchain.com
LANGCHAIN_TRACING_V2=true
```

---

## Running the Application

### Local Execution
1. Start the Ollama model locally:
   ```bash
   ollama serve
   ```

2. Run the Python application:
   ```bash
   python main.py
   ```

### Using Google Colab
To interact with the localhost Ollama instance in Google Colab:
1. Expose your localhost to the internet using ngrok:
   ```bash
   ngrok http 11434
   ```
2. Use the ngrok URL in your Colab notebook to make API requests to Ollama.

Example code in Colab:
```python
import requests

ollama_url = "https://abc123.ngrok.io"  # Replace with your ngrok URL
response = requests.post(f"{ollama_url}/api/ask", json={"query": "Hello, Ollama!"})
print(response.json())
```

---

## Project Workflow
1. **Document Ingestion**:
   - Documents are chunked into smaller pieces.
   - Each chunk is embedded using a pre-trained embedding model.
2. **Vector Store**:
   - FAISS stores embeddings for efficient similarity search.
   - New documents can be added dynamically.
3. **Query Handling**:
   - Queries are compared against stored embeddings.
   - The most relevant document is passed to Ollama for reasoning.

---

## Example Usage
### Add Documents to FAISS Vector Store
```python
from langchain.schema import Document
from langchain.vectorstores import FAISS
from langchain.embeddings import OpenAIEmbeddings

embeddings = OpenAIEmbeddings(openai_api_key="your_openai_api_key")
vector_store = FAISS(embeddings.embed_query, "vector_store_path")

documents = [
    Document(page_content="This is a sample text.", metadata={"source": "example.txt"})
]

vector_store.add_documents(documents)
```

### Query the Vector Store
```python
query = "What is Ollama?"
response = vector_store.similarity_search(query, top_k=1)
print(response)
```

---

## Troubleshooting
### Common Errors
1. **AssertionError in `add_documents`**:
   - Ensure documents are in the correct format (list of `Document` objects).
   - Check embedding dimensionality matches FAISS index.
2. **Connection Error in Colab**:
   - Verify ngrok tunnel is active and URL is correct.

### Debugging Tips
- Use `print()` statements to verify data formats and structures.
- Check the FAISS index size with:
  ```python
  print(vector_store.index.ntotal)
  ```

---

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## Acknowledgments
- [LangChain](https://www.langchain.com/) for the powerful framework.
- [Ollama](https://www.ollama.ai/) for the language model.
- [FAISS](https://faiss.ai/) for efficient similarity search.
