<!-- omit in toc -->
# ​Improving Deep Learning Accuracy by Data Augmentation Using Generative Adversarial Networks (GAN)

<!-- omit in toc -->
## Table of contents

- [About the project](#about-the-project)
- [Getting started](#getting-started)
  - [Installation](#installation)
  - [Required files](#required-files)
  - [Folder structure](#folder-structure)
- [Usage](#usage)
  - [Training GAN](#training-gan)
  - [Generating from GAN weights](#generating-from-gan-weights)
  - [Training classifier](#training-classifier)
  - [Testing classifier](#testing-classifier)
- [Credits](#credits)

## About the project

​Improving Deep Learning Accuracy by Data Augmentation Using Generative Adversarial Networks (GAN) is a project for the NTU module EE3080 Design & Innovation Project.

By [Tan Kim Wai](https://github.com/ktan119), [Ng Woon Ting Elizabeth](https://github.com/elizabethhng), [Melvin Kok Xinwei](https://github.com/melvinkokxw), [Lee Huei Min](#), [Teo Jing Yu](#), [Matthew Stanley](#), [Carina Low Su Yun](#) and [Tian Siming](#).

## Getting started

### Installation

1. Clone the repo

```bash
git clone https://github.com/melvinkokxw/improving-dl-accuracy-gan
cd improving-dl-accuracy-gan
```
2. Install required packages

The following packages are required:
- `torch`
- `torchvision`
- `Pillow`
- `numpy`
- `pandas`
- `pytorchcv`
- `opencv-python`
- `tqdm`
- `matplotlib`
- `imgaug`

Install via pip:

```bash
python3 -m pip install <package-name>
```

### Required files

The following files are required to run the program:

* Image
* Haar Cascade file. The Haar cascade file is required to run face detection can be obtained from [here](https://github.com/opencv/opencv/tree/master/data/haarcascades). Once download, place into the `weights` folder 
* ESRGAN weights file. 

### Folder structure

As a guideline, here is where to put the weights/data

```
.
├── README.md
├── classifier
├── models
├── data
│   ├── baseline
│   │   ├── test
│   │   ├── train
│   │   └── val
│   ├── esrgan
│   │   ├── test
│   │   ├── train
│   │   └── val
│   └── {wgan_gp, esrgan_wgan_gp, esrgan_wgan_gp_fd}
│       ├── 0
│       ├── ...
│       └── 6
└── weights
    ├── esrgan
    │   ├── README.md
    │   ├── RRDB_ESRGAN_x4.pth
    ├── {vgg19, resnet101, efficientnet-b2b}                                   <-- Classifier weights go here
    │   ├── {baseline, wgan_gp, esrgan, esrgan_wgan_gp, esrgan_wgan_gp_fd}
    │   │   ├── weights_1.pth
    │   │   ├── weights_2.pth
    │   │   └── weights_3.pth
    ├── {baseline, wgan_gp, esrgan, esrgan_wgan_gp, esrgan_wgan_gp_fd}         <-- GAN weights go here
        ├── 0
        │   └── weights.pth
        ├── ...
        └── 6
            └── weights.pth
```

## Usage

### Training GAN

1. Place image folders into `data/`, as specified in the [folder structure](#folder-structure)
   
2. Run the training file

```bash
python3 models/wgan_gp/train.py [-h] [--n_epochs N_EPOCHS] [--batch_size BATCH_SIZE] [--lr LR]
                                [--b1 B1] [--b2 B2] [--n_cpu N_CPU] [--latent_dim LATENT_DIM]
                                [--img_size IMG_SIZE] [--channels CHANNELS]
                                [--n_critic N_CRITIC] [--clip_value CLIP_VALUE]
                                [--sample_interval SAMPLE_INTERVAL]
                                [--dataset {baseline,esrgan}]
```

### Generating from GAN weights

1. Place weights into respective folders in `weights/`, as specified in the [folder structure](#folder-structure)

2. Run the relevant generator file

For ESRGAN:

```bash
python3 models/esrgan/generate.py
```

For WGAN-GP & its variants:

```bash
python3 models/wgan_gp/generate.py [-h] [--quality {baseline, esrgan}] [--face_detection]
```

### Training classifier

1. Place image folders into `data/`, as specified in the [folder structure](#folder-structure)

2. Run the classifier training file

```bash
python3 classifier/train.py [-h] [--dataset {baseline,esrgan,wgan_gp,esrgan_wgan_gp,esrgan_wgan_gp_fd}]
                            [--classifier {vgg19,resnet101,efficientnet-b2b}]
```

1. Weights will be saved into `weights/<classifier>/<dataset>`, while training graphs will be saved into `classifier/graphs`

### Testing classifier

1. Place weights into respective folders in `weights/`, as specified in the [folder structure](#folder-structure)

2. Run the classifier testing file

``` bash
python3 classifier/test.py [-h]
                           [--dataset {baseline,esrgan,wgan_gp,esrgan_wgan_gp,esrgan_wgan_gp_fd}]
                           [--classifier {vgg19,resnet101,efficientnet-b2b}]
```

3. The highest accuracy and corresponding weight file will be printed to `stdout`

## Credits

WGAN-GP model adapted from 