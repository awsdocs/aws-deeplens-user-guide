# Creating and Deploying an AWS DeepLens Sample Project<a name="deeplens-create-deploy-sample-project"></a>

To help you get started with AWS DeepLens, we provide a number of sample AWS DeepLens project templates that you can use to create projects and get you up and going quickly\. For more information, see [AWS DeepLens Sample Projects Overview](deeplens-templated-projects-overview.md)\.

In this walkthrough, you create the Object Detection project\. The Object Detection project analyzes images within a video stream on your AWS DeepLens device to identify objects\. It can recognize as many as 20 types of objects\.

Though the instructions here are specific to the Object Detection project, you can follow the same steps to create and deploy any of the sample projects\. When creating a sample project, the fields in the console are pre\-populated for you so you can accept the defaults\. In the **Project content** portion of the screen, you need to know the project's model and function\. That information is available for the individual projects in the [AWS DeepLens Sample Projects Overview](deeplens-templated-projects-overview.md) topic\.

Your web browser is the interface between you and your AWS DeepLens device\. You perform all of the following activities on the AWS DeepLens console using your browser\.

![\[\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deeplens-flow-chart-deploy-sample-project.png)

## Step 1: Create Your Project<a name="deeplens-getting-started-create-project"></a>

The following procedure creates the Object Detection sample project\.

**To create an AWS DeepLens project using a sample project**

The steps for creating a project encompass two screens\. On the first screen you select your project\. On the second screen, you specify the project's details\.

1. Using your browser, open the AWS DeepLens console at [https://console\.aws\.amazon\.com/deeplens/](https://console.aws.amazon.com/deeplens/)\.

1. Choose **Projects**, then choose **Create new project**\.

1. On the **Choose project type** screen

   1. Choose **Use a project template**, then choose the sample project you want to create\. For this exercise, choose **Object detection**\.

   1. Scroll to the bottom of the screen, then choose **Next**\.

1. On the **Specify project details** screen

   1. In the **Project information** section:

      1. Either accept the default name for the project, or type a name you prefer\.

      1. Either accept the default description for the project, or type a description you prefer\.

   1. In the **Project content** section:

      1. **Model**—make sure the model is the correct model for the project you're creating\. For this exercise it should be *deeplens\-object\-detection*\. If it isn't, remove the current model then choose **Add model**\. From the list of models, choose *deeplens\-object\-detection*\.

      1. **Function**—make sure the functiion is the correct function for the project you're creating\. For this exercise it should be *deeplens\-object\-detection*\. If it isn't, remove the current function then choose **Add function**\. From the list of functions, choose *deeplens\-object\-detection*\.

   1. Choose **Create**\.

      This returns you to the **Projects** screen where the project you just created is listed with your other projects\.

## Step 2: Deploy Your Project<a name="deeplens-getting-started-deploy-project"></a>

In this walkthrough, you deploy the Object Detection project\. 

Your web browser is the interface between you and your AWS DeepLens device\. You perform all of the following activities on your browser after logging on to AWS:

1. On the **Projects** screen, choose the radio button to the left of your project name, then choose **Deploy to device**\.

1. On the **Target device** screen, from the list of AWS DeepLens devices, choose the radio button to the left of the device that you want to deploy this project to\. An AWS DeepLens device can have only one project deployed to it at a time\.

1. Choose **Review**\.

   If a project is already deployed to the device, you will see an error message that deploying this project will overwrite the project that is already running on the device\. Choose **Continue project**\. 

   This will take you to the **Review and deploy** screen\.

1. On the **Review and deploy** screen, review your project and choose either **Previous** to go back and make changes, or **Deploy** to deploy the project\.
**Important**  
Deploying a project incurs costs for the AWS services that are used to run the project\. 

For instructions on viewing your project's output, see [Viewing AWS DeepLens Output Streams](deeplens-viewing-output.md)\.