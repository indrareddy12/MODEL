# Step-by-Step Guide: Training Flux LoRA on Lightning AI

This guide walks you through setting up, configuring, and training your Flux LoRA for Hana Kim on Lightning AI.

---

## Phase 1: Preparing Your Files
On your local laptop:
1. Locate the workspace folder: `C:\Users\indra\Desktop\MODEL`.
2. Select the `character_id` folder, right-click, and choose **Compress to ZIP file** (name it `character_id.zip`).

---

## Phase 2: Setting Up the Lightning AI Studio
1. Open your browser and go to [lightning.ai](https://lightning.ai).
2. Sign in using Google, GitHub, or Email.
3. Click on the **Create Studio** button on the dashboard.
4. Select the **Blank PyTorch Studio** template.
5. Wait for the Studio container to load. Once it loads:
   * Look at the top center of the screen where it lists **CPU**.
   * Click it to change your hardware instance.
   * Select **L4 GPU** (24GB VRAM, highly recommended and cheap) or **A10G GPU**.

---

## Phase 3: Uploading the Files
1. Look at the left sidebar for the **File Explorer** icon.
2. Drag and drop the following files from your laptop (`C:\Users\indra\Desktop\MODEL`) directly into the File Explorer sidebar:
   * `character_id.zip`
   * `train_lora_lightning_ai.ipynb`
3. Click the **Terminal** tab at the bottom of the screen (or open a new terminal window).
4. Run the following command to unzip your dataset:
   ```bash
   unzip character_id.zip
   ```
5. Verify that you see a folder named `character_id` in your File Explorer sidebar.

---

## Phase 4: Training Methods

### Option A: Using the Jupyter Notebook (Recommended)
1. Double-click the **`train_lora_lightning_ai.ipynb`** file in the sidebar file explorer.
2. Run the cells sequentially:
   * **Step 1 (Install Dependencies)**: Clones `ai-toolkit` and installs dependencies.
   * **Step 2 (Initialize Dataset Folders)**: Creates directories if needed.
   * **Step 3 (Configuration)**: Writes the `flux_training_config.yaml` parameters.
   * **Step 4 (Run Training)**: Starts the LoRA training loop.
3. Keep the browser tab open while training executes.

---

### Option B: Using the Terminal Commands directly
If you prefer running commands directly in the Terminal:

1. **Clone the Trainer Repository**:
   ```bash
   git clone --recursive https://github.com/ostris/ai-toolkit.git /teamspace/studios/this_studio/ai-toolkit
   cd /teamspace/studios/this_studio/ai-toolkit
   ```

2. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   pip install -U torch torchvision --index-url https://download.pytorch.org/whl/cu121
   ```

3. **Write the Configuration File**:
   Create a file inside `/teamspace/studios/this_studio/ai-toolkit/config/flux_training_config.yaml` and paste the following configuration:
   ```yaml
   job: extension
   config:
     name: "ea_f_001_hana_kim_lora"
     process:
       - type: 'sd_trainer'
         training_folder: "/teamspace/studios/this_studio/output"
         performance_log_every: 250
         device: cuda
         trigger_word: "ea_f_001_hana_kim"
         network:
           type: "lora"
           linear: 16
           linear_alpha: 16
         save:
           dtype: float16
           save_every: 500
           max_step_saves_to_keep: 4
         datasets:
           - folder_path: "/teamspace/studios/this_studio/character_id/train"
             caption_ext: "txt"
             resolution: [512, 768, 1024]
         train:
           epochs: 45
           steps: 2500
           gradient_accumulation_steps: 1
           learning_rate: 1e-4
           batch_size: 1
           gradient_checkpointing: true
           noise_scheduler: "flowmatch"
           optimizer: "adamw8bit"
           ema_decay: 0.99
           dtype: bf16
         model:
           name_or_path: "black-forest-labs/FLUX.1-dev"
           is_flux: true
           quantize: true
         sample:
           sampler: "flowmatch"
           sample_every: 250
           width: 1024
           height: 1024
           prompts:
             - "ea_f_001_hana_kim, Hana Kim, 28-year-old East Asian Korean woman, wearing elegant red halterneck dress, outdoor garden background, high fashion editorial, 85mm portrait"
             - "ea_f_001_hana_kim, Hana Kim, 28-year-old East Asian Korean woman, wearing cream sleeveless knit dress, luxury studio background, direct gaze, soft lighting"
   ```

4. **Launch Training**:
   Run this command in the terminal to start training:
   ```bash
   python run.py config/flux_training_config.yaml
   ```

---

## Phase 5: Downloading the Model
1. Once training reaches step 2500, look in the file sidebar on the left.
2. Navigate to: `/teamspace/studios/this_studio/output/ea_f_001_hana_kim_lora/`.
3. Locate **`ea_f_001_hana_kim_lora.safetensors`**.
4. Right-click the file and select **Download**.
5. Move this downloaded LoRA file into your ComfyUI `models/loras/` folder.
