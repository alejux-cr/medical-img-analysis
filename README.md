# Medical Image Analysis - PyTorch - ENV set up on Ubuntu 20.04 Focal Fossa
All steps below are specifically for Linux distro Ubuntu 20.04 LTS

## System requirements
- Python 3.7
- Conda
- Jupyter Notebook

### Setup with Conda import env with the pytorch_env_ubuntu.yml to install all dependencies

## Important info on dataset an preprocessing

For Pneumonia:
- 26684 X-Ray images [20672 without] [6012 with pneumonia]
- Original image shape (1024x1024) > Resize images to (224x224)
- Standarize the pixel valus into the interval [0, 1] by scaling with 1/255
- Split dataset into 24000 train images and 2684 validation images
- Store converted images in folders corresponding to the classes > 0 if no pneumonia / 1 if pneumonia
- Preprocessing: Compute training mean and standard deviation for normalization
- Make use of torchvision.DatasetFolder so no need for custom dataset classes
- Z-normalize images with computed mean and std
- Apply data augmentation: Random rotations, translations, scales and resized crops
- For Training: Pytorch-lightning > High levl pytorch wrapper for simple and effective training
- Network architecture: ResNet18 > change input channels from 3 to 1, change output dimension from 1000 to 1
- Loss Function: BCEWithLogitsLoss: Directly applied to logits(raw prediction), negative output = no pneumonia
- Optimizer: Adam(lr=1e-4)
- Train for 30 epochs

For Cardiac-detection:
- Same as Pneumonia for image shape and resize, however CAUTION> bounding boxes must be scaled, however in this case we skip because the labels were taken on images of size (224x224)
- Split dataset into 400 train images and 96 validation images. Store train and validation subject ids
- Compute training mean and standard deviation for normalization
- Custom Dataset
- Task> given a subject idx, load he corresponding X-ray image and bounding box coordinates(xmin, xmax, ymin, ymax)
- Z-normalize images with computed mean and std
- Apply data augmentation: (Gamma) Contrast changes, Scaling, Rotation, Translation, Augment image and bounding box IDENTICALLY!
- Network architecture: ResNet18> Change input channels from 3 to 1 and output dimension from 1000 to 4
- Loss function: Mean squared Error
- Optimizer: Adam(lr=1e-4)
- Train for 50 epochs

