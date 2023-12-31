# SonicSplit: Separate Anything You Describe

<!-- This repository contains the official implementation of ["SonicSplit: Separate Anything You Describe"](https://audio-agi.github.io/Separate-Anything-You-Describe/AudioSep_arXiv.pdf). -->

We introduce SonicSplit, a foundation model for open-domain sound separation with natural language queries. SonicSplit demonstrates strong separation performance and impressive zero-shot generalization ability on numerous tasks such as audio event separation, musical instrument separation, and speech enhancement. Check the separated audio examples in the [Demo Page](https://audio-agi.github.io/Separate-Anything-You-Describe/)!

<p align="center">
  <img align="middle" width="800" src="assets/results.png"/>
</p>

<hr>

## TODO
- [x] SonicSplit training & finetuning code release.
- [x] SonicSplit base model checkpoint release.
- [ ] Evaluation benchmark release.

<hr>

## Setup
Clone the repository and set up the conda environment: 

  ```shell
  git clone  https://github.com/ojuswisinghal17/SonicSplit.git && \
  cd AudioSep && \ 
  conda env create -f environment.yml && \
  conda activate AudioSep
  ```
Download [model weights (Google Drive)](https://drive.google.com/drive/folders/1LQIQGieAjMk5i2Hss3R3WxGIsglAsl3v?usp=sharing) at `checkpoint/`.

<hr>

## Inference 

  ```python
  from pipeline import build_audiosep, inference

  device = torch.device('cuda' if torch.cuda.is available() else 'cpu')

  model = build_audiosep(
        config_yaml='config/audiosep_base.yaml', 
        checkpoint_path='checkpoint/audiosep_base_4M_steps.ckpt', 
        device=device)

  audio_file = 'path_to_audio_file'
  text = 'textual_description'
  output_file='separated_audio.wav'

  # SonicSplit processes the audio at 32 kHz sampling rate  
  inference(model, audio_file, text, output_file, device)
  ```

<hr>

## Training 

To utilize your audio-text paired dataset:

1. Format your dataset to match our JSON structure. Refer to the provided template at `datafiles/template.json`.

2. Update the `config/audiosep_base.yaml` file by listing your formatted JSON data files under `datafiles`. For example:

```yaml
data:
    datafiles:
        - 'datafiles/your_datafile_1.json'
        - 'datafiles/your_datafile_2.json'
        ...
```

Train SonicSplit from scratch:
  ```python
  python train.py --workspace workspace/SonicSplit --config_yaml config/audiosep_base.yaml --resume_checkpoint_path checkpoint/ ''
  ```

Finetune SonicSplit from a pretrained checkpoint:
  ```python
  python train.py --workspace workspace/SonicSplit --config_yaml config/audiosep_base.yaml --resume_checkpoint_path path_to_checkpoint
  ```

<hr>

## Cite this work

If you found this tool useful, please consider citing:

```bibtex
@article{liu2023separate,
  title={SonicSplit: Separate Anything You Describe},
  author={Liu, Xubo and Kong, Qiuqiang and Zhao, Yan and Liu, Haohe and Yuan, Yi and Liu, Yuzhuo and Xia, Rui and Wang, Yuxuan and Plumbley, Mark D and Wang, Wenwu},
  journal={arXiv preprint arXiv:2308.05037},
  year={2023}
}
```

```bibtex
@inproceedings{liu22w_interspeech,
  title={Separate What You Describe: Language-Queried Audio Source Separation},
  author={Liu, Xubo and Liu, Haohe and Kong, Qiuqiang and Mei, Xinhao and Zhao, Jinzheng and Huang, Qiushi and Plumbley, Mark D and Wang, Wenwu},
  year=2022,
  booktitle={Proc. Interspeech},
  pages={1801--1805},
}
```
```
