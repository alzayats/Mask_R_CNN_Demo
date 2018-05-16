# Mask_R_CNN_Demo
Created using Colaboratory
Homepage
Hacker Noon
HOMEAIBTCDEVINGOT COIN: BRIDGING MARKETS
Go to the profile of Chengwei Zhang
Chengwei Zhang
Programmer and maker. Love to write deep learning articles.| Website: https://DLology.com | GitHub: https://github.com/Tony607
Mar 26
How to run Object Detection and Segmentation on a Video Fast for Free

TL;DR. After reading this post, you will learn how to run state of the art object detection and segmentation on a video file Fast. Even on an old laptop with an integrated graphics card, old CPU, and only 2G of RAM.


Run like Fast and Furious
So here is the catch. This will only work if you have an internet connection and own a Google Gmail account. Since you are reading this, it is very likely you are already qualified.

All the code in the post runs entirely in the cloud. Thanks to Google’s Colaboratory a.k.a. Google Colab!

I am going to show you how to run our code on Colab with a server-grade CPU , > 10 GB of RAM and a powerful GPU for FREE! Yes, you hear me right.

Using Google Colab with GPU enabled
Colab was build to facilitate machine learning professionals collaborating with each other more seamlessly. I have shared my Python notebook for this post, click to open it.

Log in to your Google Gmail account on the upper right corner if you haven’t done so. It will ask you to open it with Colab at the top of the screen. Then you are going to make a copy so you can edit it.


Now you should be able to click on the “Runtime” menu button to choose the version of Python and whether to use GPU/CPU to accelerate the computation.


The environment is all set. So easy isn’t it? No hassles like installing Cuda and cudnn to make GPU working on a local machine.

Run this code to confirm TensorFlow can see the GPU.


It outputs:

Found GPU at: /device:GPU:0
Great, We are good to go!

If you are curious about what the GPU model you are using. It is a Nvidia Tesla K80 with 24G of memory. Quite powerful.

Run this code to find out yourself.


You will see the 24G graphics memory does help later. It makes possible to process more frames at a time to accelerate the video processing.

Mask R-CNN Demo
The demo is based on the Mask R-CNN GitHub repo. It is an implementation of Mask R-CNN on Keras+TensorFlow. It not only generates the bounding box for a detected object but also generate a mask over the object area.


Install Dependencies and run Demo
Mask R-CNN has some dependencies to install before we can run the demo. Colab allows you to install Python packages through pip, and general Linux package/library through apt-get.

In case you don’t know yet. Your current instance of Google Colab is running on an Ubuntu virtual machine. You can run almost every Linux command you normally do on a Linux machine.

Mask R-CNN depends on pycocotools, we are installing it with the following cell.


It clones the coco repository from GitHub. Install build dependencies. Finally, build and install the coco API library.

All this happens in the cloud virtual machine, and quite fast.

We are now ready to clone the Mask_RCNN repo from GitHub and cd into the directory.


Notice how we change directory with Python script instead of running a shell ‘cd’ command. Since we are running Python in current notebook.

Now you should be able to run the Mask R-CNN demo on colab like you would on a local machine. So go ahead and run it in your Colab notebook.

So far those sample images came from the GitHub repo. But how do you predict with custom images?

Predict with Custom Images
In order to upload a image to Colab notebook, there are three options that I think of.

1. Use a Free image hosting provider like the imgbb.

2.Create a GitHub repo, then download the image link from colab.

After uploading images by either of those two options, you will get a link to the image. Which can be downloaded to your colab VM with Linux wget command. It downloads one image to the ./images folder.

!wget https://preview.ibb.co/cubifS/sh_expo.jpg -P ./images
The first two options will be ideal if you just want to upload 1 or 2 images and don’t care other people on the internet also be able to see it given the link.

3. Use Google Drive

The option is ideal if you have private images/videos/other files to be uploaded to colab.

Run this block to authenticate the VM to connect to your Google Drive.


It will ask for two verification code during the run.

Then execute this cell to mount the Drive to the directory ‘drive’


You can now access your Google drive content in directory ./drive

!ls drive/
Hope you are having fun so far, why not try this on a video file?

Processing Videos
Processing a video file will take three steps.

1. Video to images frames.

2. Process images

3. Turn processed images to output videos.

In our previous demo, we ask the model to process just one image at a time. As configured in the IMAGES_PER_GPU.


If we are going to process the whole video one frame at a time, it will take a long time. So instead we are going to leverage GPU to process multiple frames in parallel.

The pipeline of Mask R-CNN is quite computationally intensive and takes a lot of GPU memory. I find the Tesla K80 GPU on Colab with 24G of memory can safely process 3 images at a time. If you go beyond that the notebook might crash in the middle of processing the video.

So in the code below, we set the batch_size to 3 and use cv2 library to stage 3 images at a time before processing them with the model.


After running this code, you should now have all processed image files in one folder ./videos/save.

The next step is easy, we just need to generate the new video from those images. We are going to use cv2’s VideoWriter to accomplish this.

But two things you want to make sure:

1. The frames need to be ordered in the same way as they are extracted from the original video. (Or backward if you prefer to watch the video that way)


2.The frame rate matches the original video. You can use the following code to check the frame rate of a video or just open the file property.


Finally here is the code to generate the video from processed image frames.


If you have gone this far, the processed video should now be ready to be downloaded to your local machine.



Free free to try your favorite video clip. Maybe intentionally decrease the frame rate when reconstructing the video to watch it in slow motion.

Summary and Further reading
In the post, we walked through how to run your model on Google Colab with GPU acceleration.

You have learned how to do object detection and segmentation on a video. Thanks to the powerful GPU on Colab, made it possible to process multiple frames in parallel to speed up the process.

Further reading
If you want to learn more about the technology behind the object detection and segmentation algorithm.

Here is the original paper of Mask R-CNN goes through the detail of the model.

Or if you just get started with objection detection, check out my object detection/localization guide series goes through important basics shared between many models.

Here again the Python notebook for this post, and GitHub repo for your convenience.

Like this article? Consider 👏 and share with more people.

Share on Twitter Share on Facebook

Originally published at www.dlology.com. For more deep learning experiences like this.

KerasTensorFlowObject Detection
Like what you read? Give Chengwei Zhang a round of applause.
From a quick cheer to a standing ovation, clap to show how much you enjoyed this story.

Go to the profile of Chengwei Zhang
Chengwei Zhang
Programmer and maker. Love to write deep learning articles.| Website: https://DLology.com | GitHub: https://github.com/Tony607

Hacker Noon
Hacker Noon
how hackers start their afternoons.

More from Hacker Noon
12 “Manager READMEs” from Silicon Valley’s Top Tech Companies
Go to the profile of Brennan McEachran
Brennan McEachran
More from Hacker Noon
EOS: Goddess of the Crypto Dawn
Go to the profile of Daniel Jeffries
Daniel Jeffries
More from Hacker Noon
How to Run a Blockchain on a Deserted Island with Pen and Paper
Go to the profile of Tal Kol
Tal Kol
Responses
Hacker Noon
Never miss a story from Hacker Noon, when you sign up for Medium. Learn more
