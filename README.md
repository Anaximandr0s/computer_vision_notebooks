# Computer Vision Notebooks

A set of three educational Jupyter notebooks that present the main tasks of computer vision with **live demos**,
**step-by-step explanations**, and **evaluation metrics** that anyone can follow, from a complete beginner to a professional.

Every notebook is written in simple English, runs **locally in VS Code** (no Colab or browser camera needed), and works on a normal laptop CPU.

## Notebooks

| # | Notebook | Topics | Main models | Live camera |
|---|---|---|---|---|
| 1 | [`01_object_tracking.ipynb`](01_object_tracking.ipynb) | Object detection, object tracking, model evaluation and comparison | YOLO11n and YOLO11s (Ultralytics) with ByteTrack | Yes |
| 2 | [`02_image_generation.ipynb`](02_image_generation.ipynb) | Image generation, how diffusion works, generation metrics | Stable Diffusion v1.5 (Diffusers) | No |
| 3 | [`03_ocr_captioning_retrieval.ipynb`](03_ocr_captioning_retrieval.ipynb) | OCR, image captioning, image retrieval | RapidOCR, a CNN + LSTM captioner trained from scratch, CLIP | Yes (OCR) |

## What each notebook contains

### 1. Object tracking

- **Live demo**: the webcam finds objects, gives each one an **ID**, draws its **trajectory**, and shows the class, confidence, frame number, FPS, and speed.
- **Explanations**: detection versus tracking, how ByteTrack matches objects from frame to frame.
- **Evaluation**
  - Which metrics can be shown live (FPS, latency, confidence) and which cannot (precision, recall, mAP need ground truth).
  - Precision, recall, IoU, and mAP explained with a security guard story.
  - **Comparison of YOLO11n (nano) and YOLO11s (small)**: speed measured on the local machine, model size, and official COCO score, with charts.
  - Optional real accuracy test on the `coco128` dataset.
  - **Live side-by-side demo** of both models on the same camera frame.

### 2. Image generation

- **Stable Diffusion step by step**, with a picture at every stage:
  1. the prompt turned into tokens and numbers (embeddings)
  2. the starting canvas of pure random noise
  3. the denoising loop, with the decoded image captured at several steps and its noise level
  4. the "reverse movie": how training destroys a picture with noise
- **Controls**: guidance scale, number of steps, and seed, each tested on its own.
- **Metrics**
  - Time per image and per step.
  - **CLIP score** (does the picture match the prompt?), with an "unrelated prompt" test.
  - **Diversity** across seeds.
  - **FID** (realism) explained but not computed, because it needs thousands of images.

### 3. OCR, image captioning, and image retrieval

| Part | Content | Metrics |
|---|---|---|
| **OCR** | Detection, recognition (CNN + LSTM + CTC), live reading on the webcam, optional live error rate against a known text | CER, WER, confidence, latency, robustness test (blur, noise, low contrast, rotation, tiny text) |
| **Image captioning** | **CNN encoder + LSTM decoder** trained from scratch on a small synthetic world, `<start>` and `<end>` tokens, the probability of every next word at each step, the LSTM memory, teacher forcing, greedy versus sampling, optional BLIP bonus on a real photo | BLEU-1 to 4, accuracy per attribute (color, shape, size, position), fully correct captions |
| **Image retrieval** | CLIP embeddings of 1,000 CIFAR-10 pictures, a map of the embedding space, text-to-image and image-to-image search, optional webcam query | Precision@K, Recall@K, AP, mAP, comparison with random and raw-pixel baselines, zero-shot accuracy, confusion matrix |

## Metrics at a glance

| Task | Key metrics | Plain meaning |
|---|---|---|
| Tracking and detection | FPS, latency, confidence (live). Precision, recall, IoU, mAP (with ground truth) | How fast is it, and how often is it right? |
| Image generation | Time, CLIP score, diversity, FID | Is it fast, does it match the prompt, is it varied, does it look real? |
| OCR | CER, WER | What share of the characters or words is wrong? |
| Captioning | BLEU, attribute accuracy | How close is the sentence to what a human would write? |
| Retrieval | Precision@K, Recall@K, mAP | Are the right pictures at the top of the list? |

## Getting started

### Requirements

- Python 3.10 or newer
- VS Code with the **Python** and **Jupyter** extensions
- A webcam (notebooks 1 and 3)
- Internet access for the first run, to download models and datasets. After that, everything works offline.

### Installation

```bash
# 1. Clone the repository and enter it
git clone <repository-url>
cd <repository-folder>

# 2. Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate              # Windows PowerShell: .venv\Scripts\Activate.ps1

# 3. Install PyTorch
pip install torch torchvision          # CPU only (smaller download): add  --index-url https://download.pytorch.org/whl/cpu

# 4. Install the other dependencies (all three notebooks)
pip install ipykernel ultralytics opencv-python diffusers transformers accelerate safetensors \
            matplotlib pillow numpy rapidocr-onnxruntime nltk
```

Each notebook also starts with a `%pip install` cell, so a single notebook can be run on its own.

### Run

1. Open a notebook in VS Code.
2. Click **Select Kernel**, then **Python Environments**, then **.venv**.
3. Run the cells from top to bottom. The settings of each notebook are grouped in clearly marked cells and can be changed freely.

### Camera permission

| System | Where to allow the camera |
|---|---|
| Windows | Settings > Privacy & security > Camera > allow desktop apps |
| macOS | System Settings > Privacy & Security > Camera > enable Visual Studio Code, then restart it |
| Linux | Usually works directly |

If the camera does not open: close Zoom, Teams, or Meet, then try `CAMERA_INDEX = 1`.

## Downloads and hardware

| Item | Size | Notebook |
|---|---|---|
| YOLO11n and YOLO11s weights | about 6 MB and 19 MB | 1 |
| Stable Diffusion v1.5 | roughly 4 to 6 GB | 2 |
| CLIP (ViT-B/32) | about 0.6 GB | 2 and 3 |
| CIFAR-10 | about 170 MB | 3 |
| `coco128` (optional) | about 7 MB | 1 |
| BLIP (optional) | about 1 GB | 3 |
| OCR models | included in the `rapidocr-onnxruntime` package | 3 |

| Notebook | Expected speed on a laptop CPU |
|---|---|
| 1. Object tracking | Smooth enough for a live demo with the nano model |
| 2. Image generation | Around 1 to 4 minutes per 512 x 512 image. A GPU gives a few seconds. The multi-image experiments are skipped automatically on a CPU |
| 3. OCR, captioning, retrieval | OCR about 0.2 to 0.6 s per frame. Training the captioner about 1 to 3 minutes (done once). Indexing 1,000 pictures with CLIP about 1 minute |

## Suggested presentation flow

1. **Object tracking**: live demo, then the nano versus small comparison.
2. **Image generation**: the denoising figure, then the controls and the metrics.
3. **OCR**: live reading of a printed page or a phone screen.
4. **Image captioning**: the word-by-word prediction figure.
5. **Image retrieval**: a free-text search, then the metrics against the baselines.

To avoid waiting during a talk, run every notebook once beforehand so that models and datasets are already downloaded.

## Repository layout

```
.
├── 01_object_tracking.ipynb
├── 02_image_generation.ipynb
├── 03_ocr_captioning_retrieval.ipynb
└── README.md
```

Files created when the notebooks run: `yolo11n.pt`, `yolo11s.pt`, `captioner.pt`, `generated_image.png`, and the folders `data/` and `runs/`.
A `.gitignore` is recommended:

```
.venv/
data/
runs/
*.pt
generated_image.png
.ipynb_checkpoints/
```

## Troubleshooting

| Problem | Solution |
|---|---|
| "Cannot open the camera" | Check the camera permission, close other apps that use the camera, try `CAMERA_INDEX = 1` |
| Video window does not appear (macOS) | Set `DISPLAY_MODE = "notebook"` |
| Low FPS | Use the nano model, lower `IMAGE_SIZE`, improve the room light |
| Stable Diffusion is too slow or out of memory | Use `IMAGE_SIZE = 384` and `STEPS = 15`, or the smaller model `segmind/tiny-sd` |
| Black image from Stable Diffusion | The built-in safety checker flagged it. Change the prompt or the seed |
| `rapidocr-onnxruntime` does not install | Run `pip install easyocr` and set `OCR_ENGINE = "easyocr"` |
| The captioner makes many errors | Raise `EPOCHS` (for example 50) and set `FORCE_RETRAIN = True` |
| Library version errors | Update with `pip install -U ultralytics diffusers transformers` |

## Responsible use

- Generated images can reflect the **biases** of their training data, and realistic pictures can mislead. AI-generated images should be labelled as such.
- **OCR** can read private information (ID cards, license plates, documents). Reading someone else's documents needs permission.
- Tracking and retrieval of people can be used for **surveillance**. Use them with consent and a clear purpose.

## Models, datasets, and licenses

The notebooks download third-party models and datasets, which keep their own licenses. Check them before reusing the code or the models outside of teaching:

- **YOLO11** (Ultralytics): AGPL-3.0, with a separate commercial license available
- **Stable Diffusion v1.5**: CreativeML Open RAIL-M
- **CLIP** (OpenAI): MIT
- **COCO**, **CIFAR-10**: see the licenses of the datasets