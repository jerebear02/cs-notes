# CS50 AI Week — Artificial Intelligence

## Summary
Artificial intelligence broadly refers to systems that exhibit intelligent behavior, from simple game-playing algorithms to modern large language models. Generative AI creates new content — text, images, code — and includes tools like ChatGPT and deepfake generators. Prompt engineering is the practice of crafting inputs to get better outputs from AI models, including the use of system prompts to shape model behavior. Traditional AI uses decision trees and the Minimax algorithm to play games by evaluating all possible future states. Machine learning, particularly reinforcement learning, trains models through trial and error using reward signals rather than explicit rules. Deep learning uses neural networks with many layers to learn complex patterns from large datasets. Large language models like GPT are Transformer-based neural networks trained to predict the next token, with attention mechanisms allowing them to weigh context across long sequences.

---

## Notes

### Generative AI

- **Generative AI** — AI that produces new content: text, images, audio, video, code
- Examples: ChatGPT, DALL-E, Stable Diffusion, GitHub Copilot
- **Deepfakes** — AI-generated fake images/videos of real people — growing misinformation risk
- CS50's **rubber duck debugger** — AI assistant that helps debug code without giving answers directly

---

### Prompt Engineering

**System prompt** — instructions given to the model before the user speaks; defines its behavior/persona:
```
"You are a helpful teaching assistant. Never give direct answers —
only ask guiding questions."
```

**User prompt** — what the user actually types.

**Techniques:**
- **Zero-shot** — just ask: `"Translate this to French: Hello"`
- **Few-shot** — give examples before asking: show 2-3 input/output pairs, then ask
- **Chain of thought** — ask the model to reason step by step: `"Think step by step..."`

**Example using OpenAI API:**
```python
from openai import OpenAI
client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user",   "content": "What is recursion?"}
    ]
)
print(response.choices[0].message.content)
```

---

### Traditional AI & Game-Playing

**Decision tree** — maps all possible game states as a tree of choices

**Minimax algorithm** — AI for two-player zero-sum games (chess, tic-tac-toe):
- **Maximizer** — AI tries to maximize its score
- **Minimizer** — opponent tries to minimize AI's score
- AI looks ahead all possible moves, assumes optimal play from both sides
- Choose the move that leads to the best worst-case outcome

```
Pseudocode:
function minimax(state, isMaximizing):
    if game over: return score
    if isMaximizing:
        return max(minimax(child, False) for each child)
    else:
        return min(minimax(child, True) for each child)
```

**Limitation:** exponential branching — chess has ~10^120 possible games (too many to enumerate)

**Alpha-beta pruning** — optimization that cuts branches that can't affect the final decision

---

### Machine Learning & Reinforcement Learning

**Machine learning** — systems that learn from data rather than explicit rules

**Reinforcement learning** — learns through trial and error:
1. Agent takes an **action** in an environment
2. Receives a **reward** (positive) or **penalty** (negative)
3. Over time, learns which actions maximize cumulative reward

**Explore vs. exploit dilemma:**
- **Explore** — try new actions to discover better rewards
- **Exploit** — use known best action to maximize immediate reward
- **Epsilon-greedy strategy** — explore with probability ε, exploit with probability 1-ε
  - ε starts high (explore more early), decreases over time

---

### Deep Learning & Neural Networks

**Neural network** — layers of interconnected nodes (neurons):
```
Input layer → Hidden layers → Output layer
```

- Each connection has a **weight** — adjusted during training
- **Activation function** — determines if a node "fires" (e.g. ReLU, sigmoid)
- **Backpropagation** — algorithm that adjusts weights based on error
- **Training** — feed data in, compute error, adjust weights, repeat millions of times

**Deep learning** — neural networks with many hidden layers; learns hierarchical features automatically

---

### Large Language Models & Transformers

**LLM (Large Language Model)** — neural network trained to predict the next token in a sequence:
```
"The cat sat on the ___" → predicts "mat", "floor", "couch" with probabilities
```

**Transformer architecture:**
- **Attention mechanism** — allows the model to weigh how relevant each word is to every other word in the sequence
- **Embeddings** — words converted to high-dimensional vectors where similar words cluster together
- Trained on massive text datasets (internet-scale)

**Hallucinations** — LLMs confidently produce false information because they predict plausible text, not verified facts

**Token** — smallest unit the model processes (~3/4 of a word on average)

---

### Image Generation

- **Diffusion models** — start with random noise, iteratively denoise to generate an image matching a text prompt
- Examples: DALL-E, Midjourney, Stable Diffusion
- Can generate photorealistic images, art, and deepfakes from text descriptions

---

## Cue Column

| Term | Definition |
|---|---|
| **Generative AI** | AI that creates new content — text, images, code, audio |
| **Prompt engineering** | Crafting inputs to get better outputs from AI models |
| **System prompt** | Pre-conversation instructions that shape the model's behavior |
| **Minimax** | Game AI algorithm that assumes optimal play from both sides |
| **Reinforcement learning** | Learning through trial and error with reward/penalty signals |
| **Explore vs. exploit** | Tradeoff between trying new actions vs. using known best action |
| **Epsilon-greedy** | Strategy that explores with probability ε, exploits otherwise |
| **Neural network** | Layers of weighted nodes that learn patterns from data |
| **Backpropagation** | Algorithm that adjusts network weights based on prediction error |
| **Transformer** | Neural network architecture using attention — backbone of LLMs |
| **Attention mechanism** | Allows model to weigh relevance of each word to every other word |
| **Embedding** | High-dimensional vector representation of a word or token |
| **Hallucination** | LLM confidently producing false information |
| **Diffusion model** | Image generation by iteratively denoising from random noise |
