# Deploying an AWS DeepLens Project<a name="deeplens-deploy-project"></a>

In this walkthrough, you deploy the Object Detection project\. The Object Detection project analyzes images within a video stream on your AWS DeepLens device to identify objects\. It can recognize as many as 20 types of objects\. The steps to deploying any AWS DeepLens project are the same as we use here to deploy the Object Detection project\.

Your web browser is the interface between you and your AWS DeepLens device\. You perform all of the following activities on your browser after logging on to AWS:

1. On the **Projects** screen, choose the radio button to the left of your project name, then choose **Deploy to device**\.

1. On the **Target device** screen, from the list of AWS DeepLens devices, choose the radio button to the left of the device that you want to deploy this project to\. An AWS DeepLens device can have only one project deployed to it at a time\.

1. Choose **Review**\.

   If a project is already deployed to the device, you will see an error message that deploying this project will overwrite the project that is already running on the device\. Choose **Continue project**\. 

   This will take you to the **Review and deploy** screen\.

1. On the **Review and deploy** screen, review your project and choose either **Previous** to go back and make changes, or **Deploy** to deploy the project\.
**Important**  
Deploying a project incurs costs for the AWS services that are used to run the project\. 

For instructions on viewing your project's output, see [Viewing AWS DeepLens Project Output](deeplens-viewing-output.md)\.