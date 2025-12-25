# 🖼️ Image Captioning with Attention (ResNet50 + LSTM)

**Module:** IA générative et ingénierie des prompts
**Program:** Master SDIA
**Student:** **SALMA LIDAME**

## Project Overview

This project implements an **Image Captioning system** using a modern **Encoder–Decoder architecture with Attention**.
The model automatically generates natural language descriptions from images by combining **Computer Vision** and **Natural Language Processing** techniques.

Key components include:

* **ResNet50 (pretrained)** for visual feature extraction
* **Attention mechanism** to focus on relevant image regions
* **Custom LSTM with Attention** for sequence generation
* **Pretrained Word2Vec embeddings** for semantic-rich word representations

The system is trained and evaluated on the **Flickr30k dataset**.

---

##  Objectives

* Implement a complete image captioning pipeline
* Apply **transfer learning** using a frozen CNN encoder
* Integrate **visual attention** for better alignment between image regions and words
* Improve caption quality using **pretrained word embeddings**
* Analyze training behavior and generated captions

---

##  Dataset

**Flickr30k**

* 31,000+ images
* 158,915 captions (~5 per image)
* Train/Test split: **80% / 20%**

Each image is associated with multiple human-written descriptions capturing different perspectives.

---

## Model Architecture

### Encoder

* **ResNet50 pretrained on ImageNet**
* Final pooling and classification layers removed
* Output: **(2048, 7, 7)** spatial feature maps
* Weights frozen for efficiency and generalization

### Attention Mechanism

* Computes attention over **49 spatial regions**
* Projects visual features and LSTM hidden state into a shared attention space (512)
* Produces a **dynamic context vector** at each time step

### Decoder

* **Custom LSTM with integrated attention**
* Input per timestep:

  * Word embedding (300)
  * Attention context (2048)
  * Previous hidden state (512)
* Output vocabulary size: **20,273 words**

---

## Training Setup

* **Loss:** CrossEntropyLoss (ignoring `<PAD>`)
* **Optimizer:** Adam (lr = 0.001, weight decay = 1e-5)
* **Scheduler:** StepLR (lr / 2 every 10 epochs)
* **Epochs:** 50 (early stopping patience = 5)
* **Gradient clipping:** max norm = 5.0
* **Teacher forcing** during training
* **Monitoring:** TensorBoard + sample caption generation

---

## Results

* Training loss decreased from ~7.0 → **3.59**
* Test loss converged to **~3.42**
* Stable convergence without major overfitting
* Attention improved:

  * Caption-image alignment
  * Grammatical coherence
  * Interpretability via attention visualization

### Example

**Generated:**

> *"a man in a blue shirt is jumping on a skateboard"*

**Ground Truth:**

> *"A person wearing a blue helmet, a t-shirt, and blue jean shorts is outside on a skateboard"*

✔ Correct action and object
✖ Simplified details

---

##  Applications

* Assistive technologies for visually impaired users
* Image indexing and semantic search
* Social media caption generation
* Medical image reporting support
* Intelligent surveillance systems

---

## References

* PyTorch & TorchVision
* Flickr30k Dataset
* Google News Word2Vec
* TensorBoard
