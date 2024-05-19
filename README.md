[![pl](https://img.shields.io/badge/język-PL-red.svg)](https://github.com/pzemla/Road-sign-images-generation-using-GAN/blob/main/README.pl.md)
# Road sign images generation using GAN

**Dependencies**

Python 3.9.13

matplotlib 3.8.3

notebook 7.1.2

numpy 1.24.1

pandas  2.2.1

scikit-learn 1.4.1.post1 Python 3.9.13

torch 2.2.1+cu118

**How to run**
1. Download dataset from https://www.kaggle.com/datasets/flo2607/traffic-signs-classification
2. Put directory myData in together in directory with GAN.ipynb
3. Run GAN.ipynb in Jupyter Notebook

## Overview
The aim of this project is to build a generative adversarial network (GAN) to generate images of road signs. GAN is implemented using Python with the Pytorch library. The model is trained on a labeled dataset containing images of 42 different road signs with dimensions of 32x32x3 (the dataset contains 42 folders with photos of signs from different perspectives). The low resolution of images is caused by limited time and computing power available for training the network. This project serves as an educational exercise and practical application of deep learning techniques to image generation tasks.

Below are photos of sample road signs:

![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/792960b2-49cf-486b-a8ab-ed02187661fd)
![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/ac42f213-ca5c-47e7-8a3f-9a4e955e6ab2)

![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/66345175-ee70-45e0-8584-4718746b4412)
![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/4881b98d-4607-4530-bee8-caff6c8547a5)

![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/85d1c0f7-620a-45ce-9476-387fd9b9e14f)
![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/cf30cbb8-fc62-483b-8dc2-36df41ef23d8)

# GAN structure

A GAN network consists of two 'smaller' networks - a generator and a discriminator. In this case, the generator's task is to generate realistic images of road signs, and the discriminator's task is to recognize whether the images fed into its input are real (come from a dataset) or if they are generated by the generator. During training, the generator learns to generate more realistic images and the discriminator learns to recognize them better. The generator's input is random noise, and the discriminator's input is an image (from the generator or dataset). The output of the generator is the generated image, and discriminator’s output is evaluation whether the image is generated (0) or real (1).

**Generator:**

|Layer|Description|Input|Output|
| ------------- | ------------- | ------------- | ------------- |
|View|Converting input from 3-dimensional to 1-dimensional|32x1x1|32|
|Linear|Linear layer|32|512|
|View|Converting input from 1-dimensional to 3-dimensional|512|512x1x1|
|Transposed convolution|Stride=4, kernel=1|512x1x1|256x4x4|
|Batch Normalization|Normalization of output from the transposed convolutional layer|256x4x4|256x4x4|
|ReLU|Activation function|256x4x4|256x4x4|
|Transposed convolution|Stride=4, kernel=2, padding=1|256x4x4|128x8x8|
|Batch Normalization|Normalization of output from the transposed convolutional layer|128x8x8|128x8x8|
|ReLU|Activation function|128x8x8|128x8x8|
|Transposed convolution|Stride=4, kernel=2, padding=1|128x8x8|64x16x16|
|Batch Normalization|Normalization of output from the transposed convolutional layer|64x16x16|64x16x16|
|ReLU|Activation function|64x16x16|64x16x16|
|Transposed convolution|Stride=4, kernel=2, padding=1|64x16x16|3x32x32|
|Tanh|Activation function|3x32x32|3x32x32|

**Discriminator:**

|Layer|Description|Input|Output|
| ------------- | ------------- | ------------- | ------------- |
|Convolutional|Stride=4, kernel=2, padding=1|3x32x32|64x16x16|
|Leaky ReLU|Activation function|64x16x16|64x16x16|
|Convolutional|Stride=4, kernel=2, padding=1|64x16x16|128x8x8|
|Batch normalization|Normalization of output from the convolutional layer|128x8x8|128x8x8|
|Leaky ReLU|Activation function|128x8x8|128x8x8|
|Convolutional|Stride=4, kernel=2, padding=1|128x8x8|256x4x4|
|Batch normalization|Normalization of output from the convolutional layer|256x4x4|256x4x4|
|Leaky ReLU|Activation function|256x4x4|256x4x4|
|Convolutional|Stride=4, kernel=1|256x4x4|1x1x1|
|Sigmoid|Activation function|1x1x1|1x1x1|

# Optimizer and loss function

Optimizer – Adam (learning rate=0.001)
Loss function – Binary cross-entropy loss

The Adam optimizer was chosen because it dynamically adjusts the learning rate to each parameter during training, so there is no need to adjust the learning rate decay. Of the other optimizers tested (Adagrad and RMSprop), it provided the best results in the test dataset. It is possible for generator and discriminator to have different optimizers, but in this case both use Adam.

The binary cross-entropy (BCE) loss function was chosen because its result can be interpreted as the probability of belonging to one of two classes, which is why it is often used in true/false classification. Generator and discriminator can have two different loss functions, but in this case both use BCE because in the discriminator it can be interpreted as the probability of correctly recognizing the image, and in the generator as the probability of fooling the discriminator by the generated image.


# Results

Sample images generated after a specified number of training epochs:

100 epochs

![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/7dfe3797-8e39-4fc3-8ef7-2967a8f50ff7)

180 epochs

![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/68631bcf-9b97-41d7-8837-2abfd123588d)

260 epochs

![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/35fd3e61-8581-4151-aa1b-849d01e838ff)

300 epochs

![image](https://github.com/pzemla/Road-sign-images-generation-using-GAN/assets/135070990/5ccfc09d-23a9-41be-80c4-a33dea4f2585)

The generated images of road signs after training the generator resemble real images of road signs in shape and color. In most images, the interior of the sign (where the image or text should be) is blurred. This may be due to poor image quality in the dataset, too many different interiors of road signs (which can be resolved by using a conditional GAN), neural network size, or suboptimal parameter settings. Some of the images are almost completely darkened, this is probably not caused by the training of the neural network, but by the dataset which contains photos of signs taken at night. The background is blurred because the road sign images have many different types of backgrounds (white background, black/night background, normal background, blurred background), so discriminator does not focus on it, and because of that generator does not learn to create a realistic background.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
