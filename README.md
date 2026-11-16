# 🤖 Python AI Chat Bot

A conversational AI chat bot built entirely in **Python**, deployed and run on **Google Colab**. The bot uses the `nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free` model through the **OpenRouter** API and keeps a conversation memory so it can reply with context from earlier messages.

---

## ✨ Features

- 💬 Interactive chat directly inside a Google Colab notebook
- 🧠 Conversation memory that stores both user messages and AI responses
- 🔁 Context-aware replies based on previous turns
- 🌐 Access to the NVIDIA Nemotron model through OpenRouter
- ⚡ No local setup required
- ✅ Runs without errors

---

## 🛠️ Tech Stack

| Component    | Technology                                                  |
|--------------|-------------------------------------------------------------|
| Language     | Python 3                                                    |
| Platform     | Google Colab                                                |
| AI Provider  | [OpenRouter](https://openrouter.ai) (OpenAI-compatible API) |
| AI Model     | `nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free`        |
| Memory       | Python list-based conversation history                      |

---

## 📂 Project Structure

The project runs as a single Colab notebook:

```
📓 AI_Chat_Bot.ipynb
│
├── 🧠 Memory                      # Memory setup (conversation history list)
├── Add user's message to memory   # Saves each user input to memory
├── Add AI's response to memory    # Saves each AI reply to memory
└── Chat loop / model call         # Sends memory + new message to OpenRouter and prints the reply
```

---

## ⚙️ How It Works

1. **Model setup:** The bot is configured with the model name below:
   ```python
   model = "nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free"
   ```
2. **Memory:** A memory list stores the conversation history during the session.
3. **User input:** Each message the user types is added to memory.
4. **API request:** The full memory is sent to OpenRouter's chat completions endpoint (`https://openrouter.ai/api/v1`).
5. **AI response storage:** The model's reply is added to memory, so later messages keep their context.
6. **Output:** The reply is displayed in the notebook.

---

## 🚀 Getting Started

### 1. Get an OpenRouter API key
Sign up at [openrouter.ai](https://openrouter.ai), then create a key at [openrouter.ai/keys](https://openrouter.ai/keys).

### 2. Open the notebook in Google Colab
Upload the notebook to Google Colab, or open it from Google Drive.

### 3. Install the required packages
```python
!pip install openai
```

### 4. Add your API key securely
Store the key in Colab's **Secrets** manager (🔑 icon in the left sidebar) with the name `OPENROUTER_API_KEY`, then load it in the notebook:
```python
from google.colab import userdata
OPENROUTER_API_KEY = userdata.get("OPENROUTER_API_KEY")
```
> ⚠️ Never share or publish your API key in a public repository.

### 5. Connect to OpenRouter
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key=OPENROUTER_API_KEY,
)

model = "nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free"
```

### 6. Run all cells
Go to **Runtime → Run all**, then scroll to the chat cell and start typing.

---

## 💻 Core Code Example

```python
memory = []

def chat(user_message):
    # Add user's message to memory
    memory.append({"role": "user", "content": user_message})

    # Send the full conversation to OpenRouter
    response = client.chat.completions.create(
        model=model,
        messages=memory,
    )
    ai_reply = response.choices[0].message.content

    # Add AI's response to memory
    memory.append({"role": "assistant", "content": ai_reply})
    return ai_reply

print(chat("Hi, my name is Ravi."))
print(chat("What's my name?"))
```

---

## 💬 Example Usage

```
You: Hi, my name is Ravi.
AI:  Hello Ravi! How can I help you today?

You: What's my name?
AI:  Your name is Ravi.
```

The bot remembers earlier messages in the session, so it can answer follow-up questions using that context.

---

## 🧠 Memory System

| Step | Function                     | Purpose                                |
|------|------------------------------|----------------------------------------|
| 1    | Add user's message to memory | Stores what the user said              |
| 2    | Add AI's response to memory  | Stores what the bot replied            |

Each new request sends the stored history to the model, which allows it to respond with context.

---

## ⚠️ Limitations

- Memory is **session-based**. It resets when the Colab runtime restarts or disconnects.
- Free models on OpenRouter have rate limits and may occasionally be unavailable.
- Long conversations increase token usage, since the full history is sent on every request.
- Colab is intended for development and demos, not production hosting.

---

## 🔮 Future Improvements

- [ ] Save memory to a file or database so it persists between sessions
- [ ] Add a web interface using Gradio or Streamlit
- [ ] Add a system prompt to customize the bot's personality
- [ ] Limit memory length to control token usage
- [ ] Add error handling for rate limits and network failures

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 🙏 Acknowledgements

- [NVIDIA](https://www.nvidia.com) for the Nemotron model family
- [OpenRouter](https://openrouter.ai) for unified API access
- Google Colab for the free cloud notebook environment

---

⭐ If you found this project useful, consider giving it a star!
