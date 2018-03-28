# What Is AWS DeepLens?<a name="what-is-deeplens"></a>

Welcome to the *AWS DeepLens Developer Guide*\. AWS DeepLens is a wireless video camera and API\. It shows you how to use the latest Artificial Intelligence \(AI\) tools and technology to develop computer vision applications\. Through examples and tutorials, AWS DeepLens gives you hands\-on experience using a physical camera to run real\-time computer vision models\. 

The AWS DeepLens camera, or device, uses deep convolutional neural networks \(CNNs\) to analyze visual imagery\. You use the device as a development environment to build computer vision applications\. 

AWS DeepLens works with the following AWS services:

+ Amazon SageMaker, for model training and validation

+ AWS Lambda, for running inference against CNN models

+ AWS Greengrass, for deploying updates and functions to your device

Get started with AWS DeepLens by using any of the pretrained models that come with your device\. As you become proficient, you can develop, train, and deploy your own models\.


+ [AWS DeepLens Hardware](#deeplens-hardware)
+ [Supported Frameworks](#supported-frameworks)
+ [How AWS DeepLens Works](#how-deeplens-works)
+ [Are You an AWS DeepLens First\-time User?](#deeplens-first-time-user)
+ [More Info](#what-is-deeplens-related-topics)

## AWS DeepLens Hardware<a name="deeplens-hardware"></a>

The AWS DeepLens camera includes the following:

+ A 4\-megapixel camera with MJPEG \(Motion JPEG\)

+ 8 GB of on\-board memory

+ 16 GB of storage capacity

+ A 32\-GB SD \(Secure Digital\) card

+ WiFi support for both 2\.4 GHz and 5 GHz standard dual\-band networking

+ A micro HDMI display port

+ Audio out and USB ports

The AWS DeepLens camera is powered by an Intel® Atom processor, which can process 100 billion floating\-point operations per second \(GFLOPS\)\. This gives you all of the compute power that you need to perform inference on your device\. The micro HDMI display port, audio out, and USB ports allow you to attach peripherals, so you can get creative with your computer vision applications\.

You can use AWS DeepLens as soon as you register it\. Begin by deploying a sample project, and use it as an example for developing your own computer vision applications\.

## Supported Frameworks<a name="supported-frameworks"></a>

Currently, AWS DeepLens supports only the Apache MXNet framework and Gluon models\. For more information, see [Machine Learning Frameworks Supported by AWS DeepLens](deeplens-supported-frameworks.md)\.

## How AWS DeepLens Works<a name="how-deeplens-works"></a>

The following diagram illustrates how AWS DeepLens works\.

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-hiw-general.png)

1. When turned on, the AWS DeepLens captures a video stream\.

1. Your AWS DeepLens produces two output streams:

   + **Device stream**—The video stream passed through without processing

   + **Project stream**—The results of the model's processing video frames

     1. The Inference Lambda function receives unprocessed video frames\.

     1. The Inference Lambda function passes the unprocessed frames to the project's deep learning model, where they are processed\.

     1. The Inference Lambda function receives the processed frames from the model and passes the processed frames on in the project stream\.  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-sample-output.png)

For more information, see [Viewing AWS DeepLens Project Output](deeplens-viewing-output.md)\.

## Are You an AWS DeepLens First\-time User?<a name="deeplens-first-time-user"></a>

If you are a first\-time AWS DeepLens user, we recommend that you do the following in order:

1. **Read [How AWS DeepLens Works](#how-deeplens-works)**—Explains AWS DeepLens and how to use it to develop computer vision applications\.

1. **Explore [Amazon SageMaker](http://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html)**—Explains some of the basic functionality of Amazon SageMaker\. AWS DeepLens uses Amazon SageMaker to build and train CNN models\. You use Amazon SageMaker to create your own AWS DeepLens models and projects\.

1. **Learn about the [AWS DeepLens Device Library](deeplens-device-library.md)**—The device library describes the classes and methods that you can use in your Lambda functions\.

1. **Perform the tasks in [Getting Started with AWS DeepLens](deeplens-getting-started.md)**—Set up your AWS account, create the AWS Identity and Access Management \(IAM\) permissions and roles that you need to run AWS DeepLens, and register and set up your AWS DeepLens device\.

   After you've set up your AWS DeepLens environment and device, begin using it by trying these exercises:

   1. **[Creating and Deploying an AWS DeepLens Sample Project](deeplens-create-deploy-sample-project.md)**—Walks you through creating sample AWS DeepLens project, which is included with your device\.

   1. **[Editing an Existing Model with Amazon SageMaker](deeplens-train-model.md)**—Walks you through creating and training a model using Amazon SageMaker\.

   1. **[Extending any Project's Functionality](deeplens-extend.md)**—Walks you through taking output from your AWS DeepLens and using it to trigger an action\.

## More Info<a name="what-is-deeplens-related-topics"></a>

+ [AWS DeepLens Forum](https://forums.aws.amazon.com/forum.jspa?forumID=275)
