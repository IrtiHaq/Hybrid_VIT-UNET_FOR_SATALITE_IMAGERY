# Hybrid ViT - U-Net For Building Segmentation From Satellite Imagery
**By Irti Haq, Youngkyu Ko, Xunan Li**

**Research Questions:** Can a Hybrid ViT + U-Net model achieve high segmentation accuracy (Dice score) and low boundary error (HD95) when segmenting buildings from satellite imagery across diverse urban areas with varying geographic and urban/architectural characteristics?

**Dataset: Chinese GaoFen-7 (GF-7) satellite imagery** 

This dataset is a high-resolution building segmentation dataset. This GF-7 dataset provides an extensive coverage of urban and rural areas of China by picking 6 typical cities in China (Chen et al.2024). The dataset contains 5175 pairs of 512×512 image tiles and 170,015 buildings. Compared to other datasets constructed through satellite and aerial imagery, this dataset has various ground-truth labels for building extraction.

Link: [A benchmark GaoFen-7 dataset for building extraction from satellite images](https://doi.org/10.6084/m9.figshare.24305557)

**Model** : TransUNet

This model is a specialized adaptation of the TransUNet architecture, originally developed by Chen et al. (2021) for medical image segmentation. Here, its application has been applied for building segmentation from satellite imagery.

The Model is a Hybrid ViT + U-Net Model that uses a Vision Transformer as its Encoder and a U-Net Style Model as its Decoder. More specifically the model uses a CNN network (ResNet) and transformer blocks as the encoder and up sampling layers as the decoder to achieve the task. Inspired by a U-Net Structure, Trans-UNet uses the residual network to extract features and do down samplings. The results are then fed into a transformer to encode. Afterwards, up sampling is used to decode the information. The model uses a Pre trained Vision Transformer, specifically in this project, the [ResNet-50 Vit-B 16 ](https://github.com/google-research/vision_transformer) from Google Research's Vision Transformer implementation is used. 

Most of the Code and Architecture for the Model has been directly taken from the TransUNet project by Chen et al. (2021) and then adapted to be used for building Segmentation.  

##### **Source:** [TransUNet: Medical Image Segmentation](https://tianjinteda.github.io/Transunet.html)  & [TransUNet: Transformers Make Strong Encoders for Medical Image Segmentation](https://arxiv.org/pdf/2102.04306#page=5.23)

### Key Changes
Overall These are some of the Key Changes I have made to the models Architeture to improve perfomance for Building Segmentation from Satalite Imagery. 
1) The model optimizer was progressively changed from SGD with Momentum → Adam → AdamW → AdamW with AMSGrad. These changes significantly improved training speed, and adding AMSGrad in particular helped enhance convergence

2) The original use of polynomial learning rate decay led to inconsistent convergence behavior and slower training progress, particularly in the early epochs. To address this, I implemented CosineAnnealingWarmRestarts, which provided smoother and more adaptive learning rate scheduling. This change resulted in faster convergence, reduced training noise, and improved overall stability.

3) Introduced data augmentation using the Albumentations library. Initially applied a stronger set of transformations, but this led to worsened performance. The augmentations were then progressively simplified, ultimately leaving only mild geometric transforms (horizontal/vertical flips and 90° rotations) to improve model generalization and test performance.

4) Optimized various training parameters, including batch size, learning rate, number of skip connections, and number of training epochs.
