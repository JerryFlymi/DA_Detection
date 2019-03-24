# A Pytorch Implementation of Strong-Weak Distribution Alignment for Adaptive Object Detection

<img src='./docs/swda.png' width=900/>

## Introduction
Follow [original repository](https://github.com/jwyang/faster-rcnn.pytorch).
 to setup the environment. When installing pytorch-faster-rcnn, you may encounter some issues.
Many issues have been reported there to setup the environment.

### Data Preparation

* **PASCAL_VOC 07+12**: Please follow the instructions in [py-faster-rcnn](https://github.com/rbgirshick/py-faster-rcnn#beyond-the-demo-installation-for-training-and-testing-models) to prepare VOC datasets. Actually, you can refer to any others. After downloading the data, creat softlinks in the folder data/.
* **Clipart, WaterColor**: Dataset preparation instruction link [Cross Domain Detection ](https://github.com/naoto0804/cross-domain-detection/tree/master/datasets). Images translated by Cyclegan are available in the website.
* **Sim10k**: Website [Sim10k](https://fcav.engin.umich.edu/sim-dataset/)
* **Cityscape-Translated Sim10k**: TBA
* **CitysScape, FoggyCityscape**: Download website [Cityscape](https://www.cityscapes-dataset.com/)

All codes are written to fit for the format of PASCAL_VOC.
For example, the dataset [Sim10k](https://fcav.engin.umich.edu/sim-dataset/) is stored as follows.

```
$ cd Sim10k/VOC2012/
$ ls
Annotations  ImageSets  JPEGImages
$ cat ImageSets/Main/val.txt
3384827.jpg
3384828.jpg
3384829.jpg
.
.
.
```
If you want to test the code on your own dataset, arange the dataset
 in the format of PASCAL, make dataset class in lib/datasets/. and add
 it to  lib/datasets/factory.py, lib/datasets/config_dataset.py. Then, add the dataset option to lib/model/utils/parser_func.py.
### Data Path
Write your dataset directories' paths in lib/datasets/config_dataset.py.

### Pretrained Model

We used two pretrained models in our experiments, VGG and ResNet101. You can download these two models from:

* VGG16: [Dropbox](https://www.dropbox.com/s/s3brpk0bdq60nyb/vgg16_caffe.pth?dl=0), [VT Server](https://filebox.ece.vt.edu/~jw2yang/faster-rcnn/pretrained-base-models/vgg16_caffe.pth)

* ResNet101: [Dropbox](https://www.dropbox.com/s/iev3tkbz5wyyuz9/resnet101_caffe.pth?dl=0), [VT Server](https://filebox.ece.vt.edu/~jw2yang/faster-rcnn/pretrained-base-models/resnet101_caffe.pth)

Download them and write the path in __C.VGG_PATH and __C.RESNET_PATH at lib/model/utils/config.py.

## Train
* With only local alignment loss,
```
 CUDA_VISIBLE_DEVICES=$GPU_ID python trainval_net_local.py \
                    --dataset source_dataset --dataset_t target_dataset --net vgg16 \
                    --cuda
```
Add --lc when using context-vector based regularization loss.

* With only global alignment loss,
```
 CUDA_VISIBLE_DEVICES=$GPU_ID python trainval_net_global.py \
                    --dataset source_dataset --dataset_t target_dataset --net vgg16 \
                    --cuda
```
Add --gc when using context-vector based regularization loss.
* With global and local alignment loss,
```
 CUDA_VISIBLE_DEVICES=$GPU_ID python trainval_net_global_local.py \
                    --dataset source_dataset --dataset_t target_dataset --net vgg16 \
                    --cuda
```
Add --lc and --gc when using context-vector based regularization loss.
## Test

```
 CUDA_VISIBLE_DEVICES=$GPU_ID python test_net_global_local.py \
                    --dataset target_dataset --net vgg16 \
                    --cuda --lc --gc --load_name path_to_model
```