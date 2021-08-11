# Create and Deploy a Custom Image Classification Model Using AWS DeepLens \(\~ 2hrs\)<a name="deeplens-getting-started-hard"></a>

 AWS DeepLens is a deep learning enabled video camera that is integrated with AWS machine learning services\. The AWS DeepLens device enables users to create end\-to\-end computer vision based machine learning projects\. If you are new to computer vision projects or the AWS machine learning services you can get started by [creating and deploying a sample project in the AWS DeepLens console](deeplens-get-start-easy.md)\. 

**Note**  
To use AWS DeepLens console and other AWS services, you need an AWS account\. If you don't have an account, visit [aws\.amazon\.com](https://aws.amazon.com/) and choose **Create an AWS Account**\. For detailed instructions, see [Create and Activate an AWS Account](http://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)\.   
As a best practice, you should also create an AWS Identity and Access Management \(IAM\) user with administrator permissions and use that for all work that does not require root credentials\. Create a password for console access, and access keys to use command line tools\. For more information, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\. 

**Project Workflow**

To get started with creating a custom image classification model using AWS DeepLens you need to use several different AWS services\. This tutorial guides you through setting up the different components required from start to finish\. 

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/readme-1-project-workflow.png)

**Topics**
+ [Set Up the Project Data Store in Amazon Simple Storage Service \(S3\)\.](deeplens-getting-started-create-s3.md)
+ [Train a Model in Amazon SageMaker](deeplens-getting-started-launch-sagemaker.md)
+ [Create a Lambda Function and Deploy a Custom Trained Model to AWS DeepLens](deeplens-getting-started-create-lambda.md)
+ [Using the AWS IoT Greengrass Console to View the Output of Your Custom Trained Model \(Text Output\)](deeplens-getting-started-connect-iot-greengrass.md)