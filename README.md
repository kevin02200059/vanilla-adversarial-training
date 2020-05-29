# vanilla-adversarial-training

This repo provides the code for both **vanilla** training and **adversarial** training **VGG/ResNet** models on CIFAR10 in PyTorch.

## File Descriptions

`train.py` & `train.sh` : training python and shell scripts

`attack.py` & `attack.sh` : attacking python and shell scripts

`attackers.py` : adversarial attack functions, including FGSM & PGD attacks

`utils.py` : help functions

`model/` : model definitions


## Results

1. **clean sample** accuracy

model       | training acc.(%) | test acc.(%)
:-:         | :-:              | :-:
VGG16_bn    | 100              | 93.18
VGG16_bn-adv| 100              | 88.77
ResNet18    | 100              | 95.42
ResNet18-adv| 100              | 91.54

2. **adversarial example** accuracy (%) (only test set)
   - For FGSM attack, we test the accuracies variation w.r.t. the step size $\epsilon$
   - For PGD attack, we test the accuracies variation w.r.t. the $l_\infty$ bound $\delta$, with fixed $step\ size\ \alpha=0.01,\ iterations=7$.

FGSM-$\epsilon$ | 0   | 0.05 | 0.1 | 0.15 | 0.2 | 0.25 | 0.3 | 0.35 | 0.4 
 :-:            |:-:  | :-:  | :-: | :-:  | :-: | :-:  | :-: | :-:  | :-:
VGG16_bn        |93.18| 15.48|12.25| 11.52|10.97|10.98 |11.18| 11.40|11.49
VGG16_bn-adv    |88.77| 47.95|35.30| 31.56|28.43|25.46 |21.91| 18.71|16.20
ResNet18        |95.42| 28.54|14.55| 11.21|10.54|10.55 |10.92| 11.27|11.40
ResNet18-adv    |91.54| 46.86|36.77| 34.11|31.77|27.85 |23.17| 19.07|16.91

PGD-$\delta$ | 0   | 1/255 | 2/255 | 3/255 | 4/255 | 5/255 | 6/255 | 7/255 | 8/255 | 9/255 | 10/255 | 11/255 | 12/255
 :-:         |:-:  | :-:   | :-:   | :-:   | :-:   | :-:   | :-:   | :-:   | :-:   | :-:   | :-:    | :-:    | :-:
VGG16_bn     |93.18| 57.44 | 19.88 | 4.40  | 1.00  | 0.23  | 0.05  | 0.06  | 0.02  | 0.03  | 0.01   | 0.01   | 0.01
VGG16_bn-adv |88.77| 83.20 | 75.27 | 66.72 | 59.23 | 53.66 | 49.90 | 46.77 | 44.23 | 42.24 | 40.86  | 39.76  | 38.95
ResNet18     |95.42| 57.66 | 19.41 | 5.37  | 1.73  | 0.63  | 0.22  | 0.12  | 0.07  | 0.03  | 0.02   | 0.05   | 0.04
ResNet18-adv |91.54| 86.94 | 80.91 | 73.56 | 65.55 | 57.59 | 50.33 | 45.03 | 40.35 | 36.94 | 34.49  | 32.49  | 31.01

## Usage

### attack

We **provide 4 trained models** in `save` folder, including vanilla and adversarial training VGG and ResNet models, named as `CIFAR10-VGG.pth`, `CIFAR10-VGG-adv.pth`, `CIFAR10-ResNet18.pth` and `CIFAR10-ResNet18-adv.pth` respectively.
Users can directly run the `attack.sh` shell script on command line to test the defence ability of different models.
The results should be similar with the values in the two tables above.
In addition, users can manually change the attack parameters in the `attacker.py` python script for more results under different settings.
```
$ sh attack.sh
```
- `model` : Please specify the target model network architecture. `vgg16` or `resnet18` are optional.
- `model_path` : Please specify the target model path. 
- `dataset` & `data_dir` : Please specify the dataset name and path.
- `gpu_id` : GPU device index.

### training

To reproduce the provided model, users can run the `train.sh` shell scripts on command line.
```
$ sh train.sh
```
- `model` : Please specify the target model network architecture. `vgg16` or `resnet18` are optional.
- `dataset` & `data_dir` : Please specify the dataset name and path.
- `model_dir` : Please specify where to save the trained model.
- `log_dir` : Please specify where to save the log files.
- `gpu_id` : GPU device index.
- `adv_train` : Please specify whether to use adversarial training. `True` or `False`.

**ATTENTION** The **mean-var normalization** preprocess is removed.

## Dependencies
- python 3.6 (miniconda)
- PyTorch 1.4.0

If u find the codes useful, welcome to fork and star this repo :)