# Handwritten Digit Recognition with a Convolutional Neural Network

### My First Machine Learning Project

**Python · Keras · Convolutional Neural Networks · Image Classification · MNIST · GUI**

## Overview

This was my first machine-learning project.

I trained a convolutional neural network to recognize handwritten digits from **0 through 9**, then connected the trained model to a graphical interface where a user can draw a new digit and receive a prediction.

The project introduced me to the complete path from a labeled image dataset to a working prediction program:

```text
training images
→ image preprocessing
→ CNN training
→ model evaluation
→ saved model
→ interactive GUI
```

## Dataset

I trained the model using the **MNIST handwritten-digit dataset**.

MNIST contains **70,000 labeled grayscale images** across ten classes:

```text
0 1 2 3 4 5 6 7 8 9
```

The project uses the standard split:

```text
60,000 training images
10,000 test images
```

Each image is **28 × 28 pixels**.

Before training, I reshape the images into the format expected by the CNN:

```text
28 × 28 × 1
```

I also convert the pixel values to floating-point numbers and normalize them from the original 0–255 range to **0–1**.

The ten digit labels are converted into categorical output vectors for multiclass classification.

## CNN Architecture

I built the model with Keras using two convolutional stages followed by fully connected layers.

The architecture is:

```text
Input: 28 × 28 × 1
↓
Conv2D: 32 filters, 5 × 5, ReLU
↓
MaxPooling2D: 2 × 2
↓
Conv2D: 64 filters, 3 × 3, ReLU
↓
MaxPooling2D: 2 × 2
↓
Flatten
↓
Dense: 128 neurons, ReLU
↓
Dropout: 0.3
↓
Dense: 64 neurons, ReLU
↓
Dropout: 0.5
↓
Dense: 10 neurons, Softmax
```

The convolutional layers learn spatial patterns from the handwritten images. Pooling reduces the size of the intermediate feature maps, while the dense layers use the learned features to classify each image.

The final **softmax layer** outputs a probability for each of the ten possible digits.

## Training

The network is trained using:

- 60,000 training images
- batch size of 128
- 10 epochs
- categorical cross-entropy loss
- Adadelta optimization

During training, the model compares its predictions with the known digit labels and updates its parameters to reduce classification error.

The separate 10,000-image test set is used to measure the model's performance on images outside the training set.

After training, the model is saved as:

```text
Digit_Recognizer.h5
```

This allows the trained CNN to be loaded later without retraining it every time the GUI is opened.

## Interactive Digit Recognition

I built graphical interfaces that let a user draw a digit and send it to the trained CNN for classification.

The general prediction pipeline is:

```text
User draws a digit
↓
Drawing is captured as an image
↓
Image is resized to 28 × 28
↓
Image is converted to grayscale
↓
Pixel values are normalized
↓
Image is reshaped to 28 × 28 × 1
↓
CNN produces probabilities for digits 0–9
↓
Highest-probability digit is displayed
```

The interface includes controls for predicting the digit and clearing the drawing canvas.

## GUI Implementations

The repository contains two GUI implementations.

### `gui_model.py`

This version uses:

- Tkinter
- Pillow
- NumPy

The user draws directly on a canvas. The drawing is saved to `canvas.png`, processed by the model-loading code, and passed through the trained network.

The program displays both:

- the predicted digit
- the model's probability for that prediction

### `gui_windows.py`

This version is designed specifically for Windows.

It uses:

- Tkinter
- `win32gui`
- Pillow `ImageGrab`
- NumPy

The program captures the drawing directly from the Windows canvas, resizes it to **28 × 28 pixels**, converts it to grayscale, normalizes it, and passes it into the CNN.

## Repository Files

```text
train_model.py       Trains the CNN on MNIST and saves the model

Digit_Recognizer.h5  Saved trained CNN

load_model.py        Loads the saved model for prediction

gui_model.py         Cross-platform drawing interface

gui_windows.py       Windows-specific drawing interface

canvas.png           Image used by the GUI prediction pipeline

requirements.txt     Python dependencies
```

## Technical Focus

This project introduced me to:

- supervised machine learning
- convolutional neural networks
- image classification
- image normalization and reshaping
- multiclass softmax prediction
- convolution and max pooling
- dropout for regularization
- training/test separation
- saving and loading trained models
- preprocessing user-generated input
- connecting an ML model to an interactive GUI

It became the starting point for my later computer-vision projects, where I worked with larger image datasets, deeper CNN architectures, image augmentation, and class-imbalance experiments.

## Running the Project

Install the required Python packages:

```bash
pip install -r requirements.txt
```

To train the CNN from scratch:

```bash
python train_model.py
```

This trains the network on MNIST and saves the resulting model as:

```text
Digit_Recognizer.h5
```

To run the general GUI:

```bash
python gui_model.py
```

For the Windows-specific GUI, install `pywin32`:

```bash
pip install pywin32
```

Then run:

```bash
python gui_windows.py
```