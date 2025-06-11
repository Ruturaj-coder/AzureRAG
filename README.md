# 🔍 RAG-Based Document QA System on Azure (Terraform + Azure Functions)

This project implements a **modular Retrieval-Augmented Generation (RAG)** pipeline using **Azure resources** and **Terraform**. The system exposes two API endpoints:

- `POST /api/upload` – Upload documents (PDF, DOCX, etc.) and index them.
- `POST /api/query` – Query those documents using Azure Cognitive Search + Azure OpenAI.

---

## 🧠 Project Highlights

- ☁️ Azure Infrastructure Provisioning using **Terraform**
- 📂 Modular file and folder structure
- 📦 Azure Function deployment via **ZipDeploy**
- 🔍 Vector DB using **Azure Cognitive Search**
- 🤖 GPT-style answers using **Azure OpenAI**

---

## 📁 Project Structure

```
terraform/
│
├── main.tf               # Entry point – wires all modules
├── variables.tf          # Global Terraform variables
├── outputs.tf            # Outputs like Function URLs
│
├── modules/
│   ├── resource_group/       # Azure Resource Group
│   ├── storage/              # Blob Storage for functions and uploads
│   ├── function_upload/      # Upload API + Deployment
│   ├── function_query/       # Query API + Deployment
│   ├── cognitive_search/     # Azure Cognitive Search setup
│   └── openai/               # Azure OpenAI setup
│
└── function-code/
    ├── upload_fn/            # Python code for uploading and indexing
    └── query_fn/             # Python code for querying and answering
```

---

## 🚀 How It Works

### 1. `/api/upload` – Upload & Index
- Accepts documents via multipart/form-data
- Extracts content and generates embeddings
- Stores indexed chunks in Azure Cognitive Search

### 2. `/api/query` – Query & Answer
- Accepts user questions via JSON
- Searches relevant documents (via vector search)
- Sends context + query to Azure OpenAI
- Returns GPT-style answer

---

## 🔧 Prerequisites

- Azure CLI logged in (`az login`)
- Terraform ≥ 1.4
- Azure subscription with:
  - Azure Cognitive Search enabled
  - Azure OpenAI access (request if needed)

---

## 🛠️ Deployment

```bash
# Step 1: Initialize Terraform
cd terraform
terraform init

# Step 2: Plan your changes
terraform plan

# Step 3: Apply and provision resources
terraform apply
```

On success, the API endpoints will be shown as output variables:
- `upload_api_url`
- `query_api_url`

---

## 🔗 Example API Requests

### Upload Document
```bash
curl -X POST https://<upload-url>/api/upload \
  -F "file=@sample.pdf"
```

### Query Document
```bash
curl -X POST https://<query-url>/api/query \
  -H "Content-Type: application/json" \
  -d '{"query": "What is the purpose of this agreement?"}'
```

---

## 📌 TODOs / Enhancements

- [ ] Add authentication (API key / AAD)
- [ ] Add retry/failure logs
- [ ] Improve preprocessing for PDF/Word formats
- [ ] Add front-end to consume these APIs

---

## 📄 License

MIT © 2025 Ruturaj Amrutkar
