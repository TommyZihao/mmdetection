# EfficientDet

> [**EfficientDet: Scalable and Efficient Object Detection**](https://arxiv.org/pdf/1911.09070.pdf),
> Mingxing Tan, Ruoming Pang, Quoc V. Le,
> *CVPR 2020*

## Abstract

This is an implementation of [EfficientDet](https://github.com/google/automl) based on [MMDetection](https://github.com/open-mmlab/mmdetection/tree/3.x), [MMCV](https://github.com/open-mmlab/mmcv), and [MMEngine](https://github.com/open-mmlab/mmengine).
<br>
EfficientDet a new family of object detectors, which consistently achieve much better efficiency than prior art across a wide
spectrum of resource constraints.
In particular, with single model and single-scale, EfficientDet-D7 achieves stateof-the-art 55.1 AP on COCO test-dev with 77M parameters and 410B FLOP.
<br>
BiFPN is a simple yet highly effective weighted bi-directional feature pyramid network, which introduces learnable weights to learn the importance of different input features, while repeatedly applying topdown and bottom-up multi-scale feature fusion.
<br>
In contrast to other feature pyramid network, such as FPN, FPN + PAN, NAS-FPN, BiFPN achieves  the best accuracy with fewer parameters and FLOPs.

<div align="center">
<img src="https://github.com/zwhus/pictures/raw/main/Screenshot%20from%202023-01-31%2010-38-51.png">
</div>

## Usage

## Official TensorFlow Model

This project also supports [official tensorflow model](https://github.com/google/automl), it uses 90 categories and yxyx box encoding in training. If you want to use the original model weight to get official results, please refer to the following steps.

### Model conversion

Firstly, download EfficientDet [weights](https://github.com/google/automl/tree/master/efficientdet) and unzip,  please use the following command

```bash
tar -xzvf {EFFICIENTDET_WEIGHT}
```

Then, install tensorflow, please use the following command

```bash
pip install tensorflow-gpu==2.6.0
```

Lastly, convert weights from tensorflow to pytorch, please use the following command

```bash
python projects/EfficientDet/convert_tf_to_pt.py --backbone {BACKBONE_NAME} --tensorflow_weight {TENSORFLOW_WEIGHT_PATH} --out_weight {OUT_PATH}
```

### Testing commands

In MMDetection's root directory, run the following command to test the model:

```bash
python tools/test.py projects/EfficientDet/configs/tensorflow/efficientdet_effb0_bifpn_8xb16-crop512-300e_coco_tf.py ${CHECKPOINT_PATH}
```

## Reproduce Model

For convenience, we recommend the current implementation version, it uses 80 categories and xyxy encoding in training. On this basis, a higher result was finally achieved.

### Training commands

In MMDetection's root directory, run the following command to train the model:

```bash
python tools/train.py projects/EfficientDet/configs/efficientdet_effb3_bifpn_8xb16-crop896-300e_coco.py
```

### Testing commands

In MMDetection's root directory, run the following command to test the model:

```bash
python tools/test.py projects/EfficientDet/configs/efficientdet_effb3_bifpn_8xb16-crop896-300e_coco.py ${CHECKPOINT_PATH}
```

## Results

Based on mmdetection, this project aligns the accuracy of the [official model](https://github.com/google/automl).

|                                                        Method                                                        |    Backbone     | Pretrained Model |  Training set  |   Test set   | Epoch | Val Box AP | Official AP |                                                                                                                                                                                                Download                                                                                                                                                                                                |
| :------------------------------------------------------------------------------------------------------------------: | :-------------: | :--------------: | :------------: | :----------: | :---: | :--------: | :---------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| [efficientdet-d0\*](projects/EfficientDet/configs/tensorflow/efficientdet_effb0_bifpn_8xb16-crop512-300e_coco_tf.py) | efficientnet-b0 |     ImageNet     | COCO2017 Train | COCO2017 Val |  300  |    34.4    |    34.3     |                                                                                                                                                                                                                                                                                                                                                                                                        |
|         [efficientdet-d3](projects/EfficientDet/configs/efficientdet_effb3_bifpn_8xb16-crop896-300e_coco.py)         | efficientnet-b3 |     ImageNet     | COCO2017 Train | COCO2017 Val |  300  |    47.2    |    46.8     | [model](https://download.openmmlab.com/mmdetection/v3.0/efficientdet/efficientdet_effb3_bifpn_8xb16-crop896-300e_coco/efficientdet_effb3_bifpn_8xb16-crop896-300e_coco_20230223_122457-e6f7a833.pth) \| [log](https://download.openmmlab.com/mmdetection/v3.0/efficientdet/efficientdet_effb3_bifpn_8xb16-crop896-300e_coco/efficientdet_effb3_bifpn_8xb16-crop896-300e_coco_20230223_122457.log.json) |

**Note**:
\*means use [official tensorflow model](https://github.com/google/automl) weights to test.

## Citation

```BibTeX
@inproceedings{tan2020efficientdet,
  title={Efficientdet: Scalable and efficient object detection},
  author={Tan, Mingxing and Pang, Ruoming and Le, Quoc V},
  booktitle={Proceedings of the IEEE/CVF conference on computer vision and pattern recognition},
  pages={10781--10790},
  year={2020}
}
```

## Checklist

<!-- Here is a checklist illustrating a usual development workflow of a successful project, and also serves as an overview of this project's progress. The PIC (person in charge) or contributors of this project should check all the items that they believe have been finished, which will further be verified by codebase maintainers via a PR.
OpenMMLab's maintainer will review the code to ensure the project's quality. Reaching the first milestone means that this project suffices the minimum requirement of being merged into 'projects/'. But this project is only eligible to become a part of the core package upon attaining the last milestone.
Note that keeping this section up-to-date is crucial not only for this project's developers but the entire community, since there might be some other contributors joining this project and deciding their starting point from this list. It also helps maintainers accurately estimate time and effort on further code polishing, if needed.
A project does not necessarily have to be finished in a single PR, but it's essential for the project to at least reach the first milestone in its very first PR. -->

- [x] Milestone 1: PR-ready, and acceptable to be one of the `projects/`.

  - [x] Finish the code

    <!-- The code's design shall follow existing interfaces and convention. For example, each model component should be registered into `mmdet.registry.MODELS` and configurable via a config file. -->

  - [x] Basic docstrings & proper citation

    <!-- Each major object should contain a docstring, describing its functionality and arguments. If you have adapted the code from other open-source projects, don't forget to cite the source project in docstring and make sure your behavior is not against its license. Typically, we do not accept any code snippet under GPL license. [A Short Guide to Open Source Licenses](https://medium.com/nationwide-technology/a-short-guide-to-open-source-licenses-cf5b1c329edd) -->

  - [x] Test-time correctness

    <!-- If you are reproducing the result from a paper, make sure your model's inference-time performance matches that in the original paper. The weights usually could be obtained by simply renaming the keys in the official pre-trained weights. This test could be skipped though, if you are able to prove the training-time correctness and check the second milestone. -->

  - [x] A full README

    <!-- As this template does. -->

- [x] Milestone 2: Indicates a successful model implementation.

  - [x] Training-time correctness

    <!-- If you are reproducing the result from a paper, checking this item means that you should have trained your model from scratch based on the original paper's specification and verified that the final result matches the report within a minor error range. -->

- [ ] Milestone 3: Good to be a part of our core package!

  - [ ] Type hints and docstrings

    <!-- Ideally *all* the methods should have [type hints](https://www.pythontutorial.net/python-basics/python-type-hints/) and [docstrings](https://google.github.io/styleguide/pyguide.html#381-docstrings). [Example](https://github.com/open-mmlab/mmdetection/blob/5b0d5b40d5c6cfda906db7464ca22cbd4396728a/mmdet/datasets/transforms/transforms.py#L41-L169) -->

  - [ ] Unit tests

    <!-- Unit tests for each module are required. [Example](https://github.com/open-mmlab/mmdetection/blob/5b0d5b40d5c6cfda906db7464ca22cbd4396728a/tests/test_datasets/test_transforms/test_transforms.py#L35-L88) -->

  - [ ] Code polishing

    <!-- Refactor your code according to reviewer's comment. -->

  - [ ] Metafile.yml

    <!-- It will be parsed by MIM and Inferencer. [Example](https://github.com/open-mmlab/mmdetection/blob/3.x/configs/faster_rcnn/metafile.yml) -->

- [ ] Move your modules into the core package following the codebase's file hierarchy structure.

  <!-- In particular, you may have to refactor this README into a standard one. [Example](https://github.com/open-mmlab/mmdetection/blob/3.x/configs/faster_rcnn/README.md) -->

- [ ] Refactor your modules into the core package following the codebase's file hierarchy structure.
