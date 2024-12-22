# Building a RAG Application with LangChain

## **Overview**
This project demonstrates how to build a Retrieval-Augmented Generation (RAG) application using LangChain and Hugging Face. It retrieves relevant scientific papers, processes the data into a vector database, and uses a large language model (LLM) to answer user queries.

---

## **Project Structure**
```
rag_langchain/
├── data_source/
│   └── generative_ai/
│       └── download.py      # Script to download scientific papers
├── src/
│   ├── base/
│   │   └── llm_model.py     # Model initialization (LLM)
│   ├── rag/
│   │   ├── file_loader.py   # File processing
│   │   ├── main.py          # Chain construction for RAG
│   │   ├── offline_rag.py   # Chain templates and utilities
│   │   ├── utils.py         # Helper functions
│   │   └── vectorstore.py   # Vector database initialization
│   ├── app.py               # API initialization with FastAPI
├── requirements.txt         # Required Python libraries
└── README.md                # Project documentation
```

---

## **Setup Instructions**

### **1. Clone the Repository**
```bash
git clone <repository-url>
cd rag_langchain
```

### **2. Create a Virtual Environment**
```bash
python -m venv env
source env/bin/activate  # On Windows: env\Scripts\activate
```

### **3. Install Dependencies**
```bash
pip install -r requirements.txt
```

### **4. Add Hugging Face API Key**

#### Option 1: Directly in Code
Edit `src/base/llm_model.py` to include your API key:
```python
token = "<your_huggingface_token>"
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    use_auth_token=token
)
```

#### Option 2: Using `.env` File (Recommended)
1. Install `python-dotenv`:
   ```bash
   pip install python-dotenv
   ```
2. Create a `.env` file in the project root:
   ```
   HUGGINGFACE_TOKEN=<your_huggingface_token>
   ```
3. Ensure `src/base/llm_model.py` loads the token:
   ```python
   from dotenv import load_dotenv
   import os
   load_dotenv()
   token = os.getenv("HUGGINGFACE_TOKEN")
   ```

---

## **Run the Application**

### **1. Start the API Server**
Ensure the server is running on port `5000`:
```bash
uvicorn src.app:app --host "0.0.0.0" --port 5000 --reload
```

### **2. Access API Endpoints**
#### Check API status:
```bash
curl -X GET http://localhost:5000/check
```
#### Submit a query:
```bash
curl -X POST http://localhost:5000/generative_ai \
-H "Content-Type: application/json" \
-d '{"question": "What is LangChain?"}'
```
---

result:

<img width="718" alt="image" src="https://github.com/user-attachments/assets/06b0660d-97b5-4776-b78e-7c11ab98a2d0" />


## **Web Frontend**

Use the provided `index.html` file to interact with the API:

```html
<!DOCTYPE html>
<html>
<head>
  <title>LangChain Q&A</title>
</head>
<body>
  <h1>Ask a Question</h1>
  <textarea id="question" placeholder="Enter your question"></textarea>
  <button onclick="sendQuestion()">Submit</button>
  <div id="response"></div>
  <script>
    async function sendQuestion() {
      const question = document.getElementById("question").value;
      const response = await fetch("http://localhost:5000/generative_ai", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ question }),
      });
      const data = await response.json();
      document.getElementById("response").innerText = data.answer;
    }
  </script>
</body>
</html>
```

---

## **Features**
- **Document Processing**: Automatically downloads and splits PDF files for RAG.
- **Vector Database**: Uses FAISS or Chroma for efficient retrieval.
- **LLM Integration**: Utilizes Hugging Face models for text generation.
- **FastAPI Server**: Provides API endpoints for easy integration.

---

## **Acknowledgments**
- [Hugging Face Transformers](https://huggingface.co/transformers/)
- [LangChain Framework](https://langchain.com/)
