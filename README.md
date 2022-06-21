# CNN
CNN(Convolution Neural Network)
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## import library"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "import tensorflow as tf\n",
    "from tensorflow import keras\n",
    "from tensorflow.keras import layers, models\n",
    "import numpy as np \n",
    "import matplotlib.pyplot as plt "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Get MNIST Data. \n",
    "### MNIST data loacted in tensorflow > keras > datasets > mnist \n",
    "### Split data to (train images, train labels) and (test images, test labels)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "mnist = keras.datasets.mnist\n",
    "(train_images, train_labels), (test_images, test_labels) = mnist.load_data()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### There are Total 60000 Train images and Train labels. (6000 images for single class)\n",
    "### Shape of single image is 28 x 28 (pixel)\n",
    "### "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Shape of Train images : (60000, 28, 28)\n",
      "Shape of Train labels :  (60000,)\n",
      "\n",
      "Shape of Test images :  (10000, 28, 28)\n",
      "Shape of Test labels :  (10000,)\n"
     ]
    }
   ],
   "source": [
    "print('Shape of Train images :',train_images.shape)\n",
    "print('Shape of Train labels : ', train_labels.shape)\n",
    "print('\\nShape of Test images : ', test_images.shape)\n",
    "print(\"Shape of Test labels : \",test_labels.shape)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Train labels :  [5 0 4 ... 5 6 8]\n"
     ]
    }
   ],
   "source": [
    "print('Train labels : ',train_labels)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Plot first train image. \n",
    "### when value is close to 0 : dark \n",
    "### when value is close to 255 : white"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[[  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0  51 159 253\n",
      "  159  50   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0   0  48 238 252 252\n",
      "  252 237   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0  54 227 253 252 239\n",
      "  233 252  57   6   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0  10  60 224 252 253 252 202\n",
      "   84 252 253 122   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0 163 252 252 252 253 252 252\n",
      "   96 189 253 167   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0  51 238 253 253 190 114 253 228\n",
      "   47  79 255 168   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0  48 238 252 252 179  12  75 121  21\n",
      "    0   0 253 243  50   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0  38 165 253 233 208  84   0   0   0   0\n",
      "    0   0 253 252 165   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   7 178 252 240  71  19  28   0   0   0   0\n",
      "    0   0 253 252 195   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0  57 252 252  63   0   0   0   0   0   0   0\n",
      "    0   0 253 252 195   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0 198 253 190   0   0   0   0   0   0   0   0\n",
      "    0   0 255 253 196   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0  76 246 252 112   0   0   0   0   0   0   0   0\n",
      "    0   0 253 252 148   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0  85 252 230  25   0   0   0   0   0   0   0   0\n",
      "    7 135 253 186  12   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0  85 252 223   0   0   0   0   0   0   0   0   7\n",
      "  131 252 225  71   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0  85 252 145   0   0   0   0   0   0   0  48 165\n",
      "  252 173   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0  86 253 225   0   0   0   0   0   0 114 238 253\n",
      "  162   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0  85 252 249 146  48  29  85 178 225 253 223 167\n",
      "   56   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0  85 252 252 252 229 215 252 252 252 196 130   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0  28 199 252 252 253 252 252 233 145   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0  25 128 252 253 252 141  37   0   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]\n",
      " [  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0\n",
      "    0   0   0   0   0   0   0   0   0   0]]\n"
     ]
    }
   ],
   "source": [
    "print(train_images[1])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Plot First 10 Train images and Corresponding labels  "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "First 10 Train images in MNIST dataset\n",
      "\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 10 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "\n",
      "Train labels match with Train label sequentialy\n",
      " [5 0 4 1 9 2 1 3 1 4]\n"
     ]
    }
   ],
   "source": [
    "print('First 10 Train images in MNIST dataset\\n')\n",
    "for i in range(10):\n",
    "    plt.subplot(1, 10, i+1)\n",
    "    plt.xticks([])\n",
    "    plt.yticks([])\n",
    "    plt.imshow(train_images[i])\n",
    "plt.show()\n",
    "print('\\nTrain labels match with Train label sequentialy\\n',train_labels[:10])\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Important\n",
    "### Change data shape (60000 x 28 x 28) to (60000 x 28 x 28 x 1)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [],
   "source": [
    "train_images = tf.reshape(train_images, [-1, 28, 28, 1])\n",
    "test_images = tf.reshape(test_images, [-1, 28, 28, 1])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Select one convolution model below \n",
    "## There are 3 example models. \n",
    "## 3, 5, 7 layer each \n",
    "## MODEL 1 : 3 Layers with 1 Convolution layer  \n",
    "## MODEL 2 : 5 Layers with 2 Convolution layer \n",
    "## MODEL 3 : 7 Layers with 4 Convolution layer "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [],
   "source": [
    "def select_model(model_number):\n",
    "    if model_number == 1:\n",
    "        model = keras.models.Sequential([\n",
    "                    keras.layers.Conv2D(32, (3,3), activation = 'relu', input_shape = (28, 28,1)),  # layer 1 \n",
    "                    keras.layers.MaxPool2D((2,2)),                                                  # layer 2 \n",
    "                    keras.layers.Flatten(),\n",
    "                    keras.layers.Dense(10, activation = 'softmax')])                                # layer 3\n",
    "\n",
    "    if model_number == 2:\n",
    "        model = keras.models.Sequential([\n",
    "                    keras.layers.Conv2D(32, (3,3), activation = 'relu', input_shape=(28,28,1)),     # layer 1 \n",
    "                    keras.layers.MaxPool2D((2,2)),                                                  # layer 2\n",
    "                    keras.layers.Conv2D(64, (3,3), activation = 'relu'),                            # layer 3 \n",
    "                    keras.layers.MaxPool2D((2,2)),                                                  # layer 4\n",
    "                    keras.layers.Flatten(),\n",
    "                    keras.layers.Dense(10, activation = 'softmax')])                                # layer 5\n",
    "                    \n",
    "    if model_number == 3: \n",
    "        model = keras.models.Sequential([\n",
    "                    keras.layers.Conv2D(32, (3,3), activation = 'relu', input_shape = (28, 28,1)),  # layer 1\n",
    "                    keras.layers.MaxPool2D((2,2)),                                                  # layer 2\n",
    "                    keras.layers.Conv2D(64, (3,3), activation = 'relu'),                            # layer 3\n",
    "                    keras.layers.Conv2D(64, (3,3), activation = 'relu'),                            # layer 4\n",
    "                    keras.layers.MaxPool2D((2,2)),                                                  # layer 5\n",
    "                    keras.layers.Conv2D(128, (3,3), activation = 'relu'),                           # layer 6\n",
    "                    keras.layers.Flatten(),\n",
    "                    keras.layers.Dense(10, activation = 'softmax')])                                # layer 7\n",
    "    \n",
    "    return model \n",
    "\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [],
   "source": [
    "model = select_model(1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## If you want to see information of model, model.summary() will help\n",
    "### summary() is also built in function "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Model: \"sequential\"\n",
      "_________________________________________________________________\n",
      "Layer (type)                 Output Shape              Param #   \n",
      "=================================================================\n",
      "conv2d (Conv2D)              (None, 26, 26, 32)        320       \n",
      "_________________________________________________________________\n",
      "max_pooling2d (MaxPooling2D) (None, 13, 13, 32)        0         \n",
      "_________________________________________________________________\n",
      "flatten (Flatten)            (None, 5408)              0         \n",
      "_________________________________________________________________\n",
      "dense (Dense)                (None, 10)                54090     \n",
      "=================================================================\n",
      "Total params: 54,410\n",
      "Trainable params: 54,410\n",
      "Non-trainable params: 0\n",
      "_________________________________________________________________\n"
     ]
    }
   ],
   "source": [
    "model.summary()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Components in training step \n",
    "### Optimizer, Loss function, accuracy metrics "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [],
   "source": [
    "model.compile(\n",
    "    optimizer = 'adam',\n",
    "    loss = 'sparse_categorical_crossentropy',\n",
    "    metrics = ['accuracy']\n",
    ")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Training Step \n",
    "## Training for 5 epochs. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Train on 60000 samples\n",
      "Epoch 1/5\n",
      "60000/60000 [==============================] - 31s 512us/sample - loss: 0.7183 - accuracy: 0.9410\n",
      "Epoch 2/5\n",
      "60000/60000 [==============================] - 29s 489us/sample - loss: 0.0890 - accuracy: 0.9742\n",
      "Epoch 3/5\n",
      "60000/60000 [==============================] - 29s 486us/sample - loss: 0.0739 - accuracy: 0.9782\n",
      "Epoch 4/5\n",
      "60000/60000 [==============================] - 30s 493us/sample - loss: 0.0664 - accuracy: 0.9798\n",
      "Epoch 5/5\n",
      "60000/60000 [==============================] - 29s 488us/sample - loss: 0.0599 - accuracy: 0.9829\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "<tensorflow.python.keras.callbacks.History at 0x27cab6b9ac8>"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "model.fit(train_images, train_labels,  epochs = 5)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Test Step \n",
    "## Perform Test with Test data "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "10000/1 - 2s - loss: 0.0582 - accuracy: 0.9712\n",
      "\n",
      "Test loss :  0.11443363445440191\n",
      "Test accuracy : 0.9712\n"
     ]
    }
   ],
   "source": [
    "test_loss, accuracy = model.evaluate(test_images, test_labels, verbose = 2)\n",
    "print('\\nTest loss : ', test_loss)\n",
    "print('Test accuracy :', accuracy)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Before prediction, change test image's type to float 32. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [],
   "source": [
    "test_images = tf.cast(test_images, tf.float32)\n",
    "pred = model.predict(test_images)\n",
    "Number = [0,1,2,3,4,5,6,7,8,9]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Prediction :  (10000, 10)\n",
      "Test labels :  (10000,)\n"
     ]
    }
   ],
   "source": [
    "print('Prediction : ', pred.shape)\n",
    "print('Test labels : ', test_labels.shape)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Functions for plot images, probability"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [],
   "source": [
    "def plot_image(i, predictions_array, true_label, img):\n",
    "  predictions_array, true_label, img = predictions_array[i], true_label[i], img[i]\n",
    "  plt.grid(False)\n",
    "  plt.xticks([])\n",
    "  plt.yticks([])\n",
    "\n",
    "  plt.imshow(img, cmap=plt.cm.binary)\n",
    "\n",
    "  predicted_label = np.argmax(predictions_array)\n",
    "  if predicted_label == true_label:\n",
    "    color = 'blue'\n",
    "  else:\n",
    "    color = 'red'\n",
    "\n",
    "  plt.xlabel(\"{} {:2.0f}% ({})\".format(Number[predicted_label],\n",
    "                                100*np.max(predictions_array),\n",
    "                                Number[true_label]),\n",
    "                                color=color)\n",
    "\n",
    "def plot_value_array(i, predictions_array, true_label):\n",
    "  predictions_array, true_label = predictions_array[i], true_label[i]\n",
    "  plt.grid(False)\n",
    "  plt.xticks([])\n",
    "  plt.yticks([])\n",
    "  thisplot = plt.bar(range(10), predictions_array, color=\"#777777\")\n",
    "  plt.ylim([0, 1])\n",
    "  predicted_label = np.argmax(predictions_array)\n",
    "  plt.xticks(Number)\n",
    "\n",
    "  thisplot[predicted_label].set_color('red')\n",
    "  thisplot[true_label].set_color('blue')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [],
   "source": [
    "(train_images, train_labels), (test_images, test_labels) = mnist.load_data()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x216 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "i = 1\n",
    "plt.figure(figsize=(6,3))\n",
    "plt.subplot(1,2,1)\n",
    "plot_image(i, pred, test_labels, test_images)\n",
    "plt.subplot(1,2,2)\n",
    "plot_value_array(i, pred,  test_labels)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAq4AAAI/CAYAAAC2xVvgAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4xLjEsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy8QZhcZAAAgAElEQVR4nOzdd7wcVd3H8e+PFFIgBRKKBHKRIBAjqZQgRKQZyhO6ggjIgyJNKT6gICBNpIlIeaiC+iAEiIAhlEgRCAJCeiGACQSMCYaEGHpIwu/5YyeHmc3u3t3bds+9n/frNa/8Zs+ZmXOz93fn7JmzM+buAgAAAGrdWtVuAAAAAFAOOq4AAACIAh1XAAAARIGOKwAAAKJAxxUAAABRoOMKAACAKLSvpHKvXr28rq6umZqCQubNm6fFixdbU+6T9zFn2jRp5crCZe3bSwMHNv0xJ02atNjdezfV/ngvqyOW97Eav+MxiuX9RGm18D6Sc02j1HtZUce1rq5OEydObJpWoSzDhg1r8n3yPuZYiY8DK1dKzfFfZGZvNuX+eC+rI5b3sRq/4zGK5f1EabXwPpJzTaPUe8lUAQAAAESBjisAAACiQMcVAAAAUaDjCgAAgCjQcQUAAEAU6LgCAAAgCnRcAQAAEAU6rgAAAIhCRQ8gAIBCrrzyyhB//PHHIZ4+fXqm3pgxYwpuf8IJJ2TWhw8fHuIjjzyyKZoIAGgFGHEFAABAFOi4AgAAIApMFQBQsW9961uZ9Xvvvbes7azIg7xvvPHGzPrjjz8e4q997Wsh3myzzcptIoAm9tprr4V4q622CvE111yTqffDH/6wxdqEtocRVwAAAESBjisAAACiQMcVAAAAUWCOK4CypOe1ljundeutt86sjxw5MsSvv/56iMeOHZupN2fOnBDfcccdIT777LPLayyAJjdlypQQr7XW5+Nem2yySTWagzaKEVcAAABEgY4rAAAAosBUAQAFTZw4MbN+//33F607YMCAEKcv+/fq1StTb5111gnxp59+GuIddtghU2/atGkhXrJkSZktBtCcpk6dGuJ0Lh900EHVaA7aKEZcAQAAEAU6rgAAAIhCVacKjBkzJsS33HJLpuwLX/hCiDt16pQpO+KII0K80UYbhbhfv35N3USgzVq4cGFm3d1DnJ4aIEnjx48P8cYbb1zW/q+88soQz549u2i9/fbbr6z9AWhaM2bMyKxfe+21IT7qqKNaujmAJEZcAQAAEAk6rgAAAIgCHVcAAABEoapzXM8444wQz5s3r+ztbrzxxhB369YtxP3792+SdpVj0003DfGZZ56ZKRs2bFiLtQNoLv/1X/+VWU8/zWrdddfNlK233noV7//uu+8OcfrWWABqw6uvvppZ//DDD0OcfpIe0JIYcQUAAEAU6LgCAAAgClWdKnDrrbeGOP2kHCl72f/ll1/OlE2ZMiXETz31VIhfeOGFTL3NNtssxG+99VZZberQoUOI85/6k749UPpY6WkDElMF0Dr17du30fu44oorQvzaa68VrZd+klb+U7UAtIzLL788s15XVxdiznOoFkZcAQAAEAU6rgAAAIgCHVcAAABEoapzXHffffeCcb6RI0cWLVu6dGmI03NfpewcnJdeeqmsNq299toh3mqrrTJlW2+9dYjffffdEG+xxRZl7Rtoa8aNG5dZP++880K8fPnyEG+44YaZepdeemmIu3Tp0kytA5AvfWvK/PNm+pzYtWvXlmoSkMGIKwAAAKJAxxUAAABRqOpUgabQs2fPEO+2225F65WailDMn/70p8x6elrCtttuG+LDDjus4n0DbcHEiRMz6+npAWn5T+H52te+1mxtAlDc008/XbSsd+/eLdgSoDBGXAEAABAFOq4AAACIQvRTBZraokWLQnziiSdmytw9xOlvR6+33nrN3zAgEgcccECIx48fX7Te0UcfHeKLL764WdsEoDzTp08vWnbmmWe2YEuAwhhxBQAAQBTouAIAACAKdFwBAAAQBea45rn++utDnJ7vKkk9evQIcf5TtYC2bOHChSF+7rnnQpx/+6v07XTOOeecEK+zzjrN2DoApTz//PMhvv3220M8ePDgTL0999yzxdoEFMOIKwAAAKJAxxUAAABRYKqApGeffTbEl156adF6f/7zn0M8YMCAZm0TEJODDjooxIsXLy5a74gjjgjxFlts0axtAlCeJ554IsTpJ0SOHDkyU69Tp04t1iagGEZcAQAAEAU6rgAAAIgCUwUkPfzwwyH+9NNPQ7zHHntk6g0fPrzF2gTUsrFjx2bWp0yZUrDerrvumlm/8MILm6tJABpo2rRpBV8/9NBDW7glQP0YcQUAAEAU6LgCAAAgCnRcAQAAEIU2Ocf1448/zqw/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VUn64x//GOIXX3yxSY8F1IJly5aF+IUXXiha78QTT2yJ5kiSbr755hC/8847mbL+/fuHeLfddmuxNgGtyUsvvVRWPZ4SiVrAiCsAAACiQMcVAAAAUajZqQKrVq0qWvb3v/+9Rdrg7pn1zz77rGBZflvTT+m64447mql1QNNbvnx5iOfPn58pO/zww1u6OZKkuXPnFi0bMGBAC7YEaJ2KTRXIfwJdS04RAophxBUAAABRoOMKAACAKNTMVIEbb7wxs96uXbsqteRzDz74YGZ9ypQpITazEOe39YILLmjehgHNZN111w1x/pNxZsyYEeJ33303U7beeus1aTsWLVoU4vwneKV99atfbdLjAm3Bs88+m1nPfyLdat27d8+s9+nTp9naBJSLEVcAAABEgY4rAAAAokDHFQAAAFGomTmu48aNq9qx00/jefnll0N8ySWXlLV9/hO7OnTo0DQNA1pY586dQ9yvX79M2ZgxY0K87777ZspOP/30io81c+bMEOff8urNN98McXo+eb611uKzN1CpJUuWZNbzb/242p577tkSzQEqwl99AAAARIGOKwAAAKJQM1MFqukXv/hFiK+//vqytqmrqwvx73//+0zZZptt1iTtAqrp/PPPz6ynLyfmT+057LDDKt5/7969Q5w/HWDx4sVl7eOYY46p+LhAW1fqFnPpp2Udd9xxLdEcoCKMuAIAACAKdFwBAAAQhTY5VWCfffbJrL/yyisV76N///4h3mWXXRrdJqDWbLPNNpn1e+65J8Tpp8hJa94VoByHHHJI0bKjjz46xHfccUfReum7IAAobv78+SEu9qQsKft0rO22265Z2wQ0BCOuAAAAiAIdVwAAAESBjisAAACiUDNzXPOf3LFq1aqidR955JGCr3//+9/PrC9YsKCsY5V6Mk8x1XzSF1BtgwcPLrneWF/84hfLqjdjxowQf+UrX2nSNgCtyXPPPRfiYk/KkqT999+/JZoDNBgjrgAAAIgCHVcAAABEoWamCpxwwgmZ9TPPPLNo3X333TfE7dq1K1qvWFn+NIRS+0g7/vjjy6oHoHHSlzJLXdZkegBQniVLlhQt69WrV4hPPfXUlmgO0GCMuAIAACAKdFwBAAAQhZqZKnDQQQdl1i+//PIQL168uFmPnb5Mkn5a0C233JKpt/HGGzdrOwDkpO/00ZC7fgDIGj9+fNGy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NAeoGYy4AgAAIAp0XAEAABCFqk4VGDp0aIhnzJiRKUs/pefRRx/NlF1++eUh3mCDDUJ89NFHl3XcI488MrO+7bbbFqy30047Zdbzpw4Abcnhhx8e4vwpOi+99FKIX3nllRDn5/X9998f4qVLl4Y4fYu7/LKbb745xPm5279//7LaDsQmfbu4tPynx11wwQUhHjVqVKYsf1oB0Bow4goAAIAo0HEFAABAFGrmyVk9e/bMrH/9618vGEvSZZdd1qhjvf7665n19F0GBg0aFOIrr7yyUccBWpM99tgjxPlP6Jk+fXqIt9lmmxAXu2OHJO25554hvv766zNl++23X4hfe+21EF9zzTWZejfeeGN9zQai9M4774Q4nUfpJ9hJ2akCF198cabs+OOPD/EOO+wQ4n/+85+Zev369Qvxl7/85aJtmjVrVoiHDx8eYu5sgJbEiCsAAACiQMcVAAAAUaDjCgAAgCjUzBzXlnThhRdm1tPzh9K32urdu3eLtQmodeutt16I77333kzZIYccEuJly5aFOP8pdT/60Y9CnJ6r3qlTp0y9gw46KMS//OUvQzx+/PhMvblz54aY29WhNfmf//mfEP/qV78qa5tVq1Zl1tNzx/PnkTdW+laUu+66a6Zs9OjRTXosII0RVwAAAESBjisAAACi0GamCqQvbf7+97/PlHXr1i3E66+/fou1CYhV+tZYUvYpP3feeWeI85+IlZ6mkz89IO3cc88N8ezZs0Oc/8Su9P7y8xqI2aWXXhrib37zmyE+4ogjMvVWrFgR4vnz52fK8qcONKVFixaFOH/q0IABA0J8zjnnNFsb0DYx4goAAIAo0HEFAABAFNrMVIFHHnmkaNm+++4b4iFDhrREc4BWJT11IH8aQUN07tw5xN/61rdCnD9V4K9//WuI33333RCn74AAxKhdu3Yh3m677UKcfpJcvieeeCKznp5GcP7554f4xRdfbIIWfi7/7iGTJk1q0v0DaYy4AgAAIAp0XAEAABAFOq4AAACIQpuc49q1a9dMWfoJJQBqS/pWQGPHjs2UpZ/Qc91114X4vPPOa/6GATVm9913L1o2derUEOfPce3QoUOIjznmmBB///vfz9T79a9/HeL0be+AlsSIKwAAAKJAxxUAAABRaNVTBW688cYQv/322yHecMMNM/W4BRZQu9Za6/PP12eeeWam7IEH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KwAAAKJAxxUAAABRqNk5rj/4wQ9CnH9rm2K3tsp/pN1XvvKVEKfnwnXq1KnocdNzdW644YZM2THHHBPiqVOnhnijjTbK1DvxxBNDfPDBB2fKevfuXfTYQC0799xzi5btsMMOLdiSwkaOHBni/NzNv7Ud0Fo89NBDRcu+973vNemxTjjhhILHXbp0aabeRx99VHD7bt26ZdZPP/30EJf6+wKkMeIKAACAKNBxBQAAQBSqOlXgk08+CfHll1+eKbvllltC7O6Zsg022CDE6UsXZ5xxRqZe165dK25T+jZXK1euzJRdcMEFIf7GN74R4nnz5lV8HKDWvf7665n1f/3rXyHu0aNHpiw9LadadttttxDnTxUAWov8y/ArVqwIcfq2j1L2NnWlpM91kydPDvEBBxyQqff222+HOH1ezp8Cl562l97fW2+9lal30003hfioo47KlPXt27estqPtYcQVAAAAUaDjCgAAgChUdarAU089FeL0E6uk7GWITTbZJFOWfgrO9ttvX/FxV61alVn/5z//GeL05Yp99903Uy//m5PFHHnkkSHOv6QKxOKOO+7IrKenDhxyyCGZsp122qlF2gS0dbfeemtm/d///neI03fjKWXBggWZ9ZtvvjnEF110UdHt0ufi9HkufScdac0pC6uNGjUqs56+M8HChQszZUwVQDGMuAIAACAKdFwBAAAQBTquAAAAiEJV57imb8HRrl27ovU6dOiQWf/73/8e4jFjxoT4lVdeKbqPzp07h3j27NmZsvR6r169Qpy+9UcpG264YWb9nHPOCXF+24FY3HXXXZn19HztU045paWbA0DSlClTipZtueWWZe3j4osvzqzfeOONITazEO++++6ZeldddVWIBwwYUNax0vr161fxNkA+RlwBAAAQBTquAAAAiEJVpwqkL0N8/etfz5Q99thjIX7zzTczZT/60Y/K2n/79p//ePlPwSqm1PSAtdb6vJ9/0EEHhfiaa67J1Nt4443LOhYQk6233jrEO++8cxVbArRd+beyKtdrr70W4tGjRxetd9xxx4X4N7/5TaasY8eODTp2MUOHDg3xkCFDmnTfaL0YcQUAAEAU6LgCAAAgClWdKpD+pv/999+fKfvPf/4T4ksvvTRT9re//S3E66+/fog322yzTL3ly5eHeNq0aSFO35WgEumnklxyySUh5ulYaC0+/PDDEJc7vQZAy3nvvfcy6+mnTKbjfNdee22I0+dXSTriiCNCfMMNNzS2iUV98MEHmfX0dL6mnoaA1osRVwAAAESBjisAAACiQMcVAAAAUajqHNdS0vNG8+e4NsRRRx0V4lJzXLt16xbi9FNCJOm73/1uiEs96QuI1d133x3iOXPmZMrST5WrRWPHji1axhPs0Fqkn2yVv55flpa+jVZ+vYbeYqsc6X3feuutmbKDDz642Y6L1osRVwAAAESBjisAAACiULNTBZrC5ZdfHuJSTwpJS98K5Nvf/naTtwlA05k0aVKIH3zwwaL1fvGLX7REc4CadfPNN4f4ueeey5Sl19O3ekzfAlLK3n6yXOmnTHbp0iVT9uMf/7ji/QGMuAIAACAKdFwBAAAQhVY1VSD/G4sXX3xxiFesWFF0uwEDBoSYbzkCtSs9NUCSfvWrX4U4/TSgnXfeOVNv5MiRzdswoBmlv5m/cOHCBu0jfZl/8uTJmbJRo0aF+Nxzzw3x+PHjM/XGjRsX4nXXXbfg61L23DtlypQQn3POOZl6O+64Y1ltB9IYcQUAAEAU6LgCAP6/vXsPk6K68z/+/sodYoRVMIMmollDYFkFFKM/E4JiFK+EjeYxWdEkRuNKsqBmXRN/IogxKtHH5HHNRcEVYy5oLosSMSZeN164ygAiBgkahUQQLxjzUzDf3x91plJV090zPTNMV/V8Xs9Tz3yr61TVme759jlz6nS1iEghqOMqIiIiIoVQ+DmuixcvjuPsrTW2b99ecp/k3BxI3wKrV69eHVg7kWIZMmRIHCe/Ra6W3n333Tj+1re+ldqWvM3dvvvuW7Zc9+6Ff6uTLmzw4MFx/KEPfSi17fnnn4/jBx54ILUteTur5K2oGhoaUuWWLFkSx8n5qsOGDUuVS84jT7a32c+XJM+VnNeanD8r0lYacRURERGRQlDHVUREREQKofDXz5LflvPGG2+ULdevX784XrBgQWpb9tY5Il3V0UcfHcfJy5MAr7/+ehxv3bo1tW2vvfZq13kbGxtT6zfddFMcJ2/dk7ykmfXDH/4wjj/ykY+0qz4ieTVnzpzU+oknnhjHCxcuTG079thj4/jCCy+M4+xUgaQnn3wyjpPfopXd5u5xPHTo0FS55H6TJk0qey6RttCIq4iIiIgUgjquIiIiIlIIhZwqkLxbwLXXXtuqfc4444w4HjduXEdXSaTurV27No6PO+641LZKlx5bI3kJEppPRWgycODA1PrJJ58cx2PGjGlXHUSKIHn3DIBFixbF8VFHHZXa9vjjj8fxaaedVvaYycv+Ztaqenz+85+P42w7nPyWLpGOphFXERERESkEdVxFREREpBDUcRURERGRQijEHNc333wztZ78No933nmn7H4HH3xwHN9www0dXzGROpa9Fc6sWbPiOHmLql1ht93+/j91cr5c8pY+AJdccskurYdI3iXnlz/xxBOpbT/96U/jeP369XF88803p8qdffbZcZzMvaxkuQ9/+MPVV1akA2jEVUREREQKQR1XERERESmEQkwVeOCBB1LrL730Uqv2u/766+O4d+/eHVonkXqX/cab5LdRTZgwIbVt1apV7TrXueeem1ofNWpUHJ933nntOrZIV9G/f//U+pe+9KWS5WbPnt0Z1RHZJTTiKiIiIiKFoI6riIiIiBSCOq4iIiIiUgiFmON62WWXtarcxRdfnFo/+uijd0V1RLqkwYMHx3FjY2MNayIiIl2VRlxFREREpBDUcRURERGRQijEVIFt27aV3TZo0KA4njZtWmdUR0RERERqQCOuIiIiIlII6riKiIiISCEUYqrAhRdeWHY9eceBhoaGTquTiIiIiHQujbiKiIiISCGo4yoiIiIihaCOq4iIiIgUQiHmuF5wwQUV10VERESk/mnEVUREREQKQR1XERERESkEc/fWFzbbAjy/66ojJezn7gM78oAtvI57AVtbOERXLdMRx+jQ11M5WTOd/TrWWx7krYzysj7kLS/z9DeetzJtzsmqOq5S/8xsqbsfqjK77jwi1arHPMhbGZFqddVc6az3kXI0VUBERERECkEdVxEREREpBHVcJesHKrPLzyNSrXrMg7yVEalWV82VznofKc3da7aATwVfDb4GfFqZMmPBl4PvBD81s+0s8N+H5azE44eArwJfD/4d8DCX168BbwSflyg7GXxqhTo2gN8T4p7gt4ZjrwQflyj3G/ABtXw+tWjpiAV8Avi6kD+XlClzWsjbv4Efmtn2tbDvOvDjWjou+B0hL69KPHYZ+MQKdRwFfkuILeT5+nCc0eHxgeCLav18atHS3gW8P/hd4M+ArwU/okSZ3LSVYf0g8MfD+8Qq8N7hcbWVWtq11O7E+IjQae0L3j38MR9YotyQkADzkskI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l9w/OTUgu37XXXeVPe/WrVvjeO7cua2rrIikZKcEtKacpgpIrWjEVUREREQKQR1XERERESmEupoqkPzEMsBll10Wx7/61a/i2N1T5cp9gvkb3/hGqlxDQ0McJ79BZPz48alyffr0qabaIoX3/e9/P44vvfTSqvfPXnYs9ynnrNtuuy2ONVVAZNdKfsOWSK1oxFVERERECkEdVxEREREpBHVcRURERKQQCjnHNXnrneRcuM997nOpcslb72Rvt5NU7tY72bmqL7zwQhwnbwsyb968VLkzzjij7LlE6sHq1atT67Nmzar6GNdcc00cT506NbVt+vTpcXzttddWfWwREalPGnEVERERkUJQx1VERERECqGQUwWWL18ex8cdd1zZcoMHD47j5Df49O3bt+w+zz//fNlyX/nKV+K4V69ecZy8TZZIvUpOD/j617+e2rZly5Y4Tk692W+//VLlFixYEMfDhw+P4912S/8PfcUVV8TxpEmT4viUU04pe96DDjoojhsbG8v8FiKSdfnll8fxzJkzy5ZL3g5Lt8aSWtGIq4iIiIgUgjquIiIiIlII6riKiIiISCEUYo5r9tY72XluTY455pjU+je/+c04Hj16dKvOtWnTpjieOHFiattrr70WxxdffHEcZ7/yVaQeJb9S+Z577kltS36Nco8ePeJ4ypQpqXIjRoxo1bmSxzjssMPiOHvLu+uuuy6OV61aFcfnnntuqtwPfvCDVp1XpCuqNK9VJG804ioiIiIihaCOq4iIiIgUQiGmClx55ZWp9eQtcE466aQ4Tl42BDjwwAOrPldyWkLytltZEyZMqPrYIkV27733xnGlb6IbN25cHF900UUdWoerr766bJ2SUwWWLFnSoecVEZF80IiriIiIiBSCOq4iIiIiUgi5nSpwzjnnxPH8+fNT297znvfEcfLSYVumBgDs2LEjjpN3Ikh+UhrSl0A//vGPt+lcIkXxyiuvpNaffPLJVu03efLkXVGdFs+VvNOHiIjUJ424ioiIiEghqOMqIiIiIoWgjquIiIiIFEJu57guXbo0jrO33unXr18cDx8+vOpjJ+e0Alx22WVx/Mgjj5Q97/Tp06s+l0hRLVu2LLW+cePGsmXHjh0bxyeeeOKuqlKrJb/lDmDz5s1x3NDQ0NnVERGRDqIRVxEREREpBHVcRURERKQQcjtVoKMlL3PedNNNqW3Zb9xqMnjw4NT6yJEjO7xeInmVnK7TkpkzZ8bxgAEDdkV1qvLCCy+k1pPfiKepAiJtM2PGjFpXQUQjriIiIiJSDOq4ioiIiEgh5HaqwLBhw+K4sbExtW3btm1xPGrUqFYdb8uWLXG8adOm1Lbs3QOajB8/PrXev3//Vp1LpB689dZbqfXsN8kl5eGb5CrVT0RE6oNGXEVERESkENRxFREREZFCUMdVRERERAoht3Nc58yZE8fbt29PbVu4cGEcZ+e/tsaCBQtS67fffnsc33XXXXF83nnnVX1skXqRvR1WubngeZGsX97rKiIibaMRVxEREREpBHVcRURERKQQcjtVoE+fPnF89913p7Y99NBDcVzp232GDx8exyeccEIcn3/++alyd955ZxwPHTo0jj/4wQ+2vsIikhu77757an3PPfesUU1ERKQjacRVRERERApBHVcRERERKYTcThWoZNy4cSXj1vre976XWk9+AnnMmDFxPHDgwKqPLSKdZ968eSUfnzFjRmp99OjRnVAbkWJKtqPJqXhZybzK5phIZ9GIq4iIiIgUgjquIiIiIlII6riKiIiISCEUco5rW2zcuLHstuStc6ZNm9YJtRHJv6uvvjq1/tRTT8Xxli1bUtu+8IUvxPHcuXN3bcUSkvUYNGhQHOtb70RE6pNGXEVERESkENRxFREREZFC6DJTBa644oqy20466aQ41m1zRCIjR45Mrc+ePTuOzzrrrNS2+fPnx/GXv/zlOO7ofDrnnHNS63/+85/j+NOf/nQc9+7du0PPK1JPsre8qnQLLJG80YiriIiIiBSCOq4iIiIiUgh1PVVg9erVcfzzn/+8bLkJEyZ0RnVECu3II4+M489+9rOpbT/60Y/i+OGHH47jjpgq8MADD8RxNo/33nvvOJ4+fXq7zyXSFcycObPWVRBpM424ioiIiEghqOMqIiIiIoWgjquIiIiIFEJdz3FdsWJFHL/xxhtxbGapcrp1jkjLDjjggDi+8sorU9t+97vfxXFy/lz2G7auuuqqksd+9tlnU+uLFy+O4wsvvDCOX3vttVS5r371q3E8fPjwsnUX6eqSt7xq7e2vHnzwwdT6uHHjOq5CIm2kEVcRERERKQR1XEVERESkEOp6qkDyMmVyesCIESNS5U499dROq5NIPRgyZEhq/bHHHovj8847L45vuumm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\n",
      "text/plain": [
       "<Figure size 864x720 with 30 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "num_rows = 5\n",
    "num_cols = 3\n",
    "num_images = num_rows*num_cols\n",
    "plt.figure(figsize=(2*2*num_cols, 2*num_rows))\n",
    "for i in range(num_images):\n",
    "  plt.subplot(num_rows, 2*num_cols, 2*i+1)\n",
    "  plot_image(i, pred, test_labels, test_images)\n",
    "  plt.subplot(num_rows, 2*num_cols, 2*i+2)\n",
    "  plot_value_array(i, pred, test_labels)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Plot images and probability that model predicted wrong"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [],
   "source": [
    "def error_mnist(prediction_array, true_label):\n",
    "    error_index = []\n",
    "    \n",
    "    for i in range(true_label.shape[0]):\n",
    "        if np.argmax(prediction_array[i]) != true_label[i]:\n",
    "            error_index.append(i)\n",
    "    return error_index\n",
    "\n",
    "# change num_cols, num_rows if you want to see more result.  \n",
    "def plot_error(index, prediction_array, true_label):\n",
    "    num_cols = 5\n",
    "    num_rows = 5\n",
    "    plt.figure(figsize=(2*2*num_cols, 2*num_rows))\n",
    "\n",
    "    assert len(index) < num_cols * num_rows\n",
    "    for i in range(len(index)):\n",
    "        plt.subplot(num_rows, 2*num_cols, 2*i+1)\n",
    "        idx = index[i]\n",
    "        plt.imshow(test_images[idx])\n",
    "        plt.subplot(num_rows, 2*num_cols, 2*i+2)\n",
    "        plt.bar(range(10), prediction_array[idx])\n",
    "        plt.xticks(Number)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Find index of wrong prediction\n",
    "## Plot first 10 wrong predicted images and probability"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[222, 247, 259, 282, 291, 320, 321, 340, 381, 415]\n"
     ]
    }
   ],
   "source": [
    "index = error_mnist(pred, test_labels)\n",
    "index_slice = index[:10]\n",
    "print(index[:10])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 1440x720 with 20 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plot_error(index_slice, pred, test_labels)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "DONE"
   ]
  }
 ],
 "metadata": {
  "file_extension": ".py",
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.4"
  },
  "mimetype": "text/x-python",
  "name": "python",
  "npconvert_exporter": "python",
  "pygments_lexer": "ipython3",
  "version": 3
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
