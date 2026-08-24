Handwritten Digit Recognition with a Convolutional Neural Network

My First Machine Learning Project

Python · Keras · Convolutional Neural Networks · Image Classification · MNIST · GUI

Overview

This was my first machine-learning project.

I trained a convolutional neural network to recognize handwritten digits from 0 through 9, then connected the trained model to a graphical interface where a user can draw a digit and receive a prediction.

The project introduced me to the path from a labeled image dataset to a working prediction program:

training images
→ image preprocessing
→ CNN training
→ model monitoring
→ saved model
→ interactive GUI

Dataset and Preprocessing

I used the MNIST handwritten-digit dataset, which contains 70,000 labeled grayscale images across ten classes:

0 1 2 3 4 5 6 7 8 9

The standard MNIST split contains:

60,000 training images
10,000 test images

Each image is 28 × 28 pixels. Before training, the code:

reshapes each image to 28 × 28 × 1

converts pixel values to float32

scales pixels from 0–255 to 0–1

converts digit labels to 10-class categorical vectors

The 10,000-image MNIST test split is passed to Keras as validation_data during training and is evaluated again after the last epoch. Because the same split is used for training monitoring, I treat its final score as a validation-style result.

CNN Architecture

I built the model with Keras using two convolutional stages followed by dense layers.

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

The convolutional layers learn local stroke and shape patterns from the images. Max pooling reduces the spatial size of the learned maps. The dense layers combine those learned patterns for 10-class prediction.

The final softmax layer returns one probability for each digit.

Training

The network is trained with:

60,000 training images

10,000 images used for validation-style monitoring

batch size of 128

10 epochs

categorical cross-entropy loss

Adadelta optimization

After training, the model is saved as:

Digit_Recognizer.h5

The saved model can then be loaded for prediction without retraining the CNN each time the GUI starts.

Interactive Digit Recognition

I built two graphical interfaces that accept a user-drawn digit and pass it to the trained CNN.

The prediction path is:

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
CNN returns probabilities for digits 0–9
↓
Highest-probability digit is displayed

gui_model.py

This version uses Tkinter, Pillow, OpenCV, and NumPy.

The user draws on a 600 × 600 canvas. The drawing is saved as canvas.png, inverted, resized to 28 × 28, normalized, and passed to the saved model through load_model.py.

The interface displays the predicted digit and the model probability associated with that prediction.

gui_windows.py

This Windows-specific version uses Tkinter, win32gui, Pillow ImageGrab, and NumPy.

The program captures the canvas directly from the window, resizes the captured image to 28 × 28, converts it to grayscale, normalizes it, and sends it through the CNN.

Repository Files

train_model.py       Trains the CNN on MNIST and saves the model
Digit_Recognizer.h5  Saved trained CNN
load_model.py        Loads the saved model and preprocesses canvas.png
gui_model.py         Tkinter/Pillow drawing interface
gui_windows.py       Windows-specific drawing interface
canvas.png           Image used by the gui_model.py prediction path
requirements.txt     Python dependencies

Technical Work

This project introduced me to:

supervised machine learning

convolutional neural networks

image normalization and reshaping

multiclass softmax prediction

convolution and max pooling

dropout regularization

training/validation separation

saving and loading trained models

preprocessing user-generated input

connecting a trained model to an interactive GUI

It became the starting point for later computer-vision projects using larger image datasets, deeper CNNs, augmentation, and class-imbalance experiments.

Running the Project

Install the listed dependencies:

pip install -r requirements.txt

Train the CNN:

python train_model.py

Run the Tkinter/Pillow GUI:

python gui_model.py

The Windows GUI also requires pywin32, which is not listed in the current requirements.txt:

pip install pywin32
python gui_windows.py
