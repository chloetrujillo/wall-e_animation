# Abandoned BikeBot: WALL·E-style AI Video Generator

Hi, this is my project where I’ll be building a stylized animation pipeline to generate a short film about a WALL·E-like robot navigating abandoned bikes on the Stanford campus. 

Here’s the pipeline I plan to use:

1. Fine-tune Stable Diffusion using LoRA on my own dataset of Stanford scenes and bikes
2. Use the fine-tuned model to generate image frames based on prompts
3. Use AnimateDiff to animate those frames into short clips
4. Stitch everything into a video using ffmpeg or moviepy

Here's the directory structure:

```bash
abandoned-bikebot/
├── models/
│   ├── lora_weights/
│   └── base_sd_model/
├── data/
│   ├── bikes/
│   └── stanford_scenes/
├── outputs/
│   ├── frames/
│   └── final_video.mp4
├── training/
│   └── train_lora.py
├── generation/
│   ├── generate_frames.py
│   └── animate_diff_infer.py
├── utils/
│   └── video_tools.py
├── requirements.txt
└── README.md
```

Here's what I need to do:

1. Prepare my dataset
   - Stanford campus shots
   - Photos or renders of bikes and abandoned bikes

2. Fine-tune the LoRA model
```bash
python training/train_lora.py \
  --pretrained_model runwayml/stable-diffusion-v1-5 \
  --train_data_dir data/ \
  --output_dir models/lora_weights/
```

3. Generate image frames with prompts
```bash
python generation/generate_frames.py \
  --prompt "WALL-E robot navigating abandoned bikes on Stanford campus" \
  --output_dir outputs/frames/
```

4. Animate the frames
```bash
python generation/animate_diff_infer.py \
  --model_dir models/ \
  --frames_dir outputs/frames/
```

5. Stitch frames into a video
```bash
python utils/video_tools.py \
  --frames_dir outputs/frames/ \
  --out_path outputs/final_video.mp4
```

Other stuff:
- I might use CLIP + SAM to help clean or label my dataset
- I could add sound using moviepy

Dependencies (in `requirements.txt`):
```txt
torch
diffusers
transformers
accelerate
opencv-python
ffmpeg-python
gradio
```

I'll keep updating this repo as I build.
