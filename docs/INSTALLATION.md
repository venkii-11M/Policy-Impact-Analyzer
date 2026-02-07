# Installation Guide

## System Requirements

### Operating System
- Windows 10/11
- macOS 10.15+
- Linux (Ubuntu 20.04+, Debian, Fedora)

### Software Prerequisites
- **Python**: 3.8 or higher (3.10 recommended)
- **pip**: Latest version (usually bundled with Python)
- **Git**: For cloning the repository (optional)

### Hardware Recommendations
- **RAM**: Minimum 4GB, 8GB+ recommended
- **Storage**: 500MB free space (for dependencies and models)
- **Processor**: Any modern CPU (multi-core recommended for faster processing)

### Internet Connection
Required for:
- Installing dependencies
- Downloading embedding model (~80MB)
- API calls to Google Gemini
- (Optional) ScaleDown API calls

---

## Step 1: Install Python

### Windows

1. Download Python from [python.org](https://www.python.org/downloads/)
2. Run the installer
3. ✅ **Check "Add Python to PATH"** during installation
4. Verify installation:
   ```powershell
   python --version
   pip --version
   ```

### macOS

**Option 1: Official Installer**
```bash
# Download from python.org
# Run the .pkg installer
python3 --version
```

**Option 2: Homebrew**
```bash
brew install python@3.10
python3 --version
```

### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install python3.10 python3-pip python3-venv
python3 --version
```

---

## Step 2: Get the Project

### Option A: Clone with Git

```bash
git clone <your-repository-url>
cd policy-navigator
```

### Option B: Download ZIP

1. Download the project ZIP from your repository
2. Extract to desired location
3. Open terminal/command prompt in the extracted folder

---

## Step 3: Create Virtual Environment (Recommended)

A virtual environment isolates project dependencies from your system Python.

### Windows (PowerShell)

```powershell
# Create virtual environment
python -m venv venv

# Activate it
.\venv\Scripts\Activate.ps1

# If you get an error, run this first:
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### macOS/Linux

```bash
# Create virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate
```

**Note**: You'll see `(venv)` prefix in your terminal when activated.

---

## Step 4: Install Dependencies

With the virtual environment activated:

```bash
pip install -r requirements.txt
```

This will install:
- `streamlit` - Web application framework
- `google-genai` - Google Gemini API client
- `sentence-transformers` - Embedding model
- `faiss-cpu` - Vector search library
- `pypdf` - PDF text extraction
- `langchain-text-splitters` - Text chunking
- `requests` - HTTP client
- `python-dotenv` - Environment variable management

**Installation time**: 2-5 minutes depending on internet speed

### Troubleshooting Installation Issues

#### Issue: pip is not recognized
**Solution**: Reinstall Python with "Add to PATH" checked, or use:
```bash
python -m pip install -r requirements.txt
```

#### Issue: FAISS installation fails on Windows
**Solution**: Try installing via conda:
```bash
conda install -c conda-forge faiss-cpu
```

#### Issue: Permission denied on Linux/macOS
**Solution**: Use virtual environment or install with `--user`:
```bash
pip install --user -r requirements.txt
```

---

## Step 5: Configure Environment Variables

### Create .env File

1. In the project root (`policy-navigator/`), create a file named `.env`

2. Add your API keys:

```env
# Required: Google Gemini API Key
GEMINI_API_KEY=your_actual_api_key_here

# Optional: ScaleDown API Key (for text compression)
SCALEDOWN_API_KEY=your_scaledown_key_here
```

### Get Google Gemini API Key

1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Sign in with your Google account
3. Click **"Create API Key"**
4. Copy the key and paste it in `.env` file

**Note**: The key looks like: `AIzaSyB...` (39 characters)

### Get ScaleDown API Key (Optional)

1. Visit [ScaleDown.xyz](https://scaledown.xyz)
2. Sign up for an account
3. Navigate to API settings
4. Copy your API key to `.env`

**Without ScaleDown**: The app will use fallback compression (simple truncation)

---

## Step 6: Verify Installation

### Download Embedding Model

The first run will download the `all-MiniLM-L6-v2` model (~80MB).

Test the download:

```bash
python -c "from sentence_transformers import SentenceTransformer; SentenceTransformer('all-MiniLM-L6-v2')"
```

**Expected output**: Progress bar showing download, then completion message.

### Test Import

Verify all dependencies:

```bash
python -c "import streamlit, faiss, pypdf, google.genai; print('✅ All imports successful')"
```

---

## Step 7: Run the Application

### Start Streamlit Server

```bash
streamlit run app.py
```

**Expected output**:
```
  You can now view your Streamlit app in your browser.

  Local URL: http://localhost:8501
  Network URL: http://192.168.1.x:8501
```

### Access the Application

1. Browser should open automatically
2. If not, navigate to: `http://localhost:8501`
3. You should see: **"📘 Policy Document Navigator"**

---

## Step 8: First Test

### Upload a Test PDF

1. Click **"Browse files"** or drag-and-drop a PDF
2. Wait for processing (5-15 seconds)
3. See success message: **"Policy loaded successfully!"**

### Ask a Test Question

1. Enter a question in the text area
   - Example: "What is this document about?"
2. Click **"Get Answer"**
3. Wait 2-4 seconds
4. See the answer appear below

---

## Post-Installation Configuration

### Change Port (Optional)

If port 8501 is in use:

```bash
streamlit run app.py --server.port 8502
```

### Enable Auto-Reload (Development)

Streamlit auto-reloads on file changes by default. To disable:

```bash
streamlit run app.py --server.runOnSave false
```

### Configure Streamlit Theme

Create `.streamlit/config.toml`:

```toml
[theme]
primaryColor = "#FF4B4B"
backgroundColor = "#FFFFFF"
secondaryBackgroundColor = "#F0F2F6"
textColor = "#262730"
font = "sans serif"
```

---

## Directory Structure After Installation

```
policy-navigator/
├── venv/                      # Virtual environment (created)
├── .streamlit/                # Streamlit config (optional)
│   └── config.toml
├── temp_uploads/              # PDF storage (auto-created)
├── docs/                      # Documentation
│   ├── API_REFERENCE.md
│   ├── ARCHITECTURE.md
│   └── INSTALLATION.md
├── __pycache__/               # Python cache (auto-created)
├── .env                       # Environment variables (you create)
├── .gitignore
├── app.py                     # Main Streamlit app
├── rag_engine.py              # RAG logic
├── scaledown.py               # Compression utility
├── requirements.txt           # Dependencies
└── README.md                  # Project overview
```

---

## Updating the Project

### Update Dependencies

```bash
# Activate virtual environment first
pip install --upgrade -r requirements.txt
```

### Update Embedding Model

```bash
pip install --upgrade sentence-transformers
```

### Pull Latest Code (Git)

```bash
git pull origin main
pip install -r requirements.txt  # Install any new dependencies
```

---

## Uninstallation

### Remove Virtual Environment

```bash
# Deactivate first
deactivate

# Delete the venv folder
rm -rf venv  # Linux/macOS
rmdir /s venv  # Windows
```

### Remove Project

```bash
cd ..
rm -rf policy-navigator  # Linux/macOS
rmdir /s policy-navigator  # Windows
```

---

## Common Issues and Solutions

### Issue: "Streamlit is not recognized"

**Cause**: Virtual environment not activated or pip install location not in PATH

**Solution**:
```bash
# Activate virtual environment
source venv/bin/activate  # macOS/Linux
.\venv\Scripts\Activate.ps1  # Windows

# Or run directly
python -m streamlit run app.py
```

---

### Issue: "Invalid API key" error

**Cause**: 
- Missing `.env` file
- Incorrect API key format
- API key not activated

**Solution**:
1. Verify `.env` file exists in project root
2. Check key format: `GEMINI_API_KEY=AIza...`
3. Ensure no spaces around `=`
4. Verify key is active in Google AI Studio

---

### Issue: Model download fails

**Cause**: Network issues, firewall, or disk space

**Solution**:
```bash
# Manual download with retry
pip install --upgrade sentence-transformers
python -c "from sentence_transformers import SentenceTransformer; SentenceTransformer('all-MiniLM-L6-v2')"
```

**Alternative**: Download model files manually and place in:
- Windows: `C:\Users\<username>\.cache\torch\sentence_transformers\`
- macOS/Linux: `~/.cache/torch/sentence_transformers/`

---

### Issue: FAISS import error on Mac M1/M2

**Cause**: ARM architecture compatibility

**Solution**:
```bash
# Use conda instead
conda install -c pytorch faiss-cpu
```

---

### Issue: Port 8501 already in use

**Cause**: Another Streamlit app or process using the port

**Solution**:
```bash
# Use different port
streamlit run app.py --server.port 8502

# Or kill the process
# Windows
netstat -ano | findstr :8501
taskkill /PID <process_id> /F

# macOS/Linux
lsof -ti:8501 | xargs kill -9
```

---

### Issue: PDF processing fails

**Cause**: Corrupted PDF, password-protected, or scanned image

**Solution**:
1. Test with a simple text-based PDF
2. Ensure PDF is not encrypted
3. For scanned PDFs, use OCR tools first:
   ```bash
   pip install pdf2image pytesseract
   # Convert scanned PDF to text-searchable PDF
   ```

---

## Next Steps

- ✅ [Read the API Reference](API_REFERENCE.md)
- ✅ [Understand the Architecture](ARCHITECTURE.md)
- ✅ [Check the User Guide](USER_GUIDE.md)
- ✅ Upload your first policy document
- ✅ Experiment with different question types

---

## Getting Help

### Documentation
- [README.md](../README.md) - Project overview
- [API_REFERENCE.md](API_REFERENCE.md) - Detailed API docs
- [ARCHITECTURE.md](ARCHITECTURE.md) - System design

### Community
- Open an issue on GitHub
- Check existing issues for solutions
- Contact the project maintainer

### Useful Commands

```bash
# Check Python version
python --version

# Check installed packages
pip list

# Verify Streamlit installation
streamlit --version

# Test imports
python -c "import streamlit; print(streamlit.__version__)"

# Clear Streamlit cache
streamlit cache clear

# Run with debug logging
streamlit run app.py --logger.level=debug
```

---

## Production Deployment

For deploying to production, see:

- **Streamlit Cloud**: Free hosting for public apps
  ```bash
  # Add secrets in Streamlit Cloud dashboard
  # No .env file needed
  ```

- **Docker**: Containerized deployment
  ```dockerfile
  FROM python:3.10-slim
  WORKDIR /app
  COPY requirements.txt .
  RUN pip install -r requirements.txt
  COPY . .
  CMD ["streamlit", "run", "app.py"]
  ```

- **AWS/Azure/GCP**: Cloud platform deployment
  - Use environment variables for API keys
  - Set up auto-scaling
  - Add health checks

See [DEPLOYMENT.md](DEPLOYMENT.md) for detailed production setup.
