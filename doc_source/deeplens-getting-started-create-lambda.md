# Create a Lambda Function and Deploy a Custom Trained Model to AWS DeepLens<a name="deeplens-getting-started-create-lambda"></a>

This topic explains how to add an [ AWS Lambda inference function](https://console.aws.amazon.com/lambda/#/discover) to your custom AWS DeepLens project\. The Lambda function helps you make an inference from frames in a video stream captured by your AWS DeepLens device\. If an existing and published Lambda function meets your application requirements, you can use it instead\. 

## Prerequisites<a name="lambda-function-prerequisites"></a>

In order to successfully complete this section you need the following:
+ A trained model `.tar.gz` file that is stored in an Amazon Simple Storage Service \(Amazon S3\) bucket with a name starting with `deeplens-`\.
+ To download [https://s3.amazonaws.com/deeplens-public/docs/deeplens-image-classification-lambda.zip](https://s3.amazonaws.com/deeplens-public/docs/deeplens-image-classification-lambda.zip) template file\. 

  If you modify the `lambda_function.py` in the Lambda code editor, ensure that it continues to import the `awscam` module before the `cv2` module\. 

## Link Your Custom Trained Model in the AWS DeepLens Console<a name="lambda-function-link-trained-model"></a>

To successfully deploy your project to your AWS DeepLens device you need link your trained model to your AWS DeepLens device\. This requires having a trained model in an Amazon S3 bucket added to the AWS DeepLens console\. 

1. Open the [AWS DeepLens console](https://console.aws.amazon.com/deeplens/#devices)\.

1.  In the navigation pane, choose **Models**\. 

1. Next, choose **Import model**\. 

1. On the **Import source** page, choose **Externally trained model**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deploy-model-1-select-model.png)

1. In the **Model artifact path** field, enter the path to your model in Amazon S3\. Your path should end in the following, `.tar.gz`\. 

1. Enter a **Model name**, and then select **MXNet** under **Model framework**\.

1. Choose **Import model**\.

## Create an AWS Lambda Function<a name="lambda-function-create"></a>

An Lambda function enables AWS DeepLens to process the video being captured on your AWS DeepLens device\. In this procedure, you use the Lambda console to create the Lambda function, and then upload the `deeplens-lambda.zip` file\. This file contains the necessary python scripts to process the video being captured on your AWS DeepLens device\. 

**Note**  
If you haven't done so already, make sure you have downloaded [https://s3.amazonaws.com/deeplens-public/docs/deeplens-image-classification-lambda.zip](https://s3.amazonaws.com/deeplens-public/docs/deeplens-image-classification-lambda.zip) template file\. 

**Creating a new Lambda function using the `deeplens-lambda.zip` file**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. In the navigation panel, choose **Create function**\.

1. On the **Create function** page, choose **Author from scratch**\.

1. In **Basic information**, enter a **Name** for you function\.

1. Under **Runtime**, choose **Python 2\.7**, and then open the **Choose or create an execution role** panel\.

1. Choose **Use an existing role**, and then choose **service\-role/AWSDeepLensLambdaRole**\.  
![\[Selecting the correct service role for your Lambda service.\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deploy-model-2-function-inputs.png)

1.  Choose **Create function**

1. In the **Function code** panel on the next page, change the default value in the **Code entry type** to **Upload a \.zip** file from the default **Edit code inline**\. 

1. Choose **Save** to save the code you entered\.

1. At the top of the page,choose **Actions**, and then choose **Publish new version**\. Now your function is available in your AWS DeepLens console and you can add it to your custom project\. 

## Create a New AWS DeepLens Project<a name="create-deeplens-project"></a>

1. In the navigation pane in the [AWS DeepLens console](https://us-east-1.console.aws.amazon.com/deeplens/home?region=us-east-1#devices), choose **Projects**, choose **Create a new blank project**, and then choose **Next**\.

1. Enter a **Project name** and, optionally, a **Description**\.

1. Choose **Project content** and then do the following to connect the custom trained model from Amazon SageMaker and the AWS Lambda function: 
   + Choose **Add model**, select your model's name, and then choose **Add model** again\.
   + Choose **Add function**, search for your AWS Lambda function by name, and then choose **Add function**\.

1. Choose **Create** to finish creating your custom AWS DeepLens project\.

## Deploy the Custom AWS DeepLens Project<a name="deploy-deeplens-project"></a>

1. In the navigation pane in the [AWS DeepLens console](https://us-east-1.console.aws.amazon.com/deeplens/home?region=us-east-1#devices), choose **Projects**, choose the project you would like to deploy to your AWS DeepLens device, and then select **Deploy to device**\. 

1.  On the **Target device screen**, choose your device from the list, and then choose **Review**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/deeplens/latest/dg/images/deploy-model-3-select-deeplens.png)

1.  On the **Review and deploy page**, choose **Deploy**\.

*Next Steps*
+ At this point in the tutorial, you should have successfully completed the following:
  + Registered your AWS DeepLens device
  + Created an Amazon S3 bucket
  +  Created an Amazon SageMaker instance
  + Requested a Service limit increase for a GPU instance
  + Trained your custom image classification model and saved the model output to the correct S3 bucket
  + Created an AWS Lambda function
  + Deployed your AWS Lambda function and your model to your AWS DeepLens device

Now that you’ve deployed the custom model and the AWS Lambda function to your AWS DeepLens, users can view the output\. The AWS DeepLens device uses AWS IoT Greengrass to send back inference results\. We can view the results by connecting to AWS IoT Greengrass console\.