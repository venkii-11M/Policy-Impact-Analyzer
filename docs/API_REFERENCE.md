# API Reference

## PolicyRAG Class

The core RAG engine that handles document processing and question answering.

### Location
`rag_engine.py`

### Class: `PolicyRAG`

#### Constructor

```python
PolicyRAG()
```

**Description**: Initializes the RAG engine with a sentence transformer model and empty index.

**Attributes**:
- `embedder` (SentenceTransformer): Model for converting text to embeddings (all-MiniLM-L6-v2)
- `index` (faiss.Index): FAISS vector index for similarity search
- `text_chunks` (list): Stores processed text chunks from the document

---

#### Methods

##### `load_pdf(pdf_path: str) -> None`

**Description**: Processes a PDF file and builds the vector index for retrieval.

**Parameters**:
- `pdf_path` (str): Absolute or relative path to the PDF file

**Process**:
1. Extracts text from all PDF pages using PyPDF
2. Compresses the extracted text using ScaleDown API (75% reduction)
3. Splits compressed text into chunks (500 chars, 50 char overlap)
4. Generates embeddings for each chunk
5. Builds FAISS index for fast similarity search

**Returns**: None

**Side Effects**:
- Populates `self.text_chunks` with document chunks
- Builds `self.index` with vector embeddings

**Example**:
```python
rag = PolicyRAG()
rag.load_pdf("employee_handbook.pdf")
```

---

##### `ask(question: str) -> str`

**Description**: Answers a question based on the loaded policy document.

**Parameters**:
- `question` (str): Natural language question about the policy

**Returns**:
- `str`: AI-generated answer based on relevant policy context

**Process**:
1. Converts question to embedding vector
2. Searches FAISS index for 10 most similar chunks
3. Assembles context from retrieved chunks
4. Sends context + question to Gemini 2.5 Flash
5. Returns AI-generated answer

**Example**:
```python
answer = rag.ask("What is the remote work policy?")
print(answer)
```

**Note**: Document must be loaded with `load_pdf()` before calling this method.

---

## ScaleDown Compression

### Location
`scaledown.py`

### Function: `compress(text: str) -> str`

**Description**: Compresses text using ScaleDown API or fallback truncation.

**Parameters**:
- `text` (str): Full text to compress

**Returns**:
- `str`: Compressed text (75% reduction target)

**Behavior**:
- **With API Key**: Sends text to ScaleDown API for intelligent compression
- **Without API Key**: Returns first 25% of text (simple truncation)
- **On API Error**: Falls back to truncation and logs error

**Environment Variables**:
- `SCALEDOWN_API_KEY`: Optional API key for ScaleDown service

**Example**:
```python
from scaledown import compress

long_text = "..." * 10000
compressed = compress(long_text)
print(f"Reduced to {len(compressed)/len(long_text)*100:.1f}% of original")
```

---

## Streamlit Application

### Location
`app.py`

### Main Components

#### `init_rag() -> PolicyRAG`

**Description**: Cached function that initializes the RAG engine once per session.

**Returns**:
- `PolicyRAG`: Initialized RAG instance

**Decorator**: `@st.cache_resource` (singleton pattern)

---

### UI Flow

1. **Page Configuration**
   - Title: "📘 Policy Document Navigator"
   - Description prompt

2. **File Upload**
   - Accepts: PDF files only
   - Storage: `temp_uploads/` directory

3. **Document Processing**
   - Shows spinner: "Processing policy document..."
   - Calls `rag.load_pdf(file_path)`
   - Success message on completion

4. **Question Input**
   - Text area for user questions
   - "Get Answer" button

5. **Answer Display**
   - Shows spinner: "Analyzing policy..."
   - Calls `rag.ask(question)`
   - Displays formatted answer with header

---

## Configuration Parameters

### RAG Engine (`rag_engine.py`)

| Parameter | Current Value | Location | Description |
|-----------|--------------|----------|-------------|
| Embedding Model | `all-MiniLM-L6-v2` | `PolicyRAG.__init__()` | SentenceTransformer model |
| Chunk Size | 500 | `load_pdf()` | Characters per chunk |
| Chunk Overlap | 50 | `load_pdf()` | Overlap between chunks |
| Top-K Retrieval | 10 | `ask()` | Number of chunks retrieved |
| Vector Dimension | 384 | Auto (model) | Embedding vector size |
| FAISS Index Type | `IndexFlatL2` | `load_pdf()` | L2 distance search |
| LLM Model | `gemini-2.5-flash` | `ask()` | Google Gemini model |

### Compression (`scaledown.py`)

| Parameter | Current Value | Location | Description |
|-----------|--------------|----------|-------------|
| Compression Ratio | 0.25 | `compress()` | Target 75% reduction |
| API Timeout | 10 seconds | `compress()` | Request timeout |
| Fallback Method | Truncation | `compress()` | Simple start slice |

### Streamlit App (`app.py`)

| Parameter | Current Value | Location | Description |
|-----------|--------------|----------|-------------|
| Page Title | "Policy Document Navigator" | `app.py` | Browser tab title |
| Upload Dir | `temp_uploads/` | `app.py` | Temporary file storage |
| Accepted Formats | `["pdf"]` | `app.py` | File type filter |

---

## Environment Variables

### Required

| Variable | Purpose | How to Get |
|----------|---------|------------|
| `GEMINI_API_KEY` | Google Gemini API access | [Google AI Studio](https://makersuite.google.com/app/apikey) |

### Optional

| Variable | Purpose | Default Behavior |
|----------|---------|------------------|
| `SCALEDOWN_API_KEY` | ScaleDown compression service | Falls back to truncation |

---

## Error Handling

### ScaleDown API Errors

**Location**: `scaledown.py`

**Behavior**:
- Catches `requests.exceptions.RequestException` and generic exceptions
- Prints error message to console
- Falls back to truncation (first 25% of text)
- Returns partial text instead of failing

**Example Output**:
```
ScaleDown API error: Connection timeout. Using fallback compression.
```

### Common Issues

#### Missing API Key
**Symptom**: No compression or API errors

**Solution**: Add `GEMINI_API_KEY` to `.env` file

#### PDF Processing Errors
**Symptom**: Exception during file upload

**Possible Causes**:
- Corrupted PDF file
- Encrypted/password-protected PDF
- Scanned PDF without text layer

#### Empty Answers
**Symptom**: No relevant answer returned

**Possible Causes**:
- Question not related to document content
- Document text extraction failed
- Compression removed relevant content

---

## Performance Considerations

### Memory Usage

- **Embeddings**: 384 floats × number of chunks
- **FAISS Index**: In-memory, scales with document size
- **Text Storage**: Full chunk array in memory

**Recommendation**: For large documents (>100 pages), consider:
- Increasing chunk size to reduce chunk count
- Using FAISS `IndexIVFFlat` for disk-based storage
- Implementing chunk limits or pagination

### API Costs

- **Gemini API**: Pay-per-token pricing
- **ScaleDown API**: Check rate limits and quotas
- **Optimization**: Compression reduces Gemini token usage by ~75%

### Response Time

Typical latencies:
- PDF processing: 2-10 seconds (depends on size)
- Question answering: 1-3 seconds
- FAISS search: <100ms (negligible)
