# Histopathologic Cancer Detection with a Vision Transformer (ViT)

A from-scratch Vision Transformer that classifies 96x96 histopathology image patches
as containing metastatic tumor tissue or not. Built as a course project and kept here
as a portfolio artifact.

**Authors:** Ashley Bedford ([abedford37](https://github.com/abedford37)) and Mayank Gaba.

## Problem

Metastatic cancer is hard to spot manually in digital pathology scans, and catching it
early improves treatment outcomes. The goal here is a model that flags whether an image
patch contains at least one pixel of tumor tissue, as a proof of concept for assisting
manual review.

## Dataset

[Kaggle Histopathologic Cancer Detection](https://www.kaggle.com/c/histopathologic-cancer-detection).
The full set is roughly 220,000 labeled 96x96 `.tif` patches. Because training a ViT from
scratch on the full set exceeded the memory and time available, this project uses a balanced
**16,000-image subsample** (8,000 tumor, 8,000 no-tumor), split 80/20 into train and test.

The labels file ships from Kaggle as `train_labels.csv`. The notebook expects it as
`train_image_labels.csv` in the data folder, so either rename it or adjust the filename in
the first data cell.

## Approach

- **Vision Transformer, trained from scratch.** No pretrained ViT weights were used.
- **Architecture and hyperparameters are adapted from the Keras example**
  ["Image classification with a Vision Transformer"](https://keras.io/examples/vision/image_classification_with_vision_transformer/)
  by Khalid Salama (2021). That example targets CIFAR-100, and this project reuses its patch
  size, projection dimension, head count, transformer depth, and MLP head sizing, adapted to
  a binary histopathology task. Reusing a canonical reference and adapting it is the intended
  workflow; it is credited here so the reuse is explicit.
- **Optimizer:** RMSprop (learning rate 1e-4, weight decay 1e-6), chosen over Adam after
  comparison on this dataset.
- **Training:** 100 epochs, batch size 64, with data augmentation (normalization, resize,
  random flip, rotation, zoom).

## Results

On the held-out test set:

| Metric | Score |
|---|---|
| Accuracy | 83% |
| F1 (weighted) | 0.83 |
| Precision (weighted) | 0.84 |
| Recall / Sensitivity (weighted) | 0.83 |

For context from earlier iterations on this problem: a ResNet CNN reached about 57%, and a
separate Sequential deep CNN run reached about 93%. This ViT was trained from scratch on a
small subsample, so it is not expected to beat a well-tuned CNN here. The honest read is that
more images, more epochs, and a pretrained ViT backbone would all be expected to raise these
numbers.

## Limitations and honest notes

- **From scratch on a subsample.** 16K of ~220K images and a from-scratch ViT both cap
  performance. This is a working proof of concept, not a tuned clinical model.
- **`num_classes` correction.** An earlier version carried `num_classes = 100`, a leftover
  from the CIFAR-100 Keras example. It has been corrected to 2, and the trivial top-5 metric
  (meaningless for two classes) was removed. This does not materially change the reported
  accuracy, since the binary loss only ever referenced the two real classes.
- **Not a diagnostic tool.** This is educational work and is not validated for clinical use.

## Reproduce

1. Download the dataset from Kaggle and place `train_image_labels.csv` and the `.tif` images
   in a folder.
2. Point the notebook at that folder with an environment variable:
   ```
   export DATA_DIR=/path/to/train      # Windows PowerShell: $env:DATA_DIR="C:\path\to\train"
   ```
   or edit `DATA_DIR` in the first data cell.
3. Run `histopathologic_cancer_detection_vit.ipynb` top to bottom. Training runs 100 epochs at roughly 10 minutes
   per epoch on CPU, so plan for a long run or use a GPU.

## Poster

A one-page project poster is in [`assets/poster.pdf`](assets/poster.pdf).

## License

MIT (add via GitHub's license picker when creating the repo).
