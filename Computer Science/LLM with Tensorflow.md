#AI #computer_science #LLM #tensorflow #pytorch #data_science

[[LLM - Enabeling GPUs]]
[[LLM - Creating Vocabulary]]
[[LLM - Batchs]]
[[LLM Architectures]]
[[Trainning an LLM]]
[[Generating Text with a LLM]]

If you want to use **TensorFlow** instead of PyTorch to build a language model (LLM-style), you absolutely can. Below is a **step-by-step TensorFlow-based version** of the toy GPT-style LLM implementation on Google Colab.

---

### 🧠 Building a GPT-like Language Model in TensorFlow (Google Colab)

---

## 📦 1. Install and Import Dependencies

In your Colab notebook:

```python
!pip install tensorflow datasets --quiet
```

Then import:

```python
import tensorflow as tf
from tensorflow.keras import layers
import numpy as np
import matplotlib.pyplot as plt
from datasets import load_dataset
```

Enable GPU:

```python
print("Num GPUs Available: ", len(tf.config.list_physical_devices('GPU')))
```

---

## 📚 2. Load and Prepare Dataset (Tiny Shakespeare)

```python
dataset = load_dataset("tiny_shakespeare")
text = dataset["train"][0]["text"]
```

Create vocabulary:

```python
chars = sorted(list(set(text)))
vocab_size = len(chars)
char2idx = {ch: i for i, ch in enumerate(chars)}
idx2char = {i: ch for i, ch in enumerate(chars)}

def encode(s): return [char2idx[c] for c in s]
def decode(l): return ''.join([idx2char[i] for i in l])

encoded_text = np.array(encode(text), dtype=np.int32)
```

Split:

```python
split_idx = int(0.9 * len(encoded_text))
train_data = encoded_text[:split_idx]
val_data = encoded_text[split_idx:]
```

---

## 📐 3. Create Dataset Batches

```python
block_size = 64
batch_size = 32

def make_dataset(data):
    sequences = tf.data.Dataset.from_tensor_slices(data)
    sequences = sequences.window(block_size + 1, shift=1, drop_remainder=True)
    sequences = sequences.flat_map(lambda window: window.batch(block_size + 1))
    sequences = sequences.map(lambda seq: (seq[:-1], seq[1:]))
    return sequences.shuffle(10000).batch(batch_size, drop_remainder=True)

train_dataset = make_dataset(train_data)
val_dataset = make_dataset(val_data)
```

---

## 🧠 4. Define a Simple GPT-Style Model (Minimal Version)

```python
class MiniGPT(tf.keras.Model):
    def __init__(self, vocab_size, embedding_dim=128):
        super().__init__()
        self.embedding = layers.Embedding(vocab_size, embedding_dim)
        self.dense = layers.Dense(vocab_size)

    def call(self, x):
        x = self.embedding(x)  # [batch, seq, embedding]
        return self.dense(x)   # [batch, seq, vocab_size]
```

Instantiate:

```python
model = MiniGPT(vocab_size)
loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
optimizer = tf.keras.optimizers.Adam()
```

---

## 🏋️ 5. Train the Model

```python
@tf.function
def train_step(x, y):
    with tf.GradientTape() as tape:
        logits = model(x)
        loss = loss_fn(y, logits)
    grads = tape.gradient(loss, model.trainable_variables)
    optimizer.apply_gradients(zip(grads, model.trainable_variables))
    return loss

EPOCHS = 3
for epoch in range(EPOCHS):
    print(f"\nEpoch {epoch+1}")
    for step, (x_batch, y_batch) in enumerate(train_dataset):
        loss = train_step(x_batch, y_batch)
        if step % 100 == 0:
            print(f"Step {step}: Loss {loss.numpy():.4f}")
```

---

## ✍️ 6. Generate Text

```python
def generate_text(start_string, num_chars=100):
    input_eval = tf.expand_dims(encode(start_string), 0)
    generated = []

    for _ in range(num_chars):
        predictions = model(input_eval)
        predictions = predictions[:, -1, :]
        predicted_id = tf.random.categorical(predictions, num_samples=1)[-1, 0].numpy()

        input_eval = tf.concat([input_eval, [[predicted_id]]], axis=1)
        generated.append(predicted_id)

    return start_string + decode(generated)

print(generate_text("To be, or not to be", 200))
```

---

## ⚙️ 7. Scaling Up: Optional Improvements

- Use `TransformerBlock`s with **multi-head attention**, **LayerNorm**, etc.
    
- Add **Positional Embeddings**.
    
- Use `tf.keras.Model` subclasses for modular Transformer architectures.
    
- Load Hugging Face’s pretrained models via `transformers` if needed.
    

---

## ✅ Summary: TensorFlow Flowchart

|Step|Description|
|---|---|
|✅ Dataset|Load and encode Tiny Shakespeare|
|✅ Tokenisation|Char-level index mapping|
|✅ Batching|TensorFlow dataset windowing|
|✅ Model|Minimal GPT-style model with `Embedding` + `Dense`|
|✅ Training|Manual training loop with gradient descent|
|✅ Generation|Categorical sampling for text generation|

---

Would you like me to generate this into a downloadable `.ipynb` notebook for Colab with:

- GPU memory checks
    
- Positional embeddings
    
- Transformer attention blocks?
    

Or do you want a production-style version like GPT-2 with Hugging Face + TensorFlow?