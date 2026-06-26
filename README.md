# CogniDispatch AI Service

The **AI Service** is a specialized microservice within the **CogniDispatch** platform. It integrates Azure Cognitive Services (Speech-to-Text and Text-to-Speech) and Azure OpenAI to perform real-time voice command processing and automatic emergency dispatch triage.

## 🚀 Technology Stack
*   **Runtime**: Node.js (v18+)
*   **Web Framework**: Express.js
*   **AI Integrations**:
    *   `openai` SDK (for Azure OpenAI model connectivity)
    *   Azure Cognitive Services REST APIs (for token generation)
*   **Security & Networking**: CORS, Helmet

---

## 📁 Repository Structure
```
├── controllers/          # Express route controllers (AI/Speech logic)
│   └── aiController.js   # Azure OpenAI and Speech handler
├── shared/               # Database adapter and sample data configuration
├── Dockerfile            # Container build specification
├── package.json          # Node dependencies
└── server.js             # Entry point
```

---

## ⚙️ Environment Variables & Config

This service communicates with Azure Cognitive endpoints. It expects the following variables:

| Variable | Description | Default |
| :--- | :--- | :--- |
| `PORT` | Listening TCP Port for the service | `5003` (can override to `5002` if configured) |
| `AZURE_OPENAI_ENDPOINT` | The HTTPS endpoint URL for Azure OpenAI instance | *None* |
| `AZURE_OPENAI_KEY_FILE` | Path to file containing Azure OpenAI access token | *None* |
| `AZURE_OPENAI_KEY` | Plaintext Azure OpenAI API Key (fallback) | *None* |
| `AZURE_OPENAI_DEPLOYMENT`| Model deployment name | `gpt-4.1-mini` |
| `AZURE_SPEECH_REGION` | Regional location of the Cognitive Speech service | `eastus` |
| `AZURE_SPEECH_KEY_FILE` | Path to file containing Speech service subscription key | *None* |
| `AZURE_SPEECH_KEY` | Plaintext Speech service subscription key (fallback) | *None* |
| `MOCK_AI` | Boolean toggle (`true`/`false`) to bypass Azure APIs and return mock static payloads | `false` |

---

## 🛣️ API Endpoints

All routes are prefixed with `/api/ai`.

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| **GET** | `/api/ai/health` | Health status check |
| **GET** | `/api/ai/speech-token` | Exchanges local credential files for a temporary Azure STS Speech token for use on the web client |
| **POST** | `/api/ai/triage` | Analyzes a voice transcription using Azure OpenAI to categorize emergency type, location, priority, and metadata |

---

## 🛠️ Local Development

### 1. Prerequisites
*   Node.js (v18+)
*   Azure OpenAI and Azure Cognitive Speech Services resources provisioned.

### 2. Startup Commands
From the service root:
```bash
# Install dependencies
npm install

# Run the development server
npm start
```
The server will start listening at `http://localhost:5003/`.

---

## 🐳 Docker Container Build

```bash
docker build -t cogniregistry.azurecr.io/cogni-ai-service:latest .
```
