# 🌸 Floracle — Flower Intelligence · Setup Guide

A professional Streamlit web app for flower classification using the Oxford 102 dataset
and a fine-tuned Xception model.

---

## 📁 Final Project Structure

```
floracle/
├── app.py                  ← main Streamlit application
├── cat_to_name.json        ← class index → flower name mapping
├── oxford_epoch_60.keras   ← your trained model (you must add this)
└── requirements.txt        ← Python dependencies
```

---

## ✅ Step-by-Step Setup

### Step 1 — Clone or create the project folder

```bash
mkdir floracle && cd floracle
```

### Step 2 — Create a virtual environment

```bash
python -m venv venv

# Activate it:
# macOS / Linux:
source venv/bin/activate

# Windows:
venv\Scripts\activate
```

### Step 3 — Install dependencies

```bash
pip install -r requirements.txt
```

> ⚠️ **TensorFlow note**: If you're on an Apple Silicon Mac, use:
> ```bash
> pip install tensorflow-macos tensorflow-metal
> ```
> On Windows/Linux with CUDA GPU:
> ```bash
> pip install tensorflow[and-cuda]
> ```

### Step 4 — Add your model file

Copy your trained model to the project root:

```bash
cp /path/to/oxford_epoch_60.keras ./oxford_epoch_60.keras
```

> 🔑 The app looks for `oxford_epoch_60.keras` in the **same directory as `app.py`**.
> If it doesn't find it, the app will still run (homepage + about section) but
> will show an error when you try to classify an image.

### Step 5 — Add the category mapping file

Copy the JSON label map:

```bash
cp /path/to/cat_to_name.json ./cat_to_name.json
```

### Step 6 — Run the app

```bash
streamlit run app.py
```

Your browser will open automatically at **http://localhost:8501**

---

## 🌐 Deploy to Streamlit Community Cloud (free)

1. Push your project to a **public GitHub repository**
2. Go to [share.streamlit.io](https://share.streamlit.io) → **New app**
3. Connect your GitHub repo, set `app.py` as the main file
4. Under **Secrets / Advanced**, set any secrets if needed
5. Click **Deploy**

> ⚠️ The `.keras` model file may be large (>100MB). Use **Git LFS** or upload it
> to Google Drive / Hugging Face Hub and load it in `app.py` via URL.

### Hosting large model files on Hugging Face Hub

```python
# In app.py — replace the load_model() function with:
from huggingface_hub import hf_hub_download

@st.cache_resource
def load_model():
    import tensorflow as tf
    path = hf_hub_download(
        repo_id="your-username/floracle-xception",
        filename="oxford_epoch_60.keras"
    )
    return tf.keras.models.load_model(path)
```

Then install: `pip install huggingface_hub`

---

## 🎨 App Features

| Feature | Details |
|---|---|
| **Upload** | JPEG / PNG / WebP from local disk |
| **Camera** | Live webcam capture |
| **URL** | Paste any public image URL |
| **Prediction** | Top-5 classes with confidence bar chart |
| **Wikipedia** | Auto-fetched scientific description |
| **Metrics** | Classification report table + F1 chart |
| **Training curves** | Accuracy & loss across 60 epochs |
| **Architecture diagram** | Visual pipeline of the Xception model |

---

## 🧪 Quick test without model

The homepage, architecture overview, training graphs, and all UI sections work
without the `.keras` model. Only the **Identify** button requires it.

---

## 🔧 Troubleshooting

| Problem | Fix |
|---|---|
| `ModuleNotFoundError: tensorflow` | Run `pip install tensorflow` inside your venv |
| `ModuleNotFoundError: wikipedia` | Run `pip install wikipedia` |
| Model file not found | Ensure `oxford_epoch_60.keras` is in the same folder as `app.py` |
| Wikipedia fetch fails | The `get_wiki_summary()` function has a fallback; the URL is still shown |
| Streamlit version mismatch | Run `pip install --upgrade streamlit` |

---

## 📚 Resources

- [Oxford 102 Flowers Dataset](https://www.robots.ox.ac.uk/~vgg/data/flowers/102/)
- [Xception Paper — François Chollet](https://arxiv.org/abs/1610.02357)
- [Streamlit Documentation](https://docs.streamlit.io)
- [TensorFlow Keras](https://keras.io)
