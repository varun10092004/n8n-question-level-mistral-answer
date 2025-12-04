# n8n Question → Level → Mistral Answer Workflow

> An intelligent n8n workflow that analyzes questions, determines their difficulty level, and generates customized answers using Mistral AI.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Workflow Components](#workflow-components)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

This n8n workflow automates the process of:
1. **Receiving** user questions through a chat interface
2. **Analyzing** the question to determine its difficulty level (Beginner, Intermediate, Expert)
3. **Generating** contextual answers using Mistral AI
4. **Managing** conversation memory for coherent multi-turn interactions

**Use Cases:**
- Educational Q&A systems
- AI-powered tutoring platforms
- Smart customer support chatbots
- Interactive learning assistants

---

## ✨ Features

✅ **Question Analysis** - Automatically determines question difficulty  
✅ **AI-Powered Responses** - Uses Mistral Cloud Chat Model for intelligent answers  
✅ **Memory Management** - Maintains conversation context using Simple Memory  
✅ **Webhook-Based** - Easily integrable with external applications  
✅ **Scalable** - Built on n8n for easy modifications and extensions  
✅ **Docker Support** - Runs in containerized environment

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Chat Message (Webhook)                   │
│                    (Trigger)                                 │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────────────┐
│                      AI Agent                                 │
│         (Orchestrates workflow logic)                         │
└──────────────────┬─────────────────────┬──────────────────────┘
                   │                     │
         ┌─────────▼──────────┐   ┌──────▼──────────┐
         │  Mistral Chat Model│   │  Simple Memory   │
         │ (Answer Generation)│   │ (Context Storage)│
         └────────────────────┘   └──────────────────┘
                   │
                   ▼
         ┌─────────────────────┐
         │  Response Output    │
         │  (Back to Chat)     │
         └─────────────────────┘
```

---

## 📦 Prerequisites

Before you begin, ensure you have installed:

- **Docker** (v20.10 or higher)
- **Docker Compose** (v1.29 or higher)
- **Git** (for cloning repository)
- **Mistral AI API Key** ([Get it here](https://console.mistral.ai))

---

## 🚀 Installation & Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/varun10092004/n8n-question-level-mistral-answer.git
cd n8n-question-level-mistral-answer
```

### Step 2: Set Up n8n with Docker

Create a `docker-compose.yml` file:

```yaml
version: '3.8'

services:
  n8n:
    image: docker.n8n.io/n8nio/n8n
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - WEBHOOK_URL=http://localhost:5678/
    volumes:
      - n8n_data:/home/node/.n8n
    command: start

volumes:
  n8n_data:
```

### Step 3: Start n8n

```bash
docker-compose up -d
```

Access n8n at: **http://localhost:5678**

### Step 4: Configure Mistral AI Credentials

1. Open n8n dashboard
2. Go to **Credentials**
3. Add new **Mistral Cloud Chat Model** credentials
4. Enter your Mistral API Key

### Step 5: Import the Workflow

1. In n8n, click **+** (Add workflow)
2. Select **Import from File**
3. Choose `workflows.json`
4. Click **Activate** to enable the workflow

---

## 💬 Usage

### Start a Chat

1. Navigate to the workflow execution interface
2. Go to the **Chat** tab
3. Type your question (e.g., "What is machine learning?")
4. Press Send

### Example Interactions

**Beginner Question:**
- **Q:** "What is Python?"
- **Response:** Simple explanation suitable for beginners

**Intermediate Question:**
- **Q:** "How does gradient descent work in neural networks?"
- **Response:** Technical explanation with mathematical concepts

**Expert Question:**
- **Q:** "What are the recent advances in transformer architectures?"
- **Response:** In-depth technical discussion with research references

---

## 🔧 Workflow Components

### 1. **Webhook Trigger: When Chat Message Received**
- Endpoint: `POST /webhook/[webhook-id]/chat`
- Payload: `{ "message": "user question" }`
- Activates when new chat messages arrive

### 2. **AI Agent Node**
- Orchestrates the workflow logic
- Processes user input
- Manages interactions between nodes

### 3. **Mistral Cloud Chat Model**
- LLM: Mistral 7B or higher
- Generates intelligent, context-aware responses
- Adapts response complexity based on difficulty level

### 4. **Simple Memory**
- Stores conversation history
- Maintains context between messages
- Improves response coherence in multi-turn conversations

---

## ⚙️ Configuration

### Environment Variables

```bash
# .env file (if using environment-based config)
MISTRAL_API_KEY=your_api_key_here
N8N_PORT=5678
WEBHOOK_URL=http://localhost:5678/
```

### Customize Workflow

**Modify Response Complexity:**
- Edit the AI Agent prompt to adjust answer depth
- Use: `"Provide a [difficulty_level] level explanation"`

**Change Model:**
- Go to Mistral Chat Model settings
- Select different model (mistral-small, mistral-medium, mistral-large)

**Enable Logging:**
- Add a "Log" node after AI Agent
- Set to log: `$json` (full response)

---

## 🐛 Troubleshooting

### Issue: "Webhook not triggering"
**Solution:**
```bash
# Check n8n logs
docker-compose logs n8n

# Verify webhook URL
# Ensure WEBHOOK_URL matches your n8n instance
```

### Issue: "Mistral API Error"
**Solution:**
- Verify API key is correct
- Check API key permissions on Mistral dashboard
- Ensure account has sufficient credits

### Issue: "Memory not persisting"
**Solution:**
- Check Docker volume is properly mounted
```bash
docker-compose exec n8n ls -la /home/node/.n8n
```

---

## 📁 Project Structure

```
n8n-question-level-mistral-answer/
├── workflows.json          # Exported n8n workflow
├── README.md              # This file
├── docker-compose.yml     # Docker configuration
└── .env                   # Environment variables (create this)
```

---

## 🤝 Contributing

Contributions are welcome! Here's how:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -m 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open a Pull Request

---

## 📚 Resources

- [n8n Documentation](https://docs.n8n.io/)
- [Mistral AI Docs](https://docs.mistral.ai/)
- [n8n Community Forum](https://community.n8n.io/)
- [Docker Docs](https://docs.docker.com/)

---

## 📝 License

This project is licensed under the MIT License - see LICENSE file for details.

---

## 👨‍💻 Author

**Varun Nagam**
- GitHub: [@varun10092004](https://github.com/varun10092004)
- Email: [your-email@example.com]

---

## 💡 Future Enhancements

- [ ] Multi-language support
- [ ] Response rating system
- [ ] Database integration for conversation history
- [ ] Advanced analytics dashboard
- [ ] Support for multiple AI models
- [ ] API rate limiting
- [ ] Authentication system

---

**Last Updated:** December 2025  
**Status:** Active & Maintained
