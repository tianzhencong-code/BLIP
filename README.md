## This repo is DEPRECATED and no longer supported - We recommend that you do not use the content in this repo for any production or sensitive purpose

## BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation

## Announcement: BLIP is now officially integrated into [LAVIS](https://github.com/salesforce/LAVIS) - a one-stop library for language-and-vision research and applications!

<img src="BLIP.gif" width="700">

This is the PyTorch code of the <a href="https://arxiv.org/abs/2201.12086">BLIP paper</a> [[blog](https://blog.salesforceairesearch.com/blip-bootstrapping-language-image-pretraining/)]. The code has been tested on PyTorch 1.10 and 1.11. To install the dependencies, run <pre>pip install -r requirements.txt</pre> 

Catalog:
- [x] Inference demo
- [x] Pre-trained and finetuned checkpoints
- [x] Finetuning code for Image-Text Retrieval, Image Captioning, VQA, and NLVR2
- [x] Pre-training code
- [x] Zero-shot video-text retrieval
- [x] Download of bootstrapped pre-training datasets  

### Inference demo:
Run our interactive demo using [Colab notebook](https://colab.research.google.com/github/salesforce/BLIP/blob/main/demo.ipynb) (no GPU needed).
The demo includes code for: 
1. Image captioning
2. Open-ended visual question answering
3. Multimodal / unimodal feature extraction
4. Image-text matching

Try out the [Web demo](https://huggingface.co/spaces/Salesforce/BLIP), integrated into [Huggingface Spaces 🤗](https://huggingface.co/spaces) using [Gradio](https://github.com/gradio-app/gradio). 

Replicate web demo and Docker image is also available at [![Replicate](https://replicate.com/salesforce/blip/badge)](https://replicate.com/salesforce/blip)

### Pre-trained checkpoints:
Num. pre-train images | BLIP w/ ViT-B | BLIP w/ ViT-B and target resolution 384x384 | BLIP w/ ViT-L 
--- | :---: | :---: | :---: 
14M | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_14M.pth">Download</a>| - | -
129M | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base.pth">Download</a>| <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_384.pth">Download</a> | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_large.pth">Download</a>

### Finetuned checkpoints:
Task | BLIP w/ ViT-B | BLIP w/ ViT-B and target resolution 384x384 | BLIP w/ ViT-L
--- | :---: | :---: | :---:
Image-Text Retrieval (COCO) | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_retrieval_coco.pth">Download</a>| - | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_large_retrieval_coco.pth">Download</a>
Image-Text Retrieval (Flickr30k) | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_retrieval_flickr.pth">Download</a>|  - | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_large_retrieval_flickr.pth">Download</a>
Image Captioning (COCO) | - | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_caption_capfilt_large.pth">Download</a>| <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_large_caption.pth">Download</a> | 
VQA | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_vqa_capfilt_large.pth">Download</a>| - | -
NLVR2 | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_nlvr.pth">Download</a>| - | -


### Image-Text Retrieval:
1. Download COCO and Flickr30k datasets from the original websites, and set 'image_root' in configs/retrieval_{dataset}.yaml accordingly.
2. To evaluate the finetuned BLIP model on COCO, run:
<pre>python -m torch.distributed.run --nproc_per_node=8 train_retrieval.py \
--config ./configs/retrieval_coco.yaml \
--output_dir output/retrieval_coco \
--evaluate</pre> 
3. To finetune the pre-trained checkpoint using 8 A100 GPUs, first set 'pretrained' in configs/retrieval_coco.yaml as "https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base.pth". Then run:
<pre>python -m torch.distributed.run --nproc_per_node=8 train_retrieval.py \
--config ./configs/retrieval_coco.yaml \
--output_dir output/retrieval_coco </pre> 

### Image Captioning:
1. Download COCO dataset from the original website, and set 'image_root' in configs/caption_coco.yaml accordingly.
2. To evaluate the finetuned BLIP model on COCO, run:
<pre>python -m torch.distributed.run --nproc_per_node=8 train_caption.py --evaluate \
--config ./configs/caption_coco.yaml \
--output_dir output/caption_coco</pre> 
3. To finetune the pre-trained checkpoint using 8 A100 GPUs, first set 'pretrained' in configs/caption_coco.yaml as "https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base.pth". Then run:
<pre>python -m torch.distributed.run --nproc_per_node=8 train_caption.py \
--config ./configs/caption_coco.yaml \
--output_dir output/caption_coco</pre> 

### VQA:
1. Download VQA v2 dataset and Visual Genome dataset from the original websites, and set 'vqa_root' and 'vg_root' in configs/vqa.yaml.
2. To evaluate the finetuned BLIP model on VQA, generate results with: (evaluation needs to be performed on official server)
<pre>python -m torch.distributed.run --nproc_per_node=8 train_vqa.py --evaluate \
--config ./configs/vqa.yaml \
--output_dir output/vqa</pre> 
3. To finetune the pre-trained checkpoint using 16 A100 GPUs, first set 'pretrained' in configs/vqa.yaml as "https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base.pth". Then run:
<pre>python -m torch.distributed.run --nproc_per_node=16 train_vqa.py \
--config ./configs/vqa.yaml \
--output_dir output/vqa</pre> 

### NLVR2:
1. Download NLVR2 dataset from the original websites, and set 'image_root' in configs/nlvr.yaml.
2. To evaluate the finetuned BLIP model on NLVR2, run:
<pre>python -m torch.distributed.run --nproc_per_node=8 train_nlvr.py --evaluate \
--config ./configs/nlvr.yaml \
--output_dir output/nlvr</pre> 
3. To finetune the pre-trained checkpoint using 16 A100 GPUs, first set 'pretrained' in configs/nlvr.yaml as "https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base.pth". Then run:
<pre>python -m torch.distributed.run --nproc_per_node=16 train_nlvr.py \
--config ./configs/nlvr.yaml \
--output_dir output/nlvr</pre>

In order to finetune a model with ViT-L, simply change the config file to set 'vit' as 'large', and set 'pretrained' to be the path of the large pre-trained checkpoint. Set 'image_size' to be 384, and set the learning rate / batch size to be smaller than the ones for ViT-B.

### Pre-training:
1. Prepare training json files where each json file contains a list. Each item in the list is a dictonary with two key-value pairs: {'url': url_of_image, 'caption': text_of_image}. 
2. In configs/pretrain.yaml, set 'train_file' as the paths for the json files .
3. Pre-train the model using 8 A100 GPUs:
<pre>python -m torch.distributed.run --nproc_per_node=8 pretrain.py --config ./configs/Pretrain.yaml --output_dir output/Pretrain </pre> 

### Zero-shot video-text retrieval:
1. Download MSRVTT dataset following the instructions from https://github.com/ArrowLuo/CLIP4Clip, and set 'video_root' in configs/retrieval_msrvtt.yaml accordingly.
2. Install [decord](https://github.com/dmlc/decord) with <pre>pip install decord</pre> 
3. To perform zero-shot evaluation, run
<pre>python -m torch.distributed.run --nproc_per_node=8 eval_retrieval_video.py \
--config ./configs/retrieval_msrvtt.yaml \
--output_dir output/retrieval_msrvtt \
--evaluate</pre> 

### Pre-training datasets download:
We provide bootstrapped pre-training datasets as json files. Each json file contains a list. Each item in the list is a dictonary with two key-value pairs: {'url': url_of_image, 'caption': text_of_image}. 

Image source | Filtered web caption | Filtered synthetic caption by ViT-B | Filtered synthetic caption by ViT-L
--- | :---: | :---: | :---:
CC3M+CC12M+SBU |  <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/datasets/ccs_filtered.json">Download</a>|  <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/datasets/ccs_synthetic_filtered.json">Download</a>|  <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/datasets/ccs_synthetic_filtered_large.json">Download</a>
LAION115M | <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/datasets/laion_filtered.json">Download</a>|  <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/datasets/laion_synthetic_filtered.json">Download</a>|  <a href="https://storage.googleapis.com/sfr-vision-language-research/BLIP/datasets/laion_synthetic_filtered_large.json">Download</a>

### Citation
If you find this code to be useful for your research, please consider citing.
<pre>
@inproceedings{li2022blip,
      title={BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation}, 
      author={Junnan Li and Dongxu Li and Caiming Xiong and Steven Hoi},
      year={2022},
      publisher = {ICML},
}</pre>

### Acknowledgement
The implementation of BLIP relies on resources from <a href="https://github.com/salesforce/ALBEF">ALBEF</a>, <a href="https://github.com/huggingface/transformers">Huggingface Transformers</a>, and <a href="https://github.com/rwightman/pytorch-image-models/tree/master/timm">timm</a>. We thank the original authors for their open-sourcing.

Related project: [LAVIS](https://github.com/salesforce/LAVIS)
