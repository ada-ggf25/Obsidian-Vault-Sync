#AI #computer_science #LLM #data_science 

Below are a couple of code examples that show how to fetch datasets for training an AI model. Depending on your framework and needs, you can use libraries such as Hugging Face's Datasets or TensorFlow Datasets to download and prepare large text corpora.

---

## Example 1: Using Hugging Face’s Datasets Library

The Hugging Face Datasets library offers a simple API for fetching many commonly used datasets, including large text corpora that can be used for pre-training language models. In the example below, we download the "wikitext-103-raw-v1" dataset. You can replace the dataset name with another dataset available on the Hugging Face Hub if desired.

```python

# Install the datasets package if you haven't already:
# pip install datasets

from datasets import load_dataset

# Load the "wikitext-103-raw-v1" dataset from Hugging Face Hub.
dataset = load_dataset("wikitext", "wikitext-103-raw-v1")

# The dataset contains different splits; let's see what they are.
print(dataset)

# Accessing a specific split (e.g., the training set):
train_dataset = dataset["train"]

# Print the first training example to verify it works
print(train_dataset[0])

```

### Key Points:

- **Caching:** The library automatically caches the downloaded data.
    
- **Flexibility:** Easily switch to different splits (e.g., "validation" or "test") or even other datasets.
    
- **Preprocessing:** Once the dataset is loaded, you can further process it (tokenize, batch, etc.) before training your model.
    

---

## Example 2: Using TensorFlow Datasets (tfds)

TensorFlow Datasets is a collection of ready-to-use datasets that can be integrated seamlessly with TensorFlow’s data pipelines. Below is an example of fetching a dataset like Wikipedia data using TensorFlow Datasets.

```python

# pip install tensorflow-datasets

import tensorflow_datasets as tfds

# Load a dataset from TensorFlow Datasets; here we use a Wikipedia dataset.
# Some Wikipedia datasets require a specific configuration or version.
dataset, info = tfds.load("wikipedia/20200501.en", split="train", with_info=True)

# Display basic information about the dataset
print(info)

# Inspect a sample example
for example in dataset.take(1):
    print(example)>)
```

### Key Points:

- **Integration with TensorFlow:** tfds works directly with `tf.data.Dataset`, making it ideal if you plan to build your model using TensorFlow.
    
- **Preprocessing Pipelines:** You can utilize TensorFlow’s built-in functions to perform text preprocessing, augmentation, and batching.
    

---

## Additional Considerations

1. **Custom Data Sources:**  
    If your training data is hosted elsewhere (e.g., on a server or stored locally), you can use Python’s built-in I/O or specialized libraries like `requests` (for remote files) to download and prepare the data.
    
2. **Data Preprocessing:**  
    After fetching the dataset, you may need to tokenize the text, remove unnecessary characters, or even convert the dataset into a format suitable for your training pipeline. Libraries like Hugging Face’s Transformers or TensorFlow Text can help with these tasks.
    
3. **Data Augmentation and Shuffling:**  
    Especially when training large models, consider shuffling and augmenting your data. Both Hugging Face’s Datasets and TensorFlow Datasets support methods to shuffle and batch your data efficiently.

There are several **high-quality public datasets** available for training LLMs, ranging from small corpora for prototyping to massive multi-terabyte datasets used by large AI labs. These datasets can be found on platforms like **Hugging Face**, **The Pile**, **Common Crawl**, and academic repositories.

Here's a structured overview:

---

## 🧠 🔍 LLM Training Datasets: Categories & Sources

### 📚 1. **General Text Corpora**

|Dataset|Description|Size|Source|
|---|---|---|---|
|**WikiText-103**|Full Wikipedia articles (no stubs)|~500 MB|[Hugging Face](https://huggingface.co/datasets/wikitext)|
|**OpenWebText**|Open-access clone of WebText (used in GPT-2)|~40 GB|[GitHub](https://skylion007.github.io/OpenWebTextCorpus/)|
|**BooksCorpus**|Fiction books (cleaned and deduplicated)|~1 GB|[Hugging Face](https://huggingface.co/datasets/bookcorpus)|
|**CC-News**|News articles from Common Crawl (2016–2019)|~76 GB|[Hugging Face](https://huggingface.co/datasets/cc_news)|

---

### 🧠 2. **Massive Multi-Domain Datasets**

|Dataset|Description|Size|Use Case|
|---|---|---|---|
|**The Pile**|22 diverse text sources: GitHub, ArXiv, PubMed, etc.|825 GB|Training GPT-style models (EleutherAI)|
|**C4 (Colossal Clean Crawled Corpus)**|Cleaned Common Crawl dataset|750 GB+|Used in T5, GPT models|
|**RedPajama**|Reproduction of LLaMA’s training data|~1.2 TB|Used by Together AI for open models|
|**Common Crawl**|Raw web scrape data|Petabytes|Needs processing + cleaning|

📦 Access:

- [The Pile (EleutherAI)](https://pile.eleuther.ai/)
    
- [C4 (TensorFlow Datasets)](https://www.tensorflow.org/datasets/catalog/c4)
    
- [Common Crawl](https://commoncrawl.org/)
    

---

### 💼 3. **Code Datasets**

|Dataset|Description|Size|Use|
|---|---|---|---|
|**The Stack**|Source code from GitHub (15+ languages)|6.4 TB|Train LLMs on code (e.g. CodeGen)|
|**CodeParrot**|Python code from GitHub (cleaned)|20 GB|Hugging Face pretraining|
|**CodeSearchNet**|Code+doc pairs (6 languages)|2 GB|Code retrieval and synthesis|

📦 Access:

- [The Stack](https://huggingface.co/datasets/bigcode/the-stack)
    
- [CodeParrot](https://huggingface.co/datasets/codeparrot/github-code)
    

---

### 🧾 4. **Instruction/Dialogue Datasets**

|Dataset|Description|Size|Use|
|---|---|---|---|
|**OpenAssistant Conversations**|Multi-turn human/AI chats|~50k prompts|Instruction tuning|
|**ShareGPT**|User prompts + GPT outputs|90k+|Fine-tune chatbots|
|**Alpaca**|52k instruction-following examples|Small|Good for tuning lightweight LLMs|
|**Self-Instruct**|Instructions generated by GPT-3|~82k|Align LLMs with human-like behaviour|

📦 Access:

- [OpenAssistant Dataset](https://huggingface.co/datasets/OpenAssistant/oasst1)
    
- [ShareGPT](https://huggingface.co/datasets/teknium/ShareGPT_Vicuna_unfiltered)
    
- [Alpaca + Self-Instruct](https://github.com/tatsu-lab/alpaca)
    

---

### 🌍 5. **Multilingual Datasets**

|Dataset|Description|Langs|Use|
|---|---|---|---|
|**mC4**|Multilingual version of C4|101|Large-scale translation models|
|**CC100**|Cleaned Common Crawl by language|100+|Used by Facebook for XLM-R|
|**WikiMatrix**|Parallel Wikipedia sentences|85|Translation pairs|

📦 Access:

- [mC4](https://huggingface.co/datasets/mc4)
    
- [CC100](https://data.statmt.org/cc-100/)
    
- [WikiMatrix](https://huggingface.co/datasets/wikimatrix)
    

---

## 📍 Where to Find These Datasets

### 🔗 Hugging Face Datasets Hub

👉 [https://huggingface.co/datasets](https://huggingface.co/datasets)

- Search by topic, size, language, or task.
    
- Install with:
    
    ```python
    from datasets import load_dataset
    ds = load_dataset("wikitext", "wikitext-103-v1")
    ```
    

### 🔗 GitHub + Academic Releases

- Many large datasets (e.g., OpenWebText, Alpaca) are distributed via GitHub links or academic pages.
    

### 🔗 Common Crawl

- [https://commoncrawl.org](https://commoncrawl.org)  
    → Huge but raw — requires deduplication and text extraction.
    

---

## ⚠️ Notes on Usage

|Point|Detail|
|---|---|
|📏 Size|Datasets range from MBs to TBs. Use Colab for small ones.|
|⚖️ Licensing|Some datasets (e.g. BookCorpus) are in legal grey zones — always check!|
|🧹 Preprocessing|Most raw datasets require tokenisation, cleaning, deduplication, filtering profanity, etc.|

---

Would you like:

- A Colab notebook for loading and previewing one of these?
    
- A table comparing which datasets were used in GPT-2, GPT-3, LLaMA, and Mistral?
    
- Help designing your own cleaned mini corpus for training?