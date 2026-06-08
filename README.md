# 🚀 Running GPT-OSS Locally with Ollama

This guide walks you through downloading, configuring, and running the `gpt-oss` model locally using Ollama. It also includes instructions for integrating the local model with VS Code and OpenCode.

## 📋 Prerequisites

* **Hardware:** Minimum **16 GB of VRAM** (32 GB or more is highly recommended for the 20B model).
* **OS:** Linux or macOS (Windows users can use WSL2).

---

## 🛠️ Step 1: Install and Configure Ollama

**1. Install Ollama**
Open your terminal and run the official installation script:

```bash
curl -fsSL https://ollama.com/install.sh | sh

```

**2. Start the Ollama Server**
Run the following command to start the Ollama daemon. *(Note: This will lock your current terminal. Open a new terminal tab for the next steps).*

```bash
ollama serve

```

**3. Download the GPT-OSS Model**
In a new terminal window, pull the model to your local machine. This may take a few minutes depending on your internet connection.

```bash
ollama pull gpt-oss:20b

```

> **Note:** If you have exceptional hardware (e.g., multiple high-end GPUs), you can opt for the larger parameter model by running `ollama pull gpt-oss:120b`.

---

## 💻 Step 2: Connect to VS Code

1. Open the AI extension panel in VS Code.
2. Go to the **Model Selector** dropdown at the bottom.
3. Click on **"Add Models"** or **"Other Models"**.
4. From the list of providers, select **Ollama**.
5. The extension should automatically detect the `gpt-oss:20b` model running on your machine.

> 💡 **API Endpoint:** By default, the Ollama server exposes its OpenAI-compatible API locally at `http://localhost:11434/v1`.

---

## 🔌 Step 3: Connect to OpenCode

To use GPT-OSS as your engine for OpenCode, you need to route the provider to your local Ollama server.

Simply edit your `opencode.json` configuration file and replace its contents with the following:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama-local": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Local Ollama Server",
      "options": {
        "baseURL": "http://localhost:11434/v1",
        "apiKey": "ollama"
      },
      "models": {
        "gpt-oss:20b": {
          "name": "GPT-OSS 20B (Ollama)",
          "limit": {
            "context": 65536,
            "output": 65536
          }
        }
      }
    }
  },
  "model": "ollama-local/gpt-oss:20b",
  "small_model": "ollama-local/gpt-oss:20b",
  "permission": {
    "edit": "ask",
    "bash": "ask",
    "write": "ask"
  }
}

```

## Once saved, restart OpenCode, and it will immediately begin processing your commands using your locally hosted GPT-OSS model.