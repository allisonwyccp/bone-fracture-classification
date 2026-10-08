# Bone Fracture Classification: MLP and CNN Models for X-ray Images

Binary classification of lower-leg (tibia/fibula) X-ray images as fractured or
normal, comparing fully connected networks (MLP) with convolutional neural
networks (VGG-16 transfer learning and a ResNet trained from scratch) in PyTorch.

Group project for Data Analytics & Neural Networks, MSc Medical Engineering &
Analytics, Carinthia University of Applied Sciences (2026).

## Contributors
Allison Young, Vanessa Lukasser, and Laura Telle contributed equally to this project.

## Notebooks
1. `01_data_preparation.ipynb`: data inspection, cleaning, preprocessing and
   stratified train/test/k-fold split
2. `02_mlp_network.ipynb`: shallow network and multi-layer perceptron (MLP),
   hyperparameter tuning and k-fold cross-validation
3. `03_cnn_classification.ipynb`: VGG-16 transfer learning and a custom ResNet,
   both trained with 5-fold cross-validation

## Data
The raw dataset ([dataset name], [source and link]) contained 2,127 X-ray
images. The images are not included in this repository.

Preprocessing pipeline:
- Removed exact duplicates by MD5 hash (970 of 2,127 files were duplicates,
  a major data-leakage risk)
- Removed near-blank images using mean-intensity thresholds
- Converted to single-channel grayscale, padded to square and resized to 224 × 224
- Applied CLAHE for consistent contrast across scanners
- Manually reviewed the remaining images to exclude illustrations, watermarked
  images and low-quality scans

The final curated dataset contains 108 images (77 fracture, 31 normal). A
stratified 20% test set was held out, and the remaining images were used for
stratified 5-fold cross-validation (seed 42).

`Fractures.xlsx` and `Normals.xlsx` list the image IDs selected during manual
review; `splits.csv` records the train/test/fold assignment of each image. To run
the notebooks, place the original dataset in `Bone fracture dataset/Dataset/`
with `fracture/` and `normal/` subfolders.

## Results (held-out test sets)

| Model | Accuracy | F1 score |
|---|---|---|
| MLP (fully connected) | 94.7% | 92.3% |
| VGG-16 (transfer learning) | 90.9% | 93.8% |
| ResNet (trained from scratch) | 86.4% | 90.3% |

The test sets were small (19–22 images) and differed slightly between notebooks,
so these figures are indicative rather than definitive. VGG-16 produced the most
stable learning curves across folds, showing the value of pretrained features on
a small dataset. The ResNet trained from scratch was less stable. Most
misclassifications involved very dark images or surgical hardware that was not
represented in the training data.

## Tools
Python, PyTorch, torchvision, scikit-learn, OpenCV, pandas, NumPy, Matplotlib

## How to run
    pip install -r requirements.txt

Then run the notebooks in order.
