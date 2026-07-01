# Data folder

The dataset is not committed to this repo. Download it from Kaggle and place it here.

1. Get the data from
   [Histopathologic Cancer Detection](https://www.kaggle.com/c/histopathologic-cancer-detection).
2. Unzip the training images into `data/train/` so the `.tif` files sit at
   `data/train/<id>.tif`.
3. Put the labels CSV in the same folder as `data/train/train_image_labels.csv`
   (Kaggle ships it as `train_labels.csv`, so rename it or update the filename in the
   notebook's first data cell).

The notebook reads from `DATA_DIR`, which defaults to `data/train`. Launch Jupyter from the
repo root so the relative path resolves, or set an absolute path:

```
export DATA_DIR=/absolute/path/to/train        # macOS / Linux
$env:DATA_DIR="C:\absolute\path\to\train"      # Windows PowerShell
```
