# Hana Kim (EA-F-001) Character Identity & DNA Foundation

This repository contains the complete dataset, configurations, and cloud training pipelines for establishing the **Hana Kim (EA-F-001)** South Korean fashion model identity using Flux, LoRA, and ComfyUI.

---

## 📂 Repository Structure

- **`character_id/`**: The core dataset package:
  - `identity_anchors/`: Close-up reference images of Hana's face angles (front, 3/4) and their captions.
  - `train/`: Pose and dress training references with highly descriptive caption text files matching the trigger-token formula.
  - `identity.yaml`: Physical DNA attributes and facial feature descriptors.
  - `config.json`: Hyperparameters configuration for the training run.
  - `master_prompt.txt` / `negative_prompt.txt`: Prompt assets.
- **`character_id.zip`**: pre-packaged dataset ready to upload to cloud GPUs.
- **`train_lora_lightning_ai.ipynb`**: Jupyter Notebook tailored for training on **Lightning AI Studios**.
- **`train_lora_runpod.ipynb`**: Jupyter Notebook tailored for training on **RunPod**.
- **`hana_comfyui_workflow.json`**: ComfyUI API configuration for inference using Flux + LoRA + IP-Adapter + ControlNet.
- **`lightning_ai_guide.md`**: Step-by-step tutorial for training on the Lightning AI platform.

---

## ⚡ Quick Start: Training on Lightning AI

1. Go to [lightning.ai](https://lightning.ai) and start a new **Studio** using a GPU (e.g. **L4** or **L40S**).
2. Upload `character_id.zip` and `train_lora_lightning_ai.ipynb` to the Studio.
3. Open a terminal in the Studio and unzip the dataset:
   ```bash
   unzip character_id.zip
   ```
4. Open the Jupyter Notebook `train_lora_lightning_ai.ipynb` and execute the cells.

For detailed steps and troubleshooting, see the [lightning_ai_guide.md](lightning_ai_guide.md) file.
