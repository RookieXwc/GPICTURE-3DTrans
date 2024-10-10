# GPICTURE-3DTrans

## :books:Outline

- [Location of Key Codes](#sparkles-location-of-key-codes)
- [3D Pre-training Results](#car-3d-pre-training-results)
- [Installation for GPICTURE-3DTrans](%EF%B8%8Finstallation-for-gpicture-3dtrans)
- [Run the Code](#rocketrun-the-code)



## :sparkles: Location of Key Codes

In [pcdet/models/dense_heads/pretrain_head_3D_seal.py](pcdet/models/dense_heads/pretrain_head_3D_seal.py), we provide the implementations of `High-level Voxel Feature Generation Module`, which involves the processes of data extraction, voxelization, as well as the computation of the target and loss.

In [pcdet/models/backbones_3d/I2Mask.py](pcdet/models/backbones_3d/I2Mask.py) and [pcdet/models/backbones_3d/dsvt_backbone_mae.py](pcdet/models/backbones_3d/dsvt_backbone_mae.py), we provide the implementations of  `Inter-class and Intra-class Discrimination-guided Masking` (I$^2$Mask).

In [pcdet/utils/cka_alhpa.py](pcdet/utils/cka_alhpa.py)and [pcdet/models/dense_heads/pretrain_head_3D_seal.py L172](pcdet/models/dense_heads/pretrain_head_3D_seal.py), we provide the implementations of `CKA-guided Hierarchical Reconstruction`.

In [pcdet/models/dense_heads/pretrain_head_3D_seal.py L167 def differential_gated_progressive_learning](pcdet/models/dense_heads/pretrain_head_3D_seal.py), we provide the implementations of `Differential-gated Progressive Learning`.

We provide all the configuration files in the paper and appendix in [tools/cfgs/gpicture_models/](tools/cfgs/gpicture_models/). 

👆 [BACK to Table of Contents -->](#booksoutline)

## :car: 3D Pre-training Results

**Pre-training**

nuScenes

| Model                                                        | Version  |                       Pre-train model                        |                             Log                              |
| ------------------------------------------------------------ | :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| [gpicture_nuscenes_ssl_seal_decoder_mask](tools/cfgs/gpicture_models/gpicture_nuscenes_ssl_seal_decoder_mask.yaml) | reported | [ckpt](https://www.dropbox.com/scl/fi/g1isvgnwexlymqwaqdo22/pretrain_dsvt_nuscenes.pth?rlkey=ubmvmtwoff3l1jp76ls0udy1j&st=5v3swgd7&dl=0) | [Log](https://www.dropbox.com/scl/fi/1hoyr8hhomb4m6bvcclaa/log_train_20240705-105152-pretrain_nuscenes.txt?rlkey=uq142mzjjgtybq5giqrpzwqaz&st=cmc2lsdu&dl=0) |
|                                                              | 3DTrans  | [ckpt](https://www.dropbox.com/scl/fi/1yg7nst0mr5jiqclj5hy6/pretrain_dsvt_nuscenes_3dtrans.pth?rlkey=j8ral0pqsjmaidn9a7eh1v7wu&st=04qyajbz&dl=0) | [Log](https://www.dropbox.com/scl/fi/wookz0tx4nwek5gfqmfon/log_train_20241002-162527-pretrain_nuscenes-_3dtrans.txt?rlkey=rh1jxzl119alf4cjofww457od&st=45ea8cwn&dl=0) |

ONCE_ad-pt_ps_small

| Model                                                        | Version |                       Pre-train model                        |                             Log                              |
| ------------------------------------------------------------ | :-----: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| [gpicture_once_ssl_seal_decoder_mask_small](tools/cfgs/gpicture_models/gpicture_once_ssl_seal_decoder_mask_small.yaml) | 3DTrans | [ckpt](https://www.dropbox.com/scl/fi/4b6wlqcwjzfd3gj537dg5/pretrain_dsvt_once_small_3dtrans.pth?rlkey=bn45av7cum8x9dp7juowrmgu3&st=unmk5lw9&dl=0) | [Log](https://www.dropbox.com/scl/fi/xircyc8v7o0rhpb0rduog/log_train_20241004-111342-pretrain_once-_3dtrans.txt?rlkey=c96srpgc645fpcxw3kow4e4ke&st=5bd719zr&dl=0) |



**Fine-tuning**

3D Object Detection (on nuScenes validation)

| Config                                                       | Pre-trained dataset | mAP  | NDS  | mATE | mASE | mAOE | mAVE | mAAE |                             ckpt                             |                             Log                              |
| ------------------------------------------------------------ | :-----------------: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| [gpicture_nuscenes_detection](tools/cfgs/gpicture_models/gpicture_nuscenes_detection.yaml) | nuScenes (reported) | 68.6 | 73.0 | 25.5 | 23.8 | 25.8 | 20.7 | 17.4 | [ckpt](https://www.dropbox.com/scl/fi/c2lvls942ytyygkl6kxnt/finetune_dsvt_nuscenes_detection.pth?rlkey=jodlym7pxgsy5qxcu4sehfkg7&st=26p2ji7f&dl=0) | [Log](https://www.dropbox.com/scl/fi/ik5vav74sm8e2gscvd58t/log_train_20240712-154208-finetune_nuscenes_detection.txt?rlkey=lisztc4w1i9r47ldt8g2le0ty&st=w0g5y8us&dl=0) |
|                                                              | nuScenes (3DTrans)  | 68.5 | 72.9 | 25.6 | 23.8 | 25.9 | 20.8 | 17.5 | [ckpt](https://www.dropbox.com/scl/fi/xha8q3gx3afupdopg51lb/finetune_dsvt_nuscenes_detection_3dtrans.pth?rlkey=eaq0usy0yfkd4l5r4dsw4sdb5&st=fc8dq7cc&dl=0) | [Log](https://www.dropbox.com/scl/fi/p0c460jwni40pc8sw21ur/log_train_20241005-161439-finetune_nuscenes_detection-_3dtrans.txt?rlkey=obf5f0h0g6y9kmbdm8oosojq8&st=rgeyyjp4&dl=0) |
|                                                              | ONCE_ad-pt_ps_small | 68.9 | 73.3 | 25.2 | 23.5 | 25.5 | 20.3 | 17.0 | [ckpt](https://www.dropbox.com/scl/fi/3r4phbgsbb820v2u55p9z/finetune_dsvt_nuscenes_detection_pretrain_once_3dtrans.pth?rlkey=0v45hguffiucxmgd39j5f8bsa&st=9ul1n9k5&dl=0) | [Log](https://www.dropbox.com/scl/fi/q21nrkjixq1lglirvv3m7/log_train_20241009-082644-finetune_nuscenes_detection-_3dtrans_once.txt?rlkey=qsxu0ppd1pti8ba4jtob416pr&st=e4o56s0q&dl=0) |

👆 [BACK to Table of Contents -->](#booksoutline)

## ⬇️Installation for GPICTURE-3DTrans

1.You may refer to [INSTALL.md](docs/INSTALL.md) for the installation of `3DTrans`.

2.To run GPICTURE, you need to install other packages.

```shell
# install other packages
pip install torch_scatter
pip install nuscenes-devkit==1.0.5
pip install open3d

# install the Python package for evaluating the Waymo dataset
pip install waymo-open-dataset-tf-2-5-0==1.4.1

# pay attention to specific package versions.
pip install pandas==1.4.3
pip install matplotlib==3.6.2
pip install scikit-image==0.19.3
pip install async-lru==1.0.3

# install CUDA extensions
cd common_ops
pip install .
```

3.To run GPICTURE, you need to install MinkowskiEngine.

```shell
# install MinkowskiEngine
pip install ninja
conda install openblas-devel -c anaconda
git clone https://github.com/NVIDIA/MinkowskiEngine.git
cd MinkowskiEngine
python setup.py install --blas_include_dirs=${CONDA_PREFIX}/include --blas=openblas
```

👆 [BACK to Table of Contents -->](#booksoutline)

## :rocket:Run the Code

Please refer to [Readme for AD-PT Pre-training](docs/GETTING_STARTED_PRETRAIN.md) for starting the journey of 3D perception pre-training using AD-PT.



We provide the configuration files in the paper and appendix in [tools/cfgs/gpicture_models/](tools/cfgs/gpicture_models/).

**Pre-training**

nuScenes

```shell
cd tools/
python train.py --cfg_file cfgs/gpicture_models/gpicture_nuscenes_ssl_seal_decoder_mask.yaml
```

ONCE_ad-pt_ps_small

```shell
cd tools/
python train_ad-pt.py --cfg_file cfgs/gpicture_models/gpicture_once_ssl_seal_decoder_mask_small.yaml
```



**Fine-tuning**

3D Object Detection on nuScenes:

```shell
cd tools/
python train.py --cfg_file cfgs/gpicture_models/gpicture_nuscenes_detection.yaml --pretrained_model /path/of/pretrain/model.pth
```

👆 [BACK to Table of Contents -->](#booksoutline)
