# AWS DeepLens Sample Projects Overview<a name="deeplens-templated-projects-overview"></a>

To get started with AWS DeepLens, use the sample project templates\. AWS DeepLens sample projects are projects where the model is pre\-trained so that all you have to do is create the project, import the model, deploy the project, and run the project\. Other sections in this guide teach you to extend a sample project's functionality so that it performs a specified task in response to an event, and train a sample project to do something different than the original sample\. 

## Artistic Style Transfer<a name="artistic-style-transfer"></a>

This project transfers the style of an image, such as a painting, to an entire video sequence captured by AWS DeepLens\.

This project shows how a [Convolutional Neural Network](http://gluon.mxnet.io/chapter04_convolutional-neural-networks/cnn-scratch.html) \(CNN\) can apply the style of a painting to your surroundings as it's streamed with your AWS DeepLens device\. The project uses a pretrained optimized model that is ready to be deployed to your AWS DeepLens device\. After deploying it, you can watch the stylized video stream\.

You can also use your own image\. After fine tuning the model for the image, you can watch as the CNN applies the image's style to your video stream\.

+ **Project model:** deeplens\-artistic\-style\-transfer

+ **Project function:** deeplens\-artistic\-style\-transfer

## Object Recognition<a name="object-recognition"></a>

This project shows you how a deep learning model can detect and recognize objects in a room\.

The project uses the [Single Shot MultiBox Detector \(SSD\)](http://gluon.mxnet.io/chapter08_computer-vision/object-detection.html) framework to detect objects with a pretrained [resnet\_50 network](https://mxnet.incubator.apache.org/versions/master/api/python/gluon/model_zoo.html)\. The network has been trained on the [Pascal VOC](http://host.robots.ox.ac.uk/pascal/VOC/) dataset and is capable of recognizing 20 different kinds of objects\. The model takes the video stream from your AWS DeepLens device as input and labels the objects that it identifies\. The project uses a pretrained optimized model that is ready to be deployed to your AWS DeepLens device\. After deploying it, you can watch your AWS DeepLens model recognize objects around you\.

The model is able to recognize the following objects: airplane, bicycle, bird, boat, bottle, bus, car, cat, chair, cow, dining table, dog, horse, motorbike, person, potted plant, sheep, sofa, train, and TV monitor\.

+ **Project model:** deeplens\-object\-dectection

+ **Project function:** deeplens\-object\-dectection

## Face Detection and Recognition<a name="face-detection-recognition"></a>

With this project, you use a face detection model and your AWS DeepLens device to detect the faces of people in a room\.

The model takes the video stream from your AWS DeepLens device as input and marks the images of faces that it detects\. The project uses a pretrained optimized model that is ready to be deployed to your AWS DeepLens device\. 

+ **Project model:** deeplens\-face\-detection

+ **Project function:** deeplens\-face\-detection

## Hot Dog Recognition<a name="hot-dog-not-hot-dog"></a>

Inspired by a popular television show, this project classifies food as either a hot dog or not a hot dog\.

It uses a model based on the [SqueezeNet deep neural network](http://gluon.mxnet.io/chapter08_computer-vision/fine-tuning.html)\. The model takes the video stream from your AWS DeepLens device as input, and labels images as a hot dog or not a hot dog\. The project uses a pretrained, optimized model that is ready to be deployed to your AWS DeepLens device\. After deploying the model, you can use the Live View feature to watch as the model recognizes hot dogs \.

You can edit this model by creating Lambda functions that are triggered by the model's output\. For example, if the model detects a hot dog, a Lambda function could send you an SMS message\. To learn how to create this Lambda function, see [Editing an Existing Model with Amazon SageMaker](deeplens-train-model.md)

## Cat and Dog Recognition<a name="cat-or-dog"></a>

This project shows how you can use deep learning to recognize a cat or a dog\.

It is based on a convolutional neural network \(CNN\) architecture and uses a pretrained * [Resnet\-152](http://docs.aws.amazon.com/general/latest/gr/glos-chap.html#resnet-152)* topology to classify an image as a cat or a dog\. The project uses a pretrained, optimized model that is ready to be deployed to your AWS DeepLens device\. After deploying it, you can watch as AWS DeepLens uses the model to recognize your pets\.

+ **Project model:** deeplens\-cat\-and\-dog\-recognition

+ **Project function:** deeplens\-cat\-and\-dog\-recognition

## Action Recognition<a name="action-recognition"></a>

This project recognizes more than 30 kinds of activities\.

It uses the Apache MXNet framework to transfer learning from a SqueezeNet trained with ImageNet to a new task\. The network has been tuned on a subset of the UCF101 dataset and is capable of recognizing more than 30 different activities\. The model takes the video stream from your AWS DeepLens device as input and labels the actions that it identifies\. The project uses a pretrained, optimized model that is ready to be deployed to your AWS DeepLens device\.

After deploying the model, you can watch your AWS DeepLens use the model to recognize 37 different activities, such as applying makeup, applying lipstick, participating in archery, playing basketball, bench pressing, biking, playing billiards, blowing drying your hair, blowing out candles, bowling, brushing teeth, cutting things in the kitchen, playing a drum, getting a haircut, hammering, handstand walking, getting a head massage, horseback riding, hula hooping, juggling, jumping rope, doing jumping jacks, doing lunges, using nunchucks, playing a cello, playing a flute, playing a guitar, playing a piano, playing a sitar, playing a violin, doing pushups, shaving, skiing, typing, walking a dog, writing on a board, and playing with a yo\-yo\.

+ **Project model:** deeplens\-action\-recognition

+ **Project function:** deeplens\-action\-recognition