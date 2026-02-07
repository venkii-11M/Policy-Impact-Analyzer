# Changelog

All notable changes to Policy Document Navigator will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Planned
- Add support for multiple document formats (DOCX, TXT)
- Implement conversation history
- Add export functionality for Q&A sessions
- Create Docker container setup
- Add batch processing for multiple PDFs
- Implement caching for processed documents

---

## [1.0.0] - 2026-02-07

### Added
- Initial release of Policy Document Navigator
- PDF upload and processing functionality
- RAG-based question answering system
- Integration with Google Gemini 2.5 Flash
- FAISS vector search for semantic retrieval
- Text compression using ScaleDown API
- Streamlit-based web interface
- Support for natural language queries
- Automatic text chunking with RecursiveCharacterTextSplitter
- Sentence Transformers embeddings (all-MiniLM-L6-v2)
- Environment variable configuration via .env file
- Temporary file storage for uploaded PDFs

### Documentation
- Comprehensive README.md
- API Reference documentation
- System Architecture documentation
- Installation Guide
- User Guide
- Contributing Guidelines
- This Changelog

### Features
- **PDF Processing**: Extract text from policy documents
- **Smart Compression**: Reduce document size by 75% for cost efficiency
- **Semantic Search**: Find relevant information using AI embeddings
- **Accurate Answers**: Generate grounded responses from policy context
- **User-Friendly UI**: Simple drag-and-drop interface
- **Fast Retrieval**: FAISS-powered vector search

### Technical Details
- Python 3.8+ support
- Streamlit for web interface
- PyPDF for PDF text extraction
- FAISS for vector similarity search
- Sentence Transformers for embeddings
- Google Gemini API integration
- LangChain text splitters
- Optional ScaleDown API compression
- Fallback compression when API unavailable

---

## [0.2.0] - 2026-01-XX (Pre-release)

### Added
- Basic RAG engine implementation
- FAISS integration for vector search
- Google Gemini API integration
- Streamlit UI prototype

### Changed
- Improved chunking strategy for better context retrieval
- Optimized embedding generation

### Fixed
- PDF text extraction for multi-page documents
- Memory issues with large documents

---

## [0.1.0] - 2025-12-XX (Alpha)

### Added
- Initial proof of concept
- Basic PDF text extraction
- Simple question answering with Gemini
- Command-line interface

---

## Version History

| Version | Date | Description |
|---------|------|-------------|
| 1.0.0 | 2026-02-07 | First stable release |
| 0.2.0 | 2026-01-XX | Pre-release with RAG |
| 0.1.0 | 2025-12-XX | Alpha version |

---

## Future Roadmap

### Version 1.1.0 (Q2 2026)
- [ ] Add support for DOCX files
- [ ] Implement conversation history
- [ ] Add export to PDF/Markdown
- [ ] Improve error handling
- [ ] Add logging system

### Version 1.2.0 (Q3 2026)
- [ ] Multi-document support
- [ ] Comparison mode (compare two policies)
- [ ] Custom embedding models
- [ ] Advanced search filters
- [ ] User authentication

### Version 2.0.0 (Q4 2026)
- [ ] Database integration for persistence
- [ ] Multi-user support
- [ ] Admin dashboard
- [ ] Analytics and usage tracking
- [ ] API endpoints for integration
- [ ] Mobile-responsive UI

---

## Breaking Changes

### Version 1.0.0
- None (initial stable release)

---

## Deprecations

### Version 1.0.0
- None

---

## Security Updates

### Version 1.0.0
- Environment variables for API key management
- Local file storage for uploaded documents
- No permanent data storage

---

## Performance Improvements

### Version 1.0.0
- Optimized chunking with 500-char chunks and 50-char overlap
- Efficient FAISS IndexFlatL2 for fast L2 distance search
- Text compression reduces LLM API costs by ~75%
- Streamlit caching for RAG instance (@st.cache_resource)

---

## Known Issues

### Version 1.0.0

#### High Priority
- No automatic cleanup of `temp_uploads/` directory
- No file size limit for PDF uploads
- Single document limitation (no multi-document comparison)

#### Medium Priority
- No conversation history tracking
- Scanned PDFs not supported (text-based only)
- Limited error messages for failed uploads

#### Low Priority
- No progress indicator for large file processing
- UI could be more responsive on mobile devices

### Workarounds

**Temp File Cleanup**:
```python
# Manually delete old files
import os
import time

def cleanup_old_files(directory, max_age=3600):
    now = time.time()
    for filename in os.listdir(directory):
        filepath = os.path.join(directory, filename)
        if os.path.getmtime(filepath) < now - max_age:
            os.remove(filepath)
```

**File Size Limit**:
```python
# Add to app.py
MAX_FILE_SIZE = 10 * 1024 * 1024  # 10MB
if uploaded_file.size > MAX_FILE_SIZE:
    st.error("File too large. Maximum size: 10MB")
```

---

## Migration Guides

### Upgrading from 0.2.0 to 1.0.0

No migration needed. Fresh install recommended.

**Steps**:
1. Pull latest code
2. Update dependencies: `pip install -r requirements.txt`
3. Verify .env file has required keys
4. Run application: `streamlit run app.py`

---

## Contributors

### Version 1.0.0
- **Primary Developer**: [Your Name]
- **Documentation**: [Contributors]
- **Testing**: [Contributors]

---

## Acknowledgments

### Version 1.0.0
- Google Gemini team for the AI API
- Streamlit team for the web framework
- Facebook AI Research for FAISS
- Sentence Transformers team
- ScaleDown team for compression API
- Open-source community

---

## Links

- [Homepage](https://github.com/yourusername/policy-navigator)
- [Documentation](./docs/)
- [Issue Tracker](https://github.com/yourusername/policy-navigator/issues)
- [Release Notes](https://github.com/yourusername/policy-navigator/releases)

---

**Note**: This changelog is maintained manually. For a detailed commit history, see the git log:
```bash
git log --oneline --decorate --graph
```
