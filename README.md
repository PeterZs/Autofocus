# Autofocus project

## Hardware

### Raspberry

- Model: Raspberry Pi 3 Model B+

- Memory: 1GB LPDDR2 SDRAM

- Processor: ARMv7 Processor rev 4 (v7l)

### ArduCam

- Resolution: 5MP

- Frame Rate: 30fps@1080px, 60fps@720px

- Picture: 2592x1944 Max

- Focus type: Motorized focus

#### Configuration

Enable the camera on the raspberry. Run a terminal type the next command:

     $ sudo raspi-config

After that select enable camera.

#### Installation

Install python dependency libraries
    
     $ sudo apt-get install python-opencv

Enable the I2C0 port
    
     $ chmod +x enable_i2c_vc.sh
    
     $ ./enable_i2c_vc.sh

Reboot the raspberry after enabling the i2c port.


## Focus measurement

### Laplacian

Laplacian operator is used to measure the second derivative of an image. The Laplacian highlights regions of an image containing rapid intensity changes, like the Sobel and Scharr oeprators. The more an image is blurred, the less edges there are.

### S3

Most of the focus measurement are based on edge detection.
S3 measure can yield a local sharpness map in which greater values correspond to greater preceived sharpness within an image and across different images.

https://sites.google.com/site/cuongvt101/research/Sharpness-measure

### LBP-Based Segmentation

Defocus blur is extremely common in images captured using optical imaging systems. It may be undesirable, but may also be an intentional artistic effect, thus, it can either enhance or inhibit our visual perception of the image scene. For tasks such as image restoration and object recognition, one might want to segment a partially blurred image into blurred and non-blurred regions.

https://www.cs.usask.ca/faculty/eramian/defocusseg/



## Tree Decision Algorithm

1. First we need to train the model. To do that we have done various executions of the script cam.py which automatically generates a csv file with information from the images captured. 


2. Once we have our model saved we can start to test images captured. simulation.py captured three images and returned and array with 'due' step need to be done. If value returned is equal to 0 a small step (1 focus encrease/decrease) should be perform and if it is equal to 1 then it is a big step (3 focus increase/decrease).

<code>

      pi@raspberrypi:~/arducam $ python3 simulation.py 
     FOCUS: 7
     RANDOM FOCUS: 7
     PHOTO 0 DONE
     PHOTO 1 DONE
     PHOTO 2 DONE
     [1 1 1]

</code>
<code>

      pi@raspberrypi:~/arducam $ python3 simulation.py 
     FOCUS: 0
     RANDOM FOCUS: 0
     PHOTO 0 DONE
     PHOTO 1 DONE
     PHOTO 2 DONE
     [0 0 0]

</code>

3. Script takePhoto help us test our results visually. 

<code>

      small   0  | big 1

      backward 0 | fordward 1
   
</code>
<code>

      $ python3 takePhoto.py <focus> <due small|big> <direction backward|fordward>
    
</code>

## Convolutional Network

<p align="center">
  <img src="CNN.png">
  <img src="summary.png">
</p>



1. Script preprocess.py blur images on cifar-10 with diffrent blur kernel and different mask regions. We have prepared an album with 8 different classes. Each class the same 50.000 images but with different characteristics:

    - Gray: Original Images
    - Blur3: Gaussian blur kernel 3
    - Blur15: Gaussian blur kernel 15
    - maskBlur1: Gaussian blur on mask with triangular shape.
    - maskBlur2: Gaussian blur on mask with rectangular shape on top of the image.
    - maskBlur3: Gaussian blur on mask with rectangular shape on center of the image.

Once we have 8 folders representing the eigth classes we want to classificate, we divide each folder into train and test subfolders using the next command in the terminal:

      antonio cifar-10 $ split_folders IMG50K/ --output out/ --ratio .8 .2 --fixed 100


