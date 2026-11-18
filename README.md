# Project_deep_fashion_detection


## Creates the Venv environment on their machine:

```bash
   python -m venv fashion
   .\fashion\Scripts\Activate.ps1      # Mac/Linux: source fashion/bin/activate
   pip install -r requirements.txt
```
They end up with the same packages, built for their own system.

```

# Leaf disease segmentation — Part 1: segmentation by classification

Binary semantic segmentation (**diseased area vs. everything else**) on the
[Leaf Disease Segmentation dataset with train/valid split](https://www.kaggle.com/datasets/sovitrath/leaf-disease-segmentation-with-trainvalid-split).

```
├── part1_segmentation_by_classification.ipynb    # everything: data exploration + preparation, helpers, Part 1.1 / 1.2 / 1.3
└── requirements.txt
```

## Setup

```bash
source venv/bin/activate            # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## 1. Get the data

Download the dataset from Kaggle (website, or
`kaggle datasets download sovitrath/leaf-disease-segmentation-with-trainvalid-split`) and unzip it
**outside the repo** (or in a git-ignored folder), e.g. `~/datasets/leaf-disease`.

Then set `DATA_ROOT` in the notebook's configuration cell to that folder. Section 0 of the notebook does
the rest directly in memory, with no preprocessing script and no copies written to disk:

- shows the folder structure, the image sizes and the colours used in the masks;
- uses only `orig_data` (`aug_data` contains augmented copies of the same leaves, which would put
  near-duplicates of training images into validation);
- turns the red `(128, 0, 0)` masks into 0/1 (`MASK_THRESHOLD`) and resizes everything to 256×256 (`IMG_SIZE`);
- splits **train / val** = random 85/15 split of the official `train_images` (`VAL_FRACTION`),
  **test** = the official `valid_images`. Splitting by image avoids leakage between pixels of the same leaf.

## 2. Run the notebook

```bash
jupyter lab part1_segmentation_by_classification.ipynb
```

Each experiment adds a row to a results table (val/test AUC, accuracy, F1, mean IoU and Dice on full
test images) shown at the end of the notebook. Diseased pixels are a minority class, so rely on
AUC / F1 / IoU rather than accuracy.

```