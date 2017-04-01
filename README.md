# Tankbuster

Tankbuster is a neural network trained to recognize Soviet/Russian <a href="http://en.wikipedia.org/wiki/T-72">T-72</a> main battle tanks and <a href="http://en.wikipedia.org/wiki/BMP_development">BMP</a> armored personnel carriers in photographs.

Built using <a href="http://keras.io">Keras</a>, the network has been trained using a collection of images showing T-72s (1457 images) and BMPs (1088 images) from various angles and at various distances against a collection of photographs featuring street and natural scenes (1477 images). 

Tankbuster provides two different architectures and models for recognizing the aforementioned vehicles / classes. 

The first alternative is a small convolutional neural net (ConvNet), which has been trained using augmented data, generating additional training data from the source images by introducing random shifts, flips, zooms and shears. The convolutional neural network achieves a 10-fold validation accuracy of 79%.

The second alternative is a model based on a 50-layer residual neural network pre-trained on ImageNet (ResNet), which ships with Keras. <a href="https://keras.io/applications/#resnet50">This network</a> has been used as a feature extractor, recording the output from the final average pooling layer and training a fully-connected block on top. The model achieves a 10-fold validation accuracy of 95%!

While the convolutional neural network is faster, especially when running tankbuster on a CPU, the residual neural network provides superior performance, and frankly, generalizes much better.

## Installation

<b>Note: to keep your system clean, it is sincerely recommended to install Tankbuster into its own <a href="http://docs.python-guide.org/en/latest/dev/virtualenvs/">virtual environment</a>.</b> 

Assuming you have <a href="https://pip.pypa.io/en/stable/installing/">installed pip</a>, Tankbuster may be installed from the Python Package Index (PyPI) by typing the following command on the command line:

<code>pip install tankbuster</code>

This will also install the required packages, including Keras (2.0.2) and TensorFlow (1.0.1).

Naturally, you can also clone or download the <a href="https://github.com/thiippal/tankbuster">GitHub repository</a> to get the latest version. Enter the following command to install Tankbuster and the required packages:

<code>pip install .</code>

## Usage

### Running Tankbuster from the command line

If you wish to run Tankbuster from the command line, it is best to clone this repository to get the driver script, <code>bust.py</code>.

To feed a single image to the neural network, use the command line prompt to enter the following command:

<code>python bust.py -i image.jpg -n ResNet</code>

Alternatively, to use the faster convolutional neural network, replace "ResNet" with "ConvNet".

To examine all images in a directory, simply enter a directory name instead of the filename:

<code>python bust.py -i directory -n ResNet</code>

### Integrating Tankbuster into your own program

If you wish to integrate Tankbuster into your Python program, import the key function of the module using:

<code>from tankbuster import bust</code>

This allows you to call the <i>bust</i> function, which takes a path to an image as the input:

<code>bust('image.png', network="ResNet")</code>

Again, if you wish to use the convolutional neural network instead of the residual neural network, pass the "ResNet" to the parameter <i>network</i> instead of "ConvNet".

The <i>bust</i> function returns a dictionary of predicted class labels with their associated probabilities. These labels and probabilities can be used as the basis for further actions, such as flagging the image for further inspection.

## In action

<b>Parked next to a Lada</b><br>
ResNet: predicted T-72, probability 98.89%.<br>
ConvNet: predicted T-72, probability 95.44%.


<image src="demo_images/with_lada.jpg" width="400px">

<b>Featured in a low-quality screen capture</b><br>
ResNet: predicted T-72, probability 97.42%<br>
ConvNet: predicted BMP, probability 75.57%.


<image src="demo_images/from_screen_capture.png" width="400px">

<b>Parked in the backyard</b><br>
ResNet: predicted BMP, probability 98.89%.<br>
ConvNet: predicted BMP, probability 49.17%.


<image src="demo_images/backyard.jpg" width="400px">

<b>Knocked out in group</b><br>
ResNet: predicted BMP, probability 99.14%.<br>
ConvNet: predicted T-72, probability 41.63%.


<image src="demo_images/knocked_out.jpg" width="400px">
