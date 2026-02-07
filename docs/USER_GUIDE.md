# User Guide

## Introduction

Welcome to **Policy Document Navigator**! This guide will help you make the most of this AI-powered tool for understanding policy documents.

## What Can You Do?

- 📄 Upload policy documents in PDF format
- 💬 Ask questions in natural language
- ✅ Get accurate answers extracted from the policy
- 🔍 Quickly find specific information without reading the entire document
- 📊 Understand complex policies through conversational interface

---

## Getting Started

### 1. Launch the Application

Open your terminal and run:

```bash
streamlit run app.py
```

The application will open in your browser at `http://localhost:8501`

### 2. Upload a Policy Document

1. **Click "Browse files"** or **drag and drop** a PDF file
2. Wait for the processing message: *"Processing policy document..."*
3. When you see *"Policy loaded successfully!"*, you're ready to ask questions

**Supported Formats**: PDF only

**File Size**: Works best with documents under 100 pages

**Processing Time**: 
- Small PDFs (1-10 pages): 2-5 seconds
- Medium PDFs (10-50 pages): 5-10 seconds
- Large PDFs (50+ pages): 10-20 seconds

---

## Asking Questions

### Question Input

1. Type your question in the text area
2. Click **"Get Answer"**
3. Wait 2-4 seconds for the AI to analyze and respond
4. Read the answer displayed below

### Best Practices for Questions

#### ✅ Good Questions

**Specific and Direct**:
- ✅ "What is the remote work policy?"
- ✅ "How many vacation days do employees get?"
- ✅ "What are the eligibility requirements for health insurance?"

**Policy-Focused**:
- ✅ "What is the procedure for requesting time off?"
- ✅ "Are there restrictions on working hours?"
- ✅ "What happens if I violate the confidentiality agreement?"

**Factual Queries**:
- ✅ "When do probation periods end?"
- ✅ "Who approves expense reports?"
- ✅ "What is the dress code policy?"

#### ❌ Questions to Avoid

**Vague or Open-Ended**:
- ❌ "Tell me everything about this policy"
- ❌ "What's interesting here?"
- ❌ "Give me a summary"

**Outside Policy Scope**:
- ❌ "What's the weather today?"
- ❌ "How do I write a resume?"
- ❌ "What are industry best practices?" (unless mentioned in the policy)

**Hypothetical or Opinion-Based**:
- ❌ "What should the company do differently?"
- ❌ "Is this policy fair?"
- ❌ "How would this compare to other companies?"

---

## Understanding Answers

### Answer Format

Answers are displayed in a formatted box below your question:

```
### ✅ Answer
[AI-generated answer based on policy content]
```

### Types of Answers

#### 1. Direct Answers

When the policy explicitly covers your question:

**Question**: "How many vacation days do employees get?"

**Answer**: "According to the policy, full-time employees receive 15 vacation days per year, accrued at a rate of 1.25 days per month."

#### 2. Implied or Inferred Answers

When the answer requires connecting policy points:

**Question**: "Can I work from home on Fridays?"

**Answer**: "The policy states that remote work is allowed up to 2 days per week with manager approval. While it doesn't specify which days, Friday could be one of those days if approved by your manager."

#### 3. Not Found Answers

When the policy doesn't address your question:

**Question**: "What is the pet policy in the office?"

**Answer**: "The uploaded policy document does not explicitly mention a pet policy or rules regarding pets in the office."

---

## Example Use Cases

### Use Case 1: Employee Handbook Navigation

**Scenario**: New employee wants to understand benefits

**Steps**:
1. Upload `employee_handbook.pdf`
2. Ask: "What health insurance options are available?"
3. Get answer with plan details
4. Follow up: "When does health insurance coverage start?"
5. Get answer about enrollment periods

### Use Case 2: Student Policy Research

**Scenario**: Student needs to understand academic integrity rules

**Steps**:
1. Upload `academic_integrity_policy.pdf`
2. Ask: "What constitutes plagiarism?"
3. Get detailed definition
4. Ask: "What are the penalties for plagiarism?"
5. Get information about consequences

### Use Case 3: Compliance Verification

**Scenario**: HR professional checking leave policies

**Steps**:
1. Upload `leave_policy.pdf`
2. Ask: "How much parental leave is provided?"
3. Get specific leave duration
4. Ask: "Is parental leave paid or unpaid?"
5. Get compensation details
6. Ask: "What documentation is required?"
7. Get process requirements

---

## Tips for Better Results

### 1. Be Specific

**Instead of**: "Tell me about benefits"

**Try**: "What are the eligibility criteria for dental benefits?"

---

### 2. Ask One Thing at a Time

**Instead of**: "What are the vacation, sick leave, and parental leave policies?"

**Try**: 
- First: "What is the vacation policy?"
- Then: "What is the sick leave policy?"
- Finally: "What is the parental leave policy?"

---

### 3. Use Policy Terminology

**If the policy uses "Remote Work"**:

**Instead of**: "Can I work from home?"

**Try**: "What is the remote work policy?"

---

### 4. Follow Up for Details

**Initial question**: "What is the expense reimbursement policy?"

**Follow-up questions**:
- "What expenses are eligible for reimbursement?"
- "What is the deadline for submitting expense reports?"
- "Who approves expense reimbursements?"

---

### 5. Rephrase if Needed

If the answer isn't helpful, try rephrasing:

**First attempt**: "What's the rule about working hours?"

**Rephrased**: "What are the standard working hours?"

**Alternative**: "Is there a policy on flexible working hours?"

---

## Common Scenarios

### Scenario: Answer Seems Incomplete

**Possible Reasons**:
- The policy doesn't cover all aspects
- Information is spread across multiple sections
- The question is too broad

**Solutions**:
- Ask more specific follow-up questions
- Try different phrasings
- Check if the policy actually contains that information

---

### Scenario: Answer Says "Not Mentioned"

**What It Means**:
The AI couldn't find relevant information in the uploaded policy.

**What To Do**:
1. Verify you uploaded the correct document
2. Try alternative keywords
3. Check if the policy is comprehensive enough
4. Consider that the topic might not be covered

---

### Scenario: Multiple Uploads

**Current Behavior**: Uploading a new PDF replaces the previous one.

**Best Practice**:
- To analyze a different policy, simply upload the new file
- The system will process it and clear the previous index
- Previous answers won't be accessible after new upload

---

## Limitations

### What the System CAN Do

✅ Extract information directly stated in the policy

✅ Connect related policy points

✅ Identify when information is not in the policy

✅ Handle policies up to ~100 pages effectively

✅ Process text-based PDFs (not scanned images)

### What the System CANNOT Do

❌ Provide legal advice or interpretation

❌ Answer questions outside the policy scope

❌ Modify or update the policy content

❌ Process scanned PDFs without OCR

❌ Compare multiple policies simultaneously

❌ Remember previous conversations after refresh

---

## Troubleshooting

### Problem: PDF Upload Fails

**Possible Causes**:
- File is not a PDF
- PDF is password-protected
- PDF is corrupted
- File is too large

**Solutions**:
1. Verify the file is a `.pdf`
2. Remove password protection
3. Try a smaller file
4. Re-download or re-save the PDF

---

### Problem: Processing Takes Too Long

**Normal Times**:
- Small PDFs: 5-10 seconds
- Medium PDFs: 10-20 seconds
- Large PDFs: 20-30 seconds

**If Longer**:
1. Check internet connection (needed for API calls)
2. Wait a bit longer (complex PDFs may take time)
3. Refresh and try again
4. Try a smaller document to test

---

### Problem: Answer Not Relevant

**Possible Causes**:
- Question is ambiguous
- Policy doesn't contain the information
- Too many unrelated chunks retrieved

**Solutions**:
1. Rephrase question more specifically
2. Use exact terminology from the policy
3. Break complex questions into simpler parts
4. Verify the uploaded document is correct

---

### Problem: Error Messages

**"Please enter a question"**:
- You clicked "Get Answer" with an empty text area
- Type a question before clicking

**"Policy loaded successfully!" doesn't appear**:
- Upload may have failed
- Try uploading again
- Check file format and size

---

## Advanced Features

### Using the System Effectively

#### For Long Policies

1. **Start with overview questions**:
   - "What are the main sections of this policy?"
   - "What topics does this policy cover?"

2. **Drill down into specifics**:
   - "What does Section 3 say about [topic]?"
   - "What are the requirements for [specific process]?"

#### For Complex Topics

1. **Break down into components**:
   - "What is [term] defined as?"
   - "What are the steps in the [process] procedure?"
   - "Who is responsible for [task]?"

2. **Verify understanding**:
   - "Are there any exceptions to [rule]?"
   - "What happens if [condition] is not met?"

---

## Privacy and Data

### What Happens to Your Data

1. **PDF Upload**:
   - Stored temporarily in `temp_uploads/` folder
   - Remains until app is restarted or manually deleted

2. **Text Processing**:
   - Text extracted and processed locally
   - Compressed text sent to ScaleDown API (if configured)
   - Context + question sent to Google Gemini API

3. **Answers**:
   - Not stored permanently
   - Lost when you refresh the page

### Best Practices for Sensitive Documents

- ✅ Run locally on your own machine
- ✅ Remove the ScaleDown API to avoid external compression
- ✅ Manually delete files from `temp_uploads/` after use
- ⚠️ Be aware that questions and context are sent to Gemini API
- ⚠️ Don't upload documents with PII (Personal Identifiable Information) unless necessary

---

## Keyboard Shortcuts

When using the web interface:

- **Tab**: Navigate between elements
- **Enter**: Submit question (when in text area)
- **Ctrl/Cmd + R**: Refresh page (clears session)
- **F11**: Toggle fullscreen

---

## Best Practices Summary

### DO ✅

- Upload text-based PDFs (not scanned images)
- Ask specific, focused questions
- Use terminology from the policy
- Follow up with clarifying questions
- Verify answers against the original policy (for critical decisions)

### DON'T ❌

- Upload sensitive documents on shared computers
- Rely solely on AI for legal decisions
- Ask questions outside the policy scope
- Upload multiple PDFs expecting combined analysis
- Assume the AI is perfect—always verify critical information

---

## Frequently Asked Questions

### Can I upload multiple PDFs at once?

No, currently only one PDF at a time. Upload a new PDF to replace the previous one.

---

### Does the system remember previous questions?

No, each question is independent. The system doesn't maintain conversation history.

---

### Can I save answers?

Not automatically. Copy and paste answers you want to keep.

---

### What if the policy is scanned?

Scanned PDFs (images) won't work well. Use OCR software to convert to text-searchable PDF first.

---

### Can I use this offline?

Partially. You need internet for:
- Initial model download (~80MB)
- API calls to Google Gemini
- (Optional) ScaleDown compression

After setup, you can use it offline if you disable API calls, but functionality will be limited.

---

### Is my data secure?

- PDFs are stored locally in `temp_uploads/`
- Questions and context are sent to Google Gemini API
- Follow your organization's data handling policies

---

### Can I customize the AI responses?

Not through the UI. Developers can modify prompts in `rag_engine.py` to adjust response style.

---

## Getting Help

### If You're Stuck

1. **Check this User Guide** for common issues
2. **Review error messages** carefully
3. **Try a test PDF** to verify the system works
4. **Check logs** in the terminal where you ran `streamlit run app.py`

### Reporting Issues

When reporting problems, include:
- What you were trying to do
- Steps to reproduce the issue
- Error messages (if any)
- PDF characteristics (size, pages, type)
- System information (OS, Python version)

---

## Next Steps

- 📖 [Read the API Reference](API_REFERENCE.md) for technical details
- 🏗️ [Understand the Architecture](ARCHITECTURE.md) for system design
- 💻 [Check Installation Guide](INSTALLATION.md) for setup help
- 🎯 Start uploading your policy documents and exploring!

---

## Feedback

Your feedback helps improve this tool! Consider:
- What features would be most helpful?
- What questions did the system struggle with?
- How can the interface be more intuitive?

Share your thoughts with the development team or open an issue on GitHub.

---

**Happy Policy Navigating! 📘✨**
