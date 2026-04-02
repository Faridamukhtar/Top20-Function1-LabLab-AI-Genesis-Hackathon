# Holistic Hires

> **AI-Powered Technical Hiring Platform** - Revolutionizing recruitment with bias-free, comprehensive candidate evaluation
---

## **What Makes This Special?**

Traditional hiring is broken. Resumes lie, interviews are biased, and coding challenges are subjective. **Holistic Hires** fixes this with:

- **AI-Powered Code Analysis** - Gemini evaluates actual GitHub repositories, not just claims
- **AI Video Interviews** - Generates custom questions based on candidate's code, transcribes responses
- **Multi-Dimensional Scoring** - 5-factor evaluation (Code Quality, Resume Fit, Code Alignment, MCQ, Interview)
- **Bias-Free Evaluation** - Consistent AI assessment removes human prejudice
- **End-to-End Automation** - From submission to comprehensive report in minutes

---

## **Key Features**

### **For Companies**
- Post positions with custom coding challenges
- Define ideal candidate profiles
- Get comprehensive AI-generated candidate reports
- Search and rank candidates by semantic fit

### **For Candidates**
- Browse open positions
- Submit GitHub repositories (real code, not whiteboard puzzles)
- Take AI-powered video interviews with personalized questions
- Receive detailed feedback on all evaluation areas

---

## **Architecture**

```
┌─────────────────┐
│   Frontend      │  React + Tailwind
│   (Candidate)   │  - Video recording
│                 │  - MCQ interface
└────────┬────────┘  - Results dashboard
         │
         ↓
┌─────────────────┐
│   FastAPI       │  Python Backend
│   Backend       │  - Session management
│                 │  - File handling
└────────┬────────┘  - Orchestration
         │
    ┌────┴────┬─────────────┬──────────────┐
    ↓         ↓             ↓              ↓
┌───────┐ ┌──────┐ ┌─────────────┐ ┌──────────┐
│Gemini │ │Qdrant│ │GitHub API   │ │PyPDF2    │
│2.0    │ │Vector│ │Fetcher      │ │Extractor │
│Flash  │ │DB    │ │             │ │          │
└───────┘ └──────┘ └─────────────┘ └──────────┘
```

### **Evaluation Pipeline**

```
STAGE 1: Initial Assessment
├─ Fetch code from GitHub repository
├─ Gemini evaluates code quality (1-100)
├─ Generate 5 personalized interview questions
├─ Generate 3 MCQ questions
├─ Qdrant scores resume fit vs ideal profile
└─ Qdrant scores code fit vs task description

STAGE 2: Candidate Interview (Frontend)
├─ Play AI-generated audio questions (TTS)
├─ Record video responses (WebRTC)
└─ Complete MCQ assessment

STAGE 3: Final Analysis
├─ Gemini transcribes all video responses
├─ Deterministic MCQ scoring
├─ Gemini comprehensive multimodal analysis
│  ├─ Resume PDF (multimodal input)
│  ├─ GitHub code (fetched again)
│  └─ Interview transcripts
├─ Calculate weighted overall score:
│  ├─ 30% Code Quality
│  ├─ 25% Video Interview
│  ├─ 15% Resume Fit
│  ├─ 15% Code Fit
│  └─ 15% MCQ Score
└─ Generate: summary, strengths, weaknesses, recommendation
```

---

## **Tech Stack**

### **Backend**
- **FastAPI** - High-performance async API
- **Google Gemini 2.0 Flash** - Multimodal AI for code evaluation, question generation, transcription, and final analysis
- **Qdrant** - Vector database for semantic similarity scoring
- **Sentence Transformers** - Embeddings for resume/code matching
- **PyPDF2** - Resume text extraction
- **GitHub API** - Real repository code fetching

### **Frontend**
- **React 18** - Modern UI framework
- **Tailwind CSS** - Utility-first styling
- **Lucide Icons** - Beautiful iconography
- **WebRTC** - Native browser video recording
- **Fetch API** - Backend communication

---

## **Quick Start**

### **Prerequisites**
```bash
Python 3.9+
Node.js 18+
Google Gemini API Key
GitHub Personal Access Token (recommended)
Qdrant Cloud account (optional but recommended)
```

### **Backend Setup**

1. **Clone & Install**
```bash
git clone https://github.com/yourusername/holistic-hires.git
cd holistic-hires/backend
pip install -r requirements.txt
```

2. **Configure Environment**
```bash
# Create .env file
cat > .env << EOF
GENAI_API_KEY=your_gemini_api_key_here
GENAI_MODEL=gemini-2.0-flash
GITHUB_TOKEN=your_github_token_here
QDRANT_URL=your_qdrant_url_here
QDRANT_API_KEY=your_qdrant_key_here
EOF
```

3. **Run Backend**
```bash
uvicorn main:app --reload --port 8000
```

### **Frontend Setup**

1. **Install & Configure**
```bash
cd ../frontend
npm install

# Create .env file
echo "VITE_API_BASE_URL=http://localhost:8000" > .env
```

2. **Run Frontend**
```bash
npm run dev
```

3. **Open Browser**
```
http://localhost:5173
```

---

## **API Endpoints**

### **POST /evaluate/start**
Start candidate evaluation
- **Input**: Resume PDF, GitHub repo, JD, ideal profile, task
- **Output**: Interview questions, MCQs, initial scores

### **POST /evaluate/submit-responses**
Submit interview responses
- **Input**: Video files, MCQ answers
- **Output**: Complete evaluation results

### **GET /evaluate/status/{candidate_id}**
Check evaluation status

### **DELETE /evaluate/cancel/{candidate_id}**
Cancel in-progress evaluation

---

## **How It Works**

### **1. Candidate Applies**
```javascript
// Upload resume + GitHub repo link
FormData: {
  resume_file: PDF,
  repo_link: "https://github.com/user/project",
  job_description: "...",
  ideal_candidate_profile: "...",
  task_description: "Implement Top K Frequent Elements"
}
```

### **2. AI Evaluates Code**
```python
# Gemini fetches and analyzes actual code
code_content = fetch_github_code(repo_link)
result = gemini.evaluate_code(
    code_content,
    task_description,
    job_description
)
# Returns: quality score, description, questions
```

### **3. Personalized Interview**
```python
# Generated questions are specific to THEIR code
questions = [
  "I noticed you used a hash map. Why choose this over alternatives?",
  "Your solution has O(n log n) complexity. Could you optimize this?",
  "How would you handle edge case X in your implementation?"
]
```

### **4. Comprehensive Scoring**
```python
overall_score = (
    code_quality * 0.30 +
    video_interview * 0.25 +
    resume_fit * 0.15 +
    code_fit * 0.15 +
    mcq_score * 0.15
)
```

---

## **Sample Output**

```json
{
  "overall_score": 87,
  "recommendation": "Hire",
  "scores": {
    "code_quality": 85,
    "resume_fit": 78,
    "code_fit": 90,
    "mcq": 100,
    "video_interview": 82
  },
  "summary": "Strong candidate with excellent problem-solving skills...",
  "strengths": [
    "Clean, well-documented code with proper error handling",
    "Deep understanding of time/space complexity trade-offs",
    "Clear communication during video interview"
  ],
  "weaknesses": [
    "Could improve unit test coverage",
    "Some opportunities for code optimization"
  ]
}
```

---

## **Security & Privacy**

- All video data processed securely
- Session data cleaned after evaluation
- No persistent storage of video files
- Resume PDFs handled in-memory only
- API keys stored in environment variables

---

## **Screenshots**

### Landing Page
Beautiful dual-portal design for companies and candidates

### Candidate Flow
1. Browse positions with search
2. View detailed job descriptions
3. Submit application with resume
4. Submit GitHub repository
5. Take AI video interview
6. Complete MCQ assessment
7. View comprehensive results

---

## **Testing**

```bash
# Backend tests
pytest tests/

# Frontend tests
npm test
```
---

## **License**

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
