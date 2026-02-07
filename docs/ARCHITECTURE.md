# System Architecture

## Overview

Policy Document Navigator is built using a Retrieval-Augmented Generation (RAG) architecture, combining semantic search with large language model capabilities to answer questions from policy documents.

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER INTERFACE                            │
│                      (Streamlit Web App)                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ PDF Upload   │  │ Text Input   │  │ Answer       │          │
│  │ Component    │  │ Component    │  │ Display      │          │
│  └──────┬───────┘  └──────┬───────┘  └──────▲───────┘          │
└─────────┼──────────────────┼──────────────────┼─────────────────┘
          │                  │                  │
          ▼                  ▼                  │
┌─────────────────────────────────────────────────────────────────┐
│                      APPLICATION LAYER                           │
│                         (app.py)                                 │
│  ┌──────────────────────────────────────────────────────┐       │
│  │  • File Upload Handler                               │       │
│  │  • Session State Management                          │       │
│  │  • UI Event Handlers                                 │       │
│  └────────────┬─────────────────────────────────┬───────┘       │
└───────────────┼─────────────────────────────────┼───────────────┘
                │                                 │
                ▼                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│                    RAG ENGINE LAYER                              │
│                    (rag_engine.py)                               │
│  ┌──────────────────────┐       ┌──────────────────────┐        │
│  │  INDEXING PIPELINE   │       │  QUERY PIPELINE      │        │
│  │  ┌────────────────┐  │       │  ┌────────────────┐  │        │
│  │  │ 1. PDF Reader  │  │       │  │ 1. Embed Query │  │        │
│  │  └────────┬───────┘  │       │  └────────┬───────┘  │        │
│  │  ┌────────▼───────┐  │       │  ┌────────▼───────┐  │        │
│  │  │ 2. Compress    │  │       │  │ 2. Vector      │  │        │
│  │  │    Text        │  │       │  │    Search      │  │        │
│  │  └────────┬───────┘  │       │  └────────┬───────┘  │        │
│  │  ┌────────▼───────┐  │       │  ┌────────▼───────┐  │        │
│  │  │ 3. Split into  │  │       │  │ 3. Retrieve    │  │        │
│  │  │    Chunks      │  │       │  │    Top-K       │  │        │
│  │  └────────┬───────┘  │       │  └────────┬───────┘  │        │
│  │  ┌────────▼───────┐  │       │  ┌────────▼───────┐  │        │
│  │  │ 4. Generate    │  │       │  │ 4. Build       │  │        │
│  │  │    Embeddings  │  │       │  │    Context     │  │        │
│  │  └────────┬───────┘  │       │  └────────┬───────┘  │        │
│  │  ┌────────▼───────┐  │       │  ┌────────▼───────┐  │        │
│  │  │ 5. Build FAISS │  │       │  │ 5. Call LLM    │  │        │
│  │  │    Index       │  │       │  │    for Answer  │  │        │
│  │  └────────────────┘  │       │  └────────────────┘  │        │
│  └──────────────────────┘       └──────────────────────┘        │
└─────────────────────────────────────────────────────────────────┘
                │                                 │
                ▼                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│                    EXTERNAL SERVICES                             │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐  │
│  │  ScaleDown API   │  │  Google Gemini   │  │  Sentence    │  │
│  │  (Compression)   │  │  2.5 Flash (LLM) │  │  Transformers│  │
│  └──────────────────┘  └──────────────────┘  └──────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│                       DATA STORAGE                               │
│  ┌──────────────────┐  ┌──────────────────┐                     │
│  │  temp_uploads/   │  │  In-Memory       │                     │
│  │  (Temp PDFs)     │  │  FAISS Index     │                     │
│  └──────────────────┘  └──────────────────┘                     │
└─────────────────────────────────────────────────────────────────┘
```

## Component Details

### 1. User Interface Layer (Streamlit)

**Responsibilities**:
- File upload interface
- User input collection
- Response rendering
- Session state management

**Technology**: Streamlit 1.x

**Key Features**:
- Drag-and-drop PDF upload
- Text area for questions
- Markdown-formatted answers
- Loading spinners for feedback
- Cached resource management

---

### 2. Application Layer (app.py)

**Responsibilities**:
- Route user interactions to RAG engine
- Manage temporary file storage
- Handle UI state and caching
- Error handling and user feedback

**Key Functions**:
```python
@st.cache_resource
def init_rag() -> PolicyRAG  # Singleton RAG instance

# Main flow:
# 1. Upload → Save to temp_uploads/
# 2. Process → rag.load_pdf()
# 3. Question → rag.ask()
# 4. Display → st.write()
```

---

### 3. RAG Engine Layer (rag_engine.py)

#### 3.1 Indexing Pipeline

**Step 1: PDF Text Extraction**
- Library: PyPDF (PdfReader)
- Process: Extract text from each page, join with newlines
- Output: Full document text string

**Step 2: Text Compression**
- Service: ScaleDown API (optional)
- Ratio: 75% reduction (0.25 compression)
- Fallback: First 25% of text if API unavailable
- Purpose: Reduce token costs, improve chunk quality

**Step 3: Text Chunking**
- Library: LangChain RecursiveCharacterTextSplitter
- Chunk Size: 500 characters
- Overlap: 50 characters (10%)
- Purpose: Create manageable, context-preserving segments

**Step 4: Embedding Generation**
- Model: all-MiniLM-L6-v2 (SentenceTransformer)
- Dimension: 384-dimensional vectors
- Process: Each chunk → embedding vector
- Output: NumPy array of embeddings

**Step 5: Vector Index Construction**
- Library: FAISS (Facebook AI Similarity Search)
- Index Type: IndexFlatL2 (exhaustive L2 distance)
- Storage: In-memory
- Purpose: Fast similarity search

#### 3.2 Query Pipeline

**Step 1: Query Embedding**
- Input: User question string
- Process: Convert to 384-dim vector using same embedder
- Output: Query vector

**Step 2: Vector Similarity Search**
- Method: L2 distance search in FAISS index
- K-value: 10 (retrieve top 10 chunks)
- Output: Indices of most similar chunks

**Step 3: Context Retrieval**
- Process: Extract text chunks by indices
- Join: Concatenate with double newlines
- Output: Assembled context string

**Step 4: LLM Prompt Construction**
```
Answer the question using ONLY the policy text below.
If the policy does not explicitly mention it, say so clearly.
You may briefly explain implied alignment, if any, without assuming facts.

POLICY TEXT:
{context}

QUESTION:
{question}
```

**Step 5: LLM Generation**
- Model: Google Gemini 2.5 Flash
- Input: Prompt with context + question
- Output: Grounded answer
- Constraints: Answer only from context, acknowledge unknowns

---

### 4. External Services

#### 4.1 ScaleDown API
- **Purpose**: Intelligent text compression
- **Endpoint**: `https://api.scaledown.xyz/compress/raw/`
- **Authentication**: Bearer token
- **Compression Ratio**: 0.25 (75% reduction)
- **Timeout**: 10 seconds
- **Fallback**: Local truncation on failure

#### 4.2 Google Gemini 2.5 Flash
- **Purpose**: Natural language generation
- **Model**: `models/gemini-2.5-flash`
- **Input**: Prompt with context and question
- **Output**: Generated text answer
- **Temperature**: Default (not customized)

#### 4.3 Sentence Transformers
- **Model**: all-MiniLM-L6-v2
- **Source**: Hugging Face
- **Download**: Automatic on first use
- **Storage**: `~/.cache/torch/sentence_transformers/`
- **Size**: ~80MB

---

### 5. Data Storage

#### 5.1 Temporary File Storage
- **Directory**: `temp_uploads/`
- **Purpose**: Store uploaded PDFs during session
- **Lifecycle**: Created on upload, persists until manual cleanup
- **Note**: No automatic cleanup (add for production)

#### 5.2 In-Memory Vector Index
- **Technology**: FAISS IndexFlatL2
- **Storage**: RAM
- **Lifecycle**: Per-session (lost on app restart)
- **Scalability**: Limited by available memory

#### 5.3 Text Chunks
- **Storage**: Python list in `PolicyRAG.text_chunks`
- **Lifecycle**: Per-document (overwritten on new upload)
- **Purpose**: Map FAISS indices back to original text

---

## Data Flow

### Indexing Flow (PDF → Searchable Index)

```
┌─────────┐    ┌─────────┐    ┌──────────┐    ┌─────────┐    ┌──────┐
│  PDF    │───▶│ PyPDF   │───▶│ScaleDown │───▶│ Text    │───▶│FAISS │
│  File   │    │ Extract │    │ Compress │    │Splitter │    │Index │
└─────────┘    └─────────┘    └──────────┘    └─────────┘    └──────┘
                   │               │               │             ▲
                   │               │               │             │
                  text         compressed        chunks      embeddings
                (10K pages)    (2.5K tokens)    (50 chunks)  (50×384)
```

### Query Flow (Question → Answer)

```
┌──────────┐    ┌─────────┐    ┌──────────┐    ┌─────────┐    ┌────────┐
│ Question │───▶│ Embed   │───▶│  FAISS   │───▶│ Retrieve│───▶│ Gemini │
│  Text    │    │ Query   │    │ Search   │    │ Context │    │  LLM   │
└──────────┘    └─────────┘    └──────────┘    └─────────┘    └────────┘
                   │               │               │               │
                   │               │               │               │
                vector         indices[0-9]     chunks[10]      answer
                (384-dim)      (top-10)         (5K chars)      (text)
```

---

## Technology Stack

### Backend
- **Python 3.8+**: Core language
- **PyPDF**: PDF parsing and text extraction
- **SentenceTransformers**: Embedding generation
- **FAISS**: Vector similarity search
- **LangChain**: Text splitting utilities
- **python-dotenv**: Environment variable management

### Frontend
- **Streamlit**: Web application framework

### APIs
- **Google Gemini 2.5 Flash**: LLM for answer generation
- **ScaleDown**: Text compression (optional)

### Libraries
| Library | Version | Purpose |
|---------|---------|---------|
| streamlit | latest | Web UI framework |
| google-genai | latest | Gemini API client |
| sentence-transformers | latest | Embedding model |
| faiss-cpu | latest | Vector search |
| pypdf | latest | PDF parsing |
| langchain-text-splitters | latest | Text chunking |
| requests | latest | HTTP client |
| python-dotenv | latest | Config management |

---

## Design Patterns

### 1. Singleton Pattern
- **Where**: `@st.cache_resource` on `init_rag()`
- **Purpose**: One RAG instance per session
- **Benefit**: Avoid reloading model multiple times

### 2. Pipeline Pattern
- **Where**: Indexing and Query flows
- **Purpose**: Sequential data transformation
- **Benefit**: Clear separation of concerns

### 3. Dependency Injection
- **Where**: `PolicyRAG` receives embedder in `__init__`
- **Purpose**: Decouple model initialization from business logic
- **Benefit**: Easier testing and model swapping

### 4. Fallback Pattern
- **Where**: ScaleDown compression
- **Purpose**: Graceful degradation on API failure
- **Benefit**: System remains functional without external service

---

## Scalability Considerations

### Current Limitations

1. **Memory-Bounded**:
   - All embeddings stored in RAM
   - Large documents (>1000 pages) may exceed memory

2. **Sequential Processing**:
   - PDF processing is single-threaded
   - Large files have linear processing time

3. **No Persistence**:
   - Index rebuilt on every session
   - No database for pre-processed documents

### Scaling Strategies

#### For Large Documents
- Use FAISS `IndexIVFFlat` with quantization
- Implement chunk-level caching
- Add document pagination (process sections)

#### For Multiple Users
- Add PostgreSQL with pgvector extension
- Implement user authentication and document isolation
- Use Redis for session management

#### For High Traffic
- Deploy on cloud (AWS, GCP, Azure)
- Use load balancer for horizontal scaling
- Implement async processing with Celery
- Add CDN for static assets

---

## Security Considerations

### Current Security Posture

1. **API Keys**:
   - ✅ Stored in `.env` file (not committed)
   - ❌ No key rotation mechanism
   - ❌ No key encryption at rest

2. **File Upload**:
   - ✅ Type validation (PDF only)
   - ❌ No file size limit
   - ❌ No malware scanning
   - ❌ No cleanup of old files

3. **Data Privacy**:
   - ✅ Local processing (except LLM call)
   - ❌ Data sent to external APIs (Gemini, ScaleDown)
   - ❌ No user consent tracking

### Recommendations for Production

1. **Add File Security**:
   ```python
   MAX_FILE_SIZE = 10 * 1024 * 1024  # 10MB
   ALLOWED_EXTENSIONS = {'.pdf'}
   ```

2. **Implement Auto-Cleanup**:
   ```python
   # Delete files older than 1 hour
   cleanup_old_files(temp_dir, max_age=3600)
   ```

3. **Add Rate Limiting**:
   ```python
   from streamlit_extras.rate_limiter import rate_limiter
   
   @rate_limiter(max_calls=10, period=60)
   def ask_question(question):
       ...
   ```

4. **Use Secret Management**:
   - Production: AWS Secrets Manager, Azure Key Vault
   - Development: `.env` with `.gitignore`

---

## Performance Metrics

### Typical Performance

| Operation | Time | Notes |
|-----------|------|-------|
| PDF Upload | <1s | Depends on file size |
| Text Extraction | 1-5s | ~1s per 100 pages |
| Compression API | 1-3s | Network latency dependent |
| Chunking | <1s | Fast for <10K chars |
| Embedding | 2-5s | ~0.1s per chunk |
| FAISS Indexing | <1s | In-memory operation |
| Query Embedding | <0.5s | Single vector |
| Vector Search | <0.1s | Sub-millisecond |
| LLM Generation | 1-3s | API latency |

**Total Time**:
- First upload: 5-15 seconds
- Subsequent queries: 2-4 seconds

### Optimization Opportunities

1. **Caching Embeddings**: Save to disk, reload on restart
2. **Async LLM Calls**: Use streaming responses
3. **Batch Processing**: Process multiple PDFs in parallel
4. **Model Quantization**: Use smaller embedding models
