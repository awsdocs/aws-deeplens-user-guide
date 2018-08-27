# What Is AWS DeepLens?<a name="what-is-deeplens"></a>

AWS DeepLens is a wireless\-enabled video camera and development platform integrated with the AWS Cloud\. It lets you use the latest Artificial Intelligence \(AI\) tools and technology to develop computer vision applications based on a deep learning model\. 

As a beginner to machine learning, you can use AWS DeepLens to explore deep learning through hands\-on tutorials based on ready\-deployed deep learning sample projects\. Each sample project contains a pre\-trained model and a pedagogically straightforward inference function\. 

As a seasoned practitioner, you can leverage the AWS DeepLens development platform to train a convolutional neural network \(CNN\) model and deploy to the AWS DeepLens device your computer vision application project containing the model\. You can train the model in any of the [supported deep learning frameworks](deeplens-supported-frameworks.md), including Caffe, MXNet and TensorFlow\.  

To create and run an AWS DeepLens\-based computer vision application project, you typically use the following AWS services:
+ Use the Amazon SageMaker service to train and validate a CNN model or import a pre\-trained model\.
+ Use the AWS Lambda service to create a project function to make inferences of video frames off the camera feeds  against the model\.
+ Use the AWS DeepLens service to create a computer vision application project that consists of the model and inference function\.
+ Use the AWS Greengrass service to deploy the application project and a Lambda runtime to your AWS DeepLens device, as well as the software or configuration updates\. 

This means that you must grant appropriate permissions to access these AWS services\. 

**Topics**
+ [AWS DeepLens Hardware](#deeplens-hardware)
+ [AWS DeepLens Project Workflow](#how-deeplens-works)
+ [Supported Modeling Frameworks](#supported-frameworks)
+ [Learning AWS DeepLens Application Development](#deeplens-first-time-user)
+ [More Info](#what-is-deeplens-related-topics)

## AWS DeepLens Hardware<a name="deeplens-hardware"></a>

The AWS DeepLens  device has the following specs:
+ A 4\-megapixel camera with MJPEG \(Motion JPEG\)
+ 8 GB of on\-board memory
+ 16 GB of storage capacity
+ A 32\-GB SD \(Secure Digital\) card
+ Wi\-Fi support for both 2\.4 GHz and 5 GHz standard dual\-band networking
+ A micro HDMI display port
+ Audio out and USB ports

The following image shows the front and back of the AWS DeepLens hardware\.

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-device-view-front-back.png)

On the front of the device, the power button is located at the bottom\. Above it are three LED indicators show the device statuses:
+ **Power status** indicates if the device is powered on or off\.
+ **Wi\-Fi** shows if the device is in the setup mode \(blinking\), connected to your home or office network \(steady\) or not connected to the Internet \(off\)\. 
+ **Camera status**: when it is on, it indicates that a project is deployed successfully to the device and the project is running\. Otherwise, the LED light is off\.

On the back of the device, you have access to the following:

1. A micro SD card slot for storing and transferring data\.

1. A micro HDMI slot for connecting a display monitor to the device using a micro\-HDMI\-to\-HDMI cable\.

1. Two USB 2\.0 slots for connecting USB mouse and keyboard, or any other USB\-enabled accessories, to the device\.

1. A reset pinhole for turning the device into the setup mode that allows you to make the device as an access point and to connect your laptop to the device to configure the device\.

1. An audio outlet for connecting to a speaker or earphones\.

1. A power supply connector for plugging in the device to an AC power source\.

The AWS DeepLens camera is powered by an Intel® Atom processor, which can process 100 billion floating\-point operations per second \(GFLOPS\)\. This gives you all of the compute power that you need to perform inference on your device\. The micro HDMI display port, audio out, and USB ports allow you to attach peripherals, so you can get creative with your computer vision applications\.

You can use AWS DeepLens as soon as you register it\. Begin by deploying a sample project, and use it as a template  for developing your own computer vision applications\.

## AWS DeepLens Project Workflow<a name="how-deeplens-works"></a>

The following diagram illustrates the basic workflow of a deployed AWS DeepLens  project\.

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-hiw-general.png)

1. When turned on, the AWS DeepLens captures a video stream\.

1. Your AWS DeepLens produces two output streams:
   + **Device stream**—The video stream passed through without processing\.
   + **Project stream**—The results of the model's processing video frames

1. The Inference Lambda function receives unprocessed video frames\.

1. The Inference Lambda function passes the unprocessed frames to the project's deep learning model, where they are processed\.

1. The Inference Lambda function receives the processed frames from the model and passes the processed frames on in the project stream\.  
![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-sample-output.png)

For more information, see [Viewing AWS DeepLens Output Streams](deeplens-viewing-output.md)\.

## Supported Modeling Frameworks<a name="supported-frameworks"></a>

With AWS DeepLens, you train a project model using a supported deep learning modeling framework\. You can train the model on the AWS Cloud or elsewhere\. Currently, AWS DeepLens supports Caffe, TensorFlow and Apache MXNet frameworks, as well Gluon models\. For more information, see [Machine Learning Frameworks Supported by AWS DeepLens](deeplens-supported-frameworks.md)\.

## Learning AWS DeepLens Application Development<a name="deeplens-first-time-user"></a>

If you are a first\-time AWS DeepLens user, we recommend that you do the following in order:

1. **Read [ AWS DeepLens Project Workflow](#how-deeplens-works)**—Explains the basic workflow of a AWS DeepLens project running inference on the device against the deployed deep learning model\.

1. **Explore [Amazon SageMaker](http://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html)**—You use Amazon SageMaker to create and train your own AWS DeepLens CNN model or import a pre\-trained one\. 

1. **Learn about the [AWS DeepLens Device Library](deeplens-device-library.md)**—The device library encapsulates  the classes and methods that you can call in your Lambda functions yo optimize the deployed model artifacts to grab a frame from the device video feeds, and to run inference on the video frame against the model\.

1. **Register your device as prescribed in [Getting Started with AWS DeepLens](deeplens-getting-started.md)**—Register your AWS device, create the AWS Identity and Access Management \(IAM\) roles and policies to grant necessary permissions to run AWS DeepLens and its dependent AWS services\. 

   After you've registered your AWS DeepLens device and configured the development environment, follow the exercises below:

   1. **[Creating and Deploying an AWS DeepLens Sample Project](deeplens-create-deploy-sample-project.md)**—Walks you through creating sample AWS DeepLens project, which is included with your device\.

   1. **[Editing an Existing Model with Amazon SageMaker](deeplens-train-model.md)**—Walks you through creating and training a model using Amazon SageMaker\.

   1. **[Extending any Project's Functionality](deeplens-extend.md)**—Walks you through taking output from your AWS DeepLens and using it to trigger an action\.

## More Info<a name="what-is-deeplens-related-topics"></a>
+ AWS DeepLens is available in the `us-east-1` region\.
+ [AWS DeepLens Forum](https://forums.aws.amazon.com/forum.jspa?forumID=275)